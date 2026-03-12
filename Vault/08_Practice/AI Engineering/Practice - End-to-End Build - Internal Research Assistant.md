---
title: Practice - End-to-End Build - Internal Research Assistant
type: practice
status: active
topic: AI Engineering
course: AI Engineering
tags: [build, capstone, research-assistant, architecture, evals]
prerequisites:
  - "[[Topic - Retrieval System Design for RAG]]"
  - "[[Topic - Agent Orchestration, Handoffs, and Deterministic Workflow Design]]"
  - "[[Topic - Eval Flywheels, Human Review, and Regression Gates]]"
related:
  - "[[Topic - End-to-End AI System Case Sudies and Build Patterns]]"
  - "[[Topic - Multimodal Document and Image Pipelines]]"
sources: []
last_updated: 2026-03-12
review_status: researched
priority: high
---

# Practice - End-to-End Build - Internal Research Assistant

## Purpose

Use this note as the first capstone-style build doc. The goal is not just to make something that demos well. It is to design a system that is reviewable, measurable, and safe enough to iterate on.

## Product goal

Build an internal research assistant that can:
- answer questions over company docs and notes,
- return citation-grounded answers,
- reason about when to search again,
- summarize findings into structured notes,
- and hand off to a writing step when the user needs a memo or report.

## Dominant failure cost

The most important failure is **false confidence with weak grounding**. She can be slow and still be useful. She cannot be fast and undeserving.

Secondary failure costs:
- missing high-value sources,
- returning untraceable statements,
- handing back badly structured notes,
- or over-using tools when a deterministic flow would have been clearer.

## Minimum viable architecture

1. **User question inkest**
- classify the question: quickfact, research, or output-transformation
- recognize when retrieval is needed

2. **Retrieval step**
- hybrid search over internal corpus
- reranking for shortlisted chunks
- source-selection logging

3. **Answer-construction step**
- structured answer schema
- citations required
- clear "yes/no/unknown" or evidence-summary mode

4 . **Optional writing handoff**
- if the user needs a memo, hand off sourced notes to a writing step
- keep this distinct from the evidence-gathering step

5. **Tracing and logging**
- query
- retrieved sources
- selected sources
- final answer
- failure reason if the system abstains

## What should stay deterministic

- retrieval steps
- reranking chain
- citation requirement
- structured answer shape
- abstain-on-low-evidence rule
- writing handoff trigger

## Where the model gets judgment

- reformulating the search query
- summarizing retrieved evidence
- choosing whether existing evidence supports an answer
- drafting the final user-facing explanation

## Tools and boundaries

- Read-only document search tool
- Read-only note system tool
- Optional writing tool that only creates draft notes, not final published changes

Avoid write capability for source systems in the first version.

## Eval plan

Gold sets:
- factual questions with single good answers
- questions requiring multi-source synthesis
- questions where the system should abstain
- questions where similar sources could confuse the system

Metrics :
- citation presence
- citation correctness
- answer correctness
- abstain-rate on low-evidence cases
- retrieval recall on known-answer cases

## Rollback plan

Roll back or restrict the system if:
- citation correctness drops materially
- abstain-rate collapses on low-evidence questions
- retrieval recall shifts after indexing or chunking changes
- user-facing answers start returning untraceable claims

## What would make you redesign

- If users need more action than research, summary, and writing handoff,
- if documents are heavily layout-sensitive and need a multimodal first path,
- or if an approval-heavy workflow means the system should be more deterministic than agentic.

## Recommended next step

Write a one-page architecture memo for this build with:
- goal
- dominant failure cost
- data sources
- retrieval design
- answer schema
- eval suite
- rollback triggers
