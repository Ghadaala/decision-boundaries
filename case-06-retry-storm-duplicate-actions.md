# Case 06 — Retry Storm & Duplicate Actions  
## When “Try Again” Turns a Single Failure Into System-Wide Damage

---

## Context

In many operational systems, retry logic is treated as a technical safeguard rather than a **decision boundary**.  
When a request fails or times out, the system (or the user) attempts the same action again — often without clear ownership, limits, or guarantees of safety.

This case explores how **uncontrolled retries** can silently escalate into duplicated actions, data corruption, or financial loss.

---

## Scenario

A client submits a request (payment, booking, confirmation, or submission).

The system does not return a clear success or failure signal within the expected time window.

From the client’s perspective, the action appears unfinished.

From the system’s perspective, the request may still be processing — or may have partially completed.

At this point, a retry is triggered.

## Decision Flow (Simplified)


```

Client submits request
|
v
Service processes request
| |
Success Timeout / Error
| |
v v
Return OK Retry decision point
|
+---------+----------+
| |
Retry immediately Stop & escalate
| |
v v
Request resent Human review / user prompt
|
v
Idempotency enforced?
| |
Yes No
| |
v v
Safe retry Duplicate action
(no side effects) (double charge / order)

```
---

## Failure Points

1. **Ambiguous request state**  
   The system cannot clearly confirm whether the original request completed.

2. **Retry without idempotency**  
   The same request is executed again as if it were new.

3. **Automatic retries without limits**  
   Multiple retries occur faster than the system can resolve state.

4. **User-driven retries without system awareness**  
   The user clicks “Try again” while the backend is still processing.

5. **Lack of escalation path**  
   The system keeps retrying instead of stopping and surfacing uncertainty.

---

## Why This Is a Decision Problem (Not Just a Technical One)

Retry logic answers a critical question:

**Who is allowed to decide that an action may happen again?**

Without explicit rules, retries become invisible decisions that:
- Re-trigger irreversible actions
- Mask real failures
- Transfer operational risk to the user or downstream systems

---

## Decision Rules That Prevent Retry Storms

- **Idempotency keys are mandatory** for all non-reversible actions.
- Retries must use **exponential backoff**, never immediate repetition.
- A retry limit must exist — after which escalation replaces automation.
- Retries must be blocked if request payload changes.
- The system must explicitly communicate uncertainty to the user.
- Human intervention must be a valid terminal state, not a failure.

---

## Testing the Boundary

- Simulate delayed responses and network timeouts.
- Force partial success scenarios.
- Verify that retries do not duplicate side effects.
- Test user behavior under ambiguous feedback.
- Confirm that escalation triggers correctly after retry limits.

---

## Takeaway

Retries are not harmless.

Every retry is a **decision to act again**.

When that decision is left implicit, systems multiply damage instead of resolving failure.

Strong systems do not retry endlessly —  
they **know when to stop**.
