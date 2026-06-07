Create or replace `CODEBASE_ONE_PAGER.md` as a plain-language overview of the take-home codebase.

The goal is to quickly understand what the app does before auditing internals. This is not a contributor-history review. Skip git history, active contributors, and team habits unless they directly explain an issue candidate or required deliverable.

Do not modify application code in this prompt.

Use this workflow:

1. Read sources of truth.
   - Read README, docs, package/config files, app entrypoints, route roots, core domain files, and representative tests.
   - Identify app purpose, users, core journeys, main data objects, and business rules.
   - Verify claims from code where possible.

2. Keep it business-first.
   - Explain what a user is trying to do.
   - Explain how the system supports that journey at a high level.
   - Avoid architecture detail that belongs in `ARCHITECTURE_OVERVIEW.md`.

3. Create `CODEBASE_ONE_PAGER.md` using this structure:

```markdown
# Codebase One-Pager

## What This Is

[One short paragraph in plain language.]

## Users And Jobs

| User / actor | Job they are trying to do | Evidence |
| --- | --- | --- |
| [actor] | [job] | `[file/path]` |

## Main User Journey

1. [Step.]
2. [Step.]
3. [Step.]

## Core Concepts

| Concept | Meaning | Evidence |
| --- | --- | --- |
| [data object/domain term] | [plain-language meaning] | `[file/path]` |

## App Surface

- UI/routes: [short list or "Not evident."]
- API/backend: [short list or "Not evident."]
- Data/storage: [short list or "Not evident."]
- External systems: [short list or "Not evident."]

## Things To Remember During Fixes

- [Behavior or domain rule that should not break.]
- [Important workflow or data object.]
- [Unclear area to verify before changing.]

## Evidence Reviewed

- `[file/path]`
- `[file/path]`
```

4. Output requirements.
   - Keep to roughly one page.
   - Use exact paths as evidence.
   - State uncertainty directly.
   - Do not analyze code quality or contributor behavior.

5. Final response.
   - Link to `CODEBASE_ONE_PAGER.md`.
   - Summarize the app purpose and highest-risk user journey.
