# Example: Autonomous Agent (Tier C, Self-Confirming)

> A Copilot coding agent picks up a well-specified ticket and runs Preflight without human interaction.

## The Ticket

**Issue #247:** Add `updatedAt` timestamp to User model

**Acceptance criteria:**
- Add `updated_at` column to `users` table (timestamp, not null, default now)
- Update `User` struct in `models/user.go` with `UpdatedAt time.Time` field
- Set `UpdatedAt` to `time.Now()` in `UpdateUser()` function
- Add migration file
- Add test verifying `UpdatedAt` changes after update

## Preflight (logged in PR body)

### Phase 1: Orient

Read `models/user.go` (User struct, lines 12-24), `models/user.go:UpdateUser` (lines 45-62), `migrations/` directory (latest: `003_add_email_verified.sql`), and `models/user_test.go`.

Current User struct has fields: `ID`, `Email`, `Name`, `CreatedAt`. No `UpdatedAt` field exists. Migrations use sequential numbering. Tests use a test database with `setupTestDB()` helper.

### Phase 2: Clarify

Ticket spec is complete:
- Column type specified (timestamp, not null, default now)
- Struct field specified (`UpdatedAt time.Time`)
- Function to modify specified (`UpdateUser()`)
- Test requirement specified
- No ambiguity detected. Proceeding.

### Phase 3: Approach

I'll add migration `004_add_updated_at.sql`, add the struct field, set it in `UpdateUser()`, and add a test. Single approach — the ticket fully specifies the implementation.

### Phase 4: Confirm

Plan logged. Files:
- Create: `migrations/004_add_updated_at.sql`
- Modify: `models/user.go` (struct + UpdateUser function)
- Modify: `models/user_test.go` (add TestUpdateUser_SetsUpdatedAt)

All acceptance criteria will be met. Proceeding.

### Phase 5: Execute

*Agent writes the migration, struct update, function update, and test.*

## Key Difference

The autonomous agent didn't ask questions — but it still verified the spec was complete (Phase 2) and logged its plan (Phase 4) before writing code. If the ticket had been missing acceptance criteria, the agent would have labeled it `needs-spec` and stopped.
