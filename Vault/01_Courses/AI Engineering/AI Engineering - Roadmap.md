---
title: AI Engineering - Roadmap
type: roadmap
status: active
topic: AI Engineering
course: AI Engineering
tags: [roadmap, ai-engineering]
prerequisites: []
related:
  - "[[AI Engineering - Course Home]]"
  - "[[AI Engineering - 80-20 Summary]]"
  - "[[Map - AI Engineering]]"
  - "[[AI Engineering Practice]]"
  - "[[Review - AI Engineering - Course Review Checklist]]"
sources: []
last_updated: 2026-03-12
review_status: researched
priority: high
---

# AI Engineering - Roadmap

## First-pass order

1. Foundations concepts
2. Retrieval systems
3. Evaluation, safety, and observability
4. Advanced systems and agent operations
5. Practice drills
6. Build track

## Phase 1 - Foundations
Read:
- [[Concept - Tokens and Tokenization]]
- [[Concept - Transformer Architecture and Attention]]
- [[Concept - Context Windows, Prefill, and Decode]]
- [[Concept - Embeddings and Vector Similarity]]

Checkpoint:
- [[Review - AI Engineering - Foundations Checkpoint]]

## Phase 2 - Retrieval systems
Read:
- [[Topic - Retrieval System Design for RAG]]
- [[Topic - Hybrid Search and Reranking]]
- [[Topic - Common RAG Failure Modes]]
- [[Technique - Retrieval Triage Checklist]]

Practice:
- [[Practice - RAG Diagnostics and Incident Reviews]]

Checkpoint:
- [[Review - AI Engineering - Retrieval and RAG Checkpoint]]

## Phase 3 - Evaluation, safety, and operations
Read:
- [[Topic - Eval Design for Serving Changes]]
- [[Topic - Eval Flywheels, Human Review, and Regression Gates]]
- [[Topic - Model Selection, Benchmarking, and Tradeoff Triage]]
- [[Topic - Guardrails, Prompt Injection, and Output Validation]]
- [[Topic - LLM Observability and Tracing]]

Practice:
- [[Practice - Benchmarking and Failure Analysis Drills]]

Checkpoint:
- [[Review - AI Engineering - Evaluation, Safety, and Operations Checkpoint]]

## Phase 4 - Advanced systems and agent operations
Read:
- [[Topic - Advanced Systems, Agents, Multimodal, and Voice]]
- [[Topic - AI Agents: Design, Boundaries, and Operations]]
- [[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]]
- [[Topic - MCP, Tool Design, and Secure Agent Execution]]
- [[Topic - Multimodal Document and Image Pipelines]]
- [[Topic - Realtime Voice Agents and Speech-to-Speech Systems]]
- [[Packet - AI Engineering - Section 02 - Advanced Systems and Agent Operations]]

Checkpoint:
- [[Review - AI Engineering - Advanced Systems and Agent Operations Checkpoint]]
- [[Episode - AI Engineering - Section 02 - Advanced Systems and Agent Operations]]

## Phase 5 - Practice ladder
Use this sequence:
1. [[Practice - Prompt, Retrieval, and Tuning Decision Drills]]
2. [[Practice - Benchmarking and Failure Analysis Drills]]
3. [[Practice - RAG Diagnostics and Incident Reviews]]
4. [[Practice - Advanced Systems Architecture Drills]]

## Phase 6 - Build track
Build in this order:
1. [[Practice - End-to-End Build - Internal Research Assistant]]
2. [[Practice - End-to-End Build - Multimodal Document Operations Copilot]]
3. [[Practice - End-to-End Build - Approval-Gated Operations Agent]]

Packet support:
- [[Packet - AI Engineering - Build Track 01 - Internal Research Assistant]]
- [[Packet - AI Engineering - Build Track 02 - Multimodal Document Operations Copilot]]
- [[Packet - AI Engineering - Build Track 03 - Approval-Gated Operations Agent]]

Episode tracking:
- [[Episode - AI Engineering - Build Track 01 - Internal Research Assistant]]
- [[Episode - AI Engineering - Build Track 02 - Multimodal Document Operations Copilot]]
- [[Episode - AI Engineering - Build Track 03 - Approval-Gated Operations Agent]]

## Finish line for a strong first pass

You should be able to:
- explain the foundations in plain language,
- debug a simple RAG system,
- compare models with an eval instead of vibes,
- justify deterministic boundaries in an agent workflow,
- and sketch one reviewable end-to-end build.
