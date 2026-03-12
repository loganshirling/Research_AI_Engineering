---
title: AI Engineering - Course Home
type: course
status: active
topic: AI Engineering
course: AI Engineering
tags: [ai-engineering, llms, serving, evals]
prerequisites: []
related: [[AI Engineering - Roadmap]], [[AI Engineering - 80-20 Summary]], [[Map - AI Engineering]], [[AI Engineering Practice]]
sources: []
last_updated: 2026-03-11
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

- set up and run modern LLM systems
- understand the main engineering tradeoffs behind inference, serving, retrieval, evaluation, safety, and observability
- speak clearly about core terms, bottlenecks, and failure modes
- build simple but credible systems using APIs or self-hosted models
- keep learning from a strong practitioner foundation

## Outcome target

By the end of this course, you should be able to:

- explain core LLM and inference concepts without hand-waving
- choose between API use, open-weight local deployment, and self-hosted serving
- reason about latency, throughput, batch size, KV cache, quantization, and memory limits
- structure simple evals and diagnose common model failures
- build a basic RAG workflow and understand where it breaks
- compare prompting, retrieval, tool use, and fine-tuning as different levers
- operate with sensible instincts about cost, observability, and safety

## How to use this course

Read in this order:

1. [[AI Engineering - 80-20 Summary]]
2. [[AI Engineering - Roadmap]]
3. [[Map - AI Engineering]]
4. [[AI Engineering Practice]]

Then expand through concepts, topics, techniques, source notes, practice drills, packets, and episode tracking notes.

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
4. Evaluation and failure analysis
5. Retrieval systems and RAG
6. Model selection and customization
7. Production operations: latency, observability, and safety
8. Practice: decision drills, diagnostics, and incident review
9. Packet and episode workflows for teaching compression

## Core concept notes

- [[Concept - Tokens and Tokenization]]
- [[Concept - Embeddings and Vector Similarity]]
- [[Concept - Transformer Architecture and Attention]]
- [[Concept - Context Windows, Prefill, and Decode]]

## Core practitioner topic notes

- [[Topic - Retrieval System Design for RAG]]
- [[Topic - Hybrid Search and Reranking]]
- [[Topic - Model Selection, Benchmarking, and Tradeoff Triage]]
- [[Topic - Tool Calling, Structured Outputs, and Agent Loops]]
- [[Topic - Latency Engineering for LLM Systems]]
- [[Topic - Guardrails, Prompt Injection, and Output Validation]]
- [[Topic - LLM Observability and Tracing]]

## Core techniques

- [[Technique - Model Selection Scorecard]]
- [[Technique - Latency Budget Worksheet]]
- [[Technique - Retrieval Triage Checklist]]

## Supporting folders

- `Vault/03_Concepts/AI Engineering/` for reusable foundations
- `Vault/04_Techniques/AI Engineering/` for repeatable engineering workflows
- `Vault/08_Practice/AI Engineering/` for drills and incident-style reps
- `Packets/` for NotebookLM packet source files
- `Episodes/` for episode tracking notes

## Definition of success

You are not aiming to become a frontier researcher first.

You are aiming to become dangerous in a good way: able to build, debug, compare approaches, and ask the right next questions.
