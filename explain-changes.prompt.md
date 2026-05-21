Explain the code changes in a guided walkthrough style.

Use this workflow:

1. Inspect the relevant changes first.
   - Use `git status --short` to understand uncommitted work.
   - If the user named a phase, read `PLAN.md` and identify that phase's goal and completed tasks.
   - Use `git diff --name-only` or the named files/phase context to identify the changed files.
   - Read the important files with line numbers before explaining them.
   - If you spot a code mismatch or failing check while preparing the walkthrough, fix or call it out before explaining stale code.

2. Explain from decision to implementation.
   - Start with the highest-level product or architecture decision.
   - Then walk through the main code files in dependency/order-of-use sequence.
   - For each section include:
     - filename with a clickable path
     - a small code snippet, usually 3-12 lines
     - what changed
     - why that decision was made
     - what this enables later, when relevant

3. Keep snippets small and purposeful.
   - Do not paste whole files or large blocks.
   - Prefer the smallest snippet that shows the idea.
   - If several lines are repetitive, summarize the pattern instead of quoting it all.
   - Use exact names from the code: interfaces, functions, tests, routes, packages, event names, and error codes.

4. Separate real behavior from scaffolding.
   - Be explicit when something is a real implementation.
   - Be explicit when something is a mock, contract, fixture, scaffold, or future integration point.
   - Do not imply a mock performs real production work.
   - If a later phase is required for end-to-end behavior, say which capability is still missing.

5. Include verification.
   - Mention the focused checks that prove the explained area.
   - If you ran commands, list them.
   - If you did not run commands, say that plainly.
   - If a check fails during the walkthrough, report the failure and either fix it or explain why it remains.

Desired answer shape:

```md
I checked the changes first. <Any important caveat or fix.>

**1. <Decision Or File Theme>**

[file-name.ts](/absolute/path/file-name.ts:line)

```ts
<small snippet>
```

Decision: <what changed>.

Why: <reason/tradeoff>.

**2. <Next Theme>**

[another-file.ts](/absolute/path/another-file.ts:line)

```ts
<small snippet>
```

Decision: <what changed>.

Why: <reason/tradeoff>.

...

Verified with:

```bash
<commands>
```

Limitations / not yet covered: <short, honest list>.
```

Style requirements:

- Keep the walkthrough concrete and easy to follow.
- Use numbered sections with short titles.
- Prefer "Decision:" and "Why:" labels for each chunk.
- Avoid broad architecture essays unless they clarify a specific code decision.
- Keep the final answer concise enough to read in one pass.
