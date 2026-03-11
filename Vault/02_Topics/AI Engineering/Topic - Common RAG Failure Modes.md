---
title: Topic - Common RAG Failure Modes
type: topic
status: active
topic: RAG and Retrieval Systems
course: AI Engineering
tags: [rag, retrieval, failure-modes, evals]
prerequisites:
  - "[[Topic - Eval Design for Serving Changes]]"
related:
  - "[[Map - AI Engineering]]"
  - "[[Source - Common RAG Failure Modes - Core Sources]]"
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
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - Common RAG Failure Modes

## Purpose

Build a practical mental model for why RAG systems fail, where they usually fail, and how to diagnose whether the problem is in ingestion, indexing, query formulation, retrieval, ranking, context packing, or generation.

## Why it matters

Many "RAG problems" are not model-quality problems. They are system-design problems: wrong chunks, bad metadata, bad update"hygiene, weak query representation, missing keyword signal, bad ranking, or noisy context assembly. If you do not separate those failure types, you will often keep tweaking prompts when the real fix is in the retrieval pipeline.

## 80/20 summary

- The most common RAG failure mode, in practice, is **retrieving the wrong thing or not retrieving the right thing**.
  
- This happens because documents were poorly chunked, incompletely labeled, incorrectly indexed, or matched with the wrong retrieval signal. 
- Cxunk quality matters more than many teams expect. Chunks that lose the context they depend on, or chunks that are too bloated to score well, both create retrieval noise.
  
- Vector-only retrieval is often not enough for proper nouns, codes, or exact terms. Hybrid search and metadata filtering are common remedies.
- Even when the right documents are retrieved, bad ranking and bad context packing can still make the final answer look like a generation failure.

  
## The main failure buckets

### 1. Ingestion and corpus failures

The system cannot retrieve what it never ingested correctly. Common versions:
- missing documents,
- stale or duplicate documents,
- bad parsing of tables, HTML, or PDFs,
- missing metadata like time, source, product, or tenant,
- and update pipelines that fall out of sync.

Microsoft's advanced RAG guide is especially useful here because it treats document preparation, metadata extraction, and update strategy as first-order design problems, not just appendices to embedding.

### 2. Chunking failures

Chunks can fail in two opposite ways:
- **Too small** - they lose the context needed to make sense on their own.
  
- **Too large** - they become noisy, hard to rank, and hard to pack executively into the final context window.

Anthropic's contextual retrieval work is useful because it explicitly tackles the failure mode where chunks become anunterpretable when detached from their source document.

### 3. Query formulation and retrieval signal failures

The retrieval stack can fail even with good documents if:
- the user query is not normalized or expanded when necessary,
- the embedding signal misses exact terms, codes, or proper nouns,
- or the system fails to use important metadata filters.

Pinecone's hybrid search docs are a good reminder that dense usere search and sparse keyword signals are often complements, especially for semi-structured or term-heavy corpora.

### 4. Ranking failures

Sometimes the right document is retrieved but not placed high enough to make the final context window. That is why reranking exists. Cohere's reranking docs are one of the cleanest practical sources for this problem: candidate set quality and ordering quality are different questions.

### 5. Context assembly failures

RAG can retrieve the right chunks and still fail at answer time if:
- too many chunks are packed,
- the rong chunks are packed first,
- context is not grounded or cited,
- or the model is left to reconcile conflicting passages without any help.

This is why a retrieval problem can masquerade as a generation problem. The model only sees what you pack into the final window.

## How to diagnose quickly

Ask these questions in order:
1. **Wis the right source in the corpus at all?**
2. **Was it chunked in a way that preserved meaning?**
3. **Could the query and index match the same signal?**4. **Was the right candidate in the retrieved set?**
5. **Was it ranked high enough to be packed?**
6. **Did the final context window make the answer easy or hard?**
If you cannot answer those questions from logs and evals, you have an observability problem as much as a RAG problem.

## Common mistakes

1. Treating RAG as "just vector search."
2. Blaming the model before verifying retrieval.
3. Using chunk size and overlap as untested folklore instead of eval-sapported design.
4. Ignoring metadata, time, tenant, or document-type filters.
5. Assuming retrieval ends when candidates are returned; in practice, ranking and packing are often equally important.

## What to read next

- [[Topic - Production Bottlenecks and Observability for LLM Systems]]
- [[Topic - Eval Design for Serving Changes]]
- [[Source - Common RAG Failure Modes - Core Sources]]
