---
title: Source - Practitioner Systems Expansion - Core Sources
type: source
status: active
topic: AI Engineering
course: AI Engineering
tags: [sources, ai-engineering, retrieval, latency, observability, safety]
prerequisites: []
related:
  - "[[Topic - Retrieval System Design for RAG]"
  - "[[Topic - Latency Engineering for LLM Systems]]"
  - "[[Topic - Guardrails, Prompt Injection, and Output Validation]]"
sources: []
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Source - Practitioner Systems Expansion - Core Sources

## Purpose

Collect the core references used to expand the practitioner side of the AI Engineering vault.

## Architecture and foundations

- Vaswani et al., "Attention Is All You Need." https://arxiv.org/abs/1706.03762
- Hugging Face docs, "Tokenizer." https://huggingface.co/docs/transformers/main_classes/tokenizer
- Hugging Face docs, "Caching." https://huggingface.co/docs/transformers/cache_explanation
- Hugging Face docs, "KV cache strategies." https://huggingface.co/docs/transformers/en/kv_cache

## Retrieval systems

- Pinecone docs, "Search overview." https://docs.pinecone.io/guides/search/search-overview
- Pinecone docs, "Indexing overview." https://docs.pinecone.io/guides/index-data/indexing-overview
- Pinecone docs, "Hybrid search." https://docs.pinecone.io/guides/search/hybrid-search
- Weaviate docs, "Search." https://docs.weaviate.io/weaviate/concepts/search
- Weaviate docs, "Hybrid search." https://docs.weaviate.io/weaviate/concepts/search/hybrid-search
- Weaviate docs, "Reranking." https://docs.weaviate.io/weaviate/search/rerank

## Tool use and structured systems

- OpenAI docs, "Structured outputs." https://platform.openai.com/docs/guides/structured-outputs
- OpenAI API reference, "Responses." https://platform.openai.com/docs/api-reference/responses
- OpenAI docs, "Evals guide." https://platform.openai.com/docs/guides/evals
- Anthropic docs, "Tool use." https://docs.anthropic.com/en/docs/build-with-claude/tool-use
- Anthropic docs, "How to implement tool use." https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/implement-tool-use

## Serving and latency

- NVIDIA Technical Blog, "LLM Inference Benchmarking: Performance Tuning with TensorrT-LLM." https://developer.nvidia.com/blog/llm-inference-benchmarking-performance-tuning-with-tensorrt-llm
- NVIDIA Technical Blog, "Streamlining AI Inference Performance and Deployment with NVIDIA TensorRT-LLM Chunked Prefill." https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/
- vLLM docs, home. https://docs.vllm.ai/en/latest/
- vLLM docs, "Metrics." https://docs.vllm.ai/en/stable/design/metrics/
- vLLM docs, "Optimization and tuning." https://docs.vllm.ai/en/stable/configuration/optimization/

## Safety and security

- OWASP Cheat Sheet Series, "LLM Prompt Injection Prevention Cheat Sheet." https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html
- OWASP GenAI Security Project, "LLM01: Prompt Injection." https://genai.owasp.org/llmrisk/llm01-prompt-injection/
- OWASP Cheat Sheet Series, "AI Agent Security Cheat Sheet." https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html

## Observability

- OpenTelemetry docs, "Semantic conventions for generative AI systems." https://opentelemetry.io/docs/specs/semconv/gen-ai/
- OpenTelemetry docs, "Semantic conventions for generative client AI spans." https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-spans/
- OpenTelemetry docs, "Semantic conventions for generative AI events." https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-events/
- OpenTelemetry docs, "Semantic conventions for generative AI metrics." https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-metrics/
