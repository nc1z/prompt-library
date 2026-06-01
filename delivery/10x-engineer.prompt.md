Act as an orchestrator agent and create `IMMEDIATE_IMPACT.md` in the repository root.

The goal is to identify the highest-value changes a new developer can make quickly, like a high-leverage engineer joining the team. The final document must rank concrete opportunities by impact, ease, and speed. Impact can come from code, architecture, workflow, observability, test speed, AI readiness, documentation, team enablement, cost savings, or risk reduction.

This is a context-heavy workflow. Use subagents whenever subagent tooling is available. If subagents are unavailable, run the same phases sequentially and state that fallback in `IMMEDIATE_IMPACT.md`.

Do not modify application code. Generated review documents are allowed.

Use this workflow:

1. Prepare the orchestration.
   - Confirm these prompt files exist:
     - `delivery/architecture-review.prompt.md`
     - `delivery/backend-request-flow.prompt.md`
     - `delivery/backend-acm.prompt.md`
     - `delivery/team-dynamics.prompt.md`
     - `delivery/whats-up.prompt.md`
     - `delivery/decompose.prompt.md`
     - `delivery/ai-readiness.prompt.md`
     - `delivery/code-quality-guards.prompt.md`
     - `delivery/makes-no-sense.prompt.md`
     - `delivery/cyclic-deps.prompt.md`
     - `delivery/observability-review.prompt.md`
     - `delivery/workflow-efficiency.prompt.md`
   - If a prompt is missing, continue with the available prompts and record the gap.
   - Tell each subagent to keep its generated markdown concise and evidence-based.
   - Tell each subagent not to modify application code.

2. Run foundation context prompts in parallel.
   - Subagent A: execute `delivery/architecture-review.prompt.md` and create/update `ARCHITECTURE-REVIEW.md`.
   - Subagent B: execute `delivery/backend-request-flow.prompt.md` and create/update `BE-REQUEST-FLOW.md`.
   - Subagent C: execute `delivery/backend-acm.prompt.md` and create/update `BE-ACM.md`.
   - Wait for all three to finish.

3. Reconcile foundation context into `tmp-context.md`.
   - Read `ARCHITECTURE-REVIEW.md`, `BE-REQUEST-FLOW.md`, and `BE-ACM.md`.
   - Create or update `tmp-context.md` with only high-signal context for later subagents:

````markdown
# Temporary Review Context

## App Shape

- [one-line architecture/runtime shape.]
- [one-line design style and main modules.]
- [one-line auth/access-control shape.]

## Important Touchpoints

| Area | Files / folders | Why it matters |
| --- | --- | --- |
| [area] | `[paths]` | [one-line reason] |

## Entrypoints And Flows

- [entrypoint or workflow] - [one-line purpose and source file.]

## Auth / Access Notes

- [one-line public/private/guard pattern.]
- [one-line role/permission/ownership note, or "Not evident".]

## Fast Context Commands

```bash
[useful rg/git commands for future subagents]
```

## Open Questions

- [missing or uncertain evidence future subagents should verify.]
````

4. Run team/context prompts in parallel.
   - Subagent D: execute `delivery/team-dynamics.prompt.md` and create/update `TEAM-DYNAMICS.md`.
   - Subagent E: execute `delivery/whats-up.prompt.md` and create/update `WHATS-UP.md`.
   - Wait for both to finish.

5. Update `tmp-context.md`.
   - Read `TEAM-DYNAMICS.md` and `WHATS-UP.md`.
   - Add concise sections to `tmp-context.md`:
     - `Team And Ownership Context`
     - `Current Conventions`
     - `Hot Areas From Git History`
     - `Likely High-Leverage Search Areas`
   - Keep this file compact. It is context for fresh subagents, not the final report.

6. Run gap-finding prompts in parallel.
   - Tell every subagent to read `tmp-context.md` first and use it to focus its search.
   - Run as many of these in parallel as subagent tooling reasonably supports:
     - `delivery/decompose.prompt.md` -> `DECOMPOSE.md`
     - `delivery/ai-readiness.prompt.md` -> `AI-READINESS.md`
     - `delivery/code-quality-guards.prompt.md` -> `CODE-QUALITY-GUARDS.md`
     - `delivery/makes-no-sense.prompt.md` -> `MAKES-NO-SENSE.md`
     - `delivery/cyclic-deps.prompt.md` -> `CYCLIC-DEPS.md`
     - `delivery/observability-review.prompt.md` -> `OBSERVABILITY-REVIEW.md`
     - `delivery/workflow-efficiency.prompt.md` -> `WORKFLOW-EFFICIENCY.md`
   - Wait for all subagents to finish.

