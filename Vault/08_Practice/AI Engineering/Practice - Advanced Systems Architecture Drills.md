---
title: Practice - Advanced Systems Architecture Drills
type: practice
status: active
topic: AI Engineering
course: AI Engineering
tags: [practice, ai-engineering, architecture, agents, serving, multimodal]
prerequisites:
  - "[[AI Engineering - Course Home]]"
  - "[[Map - AI Engineering]]"
related:
  - "[[Topic - vLLM, PagedAttention, Prefix Caching, and Continuous Batching]]"
  - "[[Topic - Context Engineering, Prompt Caching, and Long-Context Tradeoffs]]"
  - "[[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]]"
  - "[[Topic - MCP, Tool Design, and Secure Agent Execution]]"
  - "[[Topic - Eval Flywheels, Human Review, and Regression Gates]]"
sources: []
last_updated: 2026-03-12
review_status: researched
priority: high
---

# Practice - Advanced Systems Architecture Drills

## Purpose

Use the advanced topic notes to practice system design decisions instead of only reading explanations.

## How to use this note

For each drill:
1. identify the dominant failure cost,
2. choose the minimum architecture that solves the problem,
3. define the first measurements you would collect,
4. name the highest-risk failure mode,
5. state the next experiment or rollback-safe step.

## Drill 1 - Serving bottleneck triage

A customer-support assistant moved from a smaller model to a larger self-hosted model on vLLM. Total throughput fell, first-token latency doubled, and users report slow starts even on short answers.

Answer:
- What would you measure first?
- Would you look at prefill, decode, queueing, or cache pressure first?
- When would prefix caching help?
- What would make you lower concurrency rather than add hardware?

## Drill 2 - Long-context confusion

A research agent receives 100 pages of material, but key facts from the middle of the packet are often omitted. The team wants to keep adding more context because the window allows it.

Answer:
- What is the likely failure mode?
- What context engineering changes would you try before expanding the prompt further?
- What should be summarized, trimmed, or re-grounded?

## Drill 3 - Multi-agent or deterministic workflow

A compliance workflow has four fixed steps, one conditional approval, and one final external action. A team proposes a multi-agent design where the model decides the order dynamically.

Answer:
- What should stay deterministic?
- Where, if anywhere, should an LLM decide?
- What traces would you require before approving the design?

## Drill 4 - Tool-risk review

An internal operations agent can read tickets, search docs, update records, and run shell commands. The team wants all tools available in one agent to maximize flexibility.

Answer:
- Which capabilities should be separated?
- Which tools should require approval?
- What is the main prompt-injection risk path?
- How would MCP change your trust and auditing model?

## Drill 5 - Eval flywheel design

Your team changes prompts, retrieval settings, and tool descriptions weekly. Quality seems to improve overall, but every few releases a previously solved failure reappears.

Answer:
- What eval sets should exist?
- Which cases need human review?
- What should block release?
- How would you link incidents to benchmark updates?

## Drill 6 - Document pipeline selection

You need to process 50,000 PDFs, answer user questions with citations, and also extract normalized fields from invoices and forms.

Answer:
- Which parts need retrieval?
- Which parts need multimodal page understanding?
- Where do structured outputs belong?
- What would you measure separately for search, page interpretation, and extraction?

## Drill 7 - Voice-agent readiness

A voice concierge sounds intelligent in demos but feels awkward in live use. Users interrupt it often, and it sometimes keeps talking after it should stop.

Answer:
- Which metrics are missing?
- Which prompt or turn-taking changes would you try?
- When does a text-first architecture stop being good enough?

## Drill 8 - End-to-end build memo

Pick one of the above drills and write a short build memo with:
- system purpose,
- dominant failure cost,
- chosen architecture,
- approval or safety gates,
- eval plan,
- rollback plan.

## Success criteria

You are improving when you can:
- name the dominant risk quickly,
- choose a smaller architecture over a more fashionable one,
- define the first measurements before proposing a fix,
- and connect failures back to notes, traces, and eval gates.
