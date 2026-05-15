Complete the project goal from the plan.

Workflow:

1. Find the active plan and product requirements.
   - Prefer `PLAN.md` for the plan.
   - Prefer `PRD.md` or the clearest available requirements doc for product intent.
   - Stop if a plan is missing or the requirements are too unclear to execute safely.

2. Run the plan to completion.
   - Repeatedly apply `execute-next-phase.prompt.md`.
   - After each phase, update the plan and continue to the next unfinished phase.
   - Stop only for blockers, unsafe ambiguity, failed permissions, missing requirements, or required user decisions.

3. Finish with the README.
   - Once every plan phase is complete, apply `readme.prompt.md`.
   - Ensure the README lets an end user understand and test the POC without extra explanation.

4. Final review.
   - Start one reviewer subagent to check the completed repo against the PRD, acceptance criteria, README accuracy, and regression risk.
   - Fix concrete issues from the review before finishing.
   - Stop and report any review finding that needs a user or product decision.

5. Final response.
   - State whether the goal is complete.
   - Summarize completed phases, README update, validation, and any remaining blockers.
