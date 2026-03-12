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
sources: []
last_updated: 2026-03-12
review_status: researched
priority: high
---

# AI Engineering - Roadmap

## Phase 1 - Foundations

Learn:
- tokens and tokenization,
- embeddings,
- transformer attention,
- context windows,
- prefill and decode.

Deliverables:
- explain the request path from prompt to output,
- explain why long prompts are expensive,
- describe how embeddings support semantic retrieval.

## Phase 2 - Inference and serving

Learn:
- model loading,
- GPU memory pressure,
- batching,
- KV cache,
- concurrency,
- quantization,
- model serving basics.

Deliverables:
- explain why serving is not just "run the model,"
- identify likely bottlenecks in a deployment,
- explain why different systems optimize throughput vs tail latency differently.

## Phase 3 - Retrieval systems

Learn:
- chunking,
- metadata,
- embeddings,
- vector search,
- hybrid retrieval,
- reranking,
- context packing,
- citation grounding.

Deliverables:
- build a simple RAG pipeline,
- inspect bad retrieval examples,
- diagnose whether the problem is chunking, search mode, ranking, or packing.

Practice:
- [[Practice - RAG Diagnostics and Incident Reviews]]

## Phase 4 - Prompting, tool use, and structured outputs

Learn:
- prompt design basics,
- tool use,
- JSON and schema-constrained outputs,
- workflow prompts,
- loop control,
- validation boundaries.

Deliverables:
- design prompts that are stable enough for automation,
- reduce formatting failures and hallucinated structure,
- reason about when a tool call is safer than free-form generation.

## Phase 5 - Evaluation and model selection

Learn:
- task definition,
- gold sets,
- regression testing,
- rubric-based evaluation,
- human evaluation,
- model comparison,
- tradeoff scorecards.

Deliverables:
- create a small eval set,
- compare candidate models,
- explain why a model wins or loses for a specific product context.

Practice:
- [[Practice - Benchmarking and Failure Analysis Drills]]

## Phase 6 - Advanced systems and agent operations

Learn:
- vLLM, PagedAttention, prefix caching, and continuous batching,
- context engineering and prompt caching,
- handoffs and deterministic workflow design,
- MCP and tool security,
- eval flywheels and regression gates,
- multimodal document pipelines,
- realtime voice systems,
- and end-to-end build patterns.

Deliverables:
- explain when modern serving is bottlenecked by queueing, prefill, decode, or cache pressure,
- explain why larger context does not remove the need for curation,
- decide what should stay deterministic in an agent workflow,
- define approval and logging requirements for risky tools,
- turn observed failures into regression gates,
- separate document retrieval from page-level visual understanding,
- and sketch a voice-agent architecture with recovery behavior.

Core note:
- [[Topic - Advanced Systems, Agents, Multimodal, and Voice]]

Practice:
- [[Practice - Advanced Systems Architecture Drills]]

## Phase 7 - Deliberate practice and teaching compression

Learn:
- how to write short decision memos,
- how to diagnose failures from traces and artifacts,
- how to compress a section into a NotebookLM packet,
- and how to connect practice back into build decisions.

Deliverables:
- write short architecture memos from ambiguous scenarios,
- produce packet files that teach one coherent lesson,
- and connect practice notes back into retrieval, tool, and serving decisions.

## Recommended build sequence

Build in this order:
1. small API prototype,
2. structured output workflow,
3. simple eval harness,
4. simple RAG system,
5. tool-using workflow,
6. local or self-hosted open-weight model path,
7. advanced systems module on serving, context, and orchestration,
8. document or multimodal workflow,
9. voice or realtime system if justified,
10. end-to-end build notes with traces, evals, and rollback logic.
