Create or replace `QUALITY_GUARDS.md` as a concise map of tests and safeguards for the take-home repo.

The goal is to know which checks protect backwards compatibility and which checks are missing or weak.

Do not modify application code in this prompt.

Use this workflow:

1. Inspect guard evidence.
   - Read package scripts, test configs, lint/format/type configs, CI workflows, coverage settings, dependency/security tooling, docs, and env/setup files.
   - Search for suppressions and skipped tests: `TODO`, `FIXME`, `HACK`, `XXX`, `eslint-disable`, `ts-ignore`, `@ts-expect-error`, `skip`, `only`, `nocheck`, `noqa`, or equivalents.
   - Distinguish local scripts from CI-enforced checks.

2. Identify fastest reliable checks.
   - Find the smallest targeted test command for implementation.
   - Find broader final guardrails.
   - If a check exists but cannot be run without setup, state that.

3. Create `QUALITY_GUARDS.md` using this structure:

```markdown
# Quality Guards

## Guard Checklist

- [ ] Formatting - Present / Partial / Missing / Unknown
- [ ] Linting - Present / Partial / Missing / Unknown
- [ ] Type checking - Present / Partial / Missing / Unknown
- [ ] Unit tests - Present / Partial / Missing / Unknown
- [ ] Integration tests - Present / Partial / Missing / Unknown
- [ ] E2E tests - Present / Partial / Missing / Unknown
- [ ] Build gate - Present / Partial / Missing / Unknown
- [ ] CI gate - Present / Partial / Missing / Unknown
- [ ] Dependency/security scanning - Present / Partial / Missing / Unknown

## Guard Table

| Guard | Status | Evidence | Command | Assessment use |
| --- | --- | --- | --- | --- |
| Formatting | Present / Partial / Missing / Unknown | `[file]` | `[command]` | [when to run] |
| Linting | Present / Partial / Missing / Unknown | `[file]` | `[command]` | [when to run] |
| Type checking | Present / Partial / Missing / Unknown | `[file]` | `[command]` | [when to run] |
| Unit tests | Present / Partial / Missing / Unknown | `[file]` | `[command]` | [when to run] |
| Integration tests | Present / Partial / Missing / Unknown | `[file]` | `[command]` | [when to run] |
| E2E tests | Present / Partial / Missing / Unknown | `[file]` | `[command]` | [when to run] |
| Build | Present / Partial / Missing / Unknown | `[file]` | `[command]` | [when to run] |
| CI | Present / Partial / Missing / Unknown | `[file]` | `[command]` | [when to run] |

## Smell / Suppression Counts

| Marker | Count | Notes |
| --- | --- | --- |
| TODO | [n] | [where] |
| FIXME | [n] | [where] |
| Suppressions | [n] | [where] |
| Skipped/only tests | [n] | [where] |

## Recommended Validation Loop

1. Before fix: `[command]` or [why skipped.]
2. Failing test: `[command]`
3. Passing test: `[command]`
4. Final guardrails: `[command]`, `[command]`

## Gaps That Matter

- [High-impact missing or weak guard.]
- [High-impact missing or weak guard.]
```

4. Output requirements.
   - Use exact file paths and script names.
   - Mark unknowns instead of guessing.
   - Do not invent CI or coverage.
   - Keep table cells short.

5. Final response.
   - Link to `QUALITY_GUARDS.md`.
   - State strongest guard, weakest guard, and recommended validation loop.
