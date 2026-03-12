---
title: Technique - Model Selection Scorecard
type: technique
status: active
topic: AI Engineering
course: AI Engineering
tags: [model-selection, technique, evals, scorecard]
prerequisites:
  - "[[Topic - Model Selection, Benchmarking, and Tradeoff Triage]]"
related:
  - "[[Topic - Eval Design for Serving Changes]]"
sources:
  - "https://platform.openai.com/docs/guides/evals"
  - "https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/increase-consistency"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Technique - Model Selection Scorecard

## Purpose

Use one repeatable worksheet to compare candidate models without relying on vague impressions.

## When to use

Use this when choosing a new default model, validating a migration, or narrowing several candidates before deeper testing.

## Scorecard dimensions

Evaluate each candidate on a 1 to 5 scale:

1. task quality
2. structured output reliability
3. tool-use reliability
4. latency fit
5. cost fit
6. safety/refusal fit
7. operational simplicity
8. stability on regression cases

## Suggested process

1. Define the task and failure tolerance.
2. Build a small eval set with representative and adversarial examples.
3. Run every candidate against the same eval set.
4. Record both scores and failure notes.
5. Reject models that fail hard constraints before averaging softer criteria.

## Hard constraints to define first

- max acceptable latency
- max unit cost
- minimum schema correctness
- minimum pass rate on critical cases
- required tool behavior
- required safety posture

## Common mistake

Do not average everything into one number too early. A model that fails a hard requirement should usually be removed even if its average is strong.
