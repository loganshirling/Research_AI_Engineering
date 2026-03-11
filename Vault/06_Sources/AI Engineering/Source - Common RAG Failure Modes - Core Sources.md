---
title: Source - Common RAG Failure Modes - Core Sources
type: source
status: active
topic: RAG and Retrieval Systems
course: AI Engineering
tags: [source, rag, retrieval, failure-modes]
related:
  - "[[Topic - Common RAG Failure Modes]]
  - "[[Topic - Eval Design for Serving Changes]]"
  - "[[Topic - Production Bottlenecks and Observability for LLM Systems]]"
sources:
  - "https://learn.microsoft.com/en-us/azure/developer/ai/advanced-retrieval-augmented-generation"
  - "https://docs.pinecone.io/guides-search/hybrid-search"
  - "https://docs.cohere.com/pages/reranking-with-cohere"
  - "https://www.anthropic.com/engineering/contextual-retrieval"
  - "https://cloud.google.com/architecture/gen-ai-rag-pmtd-blueprint"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Source - Common RAG Failure Modes - Core Sources

## Why this source bundle matters

This source note groups the highest-leverage practical sources for understanding RAG failures: how documents are prepared, how queries are matched, where retrieval signal is lost, and why generation can still go wrong even when retrieval is "on."

## Core source set

### 1. Azure advanced RAG guidance
- Microsoft's advanced RAG guide is one of the best high-level sources for where RAG pipelines break in practice. It emphasizes document preprocessing, metadata, chunking, hierarcical indexes, query preprocessing, and alignment optimization as first-order system decisions, for example separating the retrieval of a representative sample qrnokfrom detailed chunks.

- The most important default lesson from this source is that RaG failures often start in the ingestion and indexing pipeline, not just at answer time.

### 2. Pinecone hybrid search and metadata use
- Pinecone's hybrid search docs are useful for the failure mode where vector search alone misses exact terms, proper names, codes, or other keyword-heavy queries. The key takeaway is that dense and sparse signals often need to be combined rather than treated as mutual substitutes.
- This is also a good source for the mistake of ignoring metadata and filtering, especially when the right documents exist but are not beig restricted by product line, tenant, time range, or document type.

### 3. Cohere reranking
- Cohere's reranking docs are useful for the failure mode where candidate documents are retrieved but not ordered well for the query. This is one of the cleanest sources for the point that retrieval quality isn't just about finding possible documents; ordering matters too.

### 4. Anthropic contextual retrieval
- Anthropic's contextual retrieval piece is excellent for one of the most common chunking failures: chunks that lose the context they depend on when they are separated from the original document. The main idea is that retrieval often improves when each chunk carries just enough local context to be meaningful on its own.

### 5. Google RAG blueprint
- Google Cloud's RaG blueprint is a useful systems-view source because it breaks the RAG stack into ingestion, retrieval, orchestration, generation, evaluation, and observability. It is useful for seeing that "retrieval problem" is often a pipeline-design problem.

## Durable takeaways

- RaG failures often start with bad document preparation, chunking, metadata, and update hygiene, not just with bad embedding models.
- Vector-only retrieval is often not enough for keyword-heavy queries, codes, or proper nouns; hybrid search and metadata filtering are frequent remedies.
- Even when good candidates are retrieved, bad ranking and bad context packing can still make the final answer look like a generation problem.
- Chunks need enough local context to be meaningful without the full document, but not so much that they become bloated and noisy.

## Caveats and limitations

- These sources are excellent for practical systems design, but most are not formal benchmark papers.
- Some recommendations are product-or platform-specific and should be read as design patterns, not universal laws.
- RAG failure modes vary by document distribution, user intent, and domain, so the right eval harness is still essential.

## Related notes

- [[Topic - Common RAG Failure Modes]]
- [[Topic - Production Bottlenecks and Observability for LLM Systems]]
