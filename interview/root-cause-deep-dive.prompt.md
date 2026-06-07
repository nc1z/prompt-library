Create or update `ROOT_CAUSE_DEEP_DIVE.md` for the selected issue or issues.

The goal is to prove the root cause before implementation. This prompt should produce fix cards that can be fed into `plan-execute/implementation-plan.prompt.md`.

Do not modify application code in this prompt.

Use this workflow:

1. Select the issue scope.
   - Use the issue or issues named by the user.
   - If the user does not name an issue, use the top `Fix now` issue from `ISSUE_PRIORITY.md`.
   - If the user does not name an issue and `ISSUE_PRIORITY.md` is missing or has no `Fix now` issue, stop and ask the user to choose an issue from `ISSUE_INVENTORY.md` or run `plan-execute/prioritization-matrix.prompt.md`.
   - Do not analyze unrelated issues except where they share the same root cause.

2. Trace the failing behavior.
   - Read `REQUIREMENTS_SUMMARY.md`, `DISCOVERY_CONTEXT.md`, `ISSUE_INVENTORY.md`, and `ISSUE_PRIORITY.md` when present.
   - Read the relevant assignment task, issue note, audit finding, source files, tests, configs, and scripts.
   - Trace from trigger to handler/component/service/data/external dependency/output.
   - Identify the exact behavior that is wrong, fragile, slow, unsafe, or poorly guarded.
   - Separate symptom from cause.

3. Prove or weaken the root cause hypothesis.
   - Look for direct code evidence.
   - Look for missing tests, failing tests, disabled checks, unsafe assumptions, config mismatch, dependency misuse, boundary leaks, race/retry/idempotency issues, or validation/auth gaps.
   - If useful and low-cost, run a targeted test or static command to reproduce the issue.
   - Do not run long commands unless they are necessary and likely to complete within the assessment timebox.

4. Design the minimal fix.
   - State the smallest behavior change that addresses the root cause.
   - List files likely to change.
   - Identify tests to write or update first.
   - Identify compatibility, migration, rollout, security, data, or API concerns.
   - List non-goals so implementation stays surgical.

5. Create `ROOT_CAUSE_DEEP_DIVE.md` using this structure:

```markdown
# Root Cause Deep Dive

## Summary

- Selected issue: [title.]
- Category: Security / Performance / Reliability / Tooling / Tests / Other
- Root cause confidence: High / Medium / Low
- Recommended next prompt: `plan-execute/implementation-plan.prompt.md`

## Fix Cards

### [Issue Title]

#### Problem

- Symptom: [observable behavior.]
- Trigger: [route/action/test/config/script/user flow.]
- Impact: [why it matters.]
- Assessment value: [diagnostic depth/solution quality/backwards compatibility/docs/systems thinking.]

#### Root Cause

- Cause: [specific root cause.]
- Evidence:
  - `[file path:function]` - [what this shows.]
  - `[file path:function]` - [what this shows.]
- Counter-evidence or uncertainty: [what could still be wrong.]
- Confidence: High / Medium / Low

#### Flow Trace

```text
[trigger] -> [handler/component] -> [service/helper] -> [data/external side effect] -> [wrong output/failure]
```

#### Minimal Fix

- Change: [one-line fix.]
- Likely files: `[path]`, `[path]`
- Non-goals: [nearby work not included.]
- Compatibility notes: [API/data/auth/security/migration concerns or "None obvious."]

#### Test First

- Test to add/update: `[test path]`
- Expected failing assertion/output: [what should fail before implementation.]
- Target command: `[command]`
- If no automated test: [smoke check and why automated test is not practical.]

#### Validation

- Targeted validation: `[command]`
- Broader guardrails: `[command]`, `[command]`
- Manual check, if needed: [short check.]

#### Commit And Justification

- Commit message idea: `[message]`
- Justification bullets:
  - Problem: [short.]
  - Root cause: [short.]
  - Fix: [short.]
  - Tests: [short.]
  - Compatibility: [short.]
```

6. Output requirements.
   - Produce one fix card per selected issue.
   - Use exact source evidence for every root-cause claim.
   - Mark speculation as uncertainty.
   - Keep each fix card scoped enough for one atomic commit.
   - Prefer tests before implementation.
   - Do not write a broad implementation plan here; leave detailed subtasks to `plan-execute/implementation-plan.prompt.md`.

7. Final response.
   - Link to `ROOT_CAUSE_DEEP_DIVE.md`.
   - State the selected issue or issues and root-cause confidence.
   - State whether the issue is ready for `plan-execute/implementation-plan.prompt.md`.
