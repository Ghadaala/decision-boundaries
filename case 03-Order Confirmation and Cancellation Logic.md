# Case 03-Order Confirmation and Cancellation Logic

## Overview

This case study documents the design logic behind handling order confirmation and cancellation within the Tasleem system.

The focus is on decision modeling, not interface design or implementation.

## Scenario Description

After a user completes an order selection, the system enters a confirmation phase where the user must explicitly choose whether to proceed or cancel.

At this point, the system holds valid data but lacks a final decision.
---

## Problem Statement

The confirmation stage introduces a critical system challenge:

Proceeding without confirmation violates correctness

Waiting indefinitely creates hanging states

Assuming intent introduces risk

Therefore, the system must manage this phase with clear decision rules.

---

## System Constraints

The system must:

Require explicit confirmation to proceed

Allow explicit cancellation

Handle inactivity without indefinite waiting

Ensure every path leads to a defined termination or transition

---

## Decision States

The system recognizes three distinct states:

State	Description	System Action
Confirmed	User confirms the order	Proceed to next phase
Cancelled	User cancels the order	Terminate order
Inactive	User takes no action	Retry with limit, then cancel

No other states are permitted.

---

## Interaction Flow


```

System displays order summary

System presents confirmation and cancellation options

System waits for user input

One of the following occurs:

Confirmation → transition forward

Cancellation → terminate order

No response → reminder loop with exit condition


```

---

## Design Rationale

Explicit actions prevent ambiguity

Time-bound waiting prevents hanging states

Cancellation on inactivity protects system stability

Clear exits simplify later integration and testing

---

## Key Takeaways

Decision points must be explicitly modeled

Inactivity is a system state, not a null event

Every system path must have a defined outcome

Logical correctness precedes implementation

---

## Relevance

This case reflects applied computer science practice:

Translating human behavior into system rules

Designing safe decision boundaries

Preventing failure through structure, not complexity
