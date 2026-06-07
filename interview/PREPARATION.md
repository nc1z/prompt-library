# Take-Home Preparation

## Goal

Prepare a prompt sequence for a 90-minute full-stack audit and fix assessment. The assessment rewards root-cause diagnosis, minimal fixes, backwards compatibility, passing tests, atomic commits, and concise written justifications.

This file is the workflow index. Run the referenced prompt files for the actual assessment workflow.

## Assessment Bias

- Read before changing code: architecture, data flow, build system, tests, and conventions.
- Prefer targeted fixes over broad refactors.
- Rank work by reviewer value, confidence, ease, and testability.
- Require concrete evidence: file paths, functions, routes, scripts, failing tests, and runtime symptoms.
- Document as work happens, including issues that are diagnosed but not fixed.
- Keep every fix easy to explain in a commit message and written justification.

## Prompt Outline

Run `plan-execute/discovery-orchestrator.prompt.md` as the shortcut after requirements breakdown when you want codebase discovery, issue inventory, and prioritization coordinated automatically.

| Order | Prompt outline | Purpose | Expected output | Reuse from |
| --- | --- | --- | --- | --- |
| 1 | Requirements breakdown and scoring context | Convert the assignment text into concise bullets: scope, deliverables, constraints, evaluation criteria, and non-goals. | Requirements memo with explicit requirements, inferred requirements, risks, and submission checklist. | `plan-execute/requirements-breakdown.prompt.md` |
| 2 | Codebase one-pager and product journey | Explain what the app does in plain language before diving into internals: users, core jobs, main flows, data objects, and business value. Skip contributor/history analysis unless repo history is clearly relevant. | One-page context overview suitable for a new engineer. | `plan-execute/codebase-one-pager.prompt.md` |
| 3 | Repo shape and tooling baseline | Identify stack, monorepo/package layout, package manager, scripts, build commands, test commands, env/config files, and CI visibility. Do not spend time on contributor patterns. | Fast setup and verification map, including commands to run constantly during the assessment. | `plan-execute/repo-tooling-baseline.prompt.md` |
| 4 | Architecture overview | Classify the architecture and boundaries: frontend/backend split, runtime services, modules, data stores, external systems, patterns, and architecture risks. | Concise architecture review with evidence and high-level diagrams. | `plan-execute/architecture-overview.prompt.md` |
| 5 | Coding conventions and change style | Capture the repo's real conventions: naming, imports, file layout, testing style, error handling, validation, logging, and data access. Ignore recent contributor habits unless they directly affect how to make a safe fix. | "How to change this codebase without surprising it" memo. | `plan-execute/coding-conventions.prompt.md` |
| 6 | Entrypoints and request/data flow | Map everything that starts work: routes, API handlers, frontend route roots, server handlers, jobs, queues, webhooks, cron, scripts, and CLIs. | Entrypoint table plus trigger to handler to service to data/external side-effect flow maps. | `plan-execute/entrypoints-flow.prompt.md` |
| 7 | Quality guards and test infrastructure | Audit guardrails that protect backwards compatibility: unit/integration/e2e tests, typecheck, lint, formatting, coverage, dependency/security checks, and CI gates. | Test and quality command matrix with gaps and fastest reliable verification loop. | `plan-execute/quality-guards.prompt.md` |
| 8 | Issue inventory by category | Scan for likely assessment issues across security, performance, reliability, developer tooling, tests, and maintainability. Focus on root causes, not symptoms. | Evidence-backed issue backlog with category, impact, trigger, root cause hypothesis, and likely fix surface. | `plan-execute/issue-inventory.prompt.md` |
| 9 | Prioritization matrix | Sort issue candidates by ease of completion plus reviewer value. Include confidence, blast radius, testability, estimated time, and risk of breaking behavior. | Markdown table sorted by best first fixes, plus top 2-3 recommended targets. | `plan-execute/prioritization-matrix.prompt.md` |
| 10 | Root-cause deep dive per selected issue | For each chosen issue, trace the failing path and prove the root cause before editing. Identify minimal fix, tests to add/run, compatibility concerns, and rollback/migration notes. | One fix card per issue with files to touch and expected validation. | `plan-execute/root-cause-deep-dive.prompt.md` |
| 11 | Implementation plan | Given the selected problems to solve, create a spec-driven TDD plan. Break each issue into failing-test, minimal-change, passing-test, docs/justification, and commit subtasks. | `IMPLEMENTATION_PLAN.md` with an execution checklist at the top and issue-level phases below. | `plan-execute/implementation-plan.prompt.md` |
| 12 | Execute one subtask | Read `IMPLEMENTATION_PLAN.md`, execute the next unchecked subtask only, update the checkbox, then stop for user review/approval. | One completed test, implementation, verification, docs, or commit subtask. | `plan-execute/execute-step.prompt.md` |
| 13 | Execute one phase/task | Read `IMPLEMENTATION_PLAN.md`, execute the next unchecked issue phase or testable module end-to-end using the failing-test to passing-test loop, then stop. | One issue fixed or one testable module completed, with checked subtasks and validation evidence. | `plan-execute/execute-phase.prompt.md` |
| 14 | Change justification and commit message | Produce the written justification required by the assessment: problem, severity, root cause, fix, why it is minimal, tests run, and compatibility notes. | Commit message/body or markdown note for each fix. | `plan-execute/change-justification.prompt.md` |
| 15 | Final submission review | Confirm tests pass, commits are atomic, justifications exist, unresolved findings are documented, and no broad refactor or unrelated churn slipped in. | Final checklist and concise reviewer-facing summary. | `plan-execute/final-submission-review.prompt.md` |

