Execute the next unchecked phase or testable module from `IMPLEMENTATION_PLAN.md`, then stop.

Use this prompt when the selected issue is clear enough for more autonomy than `plan-execute/execute-step.prompt.md`. It should complete one issue phase end-to-end where feasible, following the failing-test to implementation to passing-test loop internally.

Use this workflow:

1. Find the active implementation plan.
   - Prefer `IMPLEMENTATION_PLAN.md` in the repository root.
   - If the user explicitly names a different plan file, use that file.
   - If neither exists, stop and ask the user to run `plan-execute/implementation-plan.prompt.md` first.

2. Select exactly one phase.
   - Read the `Execution Checklist` first.
   - Select the earliest phase that has unchecked subtasks.
   - The selected phase is the unit of work.
   - Treat a phase as one issue or one testable module, whichever is smaller in the plan.
   - Do not continue into later phases.
   - If the phase is too broad for one testable module, split it into smaller phases in the plan, then stop.

3. Read the phase context.
   - Read the phase spec, root-cause notes, target files, target commands, acceptance criteria, non-goals, and compatibility notes.
   - Inspect source, tests, config, and scripts needed for this phase.
   - Preserve user changes and unrelated work.

4. Execute the phase with a TDD loop.
   - Write or update the focused failing test first.
   - Run the target test and confirm it fails for the expected reason.
   - Implement the smallest code change that addresses the documented root cause.
   - Run the targeted test until it passes.
   - Run the phase guardrails listed in the plan.
   - Write the required justification and make the atomic commit only if the plan explicitly includes that as part of the phase.
   - If a test-first approach is not practical, perform the documented smoke check and explain why automated coverage was not practical.
   - If a required subtask becomes unclear or blocked, stop the phase, leave that subtask and the parent phase unchecked, and record the blocker in `IMPLEMENTATION_PLAN.md`.

5. Keep scope tight.
   - Do not refactor unrelated code.
   - Do not fix unrelated test failures unless they block validation of the selected phase and are caused by the current work.
   - Do not rewrite neighboring modules unless the plan explicitly says that is required.
   - Do not change public behavior outside the selected phase's acceptance criteria.

6. Update `IMPLEMENTATION_PLAN.md`.
   - Check off every selected-phase subtask that is genuinely complete.
   - Check the parent phase only if every required subtask in that phase is complete.
   - Add concise evidence notes for failing tests, passing tests, guardrails, and any skipped checks.
   - Leave blocked or partial subtasks unchecked with a concise blocker note.

7. Final response.
   - State the phase completed.
   - Summarize the root cause and fix.
   - List files changed.
   - Report tests and guardrails run, including failures or skipped checks.
   - Mention plan checkboxes updated.
   - Stop and wait for user review before continuing.

Execute exactly one phase or testable module, then stop.
