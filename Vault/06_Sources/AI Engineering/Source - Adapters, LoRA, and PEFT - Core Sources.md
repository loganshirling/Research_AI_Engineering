---
title: Source - Adapters, LoRA, and PEFT - Core Sources
type: source
status: active
topic: Model Customization
course: AI Engineering
tags: [source, peft, adapters, lora, qlora]
related:
  - "[[Topic - Adapters, LoRA, and PEFT Tradeoffs]]"
  - "[[Topic - When to Fine-Tune vs When Not To]]"
sources:
  - "https://arxiv.org/abs/1902.00751"
  - "https://arxiv.org/abs/2106.09685"
  - "https://arxiv.org/abs/2305.14314"
  - "https://arxiv.org/abs/2403.14608"
  - "https://huggingface.co/docs/peft/conceptual_guides/adapter"
  - "https://huggingface.co/docs/peft/en/developer_guides/lora"
  - "https://huggingface.co/docs/transformers/peft"
  - "https://docs.vllm.ai/en/stable/features/lora/"
  - "https://docs.predibase.com/fine-tuning/adapters"
  - "https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/tune-models"
last_updated: 2026-03-11
review_status: researched
priority: high
---
# Source - Adapters, LoRA, and PEFT - Core Sources

## Why this source bundle matters

This bundle groups the highest-signal sources for understanding the PEFT family most relevant in practice: classic adapters, LoRA, and QLoRA, plus the framework docs that determine whether those methods are pleasant or painful to operate.

## Core source set

### 1. Houlsby et al. 2019: classic adapters
- Houlsby et al. is the foundational adapter paper for NLP.[^houlsby]
- Its central idea is still important: instead of retraining a full copy of the model for every task, insert compact task-specific modules and keep the base weights fixed.[^houlsby]
- This source is strongest for the argument that parameter efficiency matters most when many downstream tasks share one base model.[^houlsby]

### 2. LoRA paper
- Hu et al. 2021 is the key source for LoRA's design: freeze the base model and learn low-rank update matrices in selected weights.[^lora]
- The paper is also the strongest early source for why LoRA displaced many older adapter choices:
  - very small trainable parameter counts,
  - strong reported quality,
  - higher training throughput,
  - and a path to avoiding additional inference latency.[^lora]

### 3. QLoRA paper
- Dettmers et al. 2023 is the anchor source for memory-efficient fine-tuning on large models.[^qlora]
- The important idea is not just “LoRA but better.” It is specifically:
  - keep the base model frozen and quantized,
  - backprop through it,
  - train LoRA adapters,
  - and use quantization and optimizer tricks to fit larger runs into much smaller GPU memory budgets.[^qlora]

### 4. Hugging Face PEFT docs
- Hugging Face's adapter overview is the best current practical summary of PEFT families and their tradeoffs.[^hf-adapters]
- The Transformers PEFT page is useful for the umbrella definition that PEFT is a family, not one algorithm, and for seeing what the mainstream tooling supports today.[^hf-peft]
- The LoRA developer guide is especially valuable for deployment details:
  - merging,
  - unmerging,
  - mixing adapters,
  - and the fact that unmerged LoRA can introduce inference latency that merging removes.[^hf-lora]

### 5. vLLM LoRA serving docs
- vLLM is the best source in this bundle for the runtime story.[^vllm-lora]
- It shows that multi-adapter serving is practical, base and LoRA requests can run in parallel, and dynamic loading exists.
- It also provides an important production warning: runtime LoRA updating is risky.[^vllm-lora]

### 6. Predibase adapter docs
- Predibase is useful as a model-provider perspective on adapters as a product surface, not just a research idea.[^predibase-adapters]
- Their docs reinforce that adapters are attractive because you can version and manage many task-specific customizations on top of one base model.[^predibase-adapters]

### 7. Vertex AI tuning docs and PEFT survey
- Google's Vertex docs are useful for the broad engineering statement that parameter-efficient tuning is more resource-efficient, faster to adapt, and better suited to multi-task settings than full tuning.[^vertex-tuning]
- The 2024 survey is not a primary source for any one method, but it is helpful as a synthesis reference when you need broader PEFT taxonomy.[^peft-survey]

## Durable takeaways

- PEFT is the umbrella; adapters, LoRA, and related methods are specific implementations inside that umbrella.[^hf-peft][^hf-adapters]
- Classic adapters established the parameter-sharing case, while LoRA became the practical default because of its efficiency and merge-friendly deployment path.[^houlsby][^lora][^hf-lora]
- QLoRA is mainly a training-memory breakthrough, not a serving simplification breakthrough.[^qlora]
- The real deployment choice is often **merged single artifact** versus **dynamic multi-adapter system**.[^hf-lora][^vllm-lora]
- Multi-adapter serving is possible, but it is an operations problem as much as a modeling problem.[^vllm-lora][^predibase-adapters]

## Important nuance

There is a subtle but important tension between sources:

- the LoRA paper emphasizes no additional inference latency relative to the base model,[^lora]
- current framework docs warn that separately loading LoRA adapters can add latency and recommend merging to eliminate it.[^hf-lora]

The best reconciliation is:
- method-level LoRA can be latency-neutral,
- implementation-level LoRA can still be slower if you choose a modular runtime path.

That distinction is worth preserving in teaching notes.

## Caveats and limits

- Research papers are strongest on mechanism and experimental outcomes, not day-two operations.
- Framework docs are strongest on what is easy to do today, not necessarily on neutral model-quality comparisons.
- The exact balance between full SFT and PEFT can differ by architecture, task, and scale, so broad "always use LoRA" claims should be treated with caution.[^peft-survey]

## Related notes

- [[Topic - Adapters, LoRA, and PEFT Tradeoffs]]
- [[Topic - When to Fine-Tune vs When Not To]]

[^houlsby]: Neil Houlsby et al., "Parameter-Efficient Transfer Learning for NLP." ICML 2019 / arXiv 2019. Last verified 2026-03-11. https://arxiv.org/abs/1902.00751
[^lora]: Edward J. Hu et al., "LoRA: Low-Rank Adaptation of Large Language Models." arXiv 2021. Last verified 2026-03-11. https://arxiv.org/abs/2106.09685
[^qlora]: Tim Dettmers et al., "QLoRA: Efficient Finetuning of Quantized LLMs." arXiv 2023. Last verified 2026-03-11. https://arxiv.org/abs/2305.14314
[^peft-survey]: Zhenyu Han et al., "Parameter-Efficient Fine-Tuning for Large Models." arXiv 2024. Last verified 2026-03-11. https://arxiv.org/abs/2403.14608
[^hf-adapters]: Hugging Face PEFT docs, "Adapters." Last verified 2026-03-11. https://huggingface.co/docs/peft/conceptual_guides/adapter
[^hf-lora]: Hugging Face PEFT docs, "LoRA developer guide." Last verified 2026-03-11. https://huggingface.co/docs/peft/en/developer_guides/lora
[^hf-peft]: Hugging Face Transformers docs, "PEFT." Last verified 2026-03-11. https://huggingface.co/docs/transformers/peft
[^vllm-lora]: vLLM docs, "LoRA Adapters." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/features/lora/
[^predibase-adapters]: Predibase docs, "Adapters." Last verified 2026-03-11. https://docs.predibase.com/fine-tuning/adapters
[^vertex-tuning]: Google Cloud Vertex AI docs, "Introduction to tuning." Last verified 2026-03-11. https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/tune-models
