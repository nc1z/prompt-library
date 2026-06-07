Act as a take-home discovery orchestrator and create or replace `DISCOVERY_ORCHESTRATION.md`.

The goal is to run the assessment discovery sequence efficiently after `REQUIREMENTS_SUMMARY.md` exists: run the orientation prompts in parallel when subagent tooling is available, then run issue inventory and prioritization once the orientation context is complete.

The orientation prompts are:

- `plan-execute/codebase-one-pager.prompt.md` -> `CODEBASE_ONE_PAGER.md`
- `plan-execute/repo-tooling-baseline.prompt.md` -> `TOOLING_BASELINE.md`
- `plan-execute/architecture-overview.prompt.md` -> `ARCHITECTURE_OVERVIEW.md`
- `plan-execute/coding-conventions.prompt.md` -> `CODING_CONVENTIONS.md`
- `plan-execute/entrypoints-flow.prompt.md` -> `ENTRYPOINTS_FLOW.md`
- `plan-execute/quality-guards.prompt.md` -> `QUALITY_GUARDS.md`

After those finish, run:

- `plan-execute/issue-inventory.prompt.md` -> `ISSUE_INVENTORY.md`
- `plan-execute/prioritization-matrix.prompt.md` -> `ISSUE_PRIORITY.md`

Use subagents whenever subagent tooling is available to the agent running this prompt. If subagents are unavailable, run the same prompt files sequentially in the order listed below and state that fallback in `DISCOVERY_ORCHESTRATION.md`.

Do not modify application code. Generated review documents are allowed.

Use this workflow:

1. Prepare the orchestration.
   - Read `REQUIREMENTS_SUMMARY.md` first. If it is missing, stop and ask the user to run `plan-execute/requirements-breakdown.prompt.md`.
   - Confirm these prompt files exist:
     - `plan-execute/codebase-one-pager.prompt.md`
     - `plan-execute/repo-tooling-baseline.prompt.md`
     - `plan-execute/architecture-overview.prompt.md`
     - `plan-execute/coding-conventions.prompt.md`
     - `plan-execute/entrypoints-flow.prompt.md`
     - `plan-execute/quality-guards.prompt.md`
     - `plan-execute/issue-inventory.prompt.md`
     - `plan-execute/prioritization-matrix.prompt.md`
   - If any orientation prompt is missing, continue with the remaining orientation prompts and record the gap.
   - If `plan-execute/issue-inventory.prompt.md` or `plan-execute/prioritization-matrix.prompt.md` is missing, stop after orientation and record the blocker because the orchestrator cannot complete its handoff.
   - Tell every subagent to use `REQUIREMENTS_SUMMARY.md` as assessment context.
   - Tell every subagent to keep generated markdown concise, evidence-based, and take-home focused.
   - Tell every subagent to skip recent git activity, active contributors, and team habits unless directly relevant to an issue candidate or required deliverable.
   - Tell every subagent not to modify application code.

2. Run orientation prompts in parallel.
   - Subagent A: execute `plan-execute/codebase-one-pager.prompt.md` and create/update `CODEBASE_ONE_PAGER.md`.
   - Subagent B: execute `plan-execute/repo-tooling-baseline.prompt.md` and create/update `TOOLING_BASELINE.md`.
   - Subagent C: execute `plan-execute/architecture-overview.prompt.md` and create/update `ARCHITECTURE_OVERVIEW.md`.
   - Subagent D: execute `plan-execute/coding-conventions.prompt.md` and create/update `CODING_CONVENTIONS.md`.
   - Subagent E: execute `plan-execute/entrypoints-flow.prompt.md` and create/update `ENTRYPOINTS_FLOW.md`.
   - Subagent F: execute `plan-execute/quality-guards.prompt.md` and create/update `QUALITY_GUARDS.md`.
   - Wait for all orientation subagents to finish.

3. Reconcile orientation context.
   - Read every orientation document that exists:
     - `CODEBASE_ONE_PAGER.md`
     - `TOOLING_BASELINE.md`
     - `ARCHITECTURE_OVERVIEW.md`
     - `CODING_CONVENTIONS.md`
     - `ENTRYPOINTS_FLOW.md`
     - `QUALITY_GUARDS.md`
   - Create or update `DISCOVERY_CONTEXT.md` with compact context for issue discovery:

