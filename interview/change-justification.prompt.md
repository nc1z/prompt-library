Create or update `CHANGE_JUSTIFICATIONS.md` with concise written justifications for completed take-home fixes.

The goal is to satisfy the assessment requirement that every change explains the problem, root cause, fix, validation, and compatibility impact.

Use this workflow:

1. Identify completed fixes.
   - Read `IMPLEMENTATION_PLAN.md`, `ROOT_CAUSE_DEEP_DIVE.md`, `ISSUE_PRIORITY.md`, commit history, changed files, and test evidence when present.
   - If the user names a specific fix, write only that justification.
   - If commits already exist, map one justification to each atomic commit.
   - If commits do not exist yet, write pending justifications that can become commit bodies.
   - If no completed fix, changed file, or pending commit evidence exists, stop and say there is nothing to justify yet.

2. Verify evidence.
   - Read relevant changed files and tests.
   - Confirm the justification matches actual code changes.
   - Do not claim validation commands passed unless they were run successfully.
   - Do not hide skipped checks; explain them briefly.

3. Create or update `CHANGE_JUSTIFICATIONS.md` using this structure:

```markdown
# Change Justifications

## [Issue Title]

- Commit: `[sha or pending]`
- Category: Security / Performance / Reliability / Tooling / Tests / Other
- Severity: High / Medium / Low

### Problem

[One or two sentences describing what was wrong and how it could be triggered.]

### Root Cause

[One or two sentences explaining the real cause, with file/function evidence.]

### Fix

[One or two sentences explaining the minimal change and why it addresses the cause.]

### Validation

- `[command]` - Pass / Fail / Skipped: [short note.]
- `[command]` - Pass / Fail / Skipped: [short note.]

### Compatibility

[API/data/auth/security/migration/backwards compatibility notes, or "No compatibility impact expected because ..."]

### Non-Goals

- [Related work intentionally left out.]
```

4. Output requirements.
   - Keep each issue short.
   - Use exact paths, functions, test names, commands, and commit SHAs when available.
   - Make the root cause explicit.
   - Distinguish completed fixes from pending commit notes.
   - Include unresolved validation risk when checks were skipped or failed.

5. Final response.
   - Link to `CHANGE_JUSTIFICATIONS.md`.
   - List fixes documented.
   - State validation evidence and any skipped checks.
