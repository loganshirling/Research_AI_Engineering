---
title: Technique - Agent Approval Design
type: technique
status: active
topic: Agent Control
course: AI Engineering
tags: [agents, approvals, permissions, control-flows, safety]
prerequisites:
  - "[[Topic - Tool Calling, Structured Outputs, and Agent Loops]]"
related:
  - "[[Topic - AI Agents: Design, Boundaries, and Operations]]"
  - "[[Topic - OpenClaw and Self-Hosted Personal Agents]]"
  - "[[Source - Agent Systems Design, Approvals, and Monitoring - Core Sources]]"
sources:
  - "https://www.anthropic.com/research/building-effective-agents"
  - "https://docs.anthropic.com/en/docs/build-with-claude/tool-use"
  - "https://docs.openclaw.ai/gateway/security"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Technique - Agent Approval Design

## Purpose

Design approval checkpoints so agents can still be useful without being trusted with every high-impact action.

## When to use this

Use this for any workflow where the agent can:
- send messages,
- modify records,
- spend money,
- run privileged commands,
- expose sensitive data,
- or trigger external automation.

## Core principle

Approvals should be attached to **blast radius**, not to whether a system is called an "agent."

## Approval design checklist

### 1. Define the risky action clearly
Specify:
- what action is about to happen,
- what resources it affects,
- whether it is reversible,
- and what the worst realistic failure looks like.

### 2. Show the user the exact proposed action
A good approval prompt includes:
- human-readable summary,
- exact target,
- exact high-impact parameters,
- and the expected effect.

### 3. Bind approval to a specific plan
Approvals should be tied to:
- a request ID,
- a specific action type,
- a short validity window,
- and a hash or stable representation of the payload where practical.

### 4. Re-check authorization at execution time
Approval is not a substitute for authz.
Verify the actor, scope, and policy again before side effects happen.

### 5. Log the event
Record:
- proposed action,
- approver,
- timestamp,
- request ID,
- and execution result.

### 6. Separate read from write
Most systems can automate reads much earlier than writes.

## Good approval triggers

- external communication,
- destructive changes,
- privileged shell or admin actions,
- financial transactions,
- policy exceptions,
- high-cost operations,
- actions using sensitive connectors or secrets.

## Common mistakes

1. Approving vague summaries instead of exact actions.
2. Letting approvals stay valid too long.
3. Treating approval as the only safety control.
4. Mixing many actions into one approval step.
5. Failing to log what was approved.
6. Skipping re-authorization before execution.

## What good looks like

The user or operator can answer:
- what was approved,
- why it needed approval,
- who approved it,
- whether the action changed before execution,
- and what actually happened afterward.

## What to read next

- [[Topic - AI Agents: Design, Boundaries, and Operations]]
- [[Topic - OpenClaw and Self-Hosted Personal Agents]]
- [[Source - Agent Systems Design, Approvals, and Monitoring - Core Sources]]
