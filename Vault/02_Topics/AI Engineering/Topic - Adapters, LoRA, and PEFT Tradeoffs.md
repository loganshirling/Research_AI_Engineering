---
title: Topic - Adapters, LoRA, and PEFT Tradeoffs
type: topic
status: active
topic: Model Customization
course: AI Engineering
tags: [peft, adapters, lora, qlora, serving, model-customization]
prerequisites:
  - "[[Topic - When to Fine-Tune vs When Not To]]"
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
related:
  - "[[Map - AI Engineering]]"
  - "[[Source - Adapters, LoRA, and PEFT - Core Sources]]"
  - "[[Topic - Quantization Tradeoffs for LLM Inference]]"
sources:
  - "https://arxiv.org/abs/1902.00751"
  - "https://arxiv.org/abs/2106.09685"
  - "https://arxiv.org/abs/2305.14314"
  - "https://huggingface.co/docs/peft/conceptual_guides/adapter"
  - "https://huggingface.co/docs/peft/en/developer_guides/lora"
  - "https://huggingface.co/docs/transformers/peft"
  - "https://docs.vllm.ai/en/stable/features/lora/"
  - "https://docs.predibase.com/fine-tuning/adapters"
  - "https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/tune-models"
last_updated: 2026-03-11
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---
# Topic - Adapters, LoRA, and PEFT Tradeoffs

## Purpose

Understand how full supervised fine-tuning, adapters, LoRA, and broader PEFT methods differ so you can pick the cheapest method that still solves the problem.[^houlsby][^lora][^qlora][^hf-adapters]

## Why it matters

Once you decide that customization is justified, the next decision is not just **whether** to tune but **how** to tune. Full fine-tuning, classic adapters, LoRA, and quantized LoRA differ in training cost, storage, inference simplicity, batching behavior, and how easy they are to operate in production.[^houlsby][^lora][^qlora][^hf-lora][^vllm-lora]

## 80/20 summary

- **PEFT** means adapting a model by training a small subset of parameters instead of all of them. It is an umbrella, not one method.[^hf-peft][^hf-adapters]
- **Adapters** add task-specific modules to a frozen base model. They are parameter-efficient and support many downstream tasks, but they change the forward pass unless merged or otherwise optimized.[^houlsby][^hf-adapters]
- **LoRA** is an adapter-style PEFT method that represents weight updates as low-rank matrices. It became the default starting point because it usually trains cheaply and can be merged back into the base model for simpler inference.[^lora][^hf-adapters][^hf-lora]
- **QLoRA** keeps the base model quantized and frozen while training LoRA adapters, which sharply reduces memory requirements. It is mainly a training-efficiency technique, not magic for serving complexity.[^qlora]
- **Full SFT** is still the cleanest deployment story when you want one specialized model and can afford the training cost. PEFT is best when compute is tight, you want many task variants, or you want to keep one shared base model.[^vertex-tuning][^houlsby][^lora]

## Vocabulary that matters

### Full supervised fine-tuning
Train all or most model weights on your task data. Highest training cost, simplest mental model, and one specialized checkpoint at the end.[^vertex-tuning][^houlsby]

### PEFT
Parameter-efficient fine-tuning: a family of methods that update only a small parameter subset while freezing the base model.[^hf-peft][^hf-adapters]

### Adapters
Small trainable modules inserted into a frozen network. Houlsby et al. established the basic case: near full-tuning quality on many NLP tasks with far fewer task-specific parameters.[^houlsby]

### LoRA
A PEFT method that freezes the base model and learns low-rank update matrices for selected weights, usually in attention layers.[^lora][^hf-adapters]

### QLoRA
A memory-efficient training recipe: keep the base model quantized, backpropagate through it, and train LoRA adapters on top.[^qlora]

## SFT vs PEFT

## When full SFT is attractive

Full SFT is attractive when:

