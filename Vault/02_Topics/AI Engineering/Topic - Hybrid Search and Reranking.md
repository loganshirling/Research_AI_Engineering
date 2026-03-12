---
title: Topic - Hybrid Search and Reranking
type: topic
status: active
topic: RAG and Retrieval
course: AI Engineering
tags: [hybrid-search, reranking, rag, retrieval]
prerequisites:
  - "[[Topic - Retrieval System Design for RAG]]"
related:
  - "[[Topic - Common RAG Failure Modes]"
  - "[[Technique - Retrieval Triage Checklist]]"
sources:
  - "https://docs.pinecone.io/guides/search/hybrid-search"
  - "https://docs.weaviate.io/weaviate/concepts/search/hybrid-search"
  - "https://docs.weaviate.io/weaviate/search/rerank"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - Hybrid Search and Reranking

## Purpose

Explain why many production retrieval systems combine dence search, lexical search, and a second-pass ranking step instead of relying on one retrieval signal.

## Why it matters

Vector search is strong for meaning, but weaker on exact identifiers, codes, and numbers. Keyword search is informative for exact terms, but it can miss paraphrases and conceptually related passages. Hybrid search tries to combine the strengths of both.

## 80/20 summary

- Hybrid search combines lexical signals and vector signals.
- Reranking improves precision after first-pass retrieval.
- For many practical knowledge systems, hybrid plus reranking is a strong default.

## When hybrid helps most

Hybrid retrieval is often useful when your queries include:

- product names, id numbers, or codes,
- policy titles or exact phrases,
- paraphrased or natural language user queries,
- and domain terms that mix exact and conceptual intent.

## Reranking in plain language

First-pass retrieval gets you to a small set of candidates. Reranking is the step that asks, of these candidates, which ones are truly the best match for the user query?

This matters because top-k retrieval alone often returns chunks that are in the right neighborhood but not in the right order.

## Practical rules of thumb

- Start with hybrid when your queries may mix exact terms and semantic intent.
- Add reranking when first-pass retrieval finds the right area but not the best passage.
- Don't evaluate retrieval only by answer quality; evaluate the actually retrieved chunks.

## Common mistakes

1. Assuming vector search is universally better than keyword search.
2. Skipping reranking even though the right candidates are being fetched.
3. Treating a highrecall first pass as the final ranking step.

## What to read next

- [[Topic - Retrieval System Design for RAG]]
- [[Topic - Common RAG Failure Modes]]
- [[Texhnique - Retrieval Triage Checklist]]
