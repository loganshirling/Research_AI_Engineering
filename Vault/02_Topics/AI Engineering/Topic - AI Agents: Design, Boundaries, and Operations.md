---
title: Topic - AI Agents: Design, Boundaries, and Operations
type: topic
status: active
topic: AI Agents
course: AI Engineering
tags: [agents, orchestration, approvals, observability, evals, reliability]
prerequisites:
  - "[[Topic - Tool Calling, Structured Outputs, and Agent Loops]]"
  - "[[Topic - LLM Observability and Tracing]]"
  - "[[Topic - Guardrails, Prompt Injection, and Output Validation]]"
related:
  - "[[Topic - Advanced Systems, Agents, Multimodal, and Voice]]"
  - "[[Topic - OpenClaw and Self-Hosted Personal Agents]]"
  - "[[Technique - Eval Operations]]"
  - "[[Technique - Context Budgeting]]"
  - "[[Technique - Agent Approval Design]]"
  - "[[Source - Agent Systems Design, Approvals, and Monitoring - Core Sources]]"
sources:
  - "https://www.anthropic.com/engineering/building-effective-agents"
  - "https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents"
  - "https://www.anthropic.com/engineering/multi-agent-research-system"
  - "https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents"
  - "https://developers.openai.com/api/reference/resources/responses/"
  - "https://developers.openai.com/api/reference/resources/evals/methods/list"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: hard
must_know: true
---

# Topic - AI Agents: Design, Boundaries, and Operations

## Purpose

Build a durable mental model for agent systems as **controlled applications**, not magical autonomous beings.

This note is about:
- when to use agents,
- when not to use them,
- where orchestration boundaries belong,
- how approvals and control flows should work,
- and how to monitor, evaluate, and recover from failure.

## Why it matters

Agent systems often fail for engineering reasons, not model reasons:
- the model sees the wrong context,
- the tool surface is vague,
- the workflow has no stop condition,
- approvals are too weak or too late,
- or operators cannot see what happened.

That makes agent design a systems problem, not only a prompting problem.[^anthropic-effective][^anthropic-context][^anthropic-harness]

## 80/20 summary

- Start with the **simplest workflow** that can solve the task.
- Use an agent only when the system benefits from model-led step selection or recovery.
- Keep the application responsible for permissions, retries, budgets, stop conditions, and side effects.
- Treat approvals, observability, and evals as part of the architecture, not afterthoughts.
- Multi-agent systems are sometimes useful, but they add coordination cost, monitoring cost, and failure surface area.[^anthropic-effective][^anthropic-multi-agent]

## What an agent actually is

In practical AI engineering, an agent is usually a loop that:
1. inspects task state,
2. decides the next action,
3. uses a tool or produces structured output,
4. updates state,
5. and repeats until a stop rule is reached.

The important part is not the word "agent." The important part is **who owns the loop logic** and **what is allowed to happen inside it**.

## When agents are worth it

Agents are often justified when:
- the next step depends on newly discovered information,
- the task needs bounded tool use across several turns,
- the system benefits from model-led recovery or replanning,
- or the workflow spans heterogeneous tools and documents.

Examples:
- internal research assistants,
- support triage with retrieval plus ticket actions,
- coding assistants,
- or long-running operational copilots with human checkpoints.

## When agents are not worth it

Do **not** default to an agent when:
- a deterministic workflow is enough,
- the task is one-shot extraction or transformation,
- the tool sequence is already known,
- the blast radius is high,
- or the system cannot yet be observed and evaluated well.

A brittle “agent” often underperforms a simpler workflow with:
- structured outputs,
- explicit routing,
- good retrieval,
- and a small number of deterministic steps.[^anthropic-effective]

## Architecture layers that matter

## 1. Model layer
The model handles pattern recognition, planning suggestions, synthesis, and flexible reasoning.

## 2. Harness layer
The harness owns:
- loop count,
- time budget,
- context budget,
- tool call limits,
- approval checkpoints,
- and termination rules.

## 3. Tool layer
Tools should be:
- narrowly scoped,
- clearly described,
- validated on input and output,
- and permissioned independently.

## 4. State layer
Keep durable state outside free-form prompt text when possible:
- task records,
- conversation history policy,
- retrieved evidence,
- approval state,
- and logs.

## 5. Operator layer
Operators need traces, metrics, incident review paths, and rollback options.

## Orchestration boundaries

A good question is: **what should the model decide, and what should the application decide?**

Good candidates for model control:
- selecting among a small set of next steps,
- summarizing evidence,
- proposing a plan,
- deciding whether more retrieval is needed,
- or drafting a user-facing response.

