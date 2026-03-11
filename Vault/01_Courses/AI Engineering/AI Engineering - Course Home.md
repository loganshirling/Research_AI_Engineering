---
title: AI Engineering - Course Home
type: course
status: active
topic: AI Engineering
course: AI Engineering
tags: [ai-engineering, llms, serving, evals]
prerequisites: []
related: [[AI Engineering - Roadmap]], [[AI Engineering - 80-20 Summary]], [[Map - AI Engineering]]
sources: []
last_updated: 2026-03-11
review_status: seed
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---
# AI Engineering - Course Home

## Goal

Become pa3sable AI engineer with enough practical understanding to:
- set up and run modern LLM systems
- understand the main engineering tradeoffs behind inference, serving, and evaluation
- speak clearly about core terms, bottlenecks, and failure modes
- build simple but credible systems using APIs or self-hosted models
- keep learning from a strong practical foundation

## Outcome target

By the end of this course, you should be able to:
- explain core LLM and inference concepts without hand-waving
- choose between API use, open-weight local deployment, and self-hosted serving
- understand what vLLM is and why it matters
- reason about latency, throughput, batch size, KV cache, quantization, and memory limits
- structure simple  evals and diagnose common model failures
- build a basic RAG workflow and understand where it breaks
- understand the role of fine-tuning, adapters, and post-training choices
- operate with sensible instincts about cost, observability, and safety

## How to use this course

Read in this order:
1. [[AI Engineering - 80-20 Summary]]
2. [AI Engineering - Roadmap]]
3. [[Map - AI Engineering]]

Then expand into topic, concept, technique, source, practice, and review notes as the course grows.

## Main sections
1. Foundations and vocabulary
2. Inference and serving
3. vLLM and performance optimization
4. Prompting and structured outputs
5. Evaluation and failure analysis
6. RAG and retrieval systems
7. Fine-tuning and adapters
8. Production operations: cost, latency, reliability, safety

## Supporting folders
- `Packets/` for NotebookLM packet source files
- `Episodes/` for episode tracking notes

## Definition of success

You are not aiming to become a frontier researcher first.
You are aiming to become dangerous in a good way: able to build, debug, compare approaches, and ask the right next questions.

## Open questions
- Which deployment path should be emphasized first: API-first, local open-weight, or cloud GPU?
- Which frameworks should become the default practical stack in this vault?
