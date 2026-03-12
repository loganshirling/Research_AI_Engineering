# Tokens and Tokenization

---
title: Concept - Tokens and Tokenization
type: concept
status: draft
topic: AI Engineering
course: AI Engineering
tags: [tokens, tokenization, llm-foundations]
prerequisites: []
related: [[Concept - Embeddings and Vector Similarity]], [[Topic - LLM Inference Path, KV Cache, and Batching]]]
sources: []
last_updated: 2026-03-11
review_status: seed
priority: high
---

# Tokens and Tokenization

## Purpose

Tokens are the basic unit of text that large language models process.

An LOM does not read characters or words directly. Instead, text is converted into a sequence of tokens before it is processed.

## Why it matters

Tokenization directly affects:

- context window size
- inference cost
- latency
- retrieval chunking strategies

## 80/20 summary

1. Text is split into tokens before model processing.
2. Token counts determine context window usage.
3. Tokenization schemes affect model behavior.

## Key concepts

- Byte pair encoding (BPE)
- Subword tokenization
- Token limits
- Token counting for cost estimations

## Common mistakes

- Assuming 1 word = 1 token
- Not measuring token usage in production systems

## What to read next

- [[Concept - Embeddings and Vector Similarity]]
- [[Topic - Retrieval System Design for RAG]]
