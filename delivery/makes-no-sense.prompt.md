Create or update `MAKES-NO-SENSE.md` in the repository root as a concise review of confusing, overbuilt, or arbitrary code paths.

The goal is to inspect the main touchpoints and entrypoints, then check whether the controller, service, repository, handler, job, model, adapter, or equivalent layers make sense for what the code is trying to do. Look for places where the code does too much, has unnecessary layers, repeats work, hides simple logic behind complex abstractions, or follows a pattern that is inconsistent with the rest of the repo.

Keep the document short, factual, and useful. This is not a full refactor plan or a broad code-quality audit. Prefer concrete examples over general advice.

Use this workflow:

1. Find the main touchpoints.
   - Read README, docs, architecture notes, and any existing overview files such as `BE-REQUEST-FLOW.md`, `WHATS-UP.md`, or `CODE-QUALITY-GUARDS.md` if present.
   - Inspect app entrypoints, route handlers, controllers, jobs, queue consumers, CLI commands, UI page roots, core services, repositories, adapters, and shared libraries.
   - If the repo does not use controller/service/repository naming, map the equivalent layers used by this codebase.

2. Trace each important flow.
   - Follow each important entrypoint from trigger to handler/controller, service/use-case layer, repository/data layer, external calls, and output.
   - Identify what the flow is actually trying to do in one simple sentence.
   - Compare the amount of code and layering against the actual job being done.
   - Focus on high-signal examples, not every small issue.

3. Look for "makes no sense" patterns.
   - Doing many steps when one direct step would be enough.
   - Passing data through layers that add no validation, business rule, authorization, transaction, logging, or useful abstraction.
   - Services that only call one repository method without adding meaning.
   - Repositories that contain business logic that belongs elsewhere.
   - Controllers or handlers that contain too much business logic.
   - Repeated mapping between nearly identical shapes.
   - Unused abstractions, wrappers, factories, adapters, or interfaces.
   - Multiple libraries or patterns solving the same problem in nearby code.
   - Defensive code for cases that cannot happen, without a clear reason.
   - Work done twice, such as repeated validation, repeated fetches, repeated serialization, or duplicate auth checks.
   - Hidden side effects in helpers that look harmless.
   - Tests forced to mock many layers for a simple behavior.

4. Create `MAKES-NO-SENSE.md` using exactly this structure:

````markdown
# Makes No Sense Review

## Snapshot

- Codebase shape: [one-line app/library/monorepo shape.]
- Layers found: [controller/service/repository/equivalent list.]
- Main issue pattern: [one-line summary.]
- Highest-impact confusing area: [file/folder/flow, or "Not evident from repo".]

## Layer Map

| Flow / touchpoint | Handler/controller | Service/use case | Repository/data | Notes |
| --- | --- | --- | --- | --- |
| [flow name] | `[file path]` | `[file path or none]` | `[file path or none]` | [one-line note] |

## Findings

| Area | What makes no sense | Why it matters | Evidence | Severity |
| --- | --- | --- | --- | --- |
| `[file/folder/flow]` | [one-line issue] | [one-line impact] | `[file path]` | `Low|Medium|High` |

## Best Examples

### [Short Finding Title]

- Current job: [one sentence explaining what the code is trying to do.]
- Current shape: [one sentence explaining the layers or steps.]
- Simpler shape: [one sentence explaining the likely simpler path.]

```text
[Tiny code or flow example, under 12 lines.]
```

## Repeated Patterns

- [One-line repeated confusing pattern, or "Not evident from repo".]
- [One-line repeated confusing pattern, or "Not evident from repo".]
- [One-line repeated confusing pattern, or "Not evident from repo".]

## Analysis

- Biggest simplification opportunity: [one line.]
- Most confusing layer boundary: [one line.]
- Most likely test pain: [one line.]
- Best next check: [one line, not a detailed plan.]
````

5. Output requirements.
   - Keep every section concise.
   - Include only high-signal findings. Prefer 3-8 findings over a long list.
   - Use exact file paths, function names, class names, and flow names from the repo.
   - Use `Low`, `Medium`, or `High` severity based on impact to readability, testability, bug risk, or maintenance cost.
   - Include code examples only when they make the issue clearer; keep each example under 12 lines.
   - If a layer exists for a clear reason, such as transactions, authorization, validation, retries, caching, dependency isolation, or framework boundaries, do not flag it as unnecessary.
   - If the repo is too small or no confusing patterns are found, say that clearly and list what was checked.
   - Do not write detailed refactor plans.
   - Do not modify application code.

6. Style requirements.
   - Be concise over complete prose.
   - Use simple words and low jargon.
   - Prefer evidence over opinions.
   - Avoid broad best-practice essays.
   - Avoid blaming authors.
   - Do not include command output dumps, setup instructions, or change history.

7. Verification and final response.
   - Read back `MAKES-NO-SENSE.md` before finalizing.
   - For docs-only edits, tests are not required unless the repo has a docs validation command.
   - In the final response, link to `MAKES-NO-SENSE.md`, summarize the number of findings, the highest-impact confusing area, and any limits caused by missing architecture or entrypoint evidence.
