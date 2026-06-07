Create or replace `FINAL_SUBMISSION_REVIEW.md` as the final take-home readiness check.

The goal is to verify that the submitted work is coherent, minimal, tested, documented, and easy for reviewers to evaluate. This prompt should run after implementation work is complete or nearly complete.

Use this workflow:

1. Read the assessment requirements and plan artifacts.
   - Read the assignment notes, `REQUIREMENTS_SUMMARY.md`, `ISSUE_PRIORITY.md`, `ROOT_CAUSE_DEEP_DIVE.md`, `IMPLEMENTATION_PLAN.md`, `CHANGE_JUSTIFICATIONS.md`, and any issue notes when present.
   - Read changed files and relevant tests.
   - Review git status and recent commits.
   - Do not rely only on the user's summary.

2. Verify scope and atomicity.
   - Confirm each fixed issue maps to a diagnosed root cause.
   - Confirm changes are minimal and do not rewrite unrelated areas.
   - Confirm each issue has a clear written justification.
   - Confirm commits are atomic when commits exist or are required.
   - Check for uncommitted changes that should be committed or intentionally left out.

3. Verify tests and guardrails.
   - Identify the targeted tests for each fix.
   - Run or review evidence for unit, integration, e2e, typecheck, lint, build, or smoke checks that matter.
   - If a command cannot be run, explain why and what risk remains.
   - Do not claim "all tests pass" unless the relevant command was run successfully.

4. Check reviewer-facing quality.
   - Confirm the final written notes explain problem, severity, root cause, fix, tests run, and compatibility.
   - Confirm unresolved issues are documented rather than hidden.
   - Confirm no broad refactor, generated noise, or unrelated churn distracts from the fixes.
   - Confirm instructions to run the project/tests are visible if required by the assessment.

5. Create `FINAL_SUBMISSION_REVIEW.md` using this structure:

```markdown
# Final Submission Review

## Verdict

- Readiness: Ready / Needs polish / Blocked
- Best fix to emphasize: [issue and why.]
- Main residual risk: [risk or "None obvious from current evidence."]
- Submission strategy: [one-line recommendation.]

## Fixed Issues

| Issue | Category | Commit | Root cause | Fix | Validation | Justification |
| --- | --- | --- | --- | --- | --- | --- |
| [title] | [Security/Performance/Reliability/Tooling/Tests] | `[sha or pending]` | [short] | [short] | `[command]` | `[file/commit body/inline note]` |

## Requirement Coverage

| Requirement / Criterion | Status | Evidence | Notes |
| --- | --- | --- | --- |
| Root-cause diagnosis | Pass / Partial / Missing | [evidence] | [note] |
| Minimal targeted fixes | Pass / Partial / Missing | [evidence] | [note] |
| Backwards compatibility | Pass / Partial / Missing | [evidence] | [note] |
| Tests pass | Pass / Partial / Missing | [evidence] | [note] |
| Written justifications | Pass / Partial / Missing | [evidence] | [note] |
| Atomic commits | Pass / Partial / Missing | [evidence] | [note] |

## Validation Log

| Command / Check | Result | Notes |
| --- | --- | --- |
| `[command]` | Pass / Fail / Skipped | [short note] |

## Git Review

- Current branch: `[branch]`
- Commit count for fixes: [n]
- Uncommitted changes: [none/list.]
- Atomicity notes: [one-line assessment.]

## Unresolved Findings

| Finding | Why not fixed | How to discuss |
| --- | --- | --- |
| [title] | [time/risk/scope/uncertainty] | [concise reviewer-facing explanation] |

## Reviewer Summary

[Short paragraph the candidate can use to explain the submission.]

## Final Checklist

- [ ] Targeted tests for each fix have passed or skipped risk is documented.
- [ ] Relevant broader guardrails have passed or skipped risk is documented.
- [ ] Every fixed issue has a root-cause justification.
- [ ] Commits are atomic or pending commit work is clearly listed.
- [ ] No unrelated refactor or noisy churn is included.
- [ ] Unresolved findings are documented.
```

6. Output requirements.
   - Use exact commit SHAs, file paths, test names, and command names where available.
   - Keep the review concise and assessment-focused.
   - Do not invent validation results.
   - Do not grade UI polish unless the assessment explicitly requires it and it was actually checked.
   - Flag blockers plainly.
   - If work is not committed yet, show the commit plan instead of pretending commits exist.

7. Final response.
   - Link to `FINAL_SUBMISSION_REVIEW.md`.
   - State readiness verdict.
   - List validation commands run or skipped.
   - State the main remaining risk, if any.
