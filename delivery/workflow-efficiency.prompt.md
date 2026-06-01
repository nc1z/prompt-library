Create or update `WORKFLOW-EFFICIENCY.md` in the repository root as a concise checklist of how efficient the team's development workflow appears from repo evidence.

The goal is to spot what is automated, what is manual, what is cached, and what can be improved. This is an overview scan, not a deep solution plan. Keep the document short, factual, and easy for a tech lead to scan.

Use this workflow:

1. Inspect workflow sources of truth.
   - Read README, CONTRIBUTING, docs, runbooks, setup docs, release docs, testing docs, and API docs if present.
   - Inspect scripts and task runners: `package.json`, `Makefile`, `justfile`, `Taskfile.yml`, `turbo.json`, `nx.json`, `pnpm-workspace.yaml`, `pyproject.toml`, `tox.ini`, `noxfile.py`, `go.mod`, `Cargo.toml`, `pom.xml`, `build.gradle`, Docker files, compose files, and devcontainer files.
   - Inspect Git hooks and local automation: `.husky`, `.git/hooks` notes if tracked, `.pre-commit-config.yaml`, `lefthook.yml`, `lint-staged`, `commitlint`, `pre-push`, and related config.
   - Inspect CI files: `.github/workflows`, `.gitlab-ci.yml`, `.circleci`, `.buildkite`, `azure-pipelines.yml`, or similar.
   - Inspect API testing assets: Postman collections, Bruno collections, Insomnia files, `.http` / `.rest` files, curl scripts, smoke-test scripts, contract tests, and OpenAPI files.
   - Inspect cache signals: package-manager cache, CI cache, test cache, lint cache, build cache, remote cache, Docker layer cache, framework caches, and incremental typecheck.

2. Search for manual workflow hints.
   - Use `rg` for terms such as `manual`, `copy`, `paste`, `curl`, `postman`, `bruno`, `insomnia`, `seed`, `reset`, `migrate`, `deploy`, `release`, `checklist`, `run locally`, `before commit`, `before push`, `TODO`, `FIXME`, `slow`, `cache`, `flaky`, and `skip`.
   - Treat docs as evidence only when they name concrete commands or process steps.
   - If a process likely exists outside the repo, mark it `Unknown`.

3. Classify each workflow item.
   - Use `Done` when the repo has clear automation or a maintained workflow.
   - Use `Partial` when it exists but is local-only, optional, incomplete, or not wired into CI.
   - Use `Missing` when no evidence exists.
   - Use `Unknown` when it may exist outside the repo but is not visible.
   - Use `Can improve` when evidence exists but the workflow still appears manual, duplicated, slow, uncached, or hard to discover.

4. Create `WORKFLOW-EFFICIENCY.md` using exactly this structure:

````markdown
# Workflow Efficiency

## Snapshot

- Repo shape: [one-line app/library/monorepo shape.]
- Main workflow style: [one-line summary of scripts, CI, or manual docs.]
- Biggest manual step: [one-line finding, or "Not evident from repo".]
- Biggest speed lever: [one-line finding, or "Not evident from repo".]

## Workflow Checklist

| Workflow item | Status | Evidence | Manual load | Improve? |
| --- | --- | --- | --- | --- |
| One-command setup | `Done|Partial|Missing|Unknown|Can improve` | `[files/scripts]` | [low/medium/high/unknown] | [one-line note] |
| Local dev command | `Done|Partial|Missing|Unknown|Can improve` | `[files/scripts]` | [low/medium/high/unknown] | [one-line note] |
| Pre-commit hooks | `Done|Partial|Missing|Unknown|Can improve` | `[files/config]` | [low/medium/high/unknown] | [one-line note] |
| Pre-push hooks | `Done|Partial|Missing|Unknown|Can improve` | `[files/config]` | [low/medium/high/unknown] | [one-line note] |
| Formatting workflow | `Done|Partial|Missing|Unknown|Can improve` | `[files/scripts]` | [low/medium/high/unknown] | [one-line note] |
| Lint workflow | `Done|Partial|Missing|Unknown|Can improve` | `[files/scripts]` | [low/medium/high/unknown] | [one-line note] |
| Test workflow | `Done|Partial|Missing|Unknown|Can improve` | `[files/scripts]` | [low/medium/high/unknown] | [one-line note] |
| Test caching | `Done|Partial|Missing|Unknown|Can improve` | `[files/config]` | [low/medium/high/unknown] | [one-line note] |
| Lint caching | `Done|Partial|Missing|Unknown|Can improve` | `[files/config]` | [low/medium/high/unknown] | [one-line note] |
| Build caching | `Done|Partial|Missing|Unknown|Can improve` | `[files/config]` | [low/medium/high/unknown] | [one-line note] |
| CI dependency caching | `Done|Partial|Missing|Unknown|Can improve` | `[workflow files]` | [low/medium/high/unknown] | [one-line note] |
| CI test/build gate | `Done|Partial|Missing|Unknown|Can improve` | `[workflow files]` | [low/medium/high/unknown] | [one-line note] |
| API endpoint testing | `Done|Partial|Missing|Unknown|Can improve` | `[Postman/Bruno/.http/OpenAPI/etc.]` | [low/medium/high/unknown] | [one-line note] |
| Local services setup | `Done|Partial|Missing|Unknown|Can improve` | `[Docker/devcontainer/scripts]` | [low/medium/high/unknown] | [one-line note] |
| Seed/reset data workflow | `Done|Partial|Missing|Unknown|Can improve` | `[files/scripts]` | [low/medium/high/unknown] | [one-line note] |
| Migration workflow | `Done|Partial|Missing|Unknown|Can improve` | `[files/scripts]` | [low/medium/high/unknown] | [one-line note] |
| Release/deploy workflow | `Done|Partial|Missing|Unknown|Can improve` | `[files/docs/workflows]` | [low/medium/high/unknown] | [one-line note] |
| Manual runbooks/checklists | `Done|Partial|Missing|Unknown|Can improve` | `[docs]` | [low/medium/high/unknown] | [one-line note] |

## Manual Work Signals

- [One-line manual step or repeated human action.]
- [One-line manual step or repeated human action.]
- [One-line manual step or repeated human action.]

## Automation And Cache Signals

- [One-line automation/cache signal.]
- [One-line automation/cache signal.]
- [One-line automation/cache signal.]

## Analysis

- Done well: [one-line strongest workflow.]
- Can improve: [one-line biggest workflow gap.]
- Unknown: [one-line process that may exist outside the repo.]
````

5. Output requirements.
   - Keep the checklist table as the main content.
   - Keep every table cell short.
   - Use exact file paths, script names, workflow names, and tool names from the repo.
   - Add extra rows only for meaningful repo-specific workflow items.
   - If there are no backend endpoints or API testing assets, say `Missing` or `Not applicable` clearly in the API row.
   - Do not write detailed implementation plans.
   - Do not run long or destructive commands.
   - Do not modify application code.

6. Style requirements.
   - Be concise over complete prose.
   - Use simple words and low jargon.
   - Prefer evidence over recommendations.
   - Avoid broad best-practice essays.
   - Avoid judging the team.
   - Do not include command output dumps, setup instructions, or change history.

7. Verification and final response.
   - Read back `WORKFLOW-EFFICIENCY.md` before finalizing.
   - For docs-only edits, tests are not required unless the repo has a docs validation command.
   - In the final response, link to `WORKFLOW-EFFICIENCY.md`, summarize the biggest manual step, the strongest automation, and any unknown workflow areas.
