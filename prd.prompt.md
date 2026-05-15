Create a PRD for the following idea.

Use this structure:

# PRD: [Product / POC Name]

## Overview

Write a short summary of what the product or POC does and why it exists.

## Architecture

Include a Mermaid flowchart showing the main inputs, app/orchestration layer, tools or services, repositories/artifacts, and final outputs.

## Problem

Describe the business or user problem clearly.

## Goals

List concrete goals as bullets.

## Non-Goals

List what is intentionally out of scope for the first version or POC.

## POC Users

List the main user roles and what each one does.

## Demo Data Requirements

List the local fixtures, folders, mock data, or sample artifacts needed for a repeatable demo.

## Functional Requirements

Use “The system shall...” bullets for required behavior.

## Codex Workflow Requirements

If relevant, explain how Codex App, Codex CLI, or other agentic tools should be used, including review and edit boundaries.

## Evaluation Requirements

List how the POC should be tested or judged, including golden fixtures, expected outputs, false-positive traps, safety checks, or hallucination penalties.

## Animated Monitoring View Requirements

If the POC needs a visual demo, list the UI animation, timeline, counters, replay, and mock-event requirements.

## POC Acceptance Criteria

List clear pass/fail criteria for the demo.

## Production Design

Describe how the POC would translate into a production architecture.

## Production Safety Requirements

List safety, approval, audit, privacy, and rollback requirements.

## Open Questions

List unresolved product, technical, workflow, and ownership questions.

Guidelines:

- Make it demo friendly.
- Keep the PRD practical and buildable.
- Separate POC scope from production design.
- Use concrete folder names, data requirements, and output artifacts where useful.
- Include safety and human-review boundaries.
- Avoid vague feature language.
- Do not write implementation code.