# Preflight Protocol

> Append this section to your existing .github/copilot-instructions.md, or use this file as-is.

Before writing any code for a new task, you MUST complete these 5 phases in order. Do NOT create, modify, or delete any files until Phase 5.

## Phase 1: Orient

Read relevant files, docs, and recent changes to understand the current state before proposing anything.

- Bug fix: read the failing code, related tests, recent commits to the area
- New feature: read architecture docs, similar features, the module you'll modify
- Refactor: read all code that will change and all dependent code

State what you found before proceeding.

## Phase 2: Clarify

Ensure purpose, constraints, and success criteria are clear.

When working from a ticket or issue:
- Verify all acceptance criteria are present and unambiguous
- If the spec is underspecified, label it `needs-spec` and stop — do not guess
- If all criteria are clear, state the understood requirements

## Phase 3: Approach

State your implementation approach with brief rationale before writing code. For well-specified tickets, one approach is sufficient.

Example: "I'll add the validation in `handler.go:42` and test in `handler_test.go` because that's where the existing input processing lives."

## Phase 4: Confirm

Log your plan in the PR body under an "## Implementation Plan" section:
- Files to create or modify
- What changes in each file
- How to verify (which tests cover the change)

Proceed after logging the plan.

## Phase 5: Execute

Now write the code. This is the first phase where files may be created or modified.

## Rules

1. Phases 1-4 produce zero code changes. Reading is encouraged, writing is not.
2. Each new task triggers a fresh Preflight pass. Continuing the same task does not re-trigger.
3. Even trivial tasks go through all 5 phases — each phase can be a single sentence.
