---
title: Topic - MCP, Tool Design, and Secure Agent Execution
type: topic
status: active
topic: Advanced AI Systems
course: AI Engineering
tags: [mcp, tools, security, agents, execution]
prerequisites:
  - "[[Topic - Tool Calling, Structured Outputs, and Agent Loops]]"
  - "[[Topic - Guardrails, Prompt Injection, and Output Validation]]"
related:
  - "[[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]]"
  - "[[Topic - Eval Flywheels, Human Review, and Regression Gates]]"
sources:
  - "https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices"
  - "https://www.anthropic.com/engineering/writing-tools-for-agents"
  - "https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: hard
must_know: true
---

# Topic - MCP, Tool Design, and Secure Agent Execution

## Purpose

Learn how to design tools and agent-app boundaries so the system gains capability without gaining uncontrolled risk.

## Why it matters

Once an agent can call tools, the system can do more than generate text. It can read, change, delete, purchase, or execute.

That makes tool design a realiability and security problem, not just a developer-experience detail.

## 80/20 summary

- Tools should be narrow, clearly named, and hard to misuse.
- Do not treat MCP or any agent framework as a security boundary by itself.
- Keep high-impact side-effects behind explicit approval or application-level policy.
- Trace tool invocations and validate both inputs and results outside the model.

## What to understand

### Tool design matters
Good tools reduce model ambiguity. That means:
- clear names,
- clear purposes,
- tight input shapes,
- small surface areas,
- and outputs that are easy to inspect and validate.

### MCP is an interop layer
MCP matters because it standardizes how models, tools, resources, and context providers connect. But standardization is not the same as trust. You still need permissions, isolation, auditing, secret handling, and scoping.

### Prompt injection becomes a tool risk problem
Once retrieved content, webpages, or documents can influence tool selection or execution, prompt injection moves from chat quality into application risk. That is why high-impact tools need approval gates and hard policy controls.

## Design defaults

- Give each tool a single clear job.
- Keep read-only and side-effect tools separate.
- Require approval for high-impact actions.
- Sanitize, validate, and log tool inputs and results.
- Treat secret handling, network access, and file access as explicit policy choices.

## Common mistakes

1. Giving one agent too many tools.
2. Mixing read and write capabilities in the same tool surface.
3. Trusting tool output without application-level validation.
4. Assuming MCP servers are safe by default.
5. Treating prompt injection as a text-only problem.

## What to read next

- [[Topic - Eval Flywheels, Human Review, and Regression Gates]]
- [[Topic - End-to-End AI System Case Studies and Build Patterns]]
- [[Practice - End-to-End Build - Internal Research Assistant]]
