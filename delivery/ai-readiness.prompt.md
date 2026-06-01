Create or update `AI-READINESS.md` in the repository root as a concise overview of how ready this codebase is for an AI-native development workflow.

The goal is to show what AI-friendly assets, workflows, and integrations already exist: agent instructions, skills, custom agents, prompt files, specs, MCP usage, AI-assisted commit signals, and AI runtime or pipeline integrations. Keep the document short and evidence-based. Do not provide deep solutioning.

Use this workflow:

1. Inspect repo-level AI workflow files.
   - Look for agent instructions and AI docs such as `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, `.cursor/rules`, `.github/copilot-instructions.md`, `.windsurfrules`, `.aider*`, `.codex`, `prompts/`, `prompt-library/`, `skills/`, `agents/`, `mcp.json`, `.mcp.json`, and MCP server config.
   - Look for prompt/spec files such as `*.prompt.md`, `PRD.md`, `SPEC.md`, `PLAN.md`, `ARCHITECTURE.md`, design docs, issue templates, ADRs, and task breakdown docs.
   - Look for evals, fixtures, golden files, screenshots, test data, or replay scripts that help agents verify changes.

2. Inspect AI usage in code and pipelines.
   - Check package/build/config files and dependencies for AI SDKs or services such as OpenAI, Anthropic, Google Gemini, AWS Bedrock, Azure OpenAI, LangChain, LlamaIndex, Vercel AI SDK, Ollama, LiteLLM, OpenRouter, semantic-kernel, AutoGen, CrewAI, or equivalent.
   - Check CI/CD, scripts, and workflows for AI usage in review, testing, codegen, documentation, changelogs, triage, or deployment.
   - Check whether AI-generated artifacts or agent outputs are documented, validated, or ignored.

3. Inspect git history for AI-assist signals.
   - Review the last 100 commits. If fewer than 100 commits exist, review all available commits.
   - Use commands like:

```bash
git log -100 --date=short --pretty=format:'%h%x09%ad%x09%an%x09%s'
git log -100 --pretty=fuller
```

   - Search commit messages and trailers for signals such as `Co-authored-by`, `Copilot`, `Claude`, `Codex`, `ChatGPT`, `Cursor`, `Windsurf`, `Aider`, `Devin`, `generated`, or `AI`.
   - Treat these as workflow signals only. Do not infer code quality, ownership, or developer ability from AI co-author metadata.

4. Score readiness from repo evidence.
   - Use a simple percent score based on checklist rows:
     - `Present` = full credit.
     - `Partial` = half credit.
     - `Missing`, `Unknown`, or `Not applicable` = no credit.
   - If a row is not applicable, keep it in the table but exclude it from the score denominator.
   - Keep the score approximate; this is an overview, not a benchmark.

5. Create `AI-READINESS.md` using exactly this structure:

````markdown
# AI Readiness

## Snapshot

- Readiness score: [n]% ([Present count] present, [Partial count] partial, [Missing/Unknown count] missing or unknown).
- AI workflow style: [one-line summary.]
- Strongest AI-ready asset: [one-line finding, or "Not evident from repo".]
- Biggest gap: [one-line finding, or "Not evident from repo".]
- Git history signal: [one-line AI co-author/commit signal, or "Not evident from last [n] commits".]

## Readiness Checklist

| Area | Status | Evidence | Notes |
| --- | --- | --- | --- |
| Agent instructions | `Present|Partial|Missing|Unknown|Not applicable` | `[files]` | [one-line note] |
| Custom agents | `Present|Partial|Missing|Unknown|Not applicable` | `[files/folders]` | [one-line note] |
| Agent skills / tools | `Present|Partial|Missing|Unknown|Not applicable` | `[files/folders]` | [one-line note] |
| Prompt files | `Present|Partial|Missing|Unknown|Not applicable` | `[files/folders]` | [one-line note] |
| Spec / PRD / plan files | `Present|Partial|Missing|Unknown|Not applicable` | `[files]` | [one-line note] |
| Architecture / ADR docs | `Present|Partial|Missing|Unknown|Not applicable` | `[files]` | [one-line note] |
| Task templates / issue templates | `Present|Partial|Missing|Unknown|Not applicable` | `[files]` | [one-line note] |
| Evals / golden tests / fixtures | `Present|Partial|Missing|Unknown|Not applicable` | `[files/folders]` | [one-line note] |
| Replay or verification scripts | `Present|Partial|Missing|Unknown|Not applicable` | `[files/scripts]` | [one-line note] |
| MCP usage | `Present|Partial|Missing|Unknown|Not applicable` | `[files/config]` | [one-line note] |
| AI SDK/runtime integration | `Present|Partial|Missing|Unknown|Not applicable` | `[deps/files]` | [one-line note] |
| AI in CI/CD or automation | `Present|Partial|Missing|Unknown|Not applicable` | `[workflow files/scripts]` | [one-line note] |
| AI usage docs | `Present|Partial|Missing|Unknown|Not applicable` | `[docs]` | [one-line note] |
| AI-assisted commit signals | `Present|Partial|Missing|Unknown|Not applicable` | `git log -100` | [one-line note] |
| Generated artifact policy | `Present|Partial|Missing|Unknown|Not applicable` | `[files/docs]` | [one-line note] |

## Git History AI Signals

- Range reviewed: last [n] commits, [oldest date] to [newest date].
- Co-author signals: [short list, or "None evident".]
- AI tool mentions: [short list, or "None evident".]
- Limits: [one-line caveat, such as squash merges or missing commit trailers.]

## AI-Ready Assets

- [One-line asset that helps agents work safely, or "Not evident from repo".]
- [One-line asset that helps agents verify changes, or "Not evident from repo".]
- [One-line asset that helps agents understand product/code intent, or "Not evident from repo".]

## Improvements

- [One-line improvement or enhancement.]
- [One-line improvement or enhancement.]
- [One-line improvement or enhancement.]

## Analysis

- Ready now: [one-line strongest readiness signal.]
- Not ready yet: [one-line biggest missing piece.]
- Best next review: [one-line follow-up area, not a detailed plan.]
````

6. Output requirements.
   - Keep the checklist table as the main content.
   - Keep every table cell short.
   - Use exact file paths, folder names, dependency names, workflow names, and commit evidence from the repo.
   - Add extra rows only for meaningful repo-specific AI workflow signals.
   - Do not include detailed implementation plans or solution designs.
   - Do not infer code quality from AI co-author or tool metadata.
   - Do not call external AI services.
   - Do not modify application code.

7. Style requirements.
   - Be concise over complete prose.
   - Use simple words and low jargon.
   - Prefer evidence over speculation.
   - Avoid broad AI strategy essays.
   - Avoid judging contributors.
   - Do not include command output dumps, setup instructions, or change history.

8. Verification and final response.
   - Read back `AI-READINESS.md` before finalizing.
   - For docs-only edits, tests are not required unless the repo has a docs validation command.
   - In the final response, link to `AI-READINESS.md`, summarize the readiness score, strongest AI-ready asset, biggest gap, and any unknowns caused by missing repo evidence.
