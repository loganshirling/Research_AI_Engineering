---
title: Technique - Eval Operations
type: technique
status: active
topic: Evaluation Operations
course: AI Engineering
tags: [evals, regression, operations, tracing, rollouts]
prerequisites:
  - "[[Topic - Model Selection, Benchmarking, and Tradeoff Triage]]"
related:
  - "[[Topic - AI Agents: Design, Boundaries, and Operations]]"
  - "[[Technique - Model Selection Scorecard]]"
  - "[[Source - Agent Systems Design, Approvals, and Monitoring - Core Sources]]"
  - "[[Source - Eval Design for Serving Changes - Core Sources]]"
sources:
  - "https://platform.openai.com/docs/api-reference/evals/list"
  - "https://platform.openai.com/docs/api-reference/graders/run"
  - "https://www.anthropic.com/research/building-effective-agents"
  - "https://sre.google/workbook/canarying-releases/"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Technique - Eval Operations

## Purpose

Turn evals from a one-time benchmark into an operating loop that protects system quality as prompts, models, tools, and policies change.

## When to use this

Use this technique when:
- changing models,
- changing prompts,
- adding tools,
- adding approvals,
- changing retrieval,
- or investigating incidents.

## The operating loop

1. Define the product behavior that matters.
2. Collect representative cases.
3. Score them with a mix of deterministic checks, rubric/model grading, and human review where needed.
4. Track regressions, not only averages.
5. Gate risky changes with pre-release checks and small rollouts.
6. Turn incidents and surprising traces into new eval cases.

## What to measure

Do not measure only "answer quality."

Measure:
- task completion,
- groundedness or citation behavior,
- tool selection quality,
- structured output validity,
- approval correctness,
- latency,
- cost,
- escalation rate,
- and failure recovery.

## Eval set design

A good eval set has:
- common cases,
- important edge cases,
- known failure modes,
- abuse or misuse cases where relevant,
- and a small set of "must not break" golden cases.

Keep cases labeled by:
- feature area,
- risk level,
- difficulty,
- and origin.

That lets you connect incidents back to the eval suite.

## Scoring strategy

Use a layered scoring stack:

### Deterministic checks
Use for:
- schema validity,
- exact policy rules,
- known required fields,
- approval presence,
- and routing or tool constraints.

### Model or rubric grading
Use for:
- helpfulness,
- faithfulness,
- plan quality,
- or comparative preference.

### Human review
Use for:
- ambiguous cases,
- safety-sensitive behavior,
- and changes that affect user trust.

## Release discipline

Before rollout:
- run the core regression suite,
- compare against the current baseline,
- inspect trace-level failures,
- and canary risky changes.

After rollout:
- watch live metrics,
- sample traces,
- and promote new failures into the suite.

## Common mistakes

1. Using only average scores.
2. Failing to preserve hard regression cases.
3. Measuring model output but not tool or approval behavior.
4. Changing prompts and models together without attribution.
5. Skipping canary rollout for user-facing changes.
6. Treating evals as offline paperwork instead of an operational control.

## What good looks like

A healthy eval operation can answer:
- what changed,
- what got better,
- what got worse,
- how severe the regression is,
- and whether rollout should continue.

## What to read next

- [[Technique - Model Selection Scorecard]]
- [[Topic - AI Agents: Design, Boundaries, and Operations]]
- [[Source - Eval Design for Serving Changes - Core Sources]]
