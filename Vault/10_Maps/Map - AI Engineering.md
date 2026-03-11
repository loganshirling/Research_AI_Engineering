---
title: Map - AI Engineering
type: map
status: active
topic: AI Engineering
course: AI Engineering
tags: [map, ai-engineering]
prerequisites: []
related: [[AI Engineering - Course Home]], [[AI Engineering - Roadmap]], [[AI Engineering - 80-20 Summary]]
sources: []
last_updated: 2026-03-11
review_status: seed
priority: high
---
# Map - AI Engineering

## Where to start
- [[AI Engineering - Course Home]]
- [[AI Engineering - 80-20 Summary]]
- [[AI Engineering - Roadmap]]

## Main topic clusters

### Foundations
- tokens
- context windows
- embeddings
- transformers and attention
- training vs inference
- closed vs open-weight models

### Inference and serving
- model loading
- batching
- prefill and decode
- KV cache
- concurrency
- memory bottlenecks
- GPU utilization
- [[Topic - LLM Inference Path, KV Cache, and Batching]]

### vLLM and optimization
- why vLLM exists
- paged attention
- continuous batching
- quantization
- speculative decoding
- throughput tuning

### Prompting and application design
- role separation
- instruction design
- structured outputs
- tool use
- workflow prompts
- prompt brittleness

### Evaluation
- task definitions
- golden sets
- regression testing
- human review
- error taxonomy
- failure clustering

### Retrieval systems
- chunking
- embeddings
- vector databases
- reranking
- context assembly
- grounding failures

### Model customization
- fine-tuning
- SFT
- adapters and LoRA
- data quality
- overfitting risk

### Production operations
- observability
- cost tracking
- latency budgets
- reliability
- fallbacks
- safety

## Built notes in this cluster

### Topic notes
- [[Topic - LLM Inference Path, KV Cache, and Batching]]

### Source notes
- [[Source - Kwon et al. 2023 - Efficient Memory Management for Large Language Model Serving with PagedAttention]]
- [[Source - Hugging Face - KV Cache Strategies and Cache Explanation]]
- [[Source - NVIDIA - Prefill, Decode, In-flight Batching, and Chunked Prefill]]
- [[Source - vLLM - Features, Architecture, Metrics, and Tuning]]

## What to add next
- topic notes for each main cluster
- concept notes for key terms
- technique notes for build workflows
- source notes tied to specific papers, docs, and benchmarks
- practice notes for drills and mini-projects
