---
title: Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs
type: topic
status: active
topic: Retrieval and Vector Databases
course: AI Engineering
tags: [vector-databases, embeddings, rag, retrieval, ann, metadata]
prerequisites:
  - "[[Concept - Embeddings and Vector Similarity]]"
  - "[[Topic - Retrieval System Design for RAG]]"
related:
  - "[[Concept - Vector Databases, ANN Indexes, and Filtering]]"
  - "[[Topic - Hybrid Search and Reranking]]"
  - "[[Technique - Retrieval Triage Checklist]]"
  - "[[Source - Vector Databases and Retrieval Tradeoffs - Core Sources]]"
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
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs

## Purpose

Explain what vector databases are, when they matter, when they do not, and how they fit into real retrieval systems.

This note is here because many AI engineering discussions jump straight from “embeddings” to “RAG” and skip the storage, indexing, filtering, and operational tradeoffs in between.

## Why it matters

A vector database is not the same thing as a retrieval strategy.

It can help you store and search embeddings efficiently, but it does not by itself solve:
- chunking,
- metadata quality,
- exact-match retrieval,
- reranking,
- citation grounding,
- or context packing.

That is why teams often over-credit or over-blame the vector DB for problems that actually belong elsewhere.[^faiss][^pgvector][^weaviate-search][^pinecone-indexing]

## 80/20 summary

- Vector databases store embeddings and provide similarity search over them.[^faiss][^pgvector][^pinecone-indexing]
- They matter when you need semantic retrieval at non-trivial scale or with repeated query traffic.
- They matter less when your corpus is small, your queries are mostly exact-match, or the problem can be solved with normal database/search primitives.
- A good retrieval system usually combines vectors with metadata, filtering, and often lexical or hybrid search.
- Filtering, ranking, and ANN behavior are practical sources of tradeoff and failure; they are not side details.[^weaviate-filtering][^weaviate-search][^pinecone-filtering][^pinecone-hybrid]

## What a vector database is

A vector database is a system that stores vector embeddings and supports nearest-neighbor retrieval, often with metadata and filtering.

Depending on the system, it may provide:
- exact search,
- approximate nearest neighbor search,
- metadata filtering,
- hybrid lexical + vector search,
- and operational features such as scaling, namespaces, replication, and backups.

Important nuance:
- Faiss is fundamentally a library for efficient similarity search and clustering of dense vectors.[^faiss]
- pgvector extends Postgres so you can store vectors with ordinary relational data and query them in the same system.[^pgvector]
- systems like Weaviate and Pinecone expose fuller retrieval/database products with search, filtering, and operational features.[^weaviate-search][^pinecone-indexing]

## When vector databases matter

They usually matter when:
- you have enough documents that brute-force search becomes operationally awkward,
- semantic search genuinely helps query recall,
- query traffic is repeated enough that indexing pays off,
- metadata filtering matters,
- or you need a maintained retrieval layer rather than ad hoc scripts.

Typical examples:
- internal knowledge search,
- support documentation retrieval,
- long-lived RAG systems,
- recommendation and similarity systems,
- or multimodal retrieval.

## When they do not matter as much

They matter less when:
- the corpus is small,
- queries are dominated by exact names, IDs, codes, or numbers,
- a relational database plus text search is already enough,
- latency/cost of embedding/indexing is not justified,
- or retrieval is incidental instead of core.

Common overuse pattern:
teams adopt a vector DB before proving that semantic retrieval is actually the bottleneck or that retrieval quality is the main product problem.

## How they relate to embeddings and retrieval

Embeddings produce the vectors.

The vector database stores and searches them.

The retrieval system still needs:
- ingestion,
- chunking,
- metadata,
- query rewriting where useful,
- ranking or reranking,
- context selection,
- and evaluation.

That means:
- embeddings are not the database,
- the database is not the retrieval strategy,
- and retrieval strategy is not the same thing as the final generation step.

## ANN and exact search tradeoffs

