---
title: AI Engineering - 80-20 Summary
type: synthesis
status: active
topic: AI Engineering
course: AI Engineering
tags: [80-20, ai-engineering]
prerequisites: []
related: [[AI Engineering - Course Home]], [[AI Engineering - Roadmap]], [[Map - AI Engineering]]
sources: []
last_updated: 2026-03-11
review_status: seed
priority: high
core_status: core
practical_value: very_high
explanatory_power: very_high
difficulty: medium
must_know: true
---
# AI Engineering - 80-20 Summary

## What matters most

Most AI engineering work is not about inventing better base models.
It is about making model behavior useful, reliable, affordable, and observable inside a system.

## The core stack to understand

1. **Model layer**  
What model is being used, what it is good at, and what limits it has.

2. **Inference layer**  
How the model is actually run, including hardware, memory use, batching, KV cache behavior, and token generation speed.

3. **Application layer**  
Prompts, tools, structured outputs, retrieval, business logic, and user-facing behavior.

4. **Evaluation layer**  
How you detect whether the system is actually improving or quietly getting worse.

5. **Operations layer**  
Cost, latency, logging, safety, uptime, and rollback.

## The practical truth about LLM systems

A weak model in a well-designed system can beat a strong model in a sloppy system for many real tasks.

This usually happens because:
- prompts are clearer
- retrieval is better
- schemas are enforced
- the task is evaluated
- failures are visible
- cost and latency are controlled

## The most important terms to internalize early

- token
- context window
- embedding
- inference
- throughput
- latency
- prefill
- decode
- batch
- KV cache
- quantization
- structured output
- eval
- hallucination
- retrieval
- reranking
- fine-tuning
- adapter
- guardrail

## The most important instinct shifts

### 1. Think in systems, not just prompts
When output quality is poor, the cause may be:
- wrong model
- weak prompt
- bad retrieval
- poor chunking
- missing tool access
- schema mismatch
- no eval loop

### 2. Speed is usually constrained by memory and token generation realities
Model quality alone does not determine user experience.
Token speed, queueing, context size, and concurrency often matter just as much.

### 3. Evaluation is the difference between a demo and an engineering discipline
Without evals, most improvements are guesses.

### 4. Retrieval quality can dominate RAG performance
Many “generation” problems are actually “retrieval and context quality” problems.

### 5. Fine-tuning is not the first hammer
Prompting, system design, retrieval, and better evaluation often produce higher return earlier.

## What usually goes wrong

- teams optimize vibes instead of measurable task performance
- no one defines success before changing prompts or models
- people assume bigger context always helps
- retrieval adds noise instead of signal
- structured output is not enforced
- logs do not capture failures usefully
- fine-tuning is attempted before the baseline system is stable
- cost and latency are treated as cleanup instead of design inputs

## The first practical capability threshold

You are becoming passable when you can:
- explain why an LLM response failed in more than one plausible way
- design a simple eval before making changes
- compare API and self-hosted deployment rationally
- describe what vLLM is solving
- reason about performance bottlenecks without mystical thinking
- build a small RAG or structured output workflow and inspect where it breaks

## What to study next

Start with:
1. inference vocabulary and serving basics
2. structured outputs and prompt reliability
3. evaluation basics
4 . retrieval systems
5. vLLM and optimization
