---
title: Topic - When to Fine-Tune vs When Not To
type: topic
status: active
topic: Model Customization
course: AI Engineering
tags: [fine-tuning, prompting, rag, evals, model-customization]
prerequisites:
  - "[[Topic - Eval Design for Serving Changes]]"
  - "[[Topic - Common RAG Failure Modes]]"
related:
  - "[[Map - AI Engineering]]"
  - "[[Topic - Adapters, LoRA, and PEFT Tradeoffs]]"
  - "[[Source - Fine-Tuning Decision Framework - Core Sources]]"
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
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---
# Topic - When to Fine-Tune vs When Not To

## Purpose

Build a durable decision framework for choosing between prompting, retrieval, and fine-tuning so you do not use training to cover up a system-design problem.[^oa-accuracy][^ms-rag-vs-ft]

## Why it matters

Teams often reach for fine-tuning too early because it feels like the most powerful lever. In practice, prompting, retrieval, and fine-tuning solve different failure modes. Prompting and workflow design shape how the model behaves. Retrieval fixes missing, stale, or proprietary knowledge. Fine-tuning is best when the behavior itself needs to become more consistent, more specialized, or cheaper to serve at scale.[^oa-accuracy][^oa-prompt][^oa-model-opt][^ms-rag-vs-ft][^azure-ft-consider]

## 80/20 summary

- Start with a strong baseline prompt, realistic examples, and evals. Fine-tuning without a baseline and a regression set is mostly guesswork.[^oa-accuracy][^oa-sft]
- Use prompting alone when the task can be steered with clear instructions, output constraints, tool use, and a handful of in-context examples.[^oa-prompt][^oa-accuracy]
- Use RAG when the model needs facts that are missing from pretraining, out of date, or private to your organization.[^oa-accuracy][^oa-prompt][^ms-rag-vs-ft]
- Use fine-tuning when the knowledge is not the main problem and the real need is stable behavior: format compliance, style, task specialization, or correcting repeat instruction-following failures.[^oa-accuracy][^oa-sft][^oa-model-opt][^vertex-tuning]
- Do not fine-tune until data quality is good enough to define what “good” looks like and your evals can detect whether the tuned model is actually better.[^oa-sft][^oa-ft-best]

## The right order of operations

1. **Define the task and failure taxonomy.** Know what counts as success, what kinds of failures matter, and what error rate is acceptable.[^oa-accuracy]
2. **Build the baseline with prompt and workflow design.** Start with clear instructions, output constraints, examples, reference text, and tools where needed.[^oa-accuracy][^oa-prompt]
3. **Add retrieval if the failure is missing or changing knowledge.** Retrieval is the right lever when the model needs access to facts outside its weights.[^oa-accuracy][^oa-prompt][^ms-rag-vs-ft]
4. **Fine-tune only after the remaining failures are mostly behavioral.** This is where supervised fine-tuning helps most.[^oa-accuracy][^oa-sft][^vertex-tuning]

OpenAI's optimization guide is useful here because it explicitly rejects the simplistic story that optimization is always a straight line from prompt engineering to RAG to fine-tuning. The better mental model is to identify which lever matches the bottleneck.[^oa-accuracy]

## When prompting alone is enough

Prompting is usually enough when:

- the task is already within the model's capability,
- the desired behavior can be taught with instructions and a small number of examples,
- the output can be constrained with schemas, rubrics, or validators,
- tool use or workflow changes matter more than changed model weights,
- and the prompt overhead is acceptable for your latency and cost budget.[^oa-prompt][^oa-accuracy]

A practical tell is that few-shot examples materially improve performance. OpenAI's prompt guide explicitly frames few-shot learning as a way to steer the model toward a new task without fine-tuning.[^oa-prompt]

## When RAG is enough

RAG is usually enough when:

- the model is failing because it does not know the right facts,
- the facts change often,
- the data is proprietary,
- provenance matters and you want answer generation tied to retrievable evidence,
- or different users need access to different slices of knowledge without creating separate tuned models.[^oa-accuracy][^oa-prompt][^ms-rag-vs-ft]

This is the default for internal knowledge bases, fast-changing documentation, policies, catalogs, and other domains where keeping facts outside model weights is a feature, not a compromise.[^oa-accuracy][^ms-rag-vs-ft]

## When fine-tuning is justified

Fine-tuning is justified when the remaining gap is **behavioral rather than informational** and the desired behavior is stable enough to teach through examples.[^oa-accuracy][^oa-sft][^vertex-tuning]

Common good reasons:

- the model repeatedly misses a target format or schema,
- the model needs a particular tone or response style,
- the task is narrow and well-defined, such as classification, extraction, or domain-specific query writing,
- the base model can do the task sometimes, but not consistently enough,
- or the business case depends on shorter prompts, lower per-request token costs, or lower latency at meaningful scale.[^oa-accuracy][^oa-sft][^oa-model-opt][^vertex-tuning][^azure-ft-consider]

Google's Vertex AI docs make this especially concrete: supervised fine-tuning is recommended for relatively easy-to-define tasks such as classification, sentiment analysis, entity extraction, simpler summarization, and domain-specific query writing.[^vertex-tuning]

## When fine-tuning is the wrong move

Fine-tuning is usually the wrong move when:

- the real problem is missing, stale, or private knowledge that should be retrieved at runtime,[^oa-accuracy][^ms-rag-vs-ft]
- you do not yet have reliable evals and therefore cannot tell whether the tuned model is better,[^oa-sft]
- your examples are inconsistent, incomplete, or poorly aligned with real production inputs,[^oa-ft-best]
- the workflow is still changing rapidly, so the target behavior is not stable enough to encode in training data,[^oa-ft-best]
- or the only motivation is a vague hope that training will make a generally messy system “smarter.”[^oa-accuracy]

