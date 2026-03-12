---
title: Topic - Guardrails, Prompt Injection, and Output Validation
type: topic
status: active
topic: Safety and Security
course: AI Engineering
tags: [guardrails, prompt-injection, safety, validation]
prerequisites:
  - "[[Topic - Tool Calling, Structured Outputs, and Agent Loops]]"
related:
  - "[[Topic - LLM Observability and Tracing]]"
sources:
  - "https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html"
  - "https://genai.owasp.org/llmrisk/llm01-prompt-injection/"
  - "https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - Guardrails, Prompt Injection, and Output Validation

## Purpose

Explain how to think about LLM safety as a system design problem, not as a single prompt writing problem.

## Why it matters

An LLM can be influenced by user input, retrieved content, webpages, documents, and tool outputs. That means a safe system needs more than a good system prompt. It needs permissions, allowlists, validation, and control around the model.

## 80/20 summary

- Prompt injection is a system risk, not just a prompt weakness.
- Guardrails should exist around inputs, tools, retrieval, outputs, and permissions.
- Do not trust model output when a downstream system can validate it.
- Approval gates and permission boundaries matte more than verbose instructions.

## Prompt injection in plain language

Prompt injection happens when untrusted content can convince the model to ignore, override, or reindefine the intended instructions. In practice, that content might come from:

- user messages,
- retrieved documents,
- web pages,
- emails, or
- tool results.

## What guardrails actually are

Guardrails are controls surringyng the model. Useful categories include:

- input filtering and scoping,
- retrieval filtering and segregation,
- tool permissions and allowlists,
- output validation and schema checks,
- approval steps for high-risk actions, and
- observability for suspicious patterns.

## Output validation

Output validation is the practice of treating model results as candidates, not guarantees. That means:

- validate schemas,
- cast and sanitize fields,
- re-check permissions,
- and require human approval for irreversible actions.

## Useful defaults

- Keep tool surface areas small and clear.
- Separate untrusted retrieved text from system instructions.
- Require the application to check parameters before sending them to tools.
- Use human-approved steps for high-risk operations.
- Log and review instances of instruction conflict and suspicious tool choices.

## Common mistakes

1. Believing a strong system prompt alone is a security boundary.
2. Passing untrusted text directly into tool-choosing loops without controls.
3. Trusting model-generated arguments for sending email, spending money, or changing data without approval.

## What to read next

- [[Topic - Tool Calling, Structured Outputs, and Agent Loops]]
- [[Topic - LLM Observability and Tracing]]
