---
title: Topic - Tool Calling, Structured Outputs, and Agent Loops
type: topic
status: active
topic: Prompting and Workflows
course: AI Engineering
tags: [tool-calling, structured-outputs, agents, workflows]
prerequisites:
  - "[[Topic - Model Selection, Benchmarking, and Tradeoff Triage]]"
related:
  - "[[Topic - Guardrails, Prompt Injection, and Output Validation]]"
  - "[[Topic - LLM Observability and Tracing]]"
sources:
  - "https://platform.openai.com/docs/guides/structured-outputs"
  - "https://platform.openai.com/docs/api-reference/responses"
  - "https://docs.anthropic.com/en/docs/build-with-claude/tool-use"
  - "https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/implement-tool-use"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - Tool Calling, Structured Outputs, and Agent Loops

## Purpose

Build a practical mental model for when to ask the model to return free form text, when to require structured outputs, when to let it choose tools, and how agent loops should be bounded by application policy.

## Why it matters

Most useful AI systems need more than a paragraph. They need predictable JSON, safe tool calls, validated outputs, and controlled loops where the application, not the model, owns the execution boundary.

## 80/20 summary

- Use structured outputs when the application needs typed, validatable results.
- Use tool calling when the model needs to delegate tasks to external functions or systems.
- An agent loop is not magic; it is a controlled workflow of reason, select, act, and review steps.
- The application should own step limits, permissions, retries, and validation.

## Structured outputs

Structured outputs are for situations where the answer must match a known shape. That might be a schema, a fixed JSON object, or a strict set of fields for a downstream system. The practical goal is not beautiful formatting; it is reducing parsing failure, making validation easier, and keeping application logic simple.

## Tool calling

Tool calling is for situations where the model should choose from a bounded set of capabilities such as web fetch, database read, or business logic. The application should still own the tool definitions, permissions, timeouts, and success criteria.

## Agent loops in plain language

An agent loop is usually just:

1. inspect the task,
2. choose a next action,
3. call the right tool or produce a structured result,
4. review the new state, and
5. stop when the goal is met or a limit is hit.

The key engineering point is that the application, not the model, should enforce the stop rules.

## Useful defaults

- Prefer structured outputs for downstream systems.
- Keep tool surface areas small and descriptive.
- Validate tool inputs and tool results outside the model.
- Enforce max loop counts, timeouts, and permissions.
- Log each tool decision and structured output failure for evaluation.

## Common mistakes

1. Asking for free-form text when the application actually needs structured data.
2. Giving an agent too many tools without clear purposes or permissions.
3. Letting the model run unbounded loops.
4. Trusting tool results or structured outputs without separate validation.

## What to read next

- [[Topic - Guardrails, Prompt Injection, and Output Validation]]
- [[Topic - LLM Observability and Tracing]]
