---
title: Review - AI Engineering - Foundations Checkpoint
type: review
status: active
topic: AI Engineering
course: AI Engineering
tags: [review, ai-engineering]
prerequisites:
  - "[[AI Engineering - Course Home]]"
  - "[[Map - AI Engineering]]"
related:
  - "[[AI Engineering Practice]]"
  - "[[Review - AI Engineering - Course Review Checklist]]"
sources: []
last_updated: 2026-03-12
review_status: researched
priority: high
---

# Review - AI Engineering - Foundations Checkpoint

## Navigation

- Course hub: [[AI Engineering - Course Home]]
- Map: [[Map - AI Engineering]]
- Concepts index: `Vault/03_Concepts/AI Engineering/README.md`

## Scope

Use this review after the foundations section of the course.

Core notes:
- [[Concept - Tokens and Tokenization]]
- [[Concept - Transformer Architecture and Attention]]
- [[Concept - Context Windows, Prefill, and Decode]]
- [[Concept - Embeddings and Vector Similarity]]

## What you should be able to explain

- what a token is and why tokenization changes cost and behavior
- why attention and transformer layers matter for practical AI systems
- the difference between prefill and decode
- why long prompts affect latency and memory
- why embeddings are useful without being magic representations

## Recall prompts

1. Explain the path from prompt text to generated tokens in plain language.
2. Why can two prompts with similar character length have different token cost?
3. What practical problems show up when context windows grow?
4. Why is prefill a different bottleneck from decode?
5. What do embeddings preserve well, and what do they preserve poorly?

## Common confusion checks

- confusing tokens with words
- treating long context as free memory
- talking about embeddings as if they directly "understand" truth
- talking about transformers only at research-paper level instead of system level

## Move on when

You can explain the request path, token cost, embeddings, and context tradeoffs without hiding behind jargon.

## Next

- [[Review - AI Engineering - Retrieval and RAG Checkpoint]]
- [[Topic - LLM Inference Path, KV Cache, and Batching]]
