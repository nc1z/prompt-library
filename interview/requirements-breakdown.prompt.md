Create or replace `REQUIREMENTS_SUMMARY.md` as a concise take-home assessment memo.

The input may be pasted recruiter notes, assignment text, issue descriptions, screenshots, or local docs. Ignore interview-process logistics unless they affect the delivery, such as timebox, allowed tools, submission format, required commits, tests, or written justifications.

Do not modify application code in this prompt.

Use this workflow:

1. Extract assessment requirements.
   - Identify explicit tasks, definition of done, time limit, environment constraints, allowed tools, deliverable format, and evaluation criteria.
   - Separate explicit requirements from inferred requirements.
   - Preserve constraints around tests, commits, written justifications, and backwards compatibility.
   - Flag ambiguities instead of guessing.

2. Shape the execution strategy.
   - Translate the assessment into a practical 90-minute workflow.
   - Emphasize root-cause diagnosis, minimal fixes, test evidence, and atomic commits.
   - Identify what should be documented even if not fixed.

3. Create `REQUIREMENTS_SUMMARY.md` using this structure:

```markdown
# Requirements Summary

## 5-Line Overview

1. [What the take-home asks for.]
2. [Expected deliverable.]
3. [Timebox and environment.]
4. [Highest-risk constraint.]
5. [Recommended execution focus.]

## Explicit Requirements

- [Direct requirement.]

## Inferred Requirements

- [Supported inference.]

## Constraints

- [Timebox/tooling/submission/test/commit constraint.]

## Evaluation Criteria

| Criterion | What it means in practice |
| --- | --- |
| Diagnostic depth | [one-line interpretation.] |
| Solution quality | [one-line interpretation.] |
| Backwards compatibility | [one-line interpretation.] |
| Documentation | [one-line interpretation.] |
| Systems thinking | [one-line interpretation.] |

## Assessment Strategy

- [How to spend discovery time.]
- [How to select fixes.]
- [How to validate.]
- [How to document.]

## Submission Checklist

- [ ] Requirements understood and ambiguities noted.
- [ ] Baseline tests/build commands identified.
- [ ] Top issues ranked by value/ease/testability.
- [ ] Fixes are minimal and scoped.
- [ ] Tests or smoke checks run after each fix.
- [ ] Written justification exists for each fix.
- [ ] Commits are atomic.

## Ambiguities / Questions

- [Question or "None."]
```

4. Output requirements.
   - Keep it short and actionable.
   - Do not include unrelated interview logistics.
   - Do not invent requirements.
   - Prefer concrete bullets and tables.

5. Final response.
   - Link to `REQUIREMENTS_SUMMARY.md`.
   - State the biggest execution constraint and recommended focus.
