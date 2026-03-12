---
title: Technique - Context Budgeting
type: technique
status: active
topic: Context Engineering
course: AI Engineering
tags: [context, tokens, caching, memory, latency, cost]
prerequisites:
  - "[[Concept - Tokens and Tokenization]]"
  - "[[Concept - Context Windows, Prefill, and Decode]]"
related:
  - "[[Topic - AI Agents: Design, Boundaries, and Operations]]"
  - "[[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]]"
  - "[[Source - Agent Systems Design, Approvals, and Monitoring - Core Sources]]"
sources:
  - "https://platform.openai.com/docs/api-reference/responses"
  - "https://platform.openai.com/docs/api-reference/chat/completions"
  - "https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking"
  - "https://docs.anthropic.com/en/docs/build-with-claude/computer-use"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Technique - Context Budgeting

## Purpose

Decide deliberately what goes into model context, what stays out, and how to keep token cost, latency, and quality under control.

## When to use this

Use this whenever:
- prompts are getting long,
- agent loops are replaying too much state,
- tool outputs are large,
- retrieval returns too many chunks,
- or costs are climbing without clear quality gains.

## Core rule

A bigger context window does not remove the need for curation.

Context is a budget across:
- stable instructions,
- user input,
- retrieved evidence,
- tool results,
- memory,
- intermediate reasoning scaffolding,
- and expected output length.

## Practical budgeting steps

1. Reserve space for the response.
2. Reserve space for required system or policy instructions.
3. Cap conversation history.
4. Cap retrieved evidence by usefulness, not by "whatever top-k returned."
5. Summarize or compress stale state.
6. Keep durable state outside the prompt when possible.
7. Separate reusable prefix material from volatile per-request material.

## Useful mental buckets

### Fixed prefix
Stable instructions, schemas, policies, and reusable context.

### Volatile task context
The current request, fresh evidence, recent tool results.

### Durable external state
State that should live in storage, not in free-form prompt text.

### Output reserve
Space for the model to finish well.

## Practical heuristics

- Put the highest-value evidence closest to the decision.
- Drop low-signal history aggressively.
- Return tool outputs in compact structured form when possible.
- Do not pass entire documents when spans or summaries will do.
- Use retrieval quality and reranking before increasing context size.
- Watch for "context tax" where costs and latency rise but task quality does not.

## Failure patterns

1. Full conversation replay when only the last few turns matter.
2. Dumping raw tool outputs back into the model.
3. Large retrieval top-k with weak reranking.
4. Using prompt text as long-term memory.
5. Forgetting to reserve space for output or follow-up tool use.
6. Hiding policy-critical instructions in a sea of low-value context.

## What good looks like

A good context budget is:
- explicit,
- testable,
- cheap enough for the workload,
- and easy to inspect when quality changes.

## What to read next

- [[Concept - Context Windows, Prefill, and Decode]]
- [[Topic - AI Agents: Design, Boundaries, and Operations]]
- [[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]]
