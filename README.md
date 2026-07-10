# OTel for Dynatrace — Agent Skills

Vendor-neutral skills that teach AI coding agents how to instrument applications with OpenTelemetry, configured for Dynatrace as the observability backend.
Covers SDK setup across languages, semantic conventions, Collector pipelines, and OTTL transformations.

Skills are packaged instructions that extend agent capabilities, following the [Agent Skills](https://agentskills.io/) format.
Originally authored by [Dash0](https://www.dash0.com), extended for Dynatrace by [Dynatrace](https://www.dynatrace.com).

## Installation

See [INSTALL.md](./INSTALL.md) for full instructions.

**Claude Code (quickstart):**

```bash
claude plugin marketplace add johngallagherdynatrace/otel-for-dynatrace
claude plugin install otel-dt@otel-for-dynatrace
```

Restart Claude Code after installing.

## How to use

Once installed, skills load automatically and the agent picks them up when a task matches.

**Examples:**
```
Add OpenTelemetry instrumentation to my app
```
```
My traces are broken — spans show up as separate roots instead of a connected trace
```
```
Set up an OpenTelemetry Collector pipeline that forwards to Dynatrace
```
```
Write an OTTL expression to redact credit card numbers from log bodies
```
```
Ensure that my HTTP server spans have the correct attributes
```
```
Help me fix high-cardinality metrics that are blowing up my costs
```

You can also invoke skills explicitly by name (e.g. `/otel-dt:review-instrumentation 42` to review PR #42).

## Why vendor-neutral OpenTelemetry

These skills are built around the OpenTelemetry specification, not any single backend.
The output is standard OTLP telemetry that any OpenTelemetry-compatible backend can ingest.

Vendor lock-in in observability comes from proprietary agents and attribute schemas.
Skills in this repository avoid both: they guide agents to use OpenTelemetry SDKs and the OpenTelemetry Collector, and to follow the upstream [Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/) for attribute, span, and metric naming.

## Why semantic conventions matter

[OpenTelemetry Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/) define standardized names, types, and semantics for telemetry attributes, metric names, span names, and status codes.
Following them is the single highest-leverage thing you can do for observability quality.

When instrumentation follows semantic conventions:
- Auto-instrumentation libraries, dashboards, and alerting rules work out of the box.
- Service maps, operation grouping, and error tracking derive correct results without manual configuration.
- Cross-service queries return consistent results because every service speaks the same attribute language.

When conventions are missing or inconsistent, these capabilities degrade silently: no errors, just incomplete data, broken topology views, and fragmented queries.

## Instrumentation score

Guidance in these skills aligns with the [Instrumentation Score](https://github.com/instrumentation-score/spec) specification, a vendor-neutral corpus of guidance that quantifies how well a service follows OpenTelemetry best practices.
The spec defines impact-weighted rules across resources, spans, metrics, and logs.
Following this guidance helps your services score higher, which means better observability outcomes downstream.

## Available skills

### [review-instrumentation](./skills/review-instrumentation/SKILL.md)

Reviews OpenTelemetry instrumentation changes for correctness and semantic convention compliance.
Invoke explicitly with `/otel-dt:review-instrumentation` — optionally pass a PR number or URL.

**Use when:**
- Reviewing a pull request that adds or modifies instrumentation
- Checking your current branch changes before opening a PR
- Auditing spans, metrics, logs, or resource attributes against OTel best practices

**Checks:**
- Span naming, kind, status codes, and lifecycle (`span.end()` always called)
- Exception recording and error handling
- Semantic convention compliance (attribute names, deprecated names)
- Resource attributes (`service.name`, `service.version`, `deployment.environment.name`)
- Metric instrument types, units, and naming
- Log severity and trace context correlation
- Sensitive data (PII, credentials) in attribute values or log bodies
- Context propagation (extraction on inbound, injection on outbound)

**Verdict:** `PASS`, `PASS WITH WARNINGS`, or `FAIL`

### [otel-instrumentation](./skills/otel-instrumentation/SKILL.md)

Expert guidance for implementing high-quality, cost-efficient OpenTelemetry telemetry.
Covers backend and browser instrumentation across multiple languages.

**Use when:**
- Setting up observability for a new service
- Adding traces, metrics, or logs to an application
- Debugging instrumentation issues
- Optimizing telemetry costs (cardinality, sampling)
- Connecting browser traces to backend traces

**Rules covered:**
- Telemetry (signal overview and correlation)
- Resources (service identity, environment, Kubernetes attributes)
- Metrics (instrument types, naming, units, cardinality)
- Logs (structured logging, severity, trace correlation)
- Node.js (auto-instrumentation, environment variables, Kubernetes)
- Go (SDK setup, instrumentation libraries, context propagation)
- Python (auto-instrumentation, Flask, Django, FastAPI)
- Java (javaagent, Spring Boot, JVM system properties)
- .NET (auto-instrumentation, ASP.NET Core, ActivitySource)
- Ruby (SDK setup, Rails, Sinatra)
- PHP (auto-instrumentation, Laravel, Symfony)
- Browser (OpenTelemetry JS, server correlation)
- Next.js (App Router, full-stack instrumentation, common gotchas)

**Platforms:**
- Node.js (Express, Fastify, NestJS, etc.)
- Go (net/http, gin, echo, fiber, etc.)
- Python (Flask, Django, FastAPI, etc.)
- Java (Spring Boot, Servlet, JAX-RS, etc.)
- .NET (ASP.NET Core, Entity Framework, etc.)
- Ruby (Rails, Sinatra, etc.)
- PHP (Laravel, Symfony, etc.)
- Browser (React, Vue, Next.js, etc.)
- Any OTLP-compatible backend

### [otel-semantic-conventions](./skills/otel-semantic-conventions/SKILL.md)

Expert guidance for selecting, applying, and reviewing OpenTelemetry semantic conventions: the standardized names, types, and semantics for telemetry attributes, span names, and status codes.

**Use when:**
- Choosing attributes for spans, metrics, or logs
- Naming spans or selecting span kinds
- Mapping HTTP status codes to span status
- Reviewing telemetry for semantic convention compliance
- Migrating from old to new attribute names
- Understanding Dynatrace derived attributes and Davis AI topology requirements

**Rules covered:**
- Attributes (registry, selection, placement, common attributes by domain, namespaces)
- Spans (naming patterns, span kind, status code mapping)
- Versioning (stability levels, migration, Dynatrace semantic convention handling)
- Dynatrace (derived attributes, Davis AI feature dependencies)

### [otel-collector](./skills/otel-collector/SKILL.md)

Expert guidance for configuring and deploying the OpenTelemetry Collector to receive, process, and export telemetry.
Covers pipeline configuration, deployment patterns, and forwarding to Dynatrace.

**Use when:**
- Setting up an OpenTelemetry Collector pipeline
- Configuring receivers, processors, or exporters
- Deploying the Collector to Kubernetes or Docker
- Forwarding telemetry to Dynatrace
- Tuning Collector performance (memory, batching, queuing)

**Rules covered:**
- Receivers (OTLP, Prometheus, filelog, hostmetrics)
- Exporters (OTLP/HTTP to Dynatrace, debug, authentication, retry, queuing)
- Processors (memory limiter, batch, resource detection, Kubernetes attributes, ordering)
- Pipelines (service section, per-signal configuration, connectors, fan-out)
- Deployment (agent vs gateway, DaemonSet, Deployment, Docker Compose, health checks)

### [otel-ottl](./skills/otel-ottl/SKILL.md)

Expert guidance for writing and debugging OpenTelemetry Transformation Language (OTTL) expressions for the OpenTelemetry Collector's transform and filter processors.

**Use when:**
- Writing OTTL expressions to transform, filter, or enrich telemetry
- Redacting sensitive data from spans, metrics, or logs
- Configuring transform or filter processors in the Collector
- Debugging OTTL syntax or runtime errors
- Optimizing Collector pipeline performance

**Capabilities:**
- Transform (modify attributes and values)
- Filter (drop unwanted telemetry)
- Redact (hide sensitive information)
- Enrich (add contextual metadata)
- Convert (change data types and formats)

**Contexts:** resource, scope, span, spanevent, metric, datapoint, log

## Automation with Claude Code

You can configure [Claude Code](https://docs.anthropic.com/en/docs/claude-code) to apply these skills automatically, both in interactive sessions and in headless CI/CD pipelines.

### Project instructions via CLAUDE.md

Add a `CLAUDE.md` file to your repository root with instructions that tell Claude Code when to use the skills.
Claude Code loads this file at the start of every session.

```markdown
# Observability

This project uses OpenTelemetry for observability with Dynatrace as the backend.
When adding or modifying instrumentation, follow the guidance from the installed `otel-dt` skills.

When working on application code or deployment specs, use the `otel-dt:otel-instrumentation` skill.
When working on Collector configuration, use the `otel-dt:otel-collector` skill.
When choosing or reviewing telemetry attributes, use the `otel-dt:otel-semantic-conventions` skill.
When writing or debugging OTTL expressions, use the `otel-dt:otel-ottl` skill.
When reviewing instrumentation changes in a PR or branch, use the `otel-dt:review-instrumentation` skill.
```

### Headless mode in CI/CD

Use `claude -p` to run Claude Code non-interactively in a pipeline.
This enables automated instrumentation reviews, skill-guided code generation, and PR checks.

```bash
# Review instrumentation quality on a pull request
claude -p "/otel-dt:review-instrumentation $PR_NUMBER" \
  --allowedTools "Read,Bash,Glob"
```

#### GitHub Actions example

```yaml
name: Instrumentation review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install skills
        run: |
          claude plugin marketplace add johngallagherdynatrace/otel-for-dynatrace
          claude plugin install otel-dt@otel-dt

      - name: Review instrumentation
        run: |
          claude -p "/otel-dt:review-instrumentation ${{ github.event.pull_request.number }}" \
            --allowedTools "Read,Bash,Glob" \
            --output-format json > review.json

      - name: Comment on PR
        run: |
          findings=$(jq -r '.result' review.json)
          gh pr comment "$PR_NUMBER" --body "## Instrumentation review"$'\n\n'"$findings"
        env:
          PR_NUMBER: ${{ github.event.pull_request.number }}
```

## Skill structure

Each skill contains:
- `SKILL.md` - Instructions for the agent
- `rules/` - Focused guidance documents
- `README.md` - Human-readable documentation
