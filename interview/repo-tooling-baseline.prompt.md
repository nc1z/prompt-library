Create or replace `TOOLING_BASELINE.md` as a fast setup and verification map for the take-home repo.

The goal is to know how to install, run, test, lint, typecheck, build, and smoke-check the codebase without losing assessment time.

Do not modify application code in this prompt.

Use this workflow:

1. Inspect tooling evidence.
   - Read package manager files, workspace files, package scripts, build configs, test configs, env examples, Docker/compose files, CI workflows, and README setup sections.
   - For Node/TypeScript repos, prefer pnpm if the repo uses pnpm or the assessment requires pnpm.
   - Identify the fastest targeted validation loop and the broader final guardrails.
   - Do not spend time on contributor history.

2. Optionally run cheap baseline commands.
   - Run version checks or script discovery when useful.
   - Do not run long install/build/test commands unless the user asks or the assessment setup requires it.
   - If commands are not run, document them as candidate commands, not passed checks.

3. Create `TOOLING_BASELINE.md` using this structure:

```markdown
# Tooling Baseline

## Snapshot

- Package manager: [tool/version/evidence.]
- Runtime: [runtime/version requirement/evidence.]
- App shape: [monorepo/single app/full-stack/etc.]
- Fastest validation loop: `[command]`
- Final guardrails: `[command]`, `[command]`

## Commands

| Purpose | Command | Evidence | Notes |
| --- | --- | --- | --- |
| Install | `[command]` | `[file]` | [note] |
| Dev server | `[command]` | `[file]` | [note] |
| Unit tests | `[command]` | `[file]` | [note] |
| Integration tests | `[command]` | `[file]` | [note] |
| E2E tests | `[command]` | `[file]` | [note] |
| Lint | `[command]` | `[file]` | [note] |
| Typecheck | `[command]` | `[file]` | [note] |
| Build | `[command]` | `[file]` | [note] |

## Config And Environment

- Env files: [short list.]
- Required services: [database/cache/browser/etc. or "Not evident."]
- CI gates: [workflow names/commands or "Not evident."]
- Local-only caveats: [short list.]

## Assessment Loop

1. [Baseline check.]
2. [Targeted test after failing-test subtask.]
3. [Targeted test after implementation.]
4. [Broader guardrails before commit.]

## Evidence Reviewed

- `[file/path]`
```

4. Output requirements.
   - Use exact script names and config files.
   - Distinguish commands discovered from commands successfully run.
   - Keep it practical for a 90-minute run.

5. Final response.
   - Link to `TOOLING_BASELINE.md`.
   - State the fastest validation loop and final guardrails.
