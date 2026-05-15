---
name: dependency-install-safety
description: Use whenever adding, installing, updating, removing, auditing, or executing project dependencies in Node.js, JavaScript, TypeScript, Python, package.json, pyproject.toml, lockfiles, package managers, npm, npx, yarn, pnpm, pip, pipx, poetry, pipenv, uv, uvx, or dependency-related scripts. Enforces pnpm with minimumReleaseAge for Node.js, uv for Python, lockfile hygiene, and malicious-package checks before dependency changes.
---

# Dependency Install Safety

## Hard Rules

- Treat dependency installation and package execution as a supply-chain risk.
- For Node.js projects, always use `pnpm`; do not use `npm`, `npx`, `yarn`, or `bun` for dependency installs, package execution, or lockfile changes.
- For Python projects, always use `uv`; do not use `pip`, `pipx`, `poetry add`, `pipenv install`, or `easy_install` for dependency installs or package execution.
- Before adding, upgrading, installing, or executing a package, check for malicious-package signals, known vulnerabilities, typosquatting risk, and suspicious release history.
- Do not install packages released less than 7 days ago unless the user explicitly approves the exception after the risk is explained.
- Never run remote install scripts such as `curl ... | sh`, `wget ... | bash`, or unreviewed postinstall/bootstrap scripts.
- If a package risk is unclear, stop and explain the uncertainty before installing.

## Node.js Workflow

1. Confirm the project is Node.js by checking for `package.json`, `pnpm-lock.yaml`, workspace config, or JavaScript/TypeScript tooling.
2. Ensure pnpm has a 7-day minimum release age before installs:

```bash
pnpm config set minimumReleaseAge 10080
```

Prefer committing project-local policy when appropriate:

```ini
minimumReleaseAge=10080
```

3. Inspect package metadata before installing:

```bash
pnpm view <package> name version time dist-tags maintainers repository deprecated
```

4. Use pnpm commands only:

```bash
pnpm add <package>
pnpm add -D <package>
pnpm install
pnpm exec <command>
pnpm dlx <package>
```

5. After changes, run `pnpm audit` when supported and confirm dependency changes are limited to `package.json`, `pnpm-lock.yaml`, workspace config, and intentional project files.

## Python Workflow

1. Confirm the project is Python by checking for `pyproject.toml`, `uv.lock`, `requirements*.txt`, `.python-version`, or Python source/test files.
2. Use uv commands only:

```bash
uv add <package>
uv add --dev <package>
uv sync
uv lock
uv run <command>
uvx <package>
```

3. Inspect package metadata before installing. Prefer package registry metadata, the source repository, release history, maintainers, and vulnerability databases. Check for:
   - Name similarity to popular packages.
   - Recently published or suddenly transferred packages.
   - Missing or mismatched repository links.
   - Deprecated, abandoned, or yanked releases.
   - Unexpected install scripts, generated binaries, or network-heavy setup steps.
4. Preserve Python lockfile hygiene. Prefer `uv.lock` for application repositories. Do not silently introduce `requirements.txt`, Poetry lockfiles, or Pipenv lockfiles unless the project already uses them and the user wants that format retained.
5. After changes, run a vulnerability check when available in the project environment, then run the relevant tests or import smoke checks with `uv run`.

## Malicious-Package Checklist

Before installing any new package or new version:

- Verify the exact package name and scope against the user's request and surrounding ecosystem.
- Compare the package name against likely typosquats and dependency-confusion names.
- Inspect maintainers, publisher history, repository URL, license, deprecation status, and release timestamps.
- Prefer mature versions with stable release history over brand-new versions.
- Review install-time behavior when practical, especially `postinstall`, native build hooks, setup hooks, entry points, and downloaded binaries.
- Check known vulnerability sources available to the ecosystem, such as `pnpm audit`, OSV, PyPI advisories, GitHub advisories, or existing project scanners.
- Avoid packages with obfuscated code, unexplained minified sources, suspicious network calls, or mismatched package and repository contents.

## Existing Project Managers

- If a Node.js repository already uses another package manager, explain that this skill requires pnpm and ask before migrating lockfiles or package-manager metadata.
- If a Python repository already uses Poetry, Pipenv, Hatch, or plain requirements files, prefer using uv with the existing `pyproject.toml` or environment. Ask before replacing established lockfile workflows.
- Do not delete existing lockfiles or package-manager files unless the user explicitly approves the migration.

## Reporting

After dependency work, report:

- The package manager and commands used.
- The Node.js `minimumReleaseAge` setting when applicable.
- The packages and versions added, removed, or upgraded.
- Vulnerability and malicious-package checks performed.
- Tests, audits, or smoke checks run.
- Any exceptions or unresolved supply-chain risk.
