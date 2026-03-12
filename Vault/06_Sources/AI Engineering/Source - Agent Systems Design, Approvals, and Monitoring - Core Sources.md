---
title: Source - Agent Systems Design, Approvals, and Monitoring - Core Sources
type: source
status: active
topic: Agent Systems
course: AI Engineering
tags: [source, agents, approvals, observability, control]
related:
  - "[[Topic - AI Agents: Design, Boundaries, and Operations]]"
  - "[[Technique - Eval Operations]]"
  - "[[Technique - Agent Approval Design]]"
sources:
  - "https://www.anthropic.com/research/building-effective-agents"
  - "https://docs.anthropic.com/en/docs/build-with-claude/tool-use"
  - "https://docs.anthropic.com/en/docs/build-with-claude/computer-use"
  - "https://platform.openai.com/docs/api-reference/responses"
  - "https://platform.openai.com/docs/api-reference/evals/list"
  - "https://platform.openai.com/docs/api-reference/graders/run"
last_updated: 2026-03-12
review_status: researched
priority: high
---

# Source - Agent Systems Design, Approvals, and Monitoring - Core Sources

## Why this bundle matters

This bundle covers the practical operating surface for agent systems:
- architectural framing,
- tool and harness design,
- context and step control,
- and eval loops that can absorb real failures.

## Core source set

### 1. Anthropic - Building effective agents
This is one of the clearest primary references for the distinction between:
- workflows,
- agents,
- simple composable patterns,
- and the operational costs of over-complicated autonomy.

Use it for:
- agent architecture framing,
- deciding when workflows beat agents,
- and grounding approval or control design in simpler patterns.

### 2. Anthropic docs - Tool use
Useful for the mechanics of tool invocation:
- tool schemas,
- tool descriptions,
- tool result handling,
- and the idea that tool design strongly affects agent quality.

Use it for:
- tool ergonomics,
- schema clarity,
- and client-side responsibility.

### 3. Anthropic docs - Computer use
Valuable because it exposes how much harness design matters for long-running agents.
It is a good reminder that:
- tool access changes the risk model,
- long-running loops need stronger control,
- and end-to-end verification matters.

### 4. OpenAI Responses API reference
Useful as a current primary source for:
- model responses as a stateful API surface,
- built-in tools,
- prompt cache parameters,
- and request/trace identifiers.

Use it for:
- modern API operating assumptions,
- request metadata,
- and context-management hooks.

### 5. OpenAI evals and graders references
Useful for:
- formalizing evaluations,
- storing or running evals repeatedly,
- and treating grading as an explicit part of the system rather than ad hoc review.

Use it for:
- regression design,
- grader mental models,
- and operational eval workflows.

## Durable takeaways

- Simpler workflows outperform flashy autonomy surprisingly often.
- Tool schemas and descriptions are part of system design, not just API syntax.
- Long-running agents need harnesses, not just prompts.
- Eval operations should observe trace behavior and tool behavior, not only final text quality.
- Approval design belongs in the application control layer, not only in prompt instructions.

## Related notes

- [[Topic - AI Agents: Design, Boundaries, and Operations]]
- [[Technique - Eval Operations]]
- [[Technique - Agent Approval Design]]

## References

- Anthropic, "Building effective agents." Published Dec. 19, 2024. Last verified 2026-03-12. https://www.anthropic.com/research/building-effective-agents
- Anthropic Docs, "Tool use." Last verified 2026-03-12. https://docs.anthropic.com/en/docs/build-with-claude/tool-use
- Anthropic Docs, "Computer use." Last verified 2026-03-12. https://docs.anthropic.com/en/docs/build-with-claude/computer-use
- OpenAI API Reference, "Responses." Last verified 2026-03-12. https://platform.openai.com/docs/api-reference/responses
- OpenAI API Reference, "List evals." Last verified 2026-03-12. https://platform.openai.com/docs/api-reference/evals/list
- OpenAI API Reference, "Run graders." Last verified 2026-03-12. https://platform.openai.com/docs/api-reference/graders/run
