---
title: Practice - Prompt, Retrieval, and Tuning Decision Drills
type: practice
status: active
topic: Model Customization
course: AI Engineering
tags: [practice, prompting, rag, fine-tuning, evals, decision-making]
prerequisites:
  - "[[Topic - When to Fine-Tune vs When Not To]]"
  - "[[Topic - Adapters, LoRA, and PEFT Tradeoffs]]"
  - "[[Topic - Common RAG Failure Modes]]"
  - "[[Topic - Eval Design for Serving Changes]]"
related:
  - "[[AI Engineering Practice]]"
  - "[[Practice - Benchmarking and Failure Analysis Drills]]"
  - "[[Practice - RAG Diagnostics and Incident Reviews]]"
sources: []
last_updated: 2026-03-11
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Practice - Prompt, Retrieval, and Tuning Decision Drills

## Skill target

Learn to identify whether a failure is mainly:
- a prompt or workflow problem,
- a retrieval and grounding problem,
- a model-customization problem,
- or an eval-definition problem.

This note is about decision quality. The right answer is usually the next best experiment, not a grand redesign.

## Core decision checklist

Before answering any drill, ask:

1. What is the user-visible failure?
2. Is the missing piece knowledge, behavior, or system wiring?
3. Could a better prompt, schema, tool call, or workflow remove most of the error?
4. Is the needed information external, fresh, private, or user-specific?
5. Do we have a stable target behavior and enough examples to justify tuning?
6. What eval would tell us whether the chosen lever actually helped?

## Fast heuristics

### Usually prompt or workflow first
Choose prompt or workflow changes first when:
- the task already looks feasible for the base model,
- the model sometimes succeeds with better instructions,
- output format problems dominate,
- or tool usage and control flow look underspecified.

### Usually retrieval first
Choose retrieval first when:
- the model is missing facts,
- the facts change often,
- provenance matters,
- users need access to different slices of knowledge,
- or answers sound fluent but ungrounded.

### Usually tuning first
Choose tuning only after the basics are working when:
- the remaining gap is behavioral and stable,
- the model needs more consistent style or format obedience,
- prompt length is becoming a real cost or latency problem,
- or narrow task performance matters more than broad flexibility.

### Usually eval work first
Choose eval work first when:
- teams disagree about whether the system is actually better,
- success is defined vaguely,
- only anecdotal wins exist,
- or every proposed fix changes multiple things at once.

## Drill set A - classify the real bottleneck

### Drill 1 - support bot policy answers
A support bot answers return-policy questions well when the policy text is included in the prompt, but makes up details when the policy changes and the prompt is shortened for cost reasons.

#### Your task
Choose the primary next lever and explain why:
- prompt,
- retrieval,
- tuning,
- or eval.

#### Strong answer shape
A strong answer identifies this as mainly a retrieval problem, not a tuning problem. The model needs access to current policy text at runtime. Prompt cleanup may still matter, but the high-value fix is grounding the answer in retrievable policy documents.

#### Common mistake
Saying “fine-tune the model on company policy” without addressing policy churn, provenance, and update latency.

### Drill 2 - extraction JSON is unstable
A model can read invoices correctly but often returns malformed JSON. When given a schema reminder and two examples, performance improves sharply.

#### Your task
Pick the best next step.

#### Strong answer shape
Prompt and workflow first. The key signal is that better instructions and examples materially improve behavior. This suggests the task is already within capability and does not yet justify tuning.

#### Common mistake
Jumping to LoRA because the output is “inconsistent,” without testing schema constraints, validation, or repair flow.

### Drill 3 - medical summarization tone
A team wants summaries written in a very specific clinical style. The base model understands the content but drifts in tone and structure even with a long prompt.

#### Your task
Choose the best primary lever and the gating requirement before using it.

#### Strong answer shape
This is a candidate for fine-tuning after establishing a strong baseline and eval set. The gap is stable behavioral consistency, not missing knowledge. The gating requirement is a clear rubric and representative examples of the desired output style.

#### Common mistake
Calling it a retrieval problem because the domain is medical, even though the failure described is style consistency.

### Drill 4 - agent fails across many tools
An internal agent fails complex tasks, but logs show it often picks the wrong tool, retries unnecessarily, and passes incomplete arguments.

#### Your task
Name the likely first lever and one eval to add.

#### Strong answer shape
Workflow and tool-use prompt design first. Add an eval that checks tool-selection correctness and argument completeness. This is not obviously a model-customization problem yet.

#### Common mistake
Treating every multi-step failure as “the model is weak” instead of checking orchestration.

## Drill set B - choose the next experiment

### Drill 5 - short prompt vs tuned model
A team wants to replace a 2,500-token prompt with a tuned model because latency is high.

#### Your task
What must be true before that is a good decision?

#### Success criteria
A strong answer says all of the following:
- the long prompt must actually be carrying stable behavior rather than changing knowledge,
- the team needs a baseline and regression eval,
- the traffic volume must be high enough that prompt reduction matters economically,
- and the tuned behavior must remain useful as the workflow evolves.

### Drill 6 - stale product catalog
A commerce assistant gives great answers on old catalog items but misses new inventory and current prices.

#### Your task
What is the right first move? What is the wrong move?

#### Success criteria
Right first move: retrieval and grounding from current catalog data.
Wrong move: fine-tuning the model on a snapshot of catalog content and treating that as a durable fix.

#### Drill 7 - classification edge cases
A moderation classifier works on common cases but fails on nuanced in-domain edge cases. The label policy is stable and the team has hundreds of reviewed examples.

#### Your task
What makes this a plausible tuning case?

#### Success criteria
The answer should mention:
- narrow, stable task,
- reviewed examples,
- repeatable decision rule,
- and the need for a held-out regression set before training.

## Drill set C - answer with a decision memo

For each scenario below, write a five-part memo:
1. problem statement,
2. likely bottleneck,
3. best next lever,
4. eval to run next,
5. fallback if that lever fails.

### Scenario 1
A legal assistant gives well-structured answers but sometimes cites the wrong internal policy version.

### Scenario 2
A summarization pipeline is accurate but too slow because the prompt carries many style examples.

### Scenario 3
A helpdesk agent fails mostly on cases where users provide messy inputs and incomplete context.

## Rubric for self-grading

Grade each answer from 1 to 5 on:

### 1. Bottleneck identification
Cid you identify whether the main issue was knowledge, behavior, workflow, or eval?

### 2. Lever choice
Did you choose the smallest lever that could plausibly solve the problem?

### 3. Eval quality
Did you specify what measurement would confirm the decision?

### 4. Operational realism
Did you account for maintenance, freshness, latency, and rollback safety?

### 5. Restraint
Did you avoid proposing tuning before proving it was justified?

## Common failure modes

1. Treating fine-tuning as the default “ serious” solution.
2. Treating RAG as a universal cure when the real problem is output behavior.
3. Confusing changing knowledge with stable behavior.
4. Proposing a fix without naming the eval.
5. Using one anecdote as proof that a lever works.
6. Ignoring product economics when recommending shorter prompts or tuned models.

## Reflection

After each drill, write:
- what signal pointed to the right lever,
- what tempting but wrong alternative you rejected,
- and what experiment would change your mind.

## Next drill

- [[Practice - Benchmarking and Failure Analysis Drills]]
