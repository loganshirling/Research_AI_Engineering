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
  - "[[Topic - AI Agents: Design, Boundaries, and Operations]]"
  - "[[Topic - OpenClaw and Self-Hosted Personal Agents]]"
  - "[[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]]"
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
  - "https://www.anthropic.com/engineering/building-effective-agents"
  - "https://docs.openclaw.ai/"
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

This note is the **overview page** for the advanced systems section of the course.

The section now has enough depth that the right move is not to keep everything in one giant note. The goal is to treat the major subjects as separate study units that can support deeper notes, stronger internal linking, and more focused practice.

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
3. [[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]]
4. [[Topic - AI Agents: Design, Boundaries, and Operations]]
5. [[Topic - OpenClaw and Self-Hosted Personal Agents]]
6. [[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]]
7. [[Topic - MCP, Tool Design, and Secure Agent Execution]]
8. [[Topic - Eval Flywheels, Human Review, and Regression Gates]]
9. [[Topic - Multimodal Document and Image Pipelines]]
10. [[Topic - Realtime Voice Agents and Speech-to-Speech Systems]]
11. [[Topic - End-to-End AI System Case Studies and Build Patterns]]

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
Use [[Topic - Context Engineering, Prompt Caching, and Long-Context Tradeoffs]] and [[Technique - Context Budgeting]] to understand why long context is not the same as good context.

### Retrieval architecture
Use [[Topic - Vector Databases, Embeddings, and Retrieval Tradeoffs]] to understand the layer between embeddings and production retrieval systems: ANN indexes, metadata filters, hybrid search, and when vector databases help or do not help.

### Agent system design
Use [[Topic - AI Agents: Design, Boundaries, and Operations]] to reason about agent design, building, monitoring, approval flows, observability, and failure handling.

### Self-hosted personal agents
Use [[Topic - OpenClaw and Self-Hosted Personal Agents]] as a concrete case study in self-hosted agent architecture, trust boundaries, remote access, and operator control.

### Workflow control
Use [[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]] to decide what the model should control versus what the application should hard-code.

### Tool boundaries and security
Use [[Topic - MCP, Tool Design, and Secure Agent Execution]] and [[Technique - Agent Approval Design]] to reason about permissions, trust boundaries, tool ergonomics, and agent-app risk.

### Reliability and improvement
Use [[Topic - Eval Flywheels, Human Review, and Regression Gates]] and [[Technique - Eval Operations]] to turn incidents and regressions into repeatable system learning.

### Multimodal systems
Use [[Topic - Multimodal Document and Image Pipelines]] to separate search, page understanding, extraction, and grounding.

### Voice systems
Use [[Topic - Realtime Voice Agents and Speech-to-Speech Systems]] to reason about interruption, pacing, recovery, and live-session quality.

### Capstone design
Use [[Topic - End-to-End AI System Case Studies and Build Patterns]] to see how the full stack fits together in real products.

## What to read next

- [[Source - Advanced Systems Expansion - Core Sources]]
- [[Source - Agent Systems Design, Approvals, and Monitoring - Core Sources]]
- [[Source - Vector Databases and Retrieval Tradeoffs - Core Sources]]
- [[Source - OpenClaw - Current State, Security Model, and Deployment Notes]]
- [[Practice - Advanced Systems Architecture Drills]]