## Suggested 90-Minute Run Order

| Time | Action | Prompt outlines |
| --- | --- | --- |
| 0-10 min | Parse requirements, clone/setup, inspect scripts, run baseline checks if feasible. | 1 |
| 10-40 min | Run coordinated discovery: orientation prompts in parallel, then issue inventory and prioritization after orientation docs exist. | `plan-execute/discovery-orchestrator.prompt.md` |
| 40-45 min | Generate the implementation plan for selected problems. | 10, 11 |
| 45-75 min | Fix the top 1-3 issues with TDD checkpoints. Use step mode for tight control or phase mode when the scope is clear. | 12, 13 |
| 75-90 min | Run final verification, clean commits, write final justifications and unresolved notes. | 14, 15 |

## Discovery Orchestration

After `plan-execute/requirements-breakdown.prompt.md`, run `plan-execute/discovery-orchestrator.prompt.md`.

It should:

1. Read `REQUIREMENTS_SUMMARY.md`.
2. Run these orientation prompts in parallel with subagents when available:
   - `plan-execute/codebase-one-pager.prompt.md`
   - `plan-execute/repo-tooling-baseline.prompt.md`
   - `plan-execute/architecture-overview.prompt.md`
   - `plan-execute/coding-conventions.prompt.md`
   - `plan-execute/entrypoints-flow.prompt.md`
   - `plan-execute/quality-guards.prompt.md`
3. Wait for those orientation docs to finish.
4. Run `plan-execute/issue-inventory.prompt.md` after orientation docs exist.
5. Run `plan-execute/prioritization-matrix.prompt.md` after issue inventory exists.
6. Stop with ranked issues ready for the user to choose from.

## Plan / Execute Loop

1. Choose the issues to solve from `ISSUE_PRIORITY.md`.
2. Run `plan-execute/root-cause-deep-dive.prompt.md` for the selected issues.
3. Run the implementation-plan prompt with the exact problems selected from the prioritization matrix and deep dive.
4. It creates `IMPLEMENTATION_PLAN.md` with a checklist at the top.
5. Run `plan-execute/execute-step.prompt.md` when close control is useful.
   - It completes the next unchecked subtask only.
   - Typical sequence: write failing test, stop; user approves; implement fix, run test, stop; user approves; document/commit.
6. Run `plan-execute/execute-phase.prompt.md` when the issue or module is clear enough for more autonomy.
   - It completes the whole next issue phase or testable module.
   - It still follows the failing-test to implementation to passing-test loop internally.
7. After each completed issue, make an atomic commit with the root-cause justification.

## Prioritization Table Shape

Use this table in the future prioritization prompt.

| Rank | Issue | Category | Evidence | Root cause confidence | Reviewer value | Ease | Risk | Testability | Est. time | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | `[short title]` | Security / Performance / Reliability / Tooling / Tests | `[path:function]` | High / Medium / Low | High / Medium / Low | Easy / Medium / Hard | Low / Medium / High | High / Medium / Low | `n min` | Fix now / Park / Document only |

## Future Prompt Body Rules

- Start diagnosis prompts with "do not modify code" unless the prompt is explicitly for implementation.
- Require exact source evidence for every claim.
- For take-home repos, avoid spending time on recent git activity, active contributors, or team habits. Many assessment repos are synthetic, private, shallow-cloned, or fixture-like. Check commit history only if it helps explain the selected issue or required deliverable.
- Require root-cause language: what fails, why it fails, how it is triggered, and why the proposed fix addresses the cause.
- Ask for minimal fixes and explicit non-goals.
- Ask for backwards compatibility and migration notes when data, auth, security, or API behavior changes.
- Ask for validation commands before and after each change.
- Keep outputs short enough to use under time pressure.
- Prefer markdown tables and bullet lists over long prose.
- Make unresolved findings visible so they can still demonstrate diagnostic depth.

## Prompt Gaps To Write Later

- None currently listed. Add new gaps here as the prep workflow gets exercised.
