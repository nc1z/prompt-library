Create or replace `IMPLEMENTATION_PLAN.md` for the selected take-home issues the user wants to solve.

The user should provide the issue titles, issue table rows, fix cards, audit findings, or a short list of problems to fix. Treat those selected problems as the scope. If the user does not provide a selection and `ROOT_CAUSE_DEEP_DIVE.md` contains exactly one fix card, use that fix card. If multiple possible issues exist and the user has not selected one, stop and ask for the issue selection. Do not broaden the scope unless a small supporting task is required to test or safely fix the selected issue.

Do not modify application code in this prompt. This prompt only creates the plan.

Use this workflow:

1. Understand the selected scope.
   - Read the user's selected problems first.
   - Read `REQUIREMENTS_SUMMARY.md`, `ISSUE_PRIORITY.md`, `ROOT_CAUSE_DEEP_DIVE.md`, and `DISCOVERY_CONTEXT.md` when present.
   - Read any issue descriptions or assessment task notes named by the user.
   - Read relevant source files, tests, config, and scripts for each selected issue.
   - Identify the observable symptom, likely root cause, affected behavior, and acceptance criteria for each issue.

2. Build a spec-driven TDD plan.
   - For each issue, define the desired behavior before implementation details.
   - Prefer a failing test or focused regression test before code changes.
   - If a failing test is not practical in the timebox, require a clear smoke check and state why automated coverage is not practical.
   - Break each issue into small subtasks that can be executed by `plan-execute/execute-step.prompt.md`.
   - Group subtasks into issue-level phases that can be executed by `plan-execute/execute-phase.prompt.md`.

3. Keep changes surgical.
   - List exact files likely to change.
   - Preserve existing APIs and behavior unless the selected issue requires changing them.
   - Include compatibility, migration, or rollout notes for auth, security, data, API, or dependency changes.
   - Include non-goals so the executor does not refactor nearby code.

4. Create `IMPLEMENTATION_PLAN.md` using this structure:

```markdown
# Implementation Plan

## Execution Checklist

- [ ] Phase 1: [Issue title]
  - [ ] Write or update failing regression test for [behavior].
  - [ ] Run targeted test and confirm it fails for the expected reason.
  - [ ] Implement minimal fix for [root cause].
  - [ ] Run targeted test and confirm it passes.
  - [ ] Run relevant guardrails: [lint/typecheck/build/e2e/etc.].
  - [ ] Write justification and commit atomically.
- [ ] Phase 2: [Issue title]
  - [ ] ...

## Selected Issues

| Phase | Issue | Category | Evidence | Root cause hypothesis | Acceptance criteria | Risk |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | [title] | [Security/Performance/Reliability/Tooling/Tests] | `[path:function]` | [one-line cause] | [observable pass condition] | [Low/Medium/High] |

## Phase 1: [Issue Title]

### Spec

- Current behavior: [what happens now.]
- Desired behavior: [what should happen.]
- Acceptance criteria: [short list.]
- Non-goals: [nearby work not included.]

### Root Cause

- Hypothesis: [why this happens.]
- Evidence: `[file path]`, `[test path]`, `[script/config path]`
- Confidence: `High|Medium|Low`

### TDD Subtasks

- [ ] Write or update failing regression test for [specific behavior].
  - Expected failure: [error/assertion/output that proves the bug].
  - Target command: `[command]`
- [ ] Implement minimal fix.
  - Likely files: `[path]`, `[path]`
  - Boundaries: [what must not change.]
- [ ] Run targeted test until it passes.
  - Target command: `[command]`
- [ ] Run guardrails for the touched area.
  - Commands: `[command]`, `[command]`
- [ ] Write justification and atomic commit.
  - Commit shape: `[short message idea]`
  - Justification points: problem, root cause, fix, tests, compatibility.

### Compatibility Notes

- [API/data/auth/security/migration concern, or "None obvious from current evidence."]

### Blockers Or Unknowns

- [Missing info or risk, or "None."]
```

5. Output requirements.
   - Keep the checklist at the top.
   - Each phase must be independently testable.
   - Each phase must have at least one test or validation subtask before implementation.
   - Use exact file paths and command names from the repo when discoverable.
   - Keep each subtask small enough for `plan-execute/execute-step.prompt.md`.
   - Keep each phase bounded enough for `plan-execute/execute-phase.prompt.md`.
   - Do not include broad refactors, speculative hardening, or unrelated cleanup.

6. Final response.
   - Link to `IMPLEMENTATION_PLAN.md`.
   - Summarize the selected phases.
   - State the first unchecked subtask that `plan-execute/execute-step.prompt.md` would run.
   - State the first phase that `plan-execute/execute-phase.prompt.md` would run.
