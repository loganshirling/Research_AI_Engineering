---
title: AI Engineering - Course Home
type: course
status: active
topic: AI Engineering
course: AI Engineering
tags: [ai-engineering, llms, serving, evals, agents]
prerequisites: []
related:
  - "[[AI Engineering - Roadmap]]"
  - "[[AI Engineering - 80-20 Summary]]"
  - "[[Map - AI Engineering]]"
  - "[[AI Engineering Practice]]"
sources: []
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# AI Engineering - Course Home

## Goal

Become a practical AI engineer with enough understanding to:
- set up and run modern LLM systems,
- reason about inference, serving, retrieval, evaluation, safety, and observability,
- design controlled agent workflows,
- work with multimodal documents and voice interfaces,
- and build simple but credible end-to-end systems.

## Outcome target

By the end of this course, you should be able to:
- explain core LLM and inference concepts without hand-waving,
- choose between API use, open-weight local deployment, and self-hosted serving,
- reason about latency, throughput, batch size, KV cache, quantization, and memory limits,
- structure evals and diagnose failure modes,
- build a basic retrieval workflow and understand where it breaks,
- compare prompting, retrieval, tool use, and fine-tuning as different levers,
- and design safer, more observable agentic systems.

## How to use this course

Read in this order:

1. [[AI Engineering - 80-20 Summary]]
2. [[AI Engineering - Roadmap]]
3. [[Map - AI Engineering]]
4. [[AI Engineering Practice]]

Then expand through concepts, topics, techniques, sources, practice drills, packets, and episode notes.

A good rhythm is:
- read one concept cluster,
- read one related topic note,
- do one practice note,
- write a short decision memo from memory,
- then return to the map.

## Main sections

1. Foundations and vocabulary
2. Inference and serving
3. Prompting, tool use, and structured outputs
4. Retrieval systems and RAG
5. Evaluation and model selection
6. Customization and fine-tuning decisions
7. Production operations: latency, observability, and safety
8. Advanced systems: context engineering, orchestration, MCP, multimodal, and voice
9. Practice, packets, and end-to-end build notes

## Core concept notes

- [[Concept - Tokens and Tokenization]]
- [[Concept - Embeddings and Vector Similarity]]
- [[Concept - Transformer Architecture and Attention]]
- [[Concept - Context Windows, Prefill, and Decode]]

## Core practitioner topic notes

### Foundations already in place
- [[Topic - Retrieval System Design for RAG]]
- [[Topic - Hybrid Search and Reranking]]
- [[Topic - Model Selection, Benchmarking, and Tradeoff Triage]]
- [[Topic - Tool Calling, Structured Outputs, and Agent Loops]]
- [[Topic - Latency Engineering for LLM Systems]]
- [[Topic - Guardrails, Prompt Injection, and Output Validation]]
- [[Topic - LLM Observability and Tracing]]

### Advanced systems expansion
- [[Topic - Advanced Systems, Agents, Multimodal, and Voice]]

## Core techniques

- [[Technique - Model Selection Scorecard]]
- [[Technique - Latency Budget Worksheet]]
- [[Technique - Retrieval Triage Checklist]]

## Practice and teaching compression

- [[AI Engineering Practice]]
- [[Practice - Prompt, Retrieval, and Tuning Decision Drills]]
- [[Practice - Benchmarking and Failure Analysis Drills]]
- [[Practice - RAG Diagnostics and Incident Reviews]]
- [[Practice - Advanced Systems Architecture Drills]]
- [[Packet - AI Engineering - Practitioner Systems Foundations]]
- [[Packet - AI Engineering - Section 02 - Advanced Systems and Agent Operations]]

## Supporting folders

- `Vault/03_Concepts/AI Engineering/` for reusable foundations
- `Vault/04_Techniques/AI Engineering/` for repeatable workflows
- `Vault/06_Sources/AI Engineering/` for source summaries and core references
- `Vault/08_Practice/AI Engineering/` for drills and decision reps
- `Vault/01_Courses/AI Engineering/Packets/` for curated NotebookLM packet files
- `Vault/01_Courses/AI Engineering/Episodes/` for episode tracking notes

## Definition of success

You are not aiming to become a frontier researcher first.

You are aiming to become dangerous in a good way: able to build, debug, compare approaches, explain tradeoffs, and ask the right next questions.
