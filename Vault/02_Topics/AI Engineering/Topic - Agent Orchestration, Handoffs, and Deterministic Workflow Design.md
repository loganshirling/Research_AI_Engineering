---
title: Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design
type: topic
status: active
topic: Advanced AI Systems
course: AI Engineering
tags: [agents, orchestration, handoffs, workflows, deterministic]
prerequisites:
  - "[[Topic - Tool Calling, Structured Outputs, and Agent Loops]]"
  - "[[Topic - LLM Observability and Tracing]]"
related:
  - "[[Topic - MCP, Tool Design, and Secure Agent Execution]]"
  - "[[Topic - End-to-End AI System Case Studies and Build Patterns]]"
sources:
  - "https://www.anthropic.com/research/building-effective-agents"
  - "https://developers.openai.com/api/docs/guides/agents-sdk/"
  - "https://google.github.io/adk-docs/agents/workflow-agents/"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: hard
must_know: true
---

# Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design

## Purpose

Learn how to decide what parts of an AI workflow should be LLM-driven, what should stay deterministic, and when specialized agents actually help.

## Why it matters

Agents are often presented as a magic unifier for complex tasks. In practice, most production systems win by being clear about:
- what the model may decide,
- what the application must enforce,
- what transitions need approval,
- and when a new agent is a real simplification versus extra complexity.

## 80/20 summary

- Start with the simplest workflow that can possibly work.
- Use a single agent plus tools before adding multi-agent handoffs.
- Keep sequence, approval, permissions, and final side-effect steps deterministic when possible.
- Use handoffs when a task crosses a real specialization boundary, not just because multi-agent sounds more advanced.

## What to understand

### Deterministic workflows first
For many business workflows, the order of steps is already known. That makes deterministic controllers a strong default: they are easier to debug, test, and audit than free-form agent loops.

### Handoffs are for specialization
Handoffs help when you have clear boundaries such as:
- a research agent handing off to a writing agent,
- a planning agent handing off to an execution agent,
- or a low-risk agent handing off to a higher-scrutiny step.

Heavy handoff use without clear roles often adds complexity without adding capability.

### Limits are part of the design
Every agent system needs explicit:
- step limits,
- timeouts,
- retry rules,
- approval gates,
- and stop conditions.

Those are not after-the-fact safety neasures; they are the workflow.

## Design defaults

- Make the workflow state explicit.
- Keep tool calls, approvals, and final actions traceable.
- Prefer deterministic orchestration until model judgment is clearly needed.
- Add handoffs only when they reduce prompt overload or role confusion.
- Log the why not just the what of agent transitions.

## Common mistakes

1. Building multi-agent systems before a single-agent baseline works.
2. Letting the model choose critical transitions that should be fixed.
3. Missing approval logic for high-impact actions.
4. Treating tracing as an optional extra.
5. Handing off to new agents when tool design would have been enough.

## What to read next

- [[Topic - MCP, Tool Design, and Secure Agent Execution]]
- [[Topic - Eval Flywheels, Human Review, and Regression Gates]]
- [[Practice - End-to-End Build - Internal Research Assistant]]