- you want a single specialized model artifact,
- you can afford the training compute,
- deployment simplicity matters more than adapter flexibility,
- and the task is important enough that squeezing the last bit of quality is worth more than saving training cost.[^vertex-tuning][^houlsby]

This note is deliberately cautious here: the sources in this bundle make the efficiency case for PEFT very strongly, but they do not claim PEFT always dominates full tuning on every task and every scale. The practical lesson is to treat full SFT as the expensive default and PEFT as the first cheaper alternative worth testing.[^houlsby][^lora][^peft-survey]

## When PEFT is attractive

PEFT is attractive when:

- GPU memory is limited,
- you need multiple task variants on top of one base model,
- experimentation speed matters,
- or you want to store and version many customizations without duplicating full checkpoints.[^houlsby][^lora][^qlora][^hf-adapters][^vertex-tuning]

Google's Vertex AI docs make the operational case directly: parameter-efficient tuning is more resource-efficient and cost-effective than full fine-tuning, adapts faster with smaller datasets, and is flexible for multi-task learning.[^vertex-tuning]

## Adapters vs LoRA

Classic adapters and LoRA solve the same high-level problem: keep the base model frozen and learn small task-specific parameters.[^houlsby][^lora][^hf-adapters]

The difference is mostly where the parameters live and what that means operationally:

- **Adapters:** insert extra modules into the network. This is conceptually simple and supports strong task modularity.[^houlsby][^hf-adapters]
- **LoRA:** express the update as low-rank matrices applied to existing weights. This usually means fewer trainable parameters and an easier path to weight merging for inference.[^lora][^hf-adapters][^hf-lora]

Hugging Face's PEFT docs explicitly present LoRA as a good default starting point for people new to PEFT.[^hf-adapters]

## Why LoRA became the default starting point

LoRA became the default because it combines three advantages:

1. very small trainable parameter counts,
2. quality that is often comparable to full fine-tuning in the cited literature,
3. and a deployment path where the learned update can be merged back into the base model.[^lora][^hf-adapters][^hf-lora]

The merge point matters. The original LoRA paper emphasizes that LoRA avoids the inference-latency penalty associated with classic adapter layers. Hugging Face's current docs add an important implementation nuance: if you keep the base model and LoRA adapter separate at inference time, you can still see latency overhead, and merging removes it.[^lora][^hf-lora]

That is not really a disagreement. It is the difference between the **method** and a particular **serving choice**.

## QLoRA: what it changes and what it does not

QLoRA is mostly about making training feasible on smaller hardware budgets. The paper's headline result is that a 65B model can be fine-tuned on a single 48GB GPU while preserving full 16-bit fine-tuning task performance in their experiments.[^qlora]

What QLoRA does **not** do:

- it does not remove the need for good data,
- it does not remove the need for evals,
- it does not automatically simplify serving,
- and it does not guarantee better quality than a simpler LoRA run on your task.[^qlora]

Treat QLoRA as a resource-efficiency lever for training, not as a substitute for product judgment.[^qlora]

## Deployment and serving tradeoffs

### Merged adapters
Merge LoRA weights into the base model when:

- you want the simplest runtime artifact,
- you do not need to hot-swap tasks,
- and low latency matters more than modularity.[^hf-lora]

This produces the cleanest serving story because the runtime behaves like an ordinary model checkpoint.[^hf-lora]

### Unmerged adapters
Keep adapters separate when:

- you need to swap task behavior dynamically,
- you want many task-specific variants on one base model,
- or you want to combine or compare adapters without rebuilding full checkpoints.[^hf-lora][^vllm-lora]

This is the flexible path, but it pushes complexity into serving.

### Multi-adapter serving
Frameworks now support multi-adapter serving, but the operational cost is real. vLLM can process requests for base and LoRA-adapted models in parallel, and it supports dynamic runtime adapter loading, but its docs explicitly warn that runtime LoRA updating is risky in production.[^vllm-lora]

