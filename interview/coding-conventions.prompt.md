Create or replace `CODING_CONVENTIONS.md` as a concise guide to the repo's actual coding style.

The goal is to help fixes match the existing codebase. This is not a contributor-history review. Skip git history, active contributors, and team habits unless they directly affect a safe fix for an issue candidate or required deliverable.

Do not modify application code in this prompt.

Use this workflow:

1. Inspect representative code.
   - Read route roots, components, handlers, services, data access files, shared utilities, tests, and config.
   - Search for repeated patterns in imports, naming, validation, error handling, logging, test setup, mocks, fixtures, and data access.
   - Identify both consistent patterns and local inconsistencies.

2. Focus on implementation guidance.
   - Capture conventions a fixer should follow.
   - Avoid broad best-practice advice.
   - Prefer examples from the repo over abstract recommendations.

3. Create `CODING_CONVENTIONS.md` using this structure:

```markdown
# Coding Conventions

## Snapshot

- Main style: [one-line summary.]
- Strongest convention: [one-line.]
- Biggest inconsistency: [one-line.]
- Testing style: [one-line.]
- Error/logging style: [one-line.]

## Convention Table

| Area | What the repo does | Evidence | Follow this when fixing |
| --- | --- | --- | --- |
| File organization | [pattern] | `[path]` | [guidance] |
| Naming | [pattern] | `[path]` | [guidance] |
| Imports | [pattern] | `[path]` | [guidance] |
| Validation | [pattern] | `[path]` | [guidance] |
| Error handling | [pattern] | `[path]` | [guidance] |
| Logging | [pattern] | `[path]` | [guidance] |
| Data access | [pattern] | `[path]` | [guidance] |
| Tests | [pattern] | `[path]` | [guidance] |

## Examples

### Follow

```text
[tiny representative pattern, under 12 lines]
```

### Be Careful

```text
[tiny inconsistent or risky pattern, under 12 lines]
```

## Fixer Rules

- [Concrete rule.]
- [Concrete rule.]
- [Concrete rule.]

## Evidence Reviewed

- `[file/path]`
```

4. Output requirements.
   - Use exact paths and names.
   - Keep examples tiny.
   - State "Not evident from repo" when a convention is unclear.
   - Do not discuss contributor habits.

5. Final response.
   - Link to `CODING_CONVENTIONS.md`.
   - Summarize the top convention to follow and top inconsistency to avoid.
