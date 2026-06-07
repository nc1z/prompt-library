Create or replace `ISSUE_INVENTORY.md` as an evidence-backed list of likely take-home issues.

The goal is to identify candidate issues across security, performance, reliability, developer tooling, tests, and maintainability before ranking them.

Do not modify application code in this prompt.

Use this workflow:

1. Read discovery context and orientation artifacts.
   - Read `REQUIREMENTS_SUMMARY.md` first when it exists.
   - Read `DISCOVERY_CONTEXT.md` when it exists; this is the preferred handoff from `plan-execute/discovery-orchestrator.prompt.md`.
   - Read orientation artifacts when they exist: `CODEBASE_ONE_PAGER.md`, `TOOLING_BASELINE.md`, `ARCHITECTURE_OVERVIEW.md`, `CODING_CONVENTIONS.md`, `ENTRYPOINTS_FLOW.md`, `QUALITY_GUARDS.md`.
   - If none of these artifacts exist, do a fast targeted scan of source, tests, scripts, config, and docs.

2. Scan by assessment category.
   - Security: auth, authorization, validation, secrets, injection, XSS/CSRF, unsafe redirects, dependency risk, sensitive logging.
   - Performance: N+1 work, repeated fetches, unnecessary rendering, expensive loops, missing pagination, cache misuse, bundle/build issues.
   - Reliability: unhandled errors, swallowed promises, retries/idempotency, race conditions, transaction boundaries, background work, flaky tests.
   - Developer tooling: broken scripts, missing CI gates, type/lint/test gaps, env setup, package manager mismatch.
   - Tests: missing regression coverage, skipped tests, broad brittle tests, fixtures that hide behavior.
   - Maintainability: confusing layer boundaries or code that blocks surgical fixes.

3. Keep evidence tight.
   - Every candidate needs a path, function, route, script, config, test, or command as evidence.
   - Separate observed facts from hypotheses.
   - Do not rank here beyond rough severity; leave final ordering to `plan-execute/prioritization-matrix.prompt.md`.

4. Create `ISSUE_INVENTORY.md` using this structure:

```markdown
# Issue Inventory

## Snapshot

- Candidates found: [n]
- Strongest category: [category]
- Highest-confidence issue: [title]
- Best next prompt: `plan-execute/prioritization-matrix.prompt.md`

## Candidate Table

| Issue | Category | Evidence | Symptom | Root cause hypothesis | Severity | Confidence | Likely fix surface |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [title] | Security / Performance / Reliability / Tooling / Tests / Maintainability | `[path:function]` | [what is wrong] | [why it may happen] | High / Medium / Low | High / Medium / Low | `[path]` |

## Category Notes

### Security

- [candidate or "No high-signal candidate found."]

### Performance

- [candidate or "No high-signal candidate found."]

### Reliability

- [candidate or "No high-signal candidate found."]

### Developer Tooling

- [candidate or "No high-signal candidate found."]

### Tests

- [candidate or "No high-signal candidate found."]

### Maintainability

- [candidate or "No high-signal candidate found."]

## Evidence Reviewed

- `REQUIREMENTS_SUMMARY.md` or "Not present."
- `DISCOVERY_CONTEXT.md` or "Not present."
- `[file/path]`
- `[command or search]`

## Open Questions

- [question or "None."]
```

5. Output requirements.
   - Include 3-12 candidates when available.
   - Do not invent exploitability, scale, traffic, or production incidents.
   - Prefer high-signal candidates over long lists.
   - Keep candidates actionable enough for ranking.

6. Final response.
   - Link to `ISSUE_INVENTORY.md`.
   - State candidate count and highest-confidence issue.
