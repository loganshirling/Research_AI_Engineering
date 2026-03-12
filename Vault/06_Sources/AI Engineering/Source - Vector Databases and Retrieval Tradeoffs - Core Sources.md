---
title: Source - Vector Databases and Retrieval Tradeoffs - Core Sources
type: source
status: active
topic: Retrieval and Vector Databases
course: AI Engineering
tags: [source, vector-databases, ann, retrieval, filtering, hybrid-search]
related:
  - "[[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]]"
  - "[[Concept - Vector Databases, ANN Indexes, and Filtering]]"
sources:
  - "https://github.com/facebookresearch/faiss/wiki"
  - "https://github.com/pgvector/pgvector"
  - "https://docs.weaviate.io/weaviate/concepts/search"
  - "https://docs.weaviate.io/weaviate/concepts/filtering"
  - "https://docs.pinecone.io/guides/index-data/indexing-overview"
  - "https://docs.pinecone.io/guides/search/hybrid-search"
  - "https://docs.pinecone.io/guides/search/filter-by-metadata"
last_updated: 2026-03-12
review_status: researched
priority: high
---

# Source - Vector Databases and Retrieval Tradeoffs - Core Sources

## Why this bundle matters

This bundle is for the layer between embeddings and full RAG systems:
- how vectors are stored,
- how nearest-neighbor search is accelerated,
- how filtering changes retrieval behavior,
- and why vector databases do not eliminate the need for hybrid retrieval design.

## Core source set

### 1. Faiss
Faiss is a strong primary source for the basic idea of efficient similarity search and ANN indexing. It is especially useful when you want the “index and search” layer without product-marketing framing.[^faiss]

### 2. pgvector
pgvector is useful because it shows the opposite tradeoff from a dedicated vector database: keep vectors inside Postgres and combine them with ordinary relational behavior, joins, and recovery properties.[^pgvector]

### 3. Weaviate search and filtering docs
These are especially useful because they clearly distinguish:
- search from filtering,
- vector search from keyword search,
- and hybrid retrieval from pure vector retrieval.[^weaviate-search][^weaviate-filtering]

### 4. Pinecone indexing, hybrid, and metadata filtering docs
These are useful for production-oriented framing:
- dense indexes for semantic search,
- hybrid search for mixed semantic and lexical workloads,
- and metadata filtering as part of query shaping.[^pinecone-indexing][^pinecone-hybrid][^pinecone-filtering]

## Durable takeaways

- Vector databases are about retrieval mechanics and operations, not magic relevance.
- ANN is a speed/scale tradeoff, not a free improvement.
- Filtering is a product requirement as much as a search feature.
- Hybrid retrieval remains important because exact-match needs do not disappear when embeddings are added.
- The right product choice depends heavily on workload shape and operational context.

## Related notes

- [[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]]
- [[Concept - Vector Databases, ANN Indexes, and Filtering]]
- [[Topic - Retrieval System Design for RAG]]

[^faiss]: Faiss Wiki, homepage. Last verified 2026-03-12. https://github.com/facebookresearch/faiss/wiki
[^pgvector]: pgvector GitHub repository. Last verified 2026-03-12. https://github.com/pgvector/pgvector
[^weaviate-search]: Weaviate Docs, "Search." Last verified 2026-03-12. https://docs.weaviate.io/weaviate/concepts/search
[^weaviate-filtering]: Weaviate Docs, "Filtering." Last verified 2026-03-12. https://docs.weaviate.io/weaviate/concepts/filtering
[^pinecone-indexing]: Pinecone Docs, "Indexing overview." Last verified 2026-03-12. https://docs.pinecone.io/guides/index-data/indexing-overview
[^pinecone-hybrid]: Pinecone Docs, "Hybrid search." Last verified 2026-03-12. https://docs.pinecone.io/guides/search/hybrid-search
[^pinecone-filtering]: Pinecone Docs, "Filter by metadata." Last verified 2026-03-12. https://docs.pinecone.io/guides/search/filter-by-metadata
