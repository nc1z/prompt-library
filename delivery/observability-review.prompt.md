Create or update `OBSERVABILITY-REVIEW.md` in the repository root as a concise review of the codebase's observability setup.

The goal is to understand how the system is instrumented today: logs, metrics, traces, APM, error tracking, dashboards, alerts, and error-handling quality. Keep the document short, factual, and based on repo evidence. Do not assume deployed settings that are not visible in code, config, infrastructure, or docs.

Use this workflow:

1. Inspect observability sources of truth.
   - Read README, docs, runbooks, deployment docs, incident docs, architecture notes, and operations docs if present.
   - Inspect app config, environment variable docs, infrastructure files, deployment files, serverless/lambda config, Docker files, Kubernetes manifests, Terraform, CDK, Pulumi, CloudFormation, Helm charts, and CI workflows.
   - Inspect package/build files for observability dependencies such as OpenTelemetry, Datadog, New Relic, Elastic APM, Sentry, Honeycomb, Prometheus, Grafana, CloudWatch, X-Ray, Bugsnag, Rollbar, Winston, Pino, Bunyan, Log4j, Serilog, Zap, or equivalent.
   - Inspect entrypoints, middleware, background workers, queue consumers, scheduled jobs, and error boundaries.

2. Search for observability signals.
   - Use `rg` for terms such as `logger`, `log`, `trace`, `span`, `metric`, `counter`, `histogram`, `apm`, `otel`, `opentelemetry`, `xray`, `x-ray`, `sentry`, `datadog`, `newrelic`, `elastic`, `prometheus`, `grafana`, `cloudwatch`, `correlation`, `requestId`, `traceId`, `spanId`, `error`, `cause`, `stack`, `catch`, `finally`, `alert`, `dashboard`, and `runbook`.
   - For AWS Lambda, check whether X-Ray or tracing is enabled in infrastructure/config.
   - For Elastic, check whether APM agent setup and service metadata are configured.
   - For server frameworks, check request logging, request IDs, response status, duration, and error middleware.

3. Review logging quality.
   - Identify whether logs are structured or plain text.
   - Check whether logs include useful context: request ID, trace ID, route/job name, user/tenant/service ID when safe, external dependency name, status, duration, and error details.
   - Check whether logs avoid obvious secrets or sensitive payloads.
   - Check whether background jobs, queue consumers, and scheduled tasks log start, success, failure, retries, and dead-letter behavior.

4. Review tracing, metrics, and APM.
   - Identify trace instrumentation, span creation, propagation, sampling, service names, and exporter/provider config.
   - Identify metrics such as request count, latency, error count, queue depth, retry count, job duration, dependency latency, and business counters.
   - Identify APM setup and whether it covers HTTP, database, queues, serverless functions, and background workers.
   - If telemetry is only provided by platform defaults, state that clearly.

5. Review error handling.
   - Look for swallowed errors: empty `catch`, `catch` that only comments, `catch` that returns success, `Promise.catch` without logging/rethrow, callback errors ignored, broad rescue blocks, or failed jobs marked successful.
   - Check whether errors are logged with stack traces.
   - Check whether wrapped errors preserve cause, such as `new Error(message, { cause })`, `throw ... from ...`, or language-specific equivalent.
   - Check whether user-facing errors are separated from internal logs.
   - Check whether async/background errors are reported to error tracking.

6. Create `OBSERVABILITY-REVIEW.md` using exactly this structure:

````markdown
# Observability Review

## Snapshot

- Observability stack: [short list, or "Not evident from repo".]
- Trace/APM coverage: [one-line summary.]
- Logging coverage: [one-line summary.]
- Error handling risk: [one-line summary.]
- Alerts/dashboards: [one-line summary, or "Not evident from repo".]

## Observability Checklist

