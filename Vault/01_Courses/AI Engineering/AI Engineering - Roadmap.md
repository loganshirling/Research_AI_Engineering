---
title: AI Engineering - Roadmap
type: roadmap
status: active
topic: AI Engineering
course: AI Engineering
tags: [roadmap, ai-engineering]
prerequisites: []
related: [[AI Engineering - Course Home]], [[AI Engineering - 80-20 Summary]], [[Map - AI Engineering]], [[AI Engineering Practice]]
sources: []
last_updated: 2026-03-11
review_status: researched
priority: high
---

# AI Engineering - Roadmap

## Phase 1 - Foundations

Learn:

- what tokens and tokenization are
- what embeddings are for
- how transformer attention creates context dependence
- why context windows shape both behavior and cost
- why prefill and decode feel different to users

Deliverables:

- explain the request path from prompt to generated output
- explain why long prompts and long chats are expensive
- describe how embeddings support semantic retrieval

## Phase 2 - Inference and serving

Learn:

- model loading
- GPU memory pressure
- batching
- prefill vs decode
- KV cache
- concurrency
- model serving basics

Deliverables:

- explain why serving is not just "run the model"
- identify likely bottlenecks in a simple deployment
- describe why some systems optimize throughput while others optimize tail latency

## Phase 3 - Retrieval systems

Learn:

- chunking
- metadata
- embeddings
- vector search
- hybrid retrieval
- reranking
- context packing
- citation grounding

Deliverables:

- build a simple RAG pipeline
- inspect bad retrieval examples
- diagnose whether a retrieval problem is chunking, search mode, ranking, or packing

Practice:

- [[Practice - RAG Diagnostics and Incident Reviews]]

## Phase 4 - Prompting, tool use, and structured outputs

Learn:

- prompt design basics
- tool use
- JSON and schema-constrained outputs
- workflow prompts
- loop control
- validation boundaries

Deliverables:

- design prompts that are stable enough for automation
- reduce formatting failure and hallucinated structure
- reason about when a tool call is safer than free-form generation

## Phase 5 - Evaluation and model selection

Learn:

- task definition
- golden sets
- regression testing
- rubric-based evaluation
- human evaluation
- model comparison
- tradeoff scorecards

Deliverables:

- create a small eval set
- compare candidate models
- explain why a model wins or loses for a specific product context

Practice:

- [[Practice - Benchmarking and Failure Analysis Drills]]

## Phase 6 - Customization

Learn:

- when prompting is enough
- when retrieval is enough
- when tool use is enough
- when fine-tuning is justified
- LoRA and PEFT tradeoffs
- post-tuning eval burden

Deliverables:

- explain the decision logic before tuning
- understand the data and evaluation burden tuning adds
- distinguish model customization from system design

Practice:

- [[Practice - Prompt, Retrieval, and Tuning Decision Drills]]

## Phase 7 - Production operations

Learn:

- latency budgets
- observability
- token and unit cost tracking
- reliability
- guardrails
- prompt injection
- output validation
- fallbacks and rollback plans

Deliverables:

- reason about production readiness
- describe the minimum telemetry needed to operate a system
- avoid the most common early production mistakes

## Phase 8 - Deliberate practice and teaching compression

Learn:

- how to write short decision memos
- how to diagnose failures from traces and retrieval artifacts
- how to compress a section into a NotebookLM packet
- how to track sections as episode-sized learning units

Deliverables:

- write short decision memos from ambiguous scenarios
- produce packet files that teach one coherent lesson
- connect practice notes back into source and topic notes

## Recommended build sequence

Build in this order:

1. small API prototype
2. structured output workflow
3. simple eval harness
4. simple RAG system
5. tool-using workflow
6. local or self-hosted open-weight model path
7. serving and latency instrumentation
8. targeted fine-tuning only if justified
