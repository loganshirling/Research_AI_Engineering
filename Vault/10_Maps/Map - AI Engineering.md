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
- [[Topic - Quantization Tradeoffs for LLM Inference]]

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
- [[Topic - Eval Design for Serving Changes]]

### Retrieval systems
- chunking
- embeddings
- vector databases
- reranking
- context assembly
- grounding failures
- [[Topic - Common RAG Failure Modes]]

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
- [[Topic - Production Bottlenecks and Observability for LLM Systems]]

## Built notes in this cluster

### Topic notes
- [[Topic - LLM Inference Path, KV Cache, and Batching]]
- [[Topic - Quantization Tradeoffs for LLM Inference]]
- [[Topic - Eval Design for Serving Changes]]
- [[Topic - Common RAG Failure Modes]]
- [[Topic - Production Bottlenecks and Observability for LLM Systems]]

### Source notes
- [[Source - Kwon et al. 2023 - Efficient Memory Management for Large Language Model Serving with PagedAttention]]
- [[Source - Hugging Face - KV Cache Strategies and Cache Explanation]]
- [[Source - NVIDIA - Prefill, Decode, In-flight Batching, and Chunked Prefill]]
- [[Source - vLLM - Features, Architecture, Metrics, and Tuning]]
- [[Source - Quantization for LLM Inference - Core Sources]]
- [[Source - Eval Design for Serving Changes - Core Sources]]
- [[Source - Common RAG Failure Modes - Core Sources]]
- [[Source - Production Bottlenecks and Observability - Core Sources]]

## What to add next
- when to fine-tune vs when not to
- practice notes for benchmarking and failure analysis
- practice notes for RAG diagnostics and incident reviews