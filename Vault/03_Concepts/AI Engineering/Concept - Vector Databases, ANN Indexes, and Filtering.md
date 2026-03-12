---
title: Concept - Vector Databases, ANN Indexes, and Filtering
type: concept
status: active
topic: AI Engineering
course: AI Engineering
tags: [vector-databases, ann, indexing, filtering, retrieval]
prerequisites:
  - "[[Concept - Embeddings and Vector Similarity]]"
related:
  - "[[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]]"
  - "[[Topic - Retrieval System Design for RAG]]"
sources:
  - "https://github.com/facebookresearch/faiss/wiki"
  - "https://github.com/pgvector/pgvector"
  - "https://docs.weaviate.io/weaviate/concepts/search"
  - "https://docs.weaviate.io/weaviate/concepts/filtering"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: high
explanatory_power: high
difficulty: medium
must_know: true
---

# Concept - Vector Databases, ANN Indexes, and Filtering

## Purpose

Separate three ideas that are often blurred together:
- embeddings,
- ANN indexes,
- and filtered retrieval.

## 80/20 summary

- Embeddings turn content into vectors.
- ANN indexes make nearest-neighbor retrieval faster by accepting approximation tradeoffs.
- Filtering constrains which records are eligible before or during retrieval.
- A vector database is the product layer that may combine these ideas with storage and operational features.

## Embeddings are representations

Embeddings are numeric representations of text, images, or other content.

They answer:
> “How is this item represented in vector space?”

They do **not** answer:
> “How is this efficiently searched at scale?”
or
> “How is access scope enforced?”

## ANN indexes are search structures

Approximate nearest-neighbor indexes are retrieval structures that trade exactness for speed and scale.

They answer:
> “How do we find likely nearest vectors quickly enough to be useful in production?”

This is why ANN exists:
- exact search can become too slow or too expensive as the corpus grows,
- while ANN can usually retrieve “good enough” neighbors much faster.[^faiss][^pgvector]

## Filtering is structured eligibility control

Filtering is not ranking.

Filtering answers:
> “Which items are even allowed into the candidate set?”

Examples:
- only this customer’s documents,
- only policies after a certain date,
- only documents in English,
- only records from one product line.

Weaviate’s docs make this distinction explicit: filtering passes or blocks records, while search ranks them by relevance.[^weaviate-search][^weaviate-filtering]

## Why this distinction matters

If you confuse these layers, you debug the wrong thing.

Examples:
- weak recall may be an ANN/tuning problem,
- irrelevant results may be a filter or metadata problem,
- poor final ranking may be a reranking problem,
- and bad answers may be a context-packing or generation problem.

## Common mistakes

1. Calling every vector store an ANN system without asking whether it supports exact search too.
2. Treating filtering as if it were ranking.
3. Treating ANN benchmark speed as proof of better product retrieval.
4. Ignoring metadata design while tuning vector indexes.
5. Forgetting that access scope is often enforced through filtering and application logic together.

## What to read next

- [[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]]
- [[Topic - Retrieval System Design for RAG]]

[^faiss]: Faiss Wiki, homepage. Last verified 2026-03-12. https://github.com/facebookresearch/faiss/wiki
[^pgvector]: pgvector GitHub repository. Last verified 2026-03-12. https://github.com/pgvector/pgvector
[^weaviate-search]: Weaviate Docs, "Search." Last verified 2026-03-12. https://docs.weaviate.io/weaviate/concepts/search
[^weaviate-filtering]: Weaviate Docs, "Filtering." Last verified 2026-03-12. https://docs.weaviate.io/weaviate/concepts/filtering