That warning is important. Serving many adapters is feasible, but it turns customization into a serving-systems problem involving admission control, cache behavior, security, and lifecycle management, not just a model-training problem.[^vllm-lora][^predibase-adapters]

## Cost, storage, and maintenance tradeoffs

### Full SFT
- highest training cost,
- one full model artifact per specialization,
- simplest runtime,
- but expensive if you need many variants.[^houlsby][^vertex-tuning]

### Adapters / LoRA
- low training cost,
- compact task-specific artifacts,
- one base model can support many variants,
- but runtime simplicity depends on whether you merge the adapter.[^houlsby][^lora][^hf-lora]

### QLoRA
- even lower training-memory requirements,
- especially attractive when the base model is large,
- but the deployment choice after training is still merge vs dynamic adapter serving.[^qlora][^hf-lora]

## Common failure modes

1. **Treating PEFT as free.** It is cheaper than full tuning, not free. Data, eval, and operations still dominate many failures.[^qlora][^hf-adapters]
2. **Confusing training efficiency with serving efficiency.** QLoRA helps training memory. It does not by itself solve deployment simplicity.[^qlora]
3. **Keeping adapters dynamic when you really need one stable artifact.** This creates unnecessary runtime complexity.[^hf-lora][^vllm-lora]
4. **Merging too early when the product still needs fast adapter iteration.** This makes experimentation clumsy.[^hf-lora]
5. **Using many adapters without a clear governance model.** Versioning, compatibility, and rollout become real production concerns.[^vllm-lora][^predibase-adapters]

## Practical default

For most teams building one task-specific customization on open-weight models, a sensible default is:

1. validate that customization is worth doing,
2. start with LoRA,
3. move to QLoRA only if training memory is the limiting constraint,
4. merge for deployment if you want one stable production model,
5. keep adapters separate only if multi-task flexibility is genuinely valuable.[^lora][^qlora][^hf-adapters][^hf-lora][^vllm-lora]

## What to read next

- [[Topic - When to Fine-Tune vs When Not To]]
- [[Topic - Quantization Tradeoffs for LLM Inference]]
- [[Source - Adapters, LoRA, and PEFT - Core Sources]]

[^houlsby]: Neil Houlsby et al., "Parameter-Efficient Transfer Learning for NLP." ICML 2019 / arXiv 2019. Last verified 2026-03-11. https://arxiv.org/abs/1902.00751
[^lora]: Edward J. Hu et al., "LoRA: Low-Rank Adaptation of Large Language Models." arXiv 2021. Last verified 2026-03-11. https://arxiv.org/abs/2106.09685
[^qlora]: Tim Dettmers et al., "QLoRA: Efficient Finetuning of Quantized LLMs." arXiv 2023. Last verified 2026-03-11. https://arxiv.org/abs/2305.14314
[^hf-adapters]: Hugging Face PEFT docs, "Adapters." Last verified 2026-03-11. https://huggingface.co/docs/peft/conceptual_guides/adapter
[^hf-lora]: Hugging Face PEFT docs, "LoRA developer guide." Last verified 2026-03-11. https://huggingface.co/docs/peft/en/developer_guides/lora
[^hf-peft]: Hugging Face Transformers docs, "PEFT." Last verified 2026-03-11. https://huggingface.co/docs/transformers/peft
[^vllm-lora]: vLLM docs, "LoRA Adapters." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/features/lora/
[^predibase-adapters]: Predibase docs, "Adapters." Last verified 2026-03-11. https://docs.predibase.com/fine-tuning/adapters
[^vertex-tuning]: Google Cloud Vertex AI docs, "Introduction to tuning." Last verified 2026-03-11. https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/tune-models
[^peft-survey]: Zhenyu Han et al., "Parameter-Efficient Fine-Tuning for Large Models." arXiv 2024. Last verified 2026-03-11. https://arxiv.org/abs/2403.14608
