# Case Study 04- Café Acceptance and Rejection Logic 

 ## Overview

This case study documents the system-level logic used to handle café responses after a user confirms an order in the Tasleem platform.

The focus is on decision handling, timeouts, and multi-actor system behavior.

---

## Scenario Description

After order confirmation, the system forwards the order to the selected café to verify availability and readiness.

From this point onward, the café becomes an independent decision actor.

---

## Problem Statement

At this stage:

The order data is valid

The user expects progress

The system cannot proceed without café approval

The system must handle three possible café behaviors:

Acceptance

Rejection

No response

---


## System Constraints

The system must:

Wait for café response within a defined time window

Explicitly handle acceptance and rejection

Avoid indefinite waiting

Protect the user from third-party inaction

---

## Decision States
```
Café State	Description	System Action
Accepted	Café confirms availability	Proceed to next phase
Rejected	Café declines order	Cancel order and notify user
No Response	Café does not reply	Cancel after timeout and suggest alternatives
```
---

## Interaction Flow
```
System sends order to café

System enters waiting state

Café responds or remains silent

System evaluates response

System proceeds, cancels, or exits based on outcome
```
---

## Design Rationale

Third-party silence is treated as a system state

Timeouts prevent hanging processes

Explicit rejection avoids ambiguity

User experience is protected from external delays

---


## Key Takeaways

Multi-actor systems require strict decision boundaries

Not all failures are errors — some are non-responses

Systems must exit responsibly when control is shared

Clear termination paths are essential for trust

---

## Relevance to Applied Computer Science

This case demonstrates:

Real-world decision modeling

Handling uncertainty in distributed systems

Designing for failure before success

Translating human workflows into enforceable system logic