7. Gather all generated context.
   - Read every generated file that exists:
     - `tmp-context.md`
     - `ARCHITECTURE-REVIEW.md`
     - `BE-REQUEST-FLOW.md`
     - `BE-ACM.md`
     - `TEAM-DYNAMICS.md`
     - `WHATS-UP.md`
     - `DECOMPOSE.md`
     - `AI-READINESS.md`
     - `CODE-QUALITY-GUARDS.md`
     - `MAKES-NO-SENSE.md`
     - `CYCLIC-DEPS.md`
     - `OBSERVABILITY-REVIEW.md`
     - `WORKFLOW-EFFICIENCY.md`
   - Extract opportunities that are actionable now.
   - Prefer opportunities that help the whole team repeatedly over one-off local code cleanup.

8. Score opportunities.
   - Use 1-5 scores:
     - Impact: 5 = team-wide/cost/risk/delivery speed impact; 3 = important module impact; 1 = narrow local cleanup.
     - Ease: 5 = simple change with low coordination; 3 = moderate; 1 = complex or cross-cutting.
     - Speed: 5 = same-day; 3 = a few days; 1 = longer project.
     - Confidence: 5 = strong evidence; 3 = some evidence; 1 = uncertain.
   - Compute `Immediate value score` as:

```text
(Impact * 3) + (Ease * 2) + (Speed * 2) + Confidence
```

   - Sort by `Immediate value score` descending.
   - If two items tie, rank the one with higher `Impact` first.
   - Keep recommendations concrete and scoped.

9. Create `IMMEDIATE_IMPACT.md` using exactly this structure:

````markdown
# Immediate Impact

## Snapshot

- Review mode: [subagents used / sequential fallback.]
- Context files reviewed: [count] generated review files.
- Best low-hanging fruit: [one-line top recommendation.]
- Highest team-level impact: [one-line recommendation.]
- Biggest risk reducer: [one-line recommendation.]

## Ranked Opportunities

| Rank | Opportunity | Scope | Impact | Ease | Speed | Confidence | Score | Evidence | First move |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | [concrete change] | `Team|Workflow|Architecture|Tests|Observability|AI|Code|Cost|Risk` | 1-5 | 1-5 | 1-5 | 1-5 | [score] | `[source docs/files]` | [one short action] |

## Top 5 To Do First

- [Rank 1 opportunity] - [why it is high leverage in one line.]
- [Rank 2 opportunity] - [why it is high leverage in one line.]
- [Rank 3 opportunity] - [why it is high leverage in one line.]
- [Rank 4 opportunity] - [why it is high leverage in one line.]
- [Rank 5 opportunity] - [why it is high leverage in one line.]

## Team-Level Leverage

- [One-line change that improves many developers' speed, safety, or clarity.]
- [One-line change that improves many developers' speed, safety, or clarity.]
- [One-line change that improves many developers' speed, safety, or clarity.]

## Code-Level Leverage

- [One-line codebase improvement with narrower scope.]
- [One-line codebase improvement with narrower scope.]
- [One-line codebase improvement with narrower scope.]

## Cost / Time Savings

- [One-line savings opportunity, or "Not evident from repo".]
- [One-line savings opportunity, or "Not evident from repo".]

## Evidence Map

| Source review | Strongest signal used |
| --- | --- |
| `ARCHITECTURE-REVIEW.md` | [one-line signal] |
| `BE-REQUEST-FLOW.md` | [one-line signal] |
| `BE-ACM.md` | [one-line signal] |
| `TEAM-DYNAMICS.md` | [one-line signal] |
| `WHATS-UP.md` | [one-line signal] |
| `DECOMPOSE.md` | [one-line signal] |
| `AI-READINESS.md` | [one-line signal] |
| `CODE-QUALITY-GUARDS.md` | [one-line signal] |
| `MAKES-NO-SENSE.md` | [one-line signal] |
| `CYCLIC-DEPS.md` | [one-line signal] |
| `OBSERVABILITY-REVIEW.md` | [one-line signal] |
| `WORKFLOW-EFFICIENCY.md` | [one-line signal] |

## Notes

- [One-line uncertainty or missing evidence.]
- [One-line constraint or dependency.]
- [One-line next review area.]
````

10. Output requirements.
   - Keep `IMMEDIATE_IMPACT.md` concise and action-oriented.
   - Include at least 8 ranked opportunities if the evidence supports them.
   - Each opportunity must be concrete enough that a developer can start immediately.
   - Do not create broad goals like "improve architecture"; name the concrete first move.
   - Prefer team-level leverage over narrow code cleanup when impact is similar.
   - Do not inflate scores without evidence.
   - If an opportunity is high impact but hard, rank it honestly with low ease/speed.
   - Include opportunities beyond code when supported: workflow speed, testing, docs, prompts, AI assets, observability, cost, reliability, onboarding, release process.
   - Do not include long implementation plans.
   - Do not include command output dumps.
   - Do not modify application code.

11. Verification and final response.
   - Read back `IMMEDIATE_IMPACT.md` before finalizing.
   - For docs-only edits, tests are not required unless the repo has a docs validation command.
   - In the final response, link to `IMMEDIATE_IMPACT.md`, summarize the top three ranked opportunities, mention whether subagents were used or sequential fallback was used, and mention any missing review files.
