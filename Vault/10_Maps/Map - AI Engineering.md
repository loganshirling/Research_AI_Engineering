---
title: Map - AI Engineering
type: map
status: active
topic: AI Engineering
course: AI Engineering
tags: [map, ai-engineering]
prerequisites: []
related: [[AI Engineering - Course Home]], [[AI Engineering - Roadmap]], [[AI Engineering - 80-20 Summary]], [[AI Engineering Practice]]
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
- [[AI Engineering Practice]]

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
- prompt-only baselines before tuning

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
- retrieval as a knowledge lever
- [[Topic - Common RAG Failure Modes]]

### Model customization
- fine-tuning
- SFT
- adapters and LoRA
- PEFT
- data quality
- overfitting risk
- deployment tradeoffs for tuned models
- [[Topic - When to Fine-Tune vs When Not To]]
- [[Topic - Adapters, LoRA, and PEFT Tradeoffs]]

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
- [[Topic - When to Fine-Tune vs When Not To]]
- [[Topic - Adapters, LoRA, and PEFT Tradeoffs]]

### Source notes
- [[Source - Kwon et al. 2023 - Efficient Memory Management for Large Language Model Serving with PagedAttention]]
- [[Source - Hugging Face - KV Cache Strategies and Cache Explanation]]
- [[Source - NVIDIA - Prefill, Decode, In-flight Batching, and Chunked Prefill]]
- [[Source - vLLM - Features, Architecture, Metrics, and Tuning]]
- [[Source - Quantization for LLM Inference - Core Sources]]
- [[Source - Eval Design for Serving Changes - Core Sources]]
- [[Source - Common RAG Failure Modes - Core Sources]]
- [[Source - Production Bottlenecks and Observability - Core Sources]]
- [[Source - Fine-Tuning Decision Framework - Core Sources]]
- [[Source - Adapters, LoRA, and PEFT - Core Sources]]

### Practice notes
- [[AI Engineering Practice]]
- [[Practice - Prompt, Retrieval, and Tuning Decision Drills]]
- [[Practice - Benchmarking and Failure Analysis Drills]]
- [[Practice - RAG Diagnostics and Incident Reviews]]

## What to add next
- review notes that compress each major cluster into fast recall prompts
- worked examples for rollout decisions, benchmark interpretation, and incident writeups
- one or two end-to-end synthesis notes tying serving, retrieval, evals, and tuning together
