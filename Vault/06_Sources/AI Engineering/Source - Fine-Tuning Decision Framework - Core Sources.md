---
title: Source - Fine-Tuning Decision Framework - Core Sources
type: source
status: active
topic: Model Customization
course: AI Engineering
tags: [source, fine-tuning, prompting, rag, evals]
related:
  - "[[Topic - When to Fine-Tune vs When Not To]]"
  - "[[Topic - Adapters, LoRA, and PEFT Tradeoffs]]"
sources:
  - "https://developers.openai.com/api/docs/guides/optimizing-llm-accuracy/"
  - "https://developers.openai.com/api/docs/guides/prompt-engineering/"
  - "https://developers.openai.com/api/docs/guides/supervised-fine-tuning/"
  - "https://developers.openai.com/api/docs/guides/fine-tuning-best-practices/"
  - "https://developers.openai.com/api/docs/guides/model-optimization/"
  - "https://learn.microsoft.com/en-us/azure/developer/ai/augment-llm-rag-fine-tuning"
  - "https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/fine-tuning-considerations"
  - "https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/tune-models"
last_updated: 2026-03-11
review_status: researched
priority: high
---
# Source - Fine-Tuning Decision Framework - Core Sources

## Why this source bundle matters

This bundle groups the most decision-useful references for the practical question “when should I prompt, retrieve, or fine-tune?” It mixes provider docs from OpenAI, Microsoft, and Google because the strongest decision framework comes from where those sources overlap.

## Core source set

### 1. OpenAI optimization framework
- OpenAI's "Optimizing LLM Accuracy" guide is the anchor source because it frames prompting, retrieval, and fine-tuning as different levers rather than a fixed sequence.[^oa-accuracy]
- The highest-value distinction is between **context optimization** and **LLM optimization**:
  - use context optimization when the model lacks knowledge, the knowledge is stale, or the information is proprietary,
  - use LLM optimization when the main problem is consistency, formatting, tone, style, or following reasoning patterns more reliably.[^oa-accuracy]

### 2. OpenAI prompt engineering guide
- OpenAI's prompt guide is the key source for prompt-first discipline.[^oa-prompt]
- It explicitly presents few-shot learning as a way to steer a model toward a task without fine-tuning when a handful of examples in the prompt are enough.[^oa-prompt]
- It also reinforces the retrieval case by recommending additional context when you need proprietary data or data outside the model's training set.[^oa-prompt]

### 3. OpenAI supervised fine-tuning and best-practices guides
- The supervised fine-tuning guide is the best source for process requirements:
  - good evals first,
  - a robust and representative dataset,
  - and a baseline worth comparing against.[^oa-sft]
- It also gives a practical starting point: begin with about 50 strong demonstrations, evaluate, and only then scale data volume.[^oa-sft]
- The best-practices guide is strongest on data-quality failure modes:
  - inconsistent labels,
  - incomplete examples,
  - poor balance,
  - and examples that accidentally teach the model to hallucinate or over-refuse.[^oa-ft-best]

### 4. OpenAI model optimization guide
- This guide is the clearest source for the operational upside of fine-tuning:
  - shorter prompts,
  - lower token cost at scale,
  - lower latency,
  - and the ability to encode more demonstrations than fit in one prompt window.[^oa-model-opt]

### 5. Microsoft's RAG vs fine-tuning guidance
- Microsoft's "Augment LLMs with RAGs or Fine-Tuning" states the tradeoff in plain product language:
  - fine-tuning is best for specialized tasks,
  - RAG is better for flexible systems with up-to-date content.[^ms-rag-vs-ft]

### 6. Azure fine-tuning considerations
- Microsoft's fine-tuning considerations doc reinforces the production case for tuning when it lets you shorten prompts and reduce latency.[^azure-ft-consider]
- This source is especially useful when the tuning decision is driven by repeated high-volume traffic rather than by one-off experimentation.[^azure-ft-consider]

### 7. Google Vertex AI tuning guidance
- Google's tuning docs are useful because they ground supervised fine-tuning in concrete task types:
  - classification,
  - sentiment analysis,
  - entity extraction,
  - simpler summarization,
  - and domain-specific query writing.[^vertex-tuning]
- They also make the general PEFT case that parameter-efficient methods adapt faster with fewer resources.[^vertex-tuning]

## Durable takeaways

- Do prompt and workflow work first because it is the fastest way to learn what the actual bottleneck is.[^oa-accuracy][^oa-prompt]
- Use retrieval when the main gap is knowledge, freshness, or access to private information.[^oa-accuracy][^oa-prompt][^ms-rag-vs-ft]
- Use fine-tuning when the remaining gap is stable behavior: consistency, format, style, narrow task specialization, or scale-sensitive latency and cost improvement.[^oa-accuracy][^oa-sft][^oa-model-opt][^vertex-tuning]
- Treat data quality and eval quality as gating requirements, not cleanup work after training.[^oa-sft][^oa-ft-best]

## Important nuance

There is a mild framing difference across the sources:

- OpenAI presents prompting, RAG, and fine-tuning as a matrix of levers.[^oa-accuracy]
- Microsoft presents RAG versus fine-tuning as a more direct comparison.[^ms-rag-vs-ft]
- Google emphasizes task types where supervised fine-tuning is appropriate.[^vertex-tuning]

These are not real contradictions. Together they suggest a better synthesis: identify whether your bottleneck is **knowledge**, **behavior**, or **production economics**, then choose the matching lever.

## Caveats and limits

- Vendor docs are strong on product-facing decision rules but weaker on neutral cross-provider benchmarking.
- None of these docs remove the need for application-specific evals.
- The cost case for fine-tuning is real, but it is traffic-dependent. That part is an engineering judgment built on the provider docs, not a universal threshold stated by any one source.[^oa-model-opt][^azure-ft-consider]

## Related notes

- [[Topic - When to Fine-Tune vs When Not To]]
- [[Topic - Adapters, LoRA, and PEFT Tradeoffs]]

[^oa-accuracy]: OpenAI API docs, "Optimizing LLM Accuracy." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/optimizing-llm-accuracy/
[^oa-prompt]: OpenAI API docs, "Prompt engineering." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/prompt-engineering/
[^oa-sft]: OpenAI API docs, "Supervised fine-tuning." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/supervised-fine-tuning/
[^oa-ft-best]: OpenAI API docs, "Fine-tuning best practices." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/fine-tuning-best-practices/
[^oa-model-opt]: OpenAI API docs, "Model optimization." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/model-optimization/
[^ms-rag-vs-ft]: Microsoft Learn, "Augment LLMs with RAGs or Fine-Tuning." Last updated 2026-01-30. Last verified 2026-03-11. https://learn.microsoft.com/en-us/azure/developer/ai/augment-llm-rag-fine-tuning
[^azure-ft-consider]: Microsoft Learn, "Microsoft Foundry fine-tuning considerations." Last updated 2026-02-27. Last verified 2026-03-11. https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/fine-tuning-considerations
[^vertex-tuning]: Google Cloud Vertex AI docs, "Introduction to tuning." Last verified 2026-03-11. https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/tune-models
