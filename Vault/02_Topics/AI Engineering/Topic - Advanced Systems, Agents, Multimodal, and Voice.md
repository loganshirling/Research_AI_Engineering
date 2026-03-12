
---
title: Topic - Advanced Systems, Agents, Multimodal, and Voice
type: topic
status: active
topic: Advanced AI Systems
course: AI Engineering
tags: [advanced-systems, agents, multimodal, voice, serving, evals]
prerequisites:
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
  - "[[Topic - Tool Calling, Structured Outputs, and Agent Loops]]"
  - "[[Topic - Retrieval System Design for RAG]]"
related:
  - "[[Topic - vLLM, PagedAttention, Prefix Caching, and Continuous Batching]]"
  - "[[Topic - Context Engineering, Prompt Caching, and Long-Context Tradeoffs]]"
  - "[[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]]"
  - "[[Topic - MCP, Tool Design, and Secure Agent Execution]]"
  - "[[Topic - Eval Flywheels, Human Review, and Regression Gates]]"
  - "[[Topic - Multimodal Document and Image Pipelines]]"
  - "[[Topic - Realtime Voice Agents and Speech-to-Speech Systems]]"
  - "[[Topic - End-to-End AI System Case Studies and Build Patterns]]"
  - "[[Source - Advanced Systems Expansion - Core Sources]]"
sources:
  - "https://arxiv.org/abs/2309.06180"
  - "https://docs.vllm.ai/en/latest/"
  - "https://developers.openai.com/api/docs/guides/prompt-caching/"
  - "https://www.anthropic.com/research/building-effective-agents"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: hard
must_know: true
---

# Topic - Advanced Systems, Agents, Multimodal, and Voice

## Purpose

This note is now the **overview page** for the advanced systems section of the course.

The original expansion grouped eight next-step topics into one module so the course could move quickly. The next stage of the course is to treat those topics as separate study units so they can support deeper notes, stronger internal linking, and more focused practice.

## Why this section matters

The beginner version of AI engineering is learning what models, retrieval, prompts, and evals are.

The practitioner version is learning how to make those pieces work together under:
- latency limits,
- cost limits,
- approval boundaries,
- security constraints,
- observability requirements,
- and product-quality expectations.

That is the layer this section covers.

## Read this section in order

1. [[Topic - vLLM, PagedAttention, Prefix Caching, and Continuous Batching]]
2. [[Topic - Context Engineering, Prompt Caching, and Long-Context Tradeoffs]]
3. [[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]]
4. [[Topic - MCP, Tool Design, and Secure Agent Execution]]
5. [[Topic - Eval Flywheels, Human Review, and Regression Gates]]
6. [[Topic - Multimodal Document and Image Pipelines]]
7. [[Topic - Realtime Voice Agents and Speech-to-Speech Systems]]
8. [[Topic - End-to-End AI System Case Studies and Build Patterns]]

Then use:
- [[Practice - Advanced Systems Architecture Drills]]
- [[Practice - End-to-End Build - Internal Research Assistant]]
- [[Packet - AI Engineering - Section 02 - Advanced Systems and Agent Operations]]

## 80/20 summary

Advanced AI engineering is mostly about controlling:
- what context the model sees,
- what tools it can use,
- what parts of the workflow remain deterministic,
- how failures are measured,
- and what happens when the system is wrong, slow, or unsafe.

The key shift is from isolated capabilities to controlled systems.

## Section map

### Serving and latency
Use [[Topic - vLLM, PagedAttention, Prefix Caching, and Continuous Batching]] to understand where modern self-hosted serving wins come from and how to reason about queueing, prefill, decode, and cache pressure.

### Context and memory
Use [[Topic - Context Engineering, Prompt Caching, and Long-Context Tradeoffs]] to understand why long context is not the same as good context and how memory policy shapes behavior.

### Workflow control
Use [[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]] to decide what the model should control versus what the application should hard-code.

### Tool boundaries and security
Use [[Topic - MCP, Tool Design, and Secure Agent Execution]] to reason about permissions, trust boundaries, tool ergonomics, and agent-app risk.

### Reliability and improvement
Use [[Topic - Eval Flywheels, Human Review, and Regression Gates]] to turn incidents and regressions into repeatable system learning.

### Multimodal systems
Use [[Topic - Multimodal Document and Image Pipelines]] to separate search, page understanding, extraction, and grounding.

### Voice systems
Use [[Topic - Realtime Voice Agents and Speech-to-Speech Systems]] to reason about interruption, pacing, recovery, and live-session quality.

### Capstone design
Use [[Topic - End-to-End AI System Case Studies and Build Patterns]] to see how the full stack fits together in real products.

## What to read next

- [[Source - Advanced Systems Expansion - Core Sources]]
- [[Practice - Advanced Systems Architecture Drills]]
- [[Practice - End-to-End Build - Internal Research Assistant]]
