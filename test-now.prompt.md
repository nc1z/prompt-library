Explain what the user can test right now.

Use this workflow:

1. Inspect the current repo state.
   - Read `PLAN.md` and identify the latest completed phase.
   - Check `package.json` and workspace package scripts for runnable commands.
   - If there is a relevant app/server package, inspect its package scripts and config.
   - Use `git status --short` only to understand whether uncommitted work may affect testability.

2. Separate testable behavior from non-testable future work.
   - Clearly state what can be tested now.
   - Clearly state what cannot be tested yet because it belongs to future phases.
   - Do not imply that stubs, mocks, contract routes, or harnesses perform real production work.

3. Give runnable commands.
   - Prefer commands the user can paste directly.
   - Include setup/start commands first.
   - Include a small smoke test.
   - Include one or two realistic flow tests using `curl` or the repo's existing test command.
   - Use placeholders like `<session_id>` only when unavoidable, and explain where the value comes from.

4. Keep the answer concise and practical.
   - Use short sections and code blocks.
   - Include expected output for the simplest smoke test.
   - Mention automated verification commands separately.
   - Do not include broad architecture explanation unless it affects what the user can test.

5. Be honest about limitations.
   - End with a short “not yet testable” note when relevant.
   - If a server cannot be run yet, say so and give the closest automated test command instead.
   - If a command was not verified in the current session, say that plainly.

Desired answer shape:

```md
You can test <capability> now.

Run:

```bash
<start command>
```

Smoke test:

```bash
<command>
```

Expected:

```json
<expected output>
```

Try the main flow:

```bash
<commands>
```

Automated checks:

```bash
<commands>
```

Not testable yet: <short list>.
```
