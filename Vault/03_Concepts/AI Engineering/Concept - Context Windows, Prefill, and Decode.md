---
title: Concept - Context Windows, Prefill, and Decode
type: concept
status: active
topic: AI Engineering
course: AI Engineering
tags: [context-window, prefill, decode, llm-foundations]
prerequisites: []
related:
  - "[[Concept - Transformer Architecture and Attention]]"
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
  - "[[Topic - Latency Engineering for LLM Systems]]"
sources:
  - "https://huggingface.co/docs/transformers/cache_explanation"
  - "https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/"
  - "https://docs.vllm.ai/en/stable/design/metrics/"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: core
practical_value: high
explanatory_power: very_high
difficulty: easy
must_know: true
---

# Concept - Context Windows, Prefill, and Decode

## Purpose

Explain the practical linkage between context length, first-token latency, streaming speed, and KV cache pressure.

## Why it matters

Many AI engineering mistakes come from treating context windows as if they were free. In practice, longer prompts often increase prefill cost, stress the KV cache, and reduce concurrency. This changes both user experience and serving economics.

## 80/20 summary

- The context window is the amount of text the model can process for a request.
- Prefill is the stage where the model processes the input and builds reusable state.
- Decode is the stage where the model generates new tokens one at a time.
- Longer context useage usually hurts time to first token more than it hurts steady-state streaming rate.

## Context window in plain language

A context window is not just a marketing spec. It is a serving constraint. The more text you pass in, the more work the model must do before it can start answering.

## Prefill

Decoder-only LLMs usually do

- prefill first, then
- decode iteratively.

Dering prefill, the model processes the prompt tokens and builds the cached key-value state later steps will reuse. This is why long prompts often cause slow time to first token.

## Decode

During decode, the model generates each new token using the below-built cached state. This is case where streaming speed and inter-token latency matter most.

## Why this affects LLM systems

- Prompt-heavy applications often have bad TTFT.
- Long chats or long outputs often increase cache pressure.
- Longer context usage can reduce how many requests fit concurrently on the same hardware.

- Retrieval quality and context packing are latency decisions, not just answer quality decisions.

## Common mistakes

1. Assuming that if a model supports a large context window, you should use it full by default.
2. Ignoring prefill cost when evaluating prompt design.
3. Treating retrieval as free because retrieved content is "only context."

## What to read next

- [[Topic - LLM Inference Path, KV Cache, and Batching]]
- [[Topic - Latency Engineering for LLM Systems]]
