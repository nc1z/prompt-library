Execute the next unchecked subtask from `IMPLEMENTATION_PLAN.md`, then stop for user review.

Use this prompt for tight control during the take-home. It should complete exactly one checklist subtask, such as writing the failing test, implementing the minimal fix, running the passing test, writing the justification, or making the atomic commit.

Use this workflow:

1. Find the active implementation plan.
   - Prefer `IMPLEMENTATION_PLAN.md` in the repository root.
   - If the user explicitly names a different plan file, use that file.
   - If neither exists, stop and ask the user to run `plan-execute/implementation-plan.prompt.md` first.

2. Select exactly one subtask.
   - Read the `Execution Checklist` first.
   - Select the earliest unchecked leaf checkbox.
   - Do not execute parent phase checkboxes directly unless every child is already complete.
   - Do not continue into later subtasks after completing the selected one.
   - If the selected subtask is too broad, split it into smaller unchecked subtasks in the plan, then stop.

3. Read only the context needed for the selected subtask.
   - Read the relevant phase spec, root-cause notes, target files, and target commands.
   - Inspect source and tests needed for this subtask.
   - Preserve user changes and unrelated work.

4. Execute based on subtask type.
   - Failing test subtask:
     - Add or update only the focused regression test.
     - Run the target command.
     - Confirm it fails for the expected reason.
     - Do not implement the fix in the same run.
   - Implementation subtask:
     - Make the smallest code change that addresses the documented root cause.
     - Do not add unrelated refactors or opportunistic cleanup.
     - If the failing test is not present, stop and explain the plan gap unless the plan explicitly allows smoke-only validation.
   - Passing test or guardrail subtask:
     - Run the specified command.
     - Fix only issues caused by the current phase's changes.
     - Leave unrelated failures documented but unchecked.
   - Justification or commit subtask:
     - Write the required concise justification or make the atomic commit if requested by the plan.
     - Include problem, root cause, fix, tests run, and compatibility notes.
   - Unknown subtask type:
     - Infer the smallest concrete action from the subtask text and phase context.
     - If the action is still unclear, leave it unchecked, add a blocker note in `IMPLEMENTATION_PLAN.md`, and stop.

5. Update `IMPLEMENTATION_PLAN.md`.
   - Check off the selected subtask only when it is genuinely complete.
   - If a parent phase is now fully complete, check the parent phase too.
   - Add a short evidence note under the subtask when useful, especially expected failing test output or passing validation.
   - Leave blocked or partial subtasks unchecked with a concise blocker note.

6. Final response.
   - State the exact subtask completed.
   - Summarize files changed.
   - Report commands run and whether they failed or passed.
   - Mention plan checkboxes updated.
   - Stop and wait for user approval before continuing.

Do exactly one subtask, then stop.
