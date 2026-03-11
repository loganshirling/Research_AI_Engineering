---
title: AI Engineering - Roadmap
type: roadmap
status: active
topic: AI Engineering
course: AI Engineering
tags: [roadmap, ai-engineering]
prerequisites: []
related: [[AI Engineering - Course Home]], [[AI Engineering - 80-20 Summary]], [[Map - AI Engineering]]
sources: []
last_updated: 2026-03-11
review_status: seed
priority: high
---
# AI Engineering - Roadmap

## Phase 1 - Foundations

Learn:
- what a token is
- how context windows shape behavior and cost
- the difference between training, fine-tuning, and inference
- what embeddings are for
- what attention and transformers do at a high level
- why latency, throughput, memory, and quality are in tension

Deliverables:
- define the major terms in plain language
- explain the request path from prompt to generated output
- compare closed API vs open-weight deployment

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
- explain why serving is not just “run the model”
- identify likely bottlenecks in a simple deployment
- describe why some systems optimize throughput while others optimize tail latency

## Phase 3 - vLLM and performance optimization

Learn:
- why vLLM exists
- paged attention at a practical level
- memory utilization patterns
- continuous batching
- quantization tradeoffs
- speculative decoding concepts
- practical throughput tuning

Deliverables:
- explain when vLLM is a good default
- understand how it differs from naive inference servers
- identify tradeoffs among quality, speed, cost, and hardware limits

## Phase 4 - Prompting and structured outputs

Learn:
- prompt design basics
- role/system instructions
- tool use
- JSON and schema-constrained outputs
- guardrails
- prompt brittleness

Deliverables:
- design prompts that are stable enough for automation
- reduce formatting failure and hallucinated structure
- reason about prompt changes systematically

## Phase 5 - Evaluation and failure analysis

Learn:
- task definition
- golden sets
- regression testing
- rubric-based evaluation
- human evaluation
- failure clustering
- error taxonomy

Deliverables:
- create a small eval set
- compare runs
- identify whether problems come from model choice, prompt design, retrieval, or system wiring

## Phase 6 - RAG and retrieval systems

Learn:
- chunking
- embeddings
- vector search
- retrieval quality
- reranking
- citation grounding
- context packing
- common retrieval failures

Deliverables:
- build a simple RAG pipeline
- inspect bad retrieval examples
- understand why retrieval quality often matters more than fancier generation logic

## Phase 7 - Fine-tuning and adapters

Learn:
- when fine-tuning is worth it
- when prompting is enough
- SFT basics
- LoRA or adapters
- data quality issues
- overfitting and evaluation after tuning

Deliverables:
- explain the decision logic before tuning
- understand the data and eval burden fine-tuning adds
- distinguish “model customization” from “system design”

## Phase 8 - Production operations

Learn:
- observability
- cost tracking
- latency budgets
- reliability
- rate limiting
- abuse and safety considerations
- fallbacks and rollback plans

Deliverables:
- reason about production readiness
- describe the minimum telemetry needed to operate a system
- avoid the most common early production mistakes

## Recommended build sequence

Build in this order:
1. small API prototype
2. structured output workflow
3. simple eval harness
4. simple RAG system
5. local or self-hosted open-weight model path
6. vLLM-backed serving experiment
7. targeted fine-tuning only if justified
