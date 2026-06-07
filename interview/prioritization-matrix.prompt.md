Create or replace `ISSUE_PRIORITY.md` as a take-home issue prioritization matrix.

The goal is to decide which diagnosed issues are most worth fixing in a 90-minute assessment. Rank issues by reviewer value plus ease of completion, not by theoretical importance alone.

Do not modify application code in this prompt.

Use this workflow:

1. Gather issue candidates.
   - Read the assignment instructions and any provided issue list.
   - Read `REQUIREMENTS_SUMMARY.md` when present.
   - Read `DISCOVERY_CONTEXT.md` when present; this is the preferred handoff from `plan-execute/discovery-orchestrator.prompt.md`.
   - Read `ISSUE_INVENTORY.md` when present; this is the preferred source of candidates.
   - Read orientation docs when present: `CODEBASE_ONE_PAGER.md`, `TOOLING_BASELINE.md`, `ARCHITECTURE_OVERVIEW.md`, `CODING_CONVENTIONS.md`, `ENTRYPOINTS_FLOW.md`, `QUALITY_GUARDS.md`.
   - If `ISSUE_INVENTORY.md` does not exist, do a fast targeted scan of source, tests, package scripts, config, and docs to identify likely issues.
   - Focus on categories the assessment names: security, performance, reliability, developer tooling, tests, and backwards compatibility.
   - Skip recent git activity, active contributors, and team habits unless they directly explain an issue.

2. Score each candidate.
   - Reviewer value: how strongly the issue demonstrates diagnostic depth, systems thinking, and meaningful risk reduction.
   - Ease: how likely it is to complete cleanly in the remaining time.
   - Risk: blast radius, chance of breaking existing behavior, migration/API concerns, and uncertainty.
   - Testability: how easy it is to prove with a focused test or smoke check.
   - Root cause confidence: whether evidence points to a real cause, not just a symptom.
   - Time estimate: realistic elapsed time including test writing, implementation, validation, justification, and commit.

3. Prefer fixes with these traits.
   - Clear root cause.
   - Small file surface.
   - Existing test infrastructure nearby.
   - Strong assessment category match.
   - Easy written justification.
   - Low compatibility risk.

4. Deprioritize candidates with these traits.
   - Requires broad refactor.
   - Needs product clarification.
   - Depends on external services or unavailable infrastructure.
   - Hard to prove under time pressure.
   - High risk of breaking unknown behavior.
   - Interesting but not connected to assessment criteria.

5. Create `ISSUE_PRIORITY.md` using this structure:

```markdown
# Issue Priority

## Recommendation

- Fix first: [issue title] because [one-line reason].
- Fix second: [issue title] because [one-line reason].
- Park: [issue title or category] because [one-line reason].
- Strategy: [one-line summary of how to spend the remaining time.]

## Priority Matrix

| Rank | Issue | Category | Evidence | Root cause confidence | Reviewer value | Ease | Risk | Testability | Est. time | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | `[short title]` | Security / Performance / Reliability / Tooling / Tests | `[path:function]` | High / Medium / Low | High / Medium / Low | Easy / Medium / Hard | Low / Medium / High | High / Medium / Low | `n min` | Fix now / Park / Document only |

## Fix Now

### 1. [Issue Title]

- Problem: [what is wrong.]
- Root cause hypothesis: [why it happens.]
- Why it matters: [impact in assessment terms.]
- Minimal fix surface: `[file]`, `[file]`
- Test path: [focused automated test or smoke check.]
- Commit shape: [one atomic commit idea.]

## Park Or Document

| Issue | Why parked | What to document |
| --- | --- | --- |
| [title] | [too risky/large/unclear/etc.] | [diagnostic note to preserve reviewer value] |

## Inputs Reviewed

- `REQUIREMENTS_SUMMARY.md` or "Not present."
- `DISCOVERY_CONTEXT.md` or "Not present."
- `ISSUE_INVENTORY.md` or "Not present."
- [file/doc/command reviewed.]
- [file/doc/command reviewed.]

## Open Questions

- [question or "None."]
```

6. Output requirements.
   - Include 3-10 issue candidates when available.
   - Use exact source paths, function names, routes, script names, config names, and test names when discoverable.
   - Mark uncertain claims as low confidence.
   - Do not invent metrics, production traffic, or exploitability not supported by evidence.
   - Keep the top recommendation actionable enough to feed into `plan-execute/root-cause-deep-dive.prompt.md`.
   - Keep the file short enough to scan during the assessment.

7. Final response.
   - Link to `ISSUE_PRIORITY.md`.
   - Name the top 2-3 recommended issues.
   - State any important uncertainty or missing evidence.
