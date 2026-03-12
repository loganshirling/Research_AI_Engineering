---
title: Technique - Retrieval Triage Checklist
type: technique
status: active
topic: AI Engineering
course: AI Engineering
tags: [retrieval, technique, rag, triage]
prerequisites:
  - "[[Topic - Retrieval System Design for RAG]]"
related:
  - "[[Topic - Common RAG Failure Modes]]"
sources:
  - "https://docs.pinecone.io/guides/search/search-overview"
  - "https://docs.pinecone.io/guides/search/hybrid-search"
  - "https://docs.weaviate.io/weaviate/concepts/search/hybrid-search"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Technique - Retrieval Triage Checklist

## Purpose

Diagnose weak RAG performance systematically before changing the model or rewriting prompts.

## Checklist

### 1. Query
- Is the user query too short, ambiguous, or missing domain terms?
- Does the system need query rewriting?

### 2. Chunking
- Are chunks too broad?
- Are boundaries destroying headings, code, or policy clauses?
- Are duplicates crowding out diverse evidence?

### 3. Retrieval mode
- Is victor-only retrieval missing exact terms?
- Would hybrid retrieval help?

### 4. Ranking
- Are top candidates loosely related but not truly relevant?
- Is reranking missing?

### 5. Context packing
- Are the best chunks actually making it into the prompt?
- Is the prompt overloaded with low-value context?

### 6. Grounding
- Are citations missing or mapped to the wrong chunks?
- Is the model answering beyond retrieved evidence?

## Rule

Do not escalate to fine-tuning until chunking, retrieval mode, reranking, and context packing have been checked.