Good candidates for application control:
- permissions,
- budgets,
- retries,
- escalation policies,
- side-effect execution,
- identity and authorization,
- and approval enforcement.

The more expensive, destructive, or irreversible the action, the less it should depend on free-form model judgment.

## Approval and control flows

Approval design belongs to the application control layer.

Typical high-value checkpoints:
- sending external messages,
- changing records,
- financial actions,
- privileged shell or admin actions,
- policy exceptions,
- and actions involving sensitive data.

A healthy approval flow shows:
- the exact intended action,
- the exact target,
- the meaningful parameters,
- who approved it,
- when it was approved,
- and whether anything changed before execution.

See [[Technique - Agent Approval Design]] for the detailed pattern.

## Observability for agents

At minimum, log:
- request ID,
- model and prompt version,
- tool calls and tool results,
- approvals requested and granted,
- retrieval results used,
- step count,
- latency and cost,
- final status,
- and failure category.

Useful agent observability answers:
- What did the model see?
- Why did it choose this tool?
- Which step failed?
- Was the failure caused by retrieval, tool use, approval, or policy?
- Did the system recover, escalate, or silently stop?

Without traces, most “agent debugging” becomes guesswork.

## Failure handling and recovery

Design for common failure classes:
- tool timeout,
- malformed tool arguments,
- low-quality retrieval,
- approval denied,
- missing permissions,
- context overflow,
- repeated ineffective loops,
- and partial side-effect completion.

Recovery options should be explicit:
- retry with bounded count,
- replan,
- ask a clarifying question,
- reduce scope,
- switch to a deterministic fallback,
- or escalate to a human.

The system should also know when to stop instead of continuing to improvise.

## Eval strategy for agents

Agent evals should not stop at final-answer quality.

Also evaluate:
- tool selection correctness,
- approval correctness,
- step efficiency,
- recovery behavior,
- refusal/escalation behavior,
- structured output validity,
- and regression on known failure traces.

This is especially important for long-running or multi-tool agents, where the visible final output can hide expensive or risky internal behavior.[^openai-evals][^openai-responses]

## Multi-agent systems

Multi-agent architectures can help when:
- the task naturally decomposes,
- specialist contexts are truly different,
- or parallel exploration is valuable.

They are often overused.

Costs include:
- coordination overhead,
- duplicated context,
- harder attribution,
- more evaluation surface,
- more opportunities for hidden failure,
- and more operator complexity.[^anthropic-multi-agent]

Use multiple agents only when the separation is carrying real operational value.

## Common mistakes

1. Calling anything with tool use an “agent” and stopping the design work there.
2. Giving one agent too many tools with vague descriptions.
3. Letting the model control permissions or execution policy.
4. Treating longer context as a substitute for context design.
5. Shipping agent behavior without trace-level observability.
6. Measuring only final answers instead of internal behavior.
7. Adding more agents when the real problem is weak retrieval or weak tool design.

## What good looks like

A good agent system is:
- understandable,
- bounded,
- observable,
- testable,
- and easy to interrupt.

Operators can explain:
- what the agent was allowed to do,
- what it actually did,
- why it took that path,
- where approvals happened,
- and how the system behaved under failure.

## What to read next

- [[Technique - Context Budgeting]]
- [[Technique - Agent Approval Design]]
- [[Technique - Eval Operations]]
- [[Topic - OpenClaw and Self-Hosted Personal Agents]]
- [[Source - Agent Systems Design, Approvals, and Monitoring - Core Sources]]

[^anthropic-effective]: Anthropic, "Building effective agents." Published Dec. 19, 2024. Last verified 2026-03-12. https://www.anthropic.com/engineering/building-effective-agents
[^anthropic-context]: Anthropic, "Effective context engineering for AI agents." Published Sep. 29, 2025. Last verified 2026-03-12. https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
[^anthropic-multi-agent]: Anthropic, "How we built our multi-agent research system." Published Jun. 13, 2025. Last verified 2026-03-12. https://www.anthropic.com/engineering/multi-agent-research-system
[^anthropic-harness]: Anthropic, "Effective harnesses for long-running agents." Published Nov. 26, 2025. Last verified 2026-03-12. https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
[^openai-responses]: OpenAI API Reference, "Responses." Last verified 2026-03-12. https://developers.openai.com/api/reference/resources/responses/
[^openai-evals]: OpenAI API Reference, "List evals." Last verified 2026-03-12. https://developers.openai.com/api/reference/resources/evals/methods/list
