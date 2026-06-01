Create or update `TEAM-DYNAMICS.md` in the repository root as a concise, evidence-based map of who has worked on what and how the team currently changes the codebase.

The goal is to help a tech lead understand domain ownership, collaboration patterns, refactor/cleanup activity, code-quality risks, and support gaps. Keep it practical, short, and based on repo evidence.

Do not infer personality, seniority level, work ethic, overtime, or use labels like "junior", "senior", "weak", "sloppy", or "AI-generated". Git history can show contribution patterns, not private intent or ability. If a pattern suggests a coaching or review opportunity, state the observable pattern and the support needed.

Use this workflow:

1. Identify important codebase touchpoints.
   - Read README, CONTRIBUTING, docs, architecture notes, setup docs, and development docs if present.
   - Inspect package/build/config files and main source folders.
   - Identify important areas such as entrypoints, core workflows, shared libraries, data models, infra/deploy config, tests, docs, migration files, generated code, and high-change folders.
   - Use `git log --name-only`, `git log --numstat`, and `rg` to find frequently changed files and major modules.

2. Review recent git history.
   - Review the last 100 commits. If fewer than 100 commits exist, review all available commits.
   - Use commands like:

```bash
git log -100 --date=short --pretty=format:'%h%x09%ad%x09%an%x09%s'
git log -100 --name-only --pretty=format:'--- %h %ad %an %s' --date=short
git log -100 --numstat --pretty=format:'--- %h %ad %an %s' --date=short
git shortlog -sn --all
```

   - Identify recent themes, active areas, frequent contributors, large changes, refactors, test/docs changes, fixes, and churn.
   - Treat commit timestamps carefully. You may mention unusual timestamp clusters only as timestamps, not as proof of overtime or workload.

3. Run blame and line-history checks on important touchpoints.
   - For each important area, inspect representative files with `git blame`.
   - Prefer focused ranges around active or important code over blaming every file.
   - Use commands like:

```bash
git blame --date=short --line-porcelain -- path/to/file
git log --follow --date=short --pretty=format:'%h %ad %an %s' -- path/to/file
git log --numstat --date=short --pretty=format:'--- %h %ad %an %s' -- path/to/file
```

   - Use blame to identify current maintainers and domain knowledge, not to assign personal fault.
   - If a line came from a broad reformat or mechanical move, do not over-credit or over-blame it.

4. Detect pair or group contributions.
   - Treat a commit as paired/group work only when the commit message, trailers, or author metadata clearly show it, such as `Co-authored-by`, `Pair:`, `with`, or named collaborators.
   - Otherwise treat it as a single-author commit.
   - Attribute paired changes to the pair/group unless later solo commits clearly isolate ownership.
   - Do not try to identify a "weaker" individual inside a pair. Instead, note that the pattern belongs to the pair/group and list follow-up evidence if any.

5. Describe observable contribution patterns.
   - Capture who tends to work on frontend, backend, infra, data, tests, docs, tooling, refactors, cleanup, migrations, or fixes.
   - Capture who often changes shared/core files, who often adds tests/docs with code, and who often does cleanup or simplification work.
   - Capture review signals such as large commits, repeated churn, repeated suppressions, fragile areas, missing tests around changes, vague commit messages, or duplicated patterns.
   - Do not claim code was written by AI. If code looks repetitive or low-context, describe the observable pattern only.

6. Create `TEAM-DYNAMICS.md` using exactly this structure:

````markdown
# Team Dynamics

## Snapshot

- Range reviewed: [n] commits, [oldest date] to [newest date].
- Main active areas: [short comma-separated list.]
- Strongest ownership signals: [short comma-separated list.]
- Main collaboration signal: [pairing/group work summary, or "No explicit pairing found".]
- Main support gap: [one-line observable gap.]

## Evidence Reviewed

- Git history: `git log -100`, `git shortlog`, changed-file and numstat views.
- Blame checks: [short list of important files/folders checked.]
- Code areas sampled: [short list.]
- Limits: [one-line caveat, such as squash merges, generated files, missing author metadata, or shallow history.]

## Domain Ownership

| Area | Main contributors | Evidence | Confidence | Notes |
| --- | --- | --- | --- | --- |
| [area] | [person or pair] | `[files]`, `[commits]` | `High|Medium|Low` | [one-line note] |

## Important Touchpoints

| Touchpoint | Current owners / maintainers | Why it matters | Evidence |
| --- | --- | --- | --- |
| `[file or folder]` | [person or pair] | [one-line reason] | `git blame`, `git log --follow` |

## Collaboration Patterns

- Explicit pairs/groups: [pairs from commit messages, or "None evident".]
- Solo-heavy areas: [areas mostly changed by one person, or "None evident".]
- Shared areas: [areas changed by multiple people, or "None evident".]
- Handoff signals: [recent ownership shift or "None evident".]

## Contribution Patterns

| Contributor or pair | Main areas | Common change type | Stewardship signals | Review/support signals |
| --- | --- | --- | --- | --- |
| [name or pair] | [short list] | [features/fixes/refactors/docs/tests/etc.] | [tests/docs/refactors/cleanup/tooling] | [large diffs/churn/missing tests/etc.] |

## Code Examples

### Consistent Pattern

- [One-line explanation.]

```text
[small code or file-shape example, under 12 lines]
```

### Pattern That Needs Alignment

- [One-line explanation.]

```text
[small contrasting example, under 12 lines]
```

## Team Support Opportunities

- [One-line coaching, review, ownership, documentation, or pairing opportunity.]
- [One-line coaching, review, ownership, documentation, or pairing opportunity.]
- [One-line coaching, review, ownership, documentation, or pairing opportunity.]

## Analysis

- Domain knowledge: [who knows which parts best, one line.]
- Cleanup and refactor energy: [who/which pair improves shared quality, one line.]
- Risk concentration: [area or ownership concentration risk, one line.]
- Code smell pressure: [observable smell/churn/suppression pattern, one line.]
- Suggested lead action: [one-line next step.]
````

7. Output requirements.
   - Keep every section concise.
   - Use names exactly as shown in git history.
   - Use commit hashes and file paths as evidence, but do not dump command output.
   - Include paired/group attribution only when explicitly visible in git metadata or commit text.
   - Use `High`, `Medium`, or `Low` confidence for ownership claims.
   - Keep code examples short and only include them when they clarify patterns faster than prose.
   - If blame is distorted by formatting, generated files, vendored files, or large moves, state that limitation.
   - If the repo lacks enough history, state the limitation instead of overreaching.

8. Style requirements.
   - Be concise over complete prose.
   - Use simple words and low jargon.
   - Stay factual and non-judgmental.
   - Avoid ranking people.
   - Avoid private intent or personality claims.
   - Avoid claims about AI-written code.
   - Do not include setup instructions, test logs, or change history.
   - Do not modify application code.

9. Verification and final response.
   - Read back `TEAM-DYNAMICS.md` before finalizing.
   - For docs-only edits, tests are not required unless the repo has a docs validation command.
   - In the final response, link to `TEAM-DYNAMICS.md`, summarize the commit range reviewed, the main ownership areas found, and any limits caused by git history or blame accuracy.
