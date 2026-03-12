---
title: Topic - LLM Observability and Tracing
type: topic
status: active
topic: Production Operations
course: AI Engineering
tags: [observability, tracing, opentelemetry, metrics]
prerequisites:
  - "[[Topic - Latency Engineering for LLM Systems]]"
related:
  - "[[Topic - Guardrails, Prompt Injection, and Output Validation]]"
  - "[[Topic - Tool Calling, Structured Outputs, and Agent Loops]]"
sources:
  - "https://opentelemetry.io/docs/specs/semconv/gen-ai/"
  - "https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-spans/"
  - "https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-events/"
  - "https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-metrics/"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - LLM Observability and Tracing

## Purpose

Explain how to make LLM systems debuggable, measurable, and reviewable when requests span retrieval, model calls, structured outputs, tool use, and user-visible results.

## Why it matters

Without observability, LLM incidents feel random. Teams see weird outputs, slow requests, or tool-misuse but cannot tell whether the problem came from retrieval, prompt shape, decode behavior, tool selection, or a regression in the application logic.

## 80/20 summary

- Observability means traces, metrics, and events across the whole request path.
- Tracing should connect retrieval, model calls, tool calls, validation, user-facing outputs, and errors.
- Metrics should cover latency, volume, failures, token usage, tool behavior, and safety-related signals.
- Bad observability often leads to bad decisions because teams optimize the wrong bottleneck.

## What to trace

Traces should connect the major steps in a request, such as:

1. request intake,
2. retrieval and filtering,
3. prompt assembly,
4. model call,
5. tool call or schema validation.
6. response delivery.

That is what makes it possible to answer questions like: where did the time go, which step failed, which tool was chosen, and what context the model actually saw.

## What to measure

At minimum, measure:

- request count and success rate,
- end-to-end latency and step-level latency,
- token input, token output, and streaming rate,
- retrieval hit quality or reranking signals,
- tool choice frequency and tool errors,
- schema validation failures,
- and safety-related flags or approval gates.

## Why OpenTelemetry matters here

OpenTelemetry's generative AI semantic conventions are useful because they give teams a shared way to name and record LLM spans, metrics, and events. That reduces the chance that each team invents a different tracing model that cannot be compared or reviewed clearly.

## Useful defaults

- Give each request a single trace id that spans retrieval, model calls, tools, and delivery.
- Log the actual chosen model, prompt shape, and tool decisions.
- Record validation errors and approval gates as first-class events.
- Keep sensitive data protected but still log enough structure for debugging.
