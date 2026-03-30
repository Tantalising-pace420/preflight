# Example: Simple Bug Fix (Minimal Preflight)

> A developer asks their AI agent to fix a null pointer panic. The agent runs Preflight in under 30 seconds.

## The Task

**User:** "Fix the nil pointer panic in `handleOrder` — it crashes when `customer.Address` is nil."

## Preflight

### Phase 1: Orient

**Agent:** I read `api/orders.go:handleOrder` (line 47-82) and `api/orders_test.go`. The handler accesses `customer.Address.City` on line 63 without a nil check. The test file has 4 tests but none cover a nil address case.

### Phase 2: Clarify

**Agent:** The fix is straightforward — should I return a 400 error when the address is nil, or use a default empty address? This affects whether orders can be placed without an address.

**User:** Return a 400 — address is required.

### Phase 3: Approach

**Agent:** I'll add a nil check for `customer.Address` at line 62 that returns a 400 with message "customer address is required". One approach, simple guard clause.

### Phase 4: Confirm

**Agent:** Plan: add nil check in `api/orders.go:62`, add test case `TestHandleOrder_NilAddress` in `api/orders_test.go`. Shall I proceed?

**User:** Go.

### Phase 5: Execute

*Agent writes the nil check and test.*

## Without Preflight

The agent would have immediately added the nil check — but might have chosen to use a default empty address instead of returning 400, requiring rework when the user wanted validation.
