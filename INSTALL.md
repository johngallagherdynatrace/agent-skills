# Installation

## Claude Code

```bash
claude plugin marketplace add johngallagherdynatrace/otel-for-dynatrace
claude plugin install otel-for-dynatrace@otel-for-dynatrace
```

Restart Claude Code after installing. The 4 OTel skills (`otel-collector`, `otel-instrumentation`, `otel-ottl`, `otel-semantic-conventions`) will be available in every session.

## Cursor

```bash
git clone git@github.com:johngallagherdynatrace/otel-for-dynatrace.git ~/.cursor/skills/otel-for-dynatrace
```

Update with `git -C ~/.cursor/skills/otel-for-dynatrace pull`.

## Codex (OpenAI)

```bash
git clone git@github.com:johngallagherdynatrace/otel-for-dynatrace.git ~/.agents/skills/otel-for-dynatrace
```

Update with `git -C ~/.agents/skills/otel-for-dynatrace pull`.

## Copilot

```bash
git clone git@github.com:johngallagherdynatrace/otel-for-dynatrace.git ~/.copilot/skills/otel-for-dynatrace
```

## Gemini CLI

```bash
gemini extensions install https://github.com/johngallagherdynatrace/otel-for-dynatrace
```

Update with `gemini extensions update otel-for-dynatrace`.

## Openclaw

```bash
git clone git@github.com:johngallagherdynatrace/otel-for-dynatrace.git ~/.openclaw/skills/otel-for-dynatrace
```

## OpenCode

```bash
git clone git@github.com:johngallagherdynatrace/otel-for-dynatrace.git ~/.agents/skills/otel-for-dynatrace
```

OpenCode auto-discovers skills from `.agents/skills/`, `.opencode/skills/`, and `.claude/skills/`.