Exact nearest-neighbor search gives the true nearest items but becomes expensive as scale grows.

Approximate nearest-neighbor (ANN) search trades some recall for speed and scale. This is one of the central reasons vector databases exist in practice.[^faiss][^pgvector]

The key practical point:
- ANN can be the right trade when scale and latency matter,
- but it introduces tuning and recall tradeoffs,
- so you should measure real retrieval quality, not just benchmark query speed.

## Filtering is a real design constraint

Structured filtering is not a minor add-on.

You often need to constrain retrieval by:
- document type,
- customer,
- date,
- product,
- access scope,
- language,
- or policy version.

Weaviate’s docs explicitly separate filtering from search and note that filters pass or block objects while search handles relevance ranking.[^weaviate-search][^weaviate-filtering]

Pinecone likewise treats metadata filtering as a first-class part of narrowing search results.[^pinecone-filtering]

This matters because:
- a retrieval system without good filtering often leaks irrelevant context,
- but filtering can also interact poorly with ANN behavior or performance,
- so filter design is part of retrieval engineering, not just data modeling.

## Hybrid and lexical search still matter

Pure vector search is rarely the universal best answer.

Lexical search often wins for:
- IDs,
- exact product names,
- error codes,
- version strings,
- and jargon-heavy domains.

That is why hybrid search exists. Weaviate and Pinecone both document hybrid retrieval as a way to combine semantic and lexical signals.[^weaviate-search][^pinecone-hybrid]

A durable default for production knowledge systems is:
- use hybrid or mixed retrieval when both conceptual matching and exact-match recall matter.

## Common mistakes

1. Treating the vector DB as the whole RAG architecture.
2. Using pure vector search for identifier-heavy workloads.
3. Ignoring metadata and access-scope filtering.
4. Benchmarking speed without checking recall and answer quality.
5. Blaming the database when chunking or reranking is the real failure.
6. Storing vectors in a separate system when co-locating with operational data would make the product simpler.
7. Choosing a product based only on popularity instead of workload shape.

## Practical product choices in plain language

## Use a library-first approach when:
- you want tight control,
- you are comfortable owning more retrieval plumbing,
- or you have special indexing requirements.

## Use pgvector when:
- Postgres is already central,
- transactional coupling with your app data matters,
- and “keep it in one system” is more valuable than a specialized vector stack.[^pgvector]

## Use a dedicated vector database when:
- retrieval is a major product surface,
- you need stronger managed search/database features,
- or your team benefits from a more opinionated vector-search platform.

There is no universal winner. The right choice depends on scale, workload, team skills, and operational preferences.

## What to read next

- [[Concept - Vector Databases, ANN Indexes, and Filtering]]
- [[Topic - Retrieval System Design for RAG]]
- [[Topic - Hybrid Search and Reranking]]
- [[Source - Vector Databases and Retrieval Tradeoffs - Core Sources]]

[^faiss]: Faiss Wiki, homepage. Last verified 2026-03-12. https://github.com/facebookresearch/faiss/wiki
[^pgvector]: pgvector GitHub repository. Last verified 2026-03-12. https://github.com/pgvector/pgvector
[^weaviate-search]: Weaviate Docs, "Search." Last verified 2026-03-12. https://docs.weaviate.io/weaviate/concepts/search
[^weaviate-filtering]: Weaviate Docs, "Filtering." Last verified 2026-03-12. https://docs.weaviate.io/weaviate/concepts/filtering
[^pinecone-indexing]: Pinecone Docs, "Indexing overview." Last verified 2026-03-12. https://docs.pinecone.io/guides/index-data/indexing-overview
[^pinecone-hybrid]: Pinecone Docs, "Hybrid search." Last verified 2026-03-12. https://docs.pinecone.io/guides/search/hybrid-search
[^pinecone-filtering]: Pinecone Docs, "Filter by metadata." Last verified 2026-03-12. https://docs.pinecone.io/guides/search/filter-by-metadata
