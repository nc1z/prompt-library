Create or update `CODE-QUALITY-GUARDS.md` in the repository root as a concise code-quality guard map for this codebase.

The goal is to understand how prepared the repo is to prevent code smells, regressions, unsafe changes, and low-quality contributions. This prompt applies to any codebase: frontend, backend, library, app, infrastructure, or mixed repo.

Keep the document lightweight and non-verbose. Use short factual lines, simple words, and direct evidence from the repo. Do not invent tooling, CI checks, or policies that are not visible in files or documented scripts.

Use this workflow:

1. Inspect the codebase before writing.
   - Read README, CONTRIBUTING, docs, architecture notes, setup docs, and development docs if present.
   - Inspect package/build files such as `package.json`, `pnpm-lock.yaml`, `yarn.lock`, `package-lock.json`, `pyproject.toml`, `requirements*.txt`, `go.mod`, `Cargo.toml`, `pom.xml`, `build.gradle`, `.csproj`, `Gemfile`, `composer.json`, `Makefile`, `justfile`, `Taskfile.yml`, Docker files, and monorepo config.
   - Inspect lint, format, type, test, coverage, docs, and static-analysis config files.
   - Inspect CI files under `.github/workflows`, `.gitlab-ci.yml`, `.circleci`, `.buildkite`, `azure-pipelines.yml`, or similar.
   - Inspect editor/tooling files such as `.editorconfig`, `.prettierrc`, `.eslintrc*`, `eslint.config.*`, `biome.json`, `ruff.toml`, `.rubocop.yml`, `mypy.ini`, `tsconfig*.json`, `pytest.ini`, `vitest.config.*`, `jest.config.*`, `playwright.config.*`, `cypress.config.*`, `sonar-project.properties`, and `codecov.yml`.
   - Search for quality signals with `rg`: `TODO`, `FIXME`, `HACK`, `XXX`, `eslint-disable`, `ts-ignore`, `type: ignore`, `nocheck`, `skip`, `only`, `CodeQL`, `Sonar`, `Snyk`, `Dependabot`, `Renovate`, `coverage`, `pre-commit`, `husky`, `lint-staged`, `commitlint`.

2. Determine each guard's status from evidence.
   - Use `Present` only when config, scripts, or CI clearly exist.
   - Use `Partial` when tooling exists but is narrow, optional, local-only, or not wired into CI.
   - Use `Missing` when the repo has no visible guard.
   - Use `Unknown` when the guard may exist outside the repo but is not visible.
   - Prefer exact script names, config files, workflow names, and commands from the repo.
   - If scripts exist but cannot be proven to run in CI, say `local script only`.

3. Count code-smell markers.
   - Count `TODO`, `FIXME`, `HACK`, and `XXX` occurrences with `rg`.
   - Count suppressions such as `eslint-disable`, `@ts-ignore`, `@ts-expect-error`, `type: ignore`, `# noqa`, and `nocheck` where relevant.
   - Put counts in the table. If the repo is large and counting is slow, count with targeted `rg` and state the limitation.

4. Create `CODE-QUALITY-GUARDS.md` using exactly this structure:

````markdown
# Code Quality Guards

## Guard Checklist

- [ ] Formatting - `Present|Partial|Missing|Unknown`
- [ ] Linting - `Present|Partial|Missing|Unknown`
- [ ] Type checking / static typing - `Present|Partial|Missing|Unknown`
- [ ] Unit tests - `Present|Partial|Missing|Unknown`
- [ ] Integration tests - `Present|Partial|Missing|Unknown`
- [ ] End-to-end tests - `Present|Partial|Missing|Unknown`
- [ ] Coverage reporting or threshold - `Present|Partial|Missing|Unknown`
- [ ] CI build/test gate - `Present|Partial|Missing|Unknown`
- [ ] Dependency vulnerability scanning - `Present|Partial|Missing|Unknown`
- [ ] Code scanning / static analysis - `Present|Partial|Missing|Unknown`
- [ ] Secret scanning - `Present|Partial|Missing|Unknown`
- [ ] Pre-commit or pre-push hooks - `Present|Partial|Missing|Unknown`
- [ ] Documentation guardrails - `Present|Partial|Missing|Unknown`
- [ ] TODO / suppression hygiene - `Present|Partial|Missing|Unknown`