A useful operational inference is that fine-tuning for cost or latency alone is rarely a first move unless request volume is high enough for shorter prompts to repay training and maintenance overhead. The official docs support the upside of shorter prompts, but that upside only matters if you serve enough traffic for it to compound.[^oa-model-opt][^azure-ft-consider]

## Data and eval requirements before tuning

Before tuning, you want all of the following:

- a representative task set,
- a baseline prompt or workflow worth comparing against,
- a regression set that catches important failure modes,
- training examples that look like real inference inputs and outputs,
- and enough agreement in the data that the model can learn one coherent target behavior.[^oa-sft][^oa-ft-best]

OpenAI's supervised fine-tuning guide recommends starting with about 50 well-crafted demonstrations and evaluating before scaling up. The same guide notes that improvements can show up in the 50 to 100 example range, but whether they do depends on the use case.[^oa-sft]

The most important data-quality rules are simple:

- target remaining errors directly,
- remove contradictory or low-quality examples,
- match training format to inference format,
- include all information needed for the desired answer,
- and watch distribution balance so the tuned model does not overlearn rare behaviors such as excessive refusals.[^oa-ft-best]

## Cost, latency, deployment, and maintenance tradeoffs

### Prompting
- **Pros:** fastest to iterate, lowest governance burden, easy to inspect, and easy to combine with tools or validators.[^oa-prompt][^oa-accuracy]
- **Cons:** longer prompts can raise token cost and latency, and in-context examples do not always generalize consistently.[^oa-prompt][^oa-model-opt]

### RAG
- **Pros:** keeps knowledge external, easier to update, supports provenance, and works well when facts change or must remain private.[^oa-accuracy][^ms-rag-vs-ft]
- **Cons:** adds retrieval latency, indexing and ranking complexity, and its own failure modes around chunking, relevance, and grounding.[^ms-rag-vs-ft]

### Fine-tuning
- **Pros:** can make behavior more consistent, reduce prompt length, lower per-request token usage, and improve latency at scale.[^oa-model-opt][^azure-ft-consider]
- **Cons:** requires dataset creation, training runs, versioning, re-evaluation, and periodic retuning as the product, data, or base models change.[^oa-sft][^oa-ft-best]

The durable lesson is that fine-tuning moves complexity from each request into the model lifecycle. That is sometimes exactly what you want, but it is still complexity.[^oa-sft][^oa-ft-best]

## Common fine-tuning failure modes

1. **Tuning before baseline prompt work is done.** This wastes time because the examples often encode mistakes that better instructions would have removed.[^oa-accuracy][^oa-prompt]
2. **Using fine-tuning to store changing knowledge.** That creates stale weights instead of a maintainable retrieval layer.[^oa-accuracy][^ms-rag-vs-ft]
3. **Training on inconsistent examples.** The model usually converges toward the average inconsistency in the dataset.[^oa-ft-best]
4. **Training on unrealistic examples.** If training data does not resemble production inputs, the tuned model often looks great offline and weak in production.[^oa-sft][^oa-ft-best]
5. **Skipping post-tuning evals.** A fine-tuned model can look more polished while still being worse on the actual task.[^oa-sft]
6. **Ignoring maintenance cost.** Every new base model, workflow change, or policy change can trigger a retuning question.[^oa-model-opt][^oa-ft-best]

## Decision rule of thumb

Ask these questions in order:

1. **Can a better prompt, clearer workflow, or few-shot examples fix this?**
2. **Is the problem really missing or changing knowledge?**
3. **Do we have a stable target behavior and high-quality examples?**
4. **Do we have evals strong enough to tell if tuning helped?**
5. **Will shorter prompts or more consistent behavior matter enough in production to justify lifecycle overhead?**

If the answers are **yes, yes, yes** to questions 3 through 5, then fine-tuning is likely justified. If not, keep working on prompting, retrieval, or the application workflow first.[^oa-accuracy][^oa-sft][^oa-model-opt]

## What to read next

- [[Topic - Adapters, LoRA, and PEFT Tradeoffs]]
- [[Topic - Common RAG Failure Modes]]
- [[Topic - Eval Design for Serving Changes]]
- [[Source - Fine-Tuning Decision Framework - Core Sources]]

[^oa-accuracy]: OpenAI API docs, "Optimizing LLM Accuracy." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/optimizing-llm-accuracy/
[^oa-prompt]: OpenAI API docs, "Prompt engineering." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/prompt-engineering/
[^oa-sft]: OpenAI API docs, "Supervised fine-tuning." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/supervised-fine-tuning/
[^oa-ft-best]: OpenAI API docs, "Fine-tuning best practices." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/fine-tuning-best-practices/
[^oa-model-opt]: OpenAI API docs, "Model optimization." Last verified 2026-03-11. https://developers.openai.com/api/docs/guides/model-optimization/
[^ms-rag-vs-ft]: Microsoft Learn, "Augment LLMs with RAGs or Fine-Tuning." Last updated 2026-01-30. Last verified 2026-03-11. https://learn.microsoft.com/en-us/azure/developer/ai/augment-llm-rag-fine-tuning
[^azure-ft-consider]: Microsoft Learn, "Microsoft Foundry fine-tuning considerations." Last updated 2026-02-27. Last verified 2026-03-11. https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/fine-tuning-considerations
[^vertex-tuning]: Google Cloud Vertex AI docs, "Introduction to tuning." Last verified 2026-03-11. https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/tune-models