| Area | Status | Evidence | Coverage | Gap / Risk |
| --- | --- | --- | --- | --- |
| Structured logging | `Present|Partial|Missing|Unknown|Needs attention` | `[files/config]` | [what is covered] | [one-line gap] |
| Request IDs / correlation IDs | `Present|Partial|Missing|Unknown|Needs attention` | `[files/config]` | [what is covered] | [one-line gap] |
| Distributed tracing | `Present|Partial|Missing|Unknown|Needs attention` | `[files/config]` | [what is covered] | [one-line gap] |
| Lambda X-Ray / platform tracing | `Present|Partial|Missing|Unknown|Not applicable` | `[files/config]` | [what is covered] | [one-line gap] |
| APM agent | `Present|Partial|Missing|Unknown|Not applicable` | `[files/config]` | [what is covered] | [one-line gap] |
| Metrics | `Present|Partial|Missing|Unknown|Needs attention` | `[files/config]` | [what is covered] | [one-line gap] |
| Error tracking | `Present|Partial|Missing|Unknown|Needs attention` | `[files/config]` | [what is covered] | [one-line gap] |
| Error stack/cause preservation | `Present|Partial|Missing|Unknown|Needs attention` | `[files/code]` | [what is covered] | [one-line gap] |
| Swallowed-error protection | `Present|Partial|Missing|Unknown|Needs attention` | `[files/code]` | [what is covered] | [one-line gap] |
| Background job visibility | `Present|Partial|Missing|Unknown|Not applicable` | `[files/code]` | [what is covered] | [one-line gap] |
| External dependency visibility | `Present|Partial|Missing|Unknown|Needs attention` | `[files/code]` | [what is covered] | [one-line gap] |
| Dashboards | `Present|Partial|Missing|Unknown` | `[files/docs/config]` | [what is covered] | [one-line gap] |
| Alerts | `Present|Partial|Missing|Unknown` | `[files/docs/config]` | [what is covered] | [one-line gap] |
| Runbooks / incident notes | `Present|Partial|Missing|Unknown` | `[files/docs]` | [what is covered] | [one-line gap] |

## Logging

- Format: [structured/plain/mixed/unknown.]
- Main logger: [tool/library, or "Not evident from repo".]
- Context included: [short list.]
- Context missing: [short list.]
- Sensitive data risk: [one-line finding, or "Not evident from repo".]

```text
[Small representative log example or logger call, under 12 lines.]
```

## Tracing And APM

- Tracing: [one-line finding.]
- APM: [one-line finding.]
- Propagation: [trace/request/correlation ID behavior, or "Not evident from repo".]
- Serverless/platform tracing: [Lambda X-Ray/platform default status, or "Not applicable".]

## Metrics, Dashboards, And Alerts

- Metrics: [one-line finding.]
- Dashboards: [one-line finding.]
- Alerts: [one-line finding.]
- Operational blind spot: [one-line finding.]

## Error Handling

- Stack traces: [one-line finding.]
- Error causes: [one-line finding.]
- Swallowed errors: [one-line finding.]
- Async/background errors: [one-line finding.]

```text
[Small representative error-handling example, under 12 lines.]
```

## Analysis

- Strongest signal: [one-line strongest observability area.]
- Biggest blind spot: [one-line weakest observability area.]
- Highest-risk error pattern: [one-line finding.]
- Best next check: [one-line follow-up, not a detailed plan.]
````

7. Output requirements.
   - Keep the checklist table as the main content.
   - Keep every table cell short.
   - Use exact file paths, config names, dependency names, environment variable names, and tool names from the repo.
   - Add extra rows only for meaningful repo-specific observability areas.
   - Use `Not applicable` for platform-specific rows that do not apply, such as Lambda X-Ray in a non-AWS/non-Lambda repo.
   - If observability exists only outside the repo, mark it `Unknown`.
   - Include code examples only when they reduce explanation; keep each example under 12 lines.
   - Do not call external observability services.
   - Do not run long or destructive commands.
   - Do not modify application code.

8. Style requirements.
   - Be concise over complete prose.
   - Use simple words and low jargon.
   - Prefer evidence over broad advice.
   - Avoid speculative claims.
   - Do not include command output dumps, setup instructions, or change history.

9. Verification and final response.
   - Read back `OBSERVABILITY-REVIEW.md` before finalizing.
   - For docs-only edits, tests are not required unless the repo has a docs validation command.
   - In the final response, link to `OBSERVABILITY-REVIEW.md`, summarize trace/APM coverage, logging quality, error-handling risk, and any unknowns caused by missing repo evidence.
