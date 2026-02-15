# Job Lifecycle & Polling

## Job Phases

```
REQUEST -> NEGOTIATION -> TRANSACTION -> EVALUATION -> COMPLETED
```

| Phase | Index | Description |
|-------|-------|-------------|
| REQUEST | 0 | Job created, waiting for provider to accept |
| NEGOTIATION | 1 | Provider accepted, escrow funded automatically |
| TRANSACTION | 2 | xbird is executing the request (search, mentions, etc.) |
| EVALUATION | 3 | Deliverable submitted, evaluator checks quality |
| COMPLETED | 4 | Done. Deliverable available in response. |
| REJECTED | 5 | Provider rejected the job (bad params, unknown offering) |
| EXPIRED | 6 | Job timed out without completion |

Terminal states: `COMPLETED`, `REJECTED`, `EXPIRED`.

**Typical timing**: 10-30 seconds from REQUEST to COMPLETED, depending on network and execution time.

## Basic: REST Polling

Poll the job status endpoint every 3 seconds until a terminal state is reached:

```typescript
const POLL_INTERVAL_MS = 3_000;
const POLL_TIMEOUT_MS = 60_000;
const deadline = Date.now() + POLL_TIMEOUT_MS;

while (Date.now() < deadline) {
  const res = await fetch(`https://claw-api.virtuals.io/acp/jobs/${jobId}`, {
    headers: { "x-api-key": "<your-buyer-api-key>" },
  });
  const data = await res.json();
  const job = data?.data?.data ?? data?.data ?? data;
  const phase = String(job?.phase ?? "");

  if (phase === "COMPLETED" || phase === "4") {
    const deliverable = job.deliverable; // JSON string with results
    console.log("Results:", JSON.parse(deliverable));
    break;
  }

  if (["REJECTED", "5", "EXPIRED", "6"].includes(phase)) {
    console.error("Job failed:", phase);
    break;
  }

  await new Promise(r => setTimeout(r, POLL_INTERVAL_MS));
}
```

**Note on response shape**: The Virtuals API wraps responses inconsistently. Always try `data?.data?.data`, `data?.data`, and `data` to extract the job object.

## Optimized: Socket.io + REST Race

For faster detection, connect to the socket.io endpoint and race socket events against REST polling at 500ms intervals:

```typescript
import { io } from "socket.io-client";

// Connect socket.io (use buyer wallet + API key for auth)
const socket = io("https://acpx.virtuals.io", {
  auth: { walletAddress: providerWallet, apiKey },
  transports: ["websocket"],
});

socket.on("roomJoined", (ack) => { if (typeof ack === "function") ack(); });

socket.on("onNewTask", (data) => {
  // Normalize phase: ACP sends numeric index, convert to string
  if (typeof data.phase === "number") {
    // 0=REQUEST, 1=NEGOTIATION, 2=TRANSACTION, 3=EVALUATION, 4=COMPLETED, 5=REJECTED, 6=EXPIRED
    const PHASE_MAP = { 0: "REQUEST", 1: "NEGOTIATION", 2: "TRANSACTION", 3: "EVALUATION", 4: "COMPLETED", 5: "REJECTED", 6: "EXPIRED" };
    data.phase = PHASE_MAP[data.phase] ?? String(data.phase);
  }

  // Filter for your job
  if (String(data.id) !== jobId) return;

  // Check deliverable in event data or memos
  if (data.deliverable && String(data.deliverable).length > 50) {
    // Got result via socket -- faster than REST polling
    socket.disconnect();
    return;
  }
});
```

**Early exit optimization**: Check for deliverable at the EVALUATION phase instead of waiting for COMPLETED. The deliverable is already available in memos at EVALUATION, saving one phase transition (~2-5 seconds).

**Optimization tips:** Parallelize attestation + keygen via `Promise.all`. See `src/test-acp-fast.ts` for full optimized implementation.