## Guard Table

| Guard | Status | Evidence | Scope | Gap / Next Step |
| --- | --- | --- | --- | --- |
| Formatting | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what it covers] | [one-line gap or "None obvious"] |
| Linting | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what it covers] | [one-line gap or "None obvious"] |
| Type checking / static typing | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what it covers] | [one-line gap or "None obvious"] |
| Unit tests | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what it covers] | [one-line gap or "None obvious"] |
| Integration tests | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what it covers] | [one-line gap or "None obvious"] |
| End-to-end tests | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what it covers] | [one-line gap or "None obvious"] |
| Coverage reporting or threshold | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what it covers] | [one-line gap or "None obvious"] |
| CI build/test gate | `Present|Partial|Missing|Unknown` | `[workflow files]` | [what it runs] | [one-line gap or "None obvious"] |
| Dependency vulnerability scanning | `Present|Partial|Missing|Unknown` | `[files/workflows]` | [what it scans] | [one-line gap or "None obvious"] |
| Code scanning / static analysis | `Present|Partial|Missing|Unknown` | `[files/workflows]` | [what it scans] | [one-line gap or "None obvious"] |
| Secret scanning | `Present|Partial|Missing|Unknown` | `[files/workflows]` | [what it scans] | [one-line gap or "None obvious"] |
| Pre-commit or pre-push hooks | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what it blocks] | [one-line gap or "None obvious"] |
| Documentation guardrails | `Present|Partial|Missing|Unknown` | `[files/scripts]` | [what docs exist/check] | [one-line gap or "None obvious"] |
| TODO / suppression hygiene | `Present|Partial|Missing|Unknown` | `TODO: n, FIXME: n, HACK: n, XXX: n, suppressions: n` | [where found] | [one-line gap or "None obvious"] |
| Code smell detector | `Present|Partial|Missing|Unknown` | `[Sonar/CodeQL/Ruff/RuboCop/go vet/etc.]` | [what it covers] | [one-line gap or "None obvious"] |

## Analysis

- [Highest-impact missing guard, one line.]
- [Biggest partial/weak guard, one line.]
- [Most useful next improvement, one line.]
````

5. Output requirements.
   - Keep the checklist at the top.
   - In the checklist, mark completed boxes only for guards that are `Present`; leave `Partial`, `Missing`, and `Unknown` unchecked.
   - The table must include at least the rows shown in the template.
   - Add extra rows only when the repo has meaningful extra guards, such as schema checks, migration checks, API contract checks, accessibility tests, visual regression tests, performance budgets, license checks, SBOM generation, release checks, or generated-code drift checks.
   - Keep every table cell short. Prefer file paths and script names over prose.
   - If no evidence exists for a guard, use `Missing` or `Unknown`; do not guess.
   - If the repo uses multiple languages or packages, mention the covered area in `Scope`.

6. Style requirements.
   - Be concise over complete prose.
   - Use exact config file names, CI workflow names, script names, and command names from the repo.
   - Avoid broad best-practice essays.
   - Avoid scoring systems unless the repo already uses one.
   - Do not include command output dumps, setup instructions, or change history.
   - Do not modify application code.

7. Verification and final response.
   - Read back `CODE-QUALITY-GUARDS.md` before finalizing.
   - For docs-only edits, tests are not required unless the repo has a docs validation command.
   - In the final response, link to `CODE-QUALITY-GUARDS.md`, summarize the strongest and weakest guards, and mention any unknowns caused by missing repo evidence.
