Create or update `DECOMPOSE.md` in the repository root as a concise review of code that is too large, nested, or hard to read.

The goal is to find verbose areas of the codebase: huge functions, large components, deep nesting, many braces, long switch/if chains, nested callbacks, large classes, and files where one unit does too much. Then explain how to make those areas easier to read using early returns, extracted functions, smaller service methods, or clearer layer boundaries.

Keep the document short and practical. This is a decomposition guide, not a full refactor plan. Do not modify application code.

Use this workflow:

1. Inspect the codebase shape.
   - Read README, docs, architecture notes, and any existing overview files such as `WHATS-UP.md`, `CODE-QUALITY-GUARDS.md`, or `MAKES-NO-SENSE.md` if present.
   - Inspect main source folders, entrypoints, controllers, services, repositories, jobs, UI components, shared helpers, tests, and scripts.
   - Identify the main language/framework so the scan fits the repo.

2. Find decomposition candidates.
   - Prefer existing repo tools if available, such as complexity reports, lint rules, Sonar, CodeClimate, ESLint complexity/max-lines rules, RuboCop metrics, Ruff/Pylint complexity, clippy, go vet, or test coverage reports.
   - If no tool exists, use lightweight inspection with `rg`, file reads, and simple counts.
   - Look for:
     - Very long functions, methods, classes, or UI components.
     - Files with many unrelated responsibilities.
     - Deeply nested `if`, `for`, `while`, `switch`, `try`, callbacks, promises, or JSX blocks.
     - Repeated blocks that could be named once.
     - Large validation, mapping, or branching logic inside handlers.
     - Services or helpers that mix orchestration, data access, formatting, auth, logging, and external calls.
     - Tests that are hard to read because setup and assertions are tangled.

3. Judge each candidate carefully.
   - Do not flag code as bad just because it is long; note why it is hard to read or maintain.
   - Prefer high-impact candidates in frequently used or important flows.
   - If a large function is generated, vendored, migration-only, or intentionally table-driven, skip it or mark it as low priority.
   - If the repo already has a convention for large orchestrators or components, note whether the candidate follows or breaks that convention.

4. Create `DECOMPOSE.md` using exactly this structure:

````markdown
# Decomposition Review

## Snapshot

- Codebase shape: [one-line app/library/monorepo shape.]
- Scan method: [tool/script/manual scan used.]
- Main readability issue: [one-line summary.]
- Highest-impact candidate: `[file path:function/component]`

## Candidate Overview

- `[file path:function/component]` - [one-line reason it is hard to read.]
- `[file path:function/component]` - [one-line reason it is hard to read.]
- `[file path:function/component]` - [one-line reason it is hard to read.]

## Candidate Table

| Candidate | Signal | Why it is hard to read | Suggested direction |
| --- | --- | --- | --- |
| `[file path:function/component]` | [long/nested/mixed concerns/etc.] | [one-line issue] | [early returns/extract helpers/split service/etc.] |

## Decomposition Notes

### [Candidate Name]

- Location: `[file path]`
- Current shape: [one-line summary of what it does now.]
- Pain: [one-line reason it is hard to maintain.]
- Better shape: [one-line decomposition direction.]

```text
[Tiny before/after flow or pseudocode example, under 12 lines.]
```

### [Candidate Name]

- Location: `[file path]`
- Current shape: [one-line summary of what it does now.]
- Pain: [one-line reason it is hard to maintain.]
- Better shape: [one-line decomposition direction.]

```text
[Tiny before/after flow or pseudocode example, under 12 lines.]
```

## Common Fix Patterns

- Early returns / guard clauses: [where they would reduce nesting.]
- Extract readable helpers: [where naming a block would reduce mental load.]
- Split service methods: [where one method mixes multiple workflow steps.]
- Move layer responsibilities: [where handler/service/repository/component boundaries are blurred.]

## Analysis

- Biggest readability win: [one line.]
- Most repeated decomposition need: [one line.]
- Lowest-risk first change: [one line.]
- Best next check: [one line, not a detailed plan.]
````

5. Output requirements.
   - Keep every section concise.
   - Include 3-10 candidates unless the repo has fewer meaningful cases.
   - Candidate overview bullets must include function/component/class names and file paths when discoverable.
   - Use exact file paths, function names, class names, component names, and script/tool names from the repo.
   - Include code or pseudocode examples only when they make the decomposition clearer; keep each example under 12 lines.
   - Focus on readability and maintainability, not style preferences.
   - Do not write detailed implementation plans.
   - Do not modify application code.

6. Style requirements.
   - Be concise over complete prose.
   - Use simple words and low jargon.
   - Prefer concrete evidence over broad advice.
   - Avoid blaming authors.
   - Do not include command output dumps, setup instructions, or change history.

7. Verification and final response.
   - Read back `DECOMPOSE.md` before finalizing.
   - For docs-only edits, tests are not required unless the repo has a docs validation command.
   - In the final response, link to `DECOMPOSE.md`, summarize the number of candidates, the highest-impact candidate, and any limits caused by missing complexity tooling.