```markdown
# Discovery Context

## Assessment Context

- [timebox, deliverable, evaluation criteria from `REQUIREMENTS_SUMMARY.md`.]

## App Shape

- [one-line product/app purpose.]
- [one-line architecture/runtime shape.]
- [one-line tooling/test shape.]

## Important Flows

| Flow | Evidence | Why it matters |
| --- | --- | --- |
| [flow] | `[path]` | [assessment relevance] |

## Fix Constraints

- [convention, compatibility concern, test limitation, or scope boundary.]

## Fast Validation

- Targeted: `[command]`
- Final guardrails: `[command]`, `[command]`

## Discovery Search Areas

- Security: [paths/flows.]
- Performance: [paths/flows.]
- Reliability: [paths/flows.]
- Developer tooling: [paths/scripts.]
- Tests: [paths/config.]

## Open Questions

- [uncertainty or "None."]
```

4. Run issue inventory after orientation.
   - Execute `plan-execute/issue-inventory.prompt.md`.
   - Tell the agent to read `REQUIREMENTS_SUMMARY.md` and `DISCOVERY_CONTEXT.md` first.
   - It should create/update `ISSUE_INVENTORY.md`.

5. Run prioritization after issue inventory.
   - Execute `plan-execute/prioritization-matrix.prompt.md`.
   - Tell the agent to read `REQUIREMENTS_SUMMARY.md`, `DISCOVERY_CONTEXT.md`, and `ISSUE_INVENTORY.md` first.
   - It should create/update `ISSUE_PRIORITY.md`.

6. Create `DISCOVERY_ORCHESTRATION.md` using this structure:

```markdown
# Discovery Orchestration

## Snapshot

- Review mode: [subagents used / sequential fallback.]
- Requirements source: `REQUIREMENTS_SUMMARY.md`
- Orientation docs completed: [count]/6
- Issue inventory: Complete / Partial / Missing
- Prioritization: Complete / Partial / Missing
- Best next prompt: `plan-execute/root-cause-deep-dive.prompt.md`

## Prompt Run Log

| Prompt | Output | Mode | Status | Notes |
| --- | --- | --- | --- | --- |
| `plan-execute/codebase-one-pager.prompt.md` | `CODEBASE_ONE_PAGER.md` | Subagent / Sequential | Complete / Partial / Missing | [note] |
| `plan-execute/repo-tooling-baseline.prompt.md` | `TOOLING_BASELINE.md` | Subagent / Sequential | Complete / Partial / Missing | [note] |
| `plan-execute/architecture-overview.prompt.md` | `ARCHITECTURE_OVERVIEW.md` | Subagent / Sequential | Complete / Partial / Missing | [note] |
| `plan-execute/coding-conventions.prompt.md` | `CODING_CONVENTIONS.md` | Subagent / Sequential | Complete / Partial / Missing | [note] |
| `plan-execute/entrypoints-flow.prompt.md` | `ENTRYPOINTS_FLOW.md` | Subagent / Sequential | Complete / Partial / Missing | [note] |
| `plan-execute/quality-guards.prompt.md` | `QUALITY_GUARDS.md` | Subagent / Sequential | Complete / Partial / Missing | [note] |
| `plan-execute/issue-inventory.prompt.md` | `ISSUE_INVENTORY.md` | Sequential after orientation | Complete / Partial / Missing | [note] |
| `plan-execute/prioritization-matrix.prompt.md` | `ISSUE_PRIORITY.md` | Sequential after inventory | Complete / Partial / Missing | [note] |

## Top Ranked Issues

| Rank | Issue | Category | Why this first | Estimated time |
| --- | --- | --- | --- | --- |
| 1 | [title] | [category] | [one-line reason] | [n min] |

## Recommended Next Step

- User should choose [top issue(s)] or override with another issue.
- Then run `plan-execute/root-cause-deep-dive.prompt.md`.
- Then run `plan-execute/implementation-plan.prompt.md`.

## Missing Or Weak Context

- [missing prompt/output/evidence or "None."]
```

7. Output requirements.
   - Keep `DISCOVERY_ORCHESTRATION.md` concise and operational.
   - Do not include command output dumps.
   - Do not modify app code.
   - Do not proceed to root-cause deep dive or implementation planning.
   - Stop after `ISSUE_PRIORITY.md` and `DISCOVERY_ORCHESTRATION.md` are complete.

8. Final response.
   - Link to `DISCOVERY_ORCHESTRATION.md`, `ISSUE_INVENTORY.md`, and `ISSUE_PRIORITY.md`.
   - State whether subagents were used or sequential fallback was used.
   - Summarize the top 2-3 ranked issues.
   - Ask the user to choose which issue(s) to work on next.
