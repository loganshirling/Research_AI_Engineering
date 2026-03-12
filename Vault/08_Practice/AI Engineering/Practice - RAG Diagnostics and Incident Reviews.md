---
title: Practice - RAG Diagnostics and Incident Reviews
type: practice
status: active
topic: Retrieval Systems
course: AI Engineering
tags: [practice, rag, retrieval, diagnostics, incident-review, grounding]
prerequisites:
  - "[[Topic - Common RAG Failure Modes]]"
  - "[[Topic - Eval Design for Serving Changes]]"
  - "[[Topic - When to Fine-Tune vs When Not To]]"
related:
  - "[[AI Engineering Practice]]"
  - "[[Practice - Prompt, Retrieval, and Tuning Decision Drills]]"
  - "[[Practice - Benchmarking and Failure Analysis Drills]]"
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

# Practice - RAG Diagnostics and Incident Reviews

## Skill target

Learn to diagnose **where** a RAG system is actually failing:
- retrieval recall,
- ranking and relevance,
- chunking and granularity,
- context assembly,
- grounding and citation,
- freshness or authorization,
- generator obedience to evidence,
- or eval blindness.

This note is about incident-quality thinking, not just remembering the list of failure modes.

## Fast triage checklist

Before proposing a fix, ask:

1. Did the system retrieve the right document at all?
2. If yes, was the right passage ranked high enough to reach the prompt?
3. If yes, was the chunk boundary wrong for the question?
4. If yes, did context assembly bury or distort the evidence?
5. If yes, did the generator ignore, over-generalize, or contradict the retrieved text?
6. If yes, is the problem actually freshness, permissions, or source-of-truth drift?
7. What log, artifact, or eval slice would prove which stage failed?

## Common failure families

### 1. Recall failure
The system never retrieves the document or passage that contains the answer.

Typical symptoms:
- correct answer exists in the corpus,
- retrieved results look topically related but miss the decisive document,
- manual search finds the answer quickly but the system does not.

First suspects:
- weak query generation,
- bad metadata filtering,
- embedding mismatch,
- missing documents,
- stale index.

### 2. Ranking failure
The right document exists in the candidate set but is not surfaced high enough.

Typical symptoms:
- answer appears in retrieved results when looking deeper,
- top-1 or top-3 is weak but top-20 contains the answer,
- reranking changes answer quality sharply.

First suspects:
- poor ranker,
- too-small candidate set before reranking,
- query/document wording mismatch,
- ranking objective not aligned with answer usefulness.

### 3. Chunking and granularity failure
The answer is split awkwardly or buried in chunks that are too large, too small, or too repetitive.

Typical symptoms:
- retrieved chunks contain partial evidence but not enough to answer,
- neighboring chunks together would have worked,
- chunks are filled with headers, boilerplate, or repeated context.

First suspects:
- fixed chunking without document-structure awareness,
- overlap too small or too large,
- tables, lists, or policy clauses cut incorrectly.

### 4. Context assembly failure
The system retrieves useful pieces but assembles them into a confusing prompt.

Typical symptoms:
- individually good chunks become weak together,
- prompt contains too much irrelevant context,
- contradictory passages appear without disambiguation,
- answer gets worse as more context is added.

First suspects:
- bad packing order,
- no deduplication,
- weak section labeling,
- overfilling context window.

### 5. Grounding failure
The answer is fluent but not tightly bound to retrieved evidence.

Typical symptoms:
- model cites the right topic but wrong detail,
- answer sounds plausible while contradicting the source,
- citations are present but do not actually support the claim.

First suspects:
- prompt does not force evidence use,
- no citation or quote requirement,
- retrieved evidence is ambiguous,
- generation step is over-abstracting.

### 6. Freshness or authorization failure
The system answers from outdated or inaccessible information.

Typical symptoms:
- answer matches old policy or old catalog state,
- different users see answers from sources they should not access,
- logs show indexing lag or stale caches.

First suspects:
- slow index refresh,
- permission filtering bug,
- source synchronization gap,
- stale document versions.

## Drill set A - locate the failing stage

### Drill 1 - policy answer with wrong clause
A support assistant answers an employee policy question. The cited document is the correct handbook, but the answer quotes the wrong section because the retrieved chunk includes several similar policy clauses.

#### Your task
Name the most likely failure stage and the first design change you would test.

#### Strong answer shape
Call this mainly a chunking or context-assembly failure, not a pure hallucination problem. The first test is better section-aware chunking or passage labeling so the model can isolate the relevant clause.

### Drill 2 - answer exists at rank 12
A medical-support search tool usually fails top-3 retrieval, but manual inspection shows the exact answer sitting around rank 12 for many failed examples.

#### Your task
What stage is failing, and what metric would you track next?

#### Strong answer shape
This is mainly a ranking failure. Track hit rate at larger candidate sizes and reranker lift, not just final answer accuracy.

### Drill 3 - correct documents, wrong answer
The system retrieves the right contract excerpts, but the generator still summarizes them incorrectly and states the opposite obligation.

#### Your task
What is the likely failure stage, and what is one eval slice you should add?

#### Strong answer shape
This is a grounding failure. Add an evidence-faithfulness eval slice that checks whether the answer is entailed by retrieved text, not merely related to it.

### Drill 4 - new product inventory missing
A commerce assistant answers correctly for old products but misses recently added inventory that definitely exists in the data warehouse.

#### Your task
Name the likely incident class and the first log or artifact to inspect.

#### Strong answer shape
Treat this as freshness or synchronization failure. Inspect ingest timestamps, indexing lag, and document-version propagation before changing prompts or tuning anything.

## Drill set B - incident review memos

For each scenario below, write a five-part incident memo:
1. user-visible symptom,
2. likely failure family,
3. artifact or log to inspect next,
4. contained experiment,
5. rollback or mitigation trigger.

### Scenario 1
A legal assistant starts citing superseded internal guidance after a document migration.

### Scenario 2
A RAG chatbot improves after adding more retrieved chunks, then degrades again when the team doubles context size.

### Scenario 3
A support system works for English documents but fails when answers are inside tables in PDFs.

### Scenario 4
A search pipeline passes offline relevance tests but user satisfaction drops after rollout because answers feel less specific.

## Drill set C - choose the next fix, not the biggest fix

### Exercise 1
Pick one of these interventions and justify why it should come first:
- change chunking,
- add reranking,
- rewrite the grounding prompt,
- shrink context packing,
- improve freshness pipeline,
- or add evidence-faithfulness evals.

Your answer must include:
- what symptom points to that intervention,
- what cheaper alternative you rejected,
- and what result would falsify your hypothesis.

### Exercise 2
Write a “do not tune yet” memo for a RAG failure that looks tempting to solve with fine-tuning.

A strong answer names the real issue as retrieval, chunking, freshness, permissions, or grounding rather than generic “model weakness.”

## Self-grading rubric

### 1. Stage identification
Did you identify the failing stage of the pipeline rather than using “RAG is bad” as a catch-all?

### 2. Evidence discipline
Did you name the log, trace, retrieved set, or eval slice that would validate your claim?

### 3. Smallest useful intervention
Did you choose the least invasive plausible fix before proposing a redesign?

### 4. Incident realism
Did you consider stale indexes, permissions, migrations, and prompt-packing issues rather than only model quality?

### 5. Tuning restraint
Did you avoid recommending fine-tuning when the evidence still points to a retrieval-system failure?

## Common failure modes

1. Treating every wrong answer as hallucination.
2. Treating top-k tuning as a universal cure.
3. Looking only at final answer quality and not at retrieved artifacts.
4. Ignoring permissions and freshness.
5. Calling a ranking issue a chunking issue without checking candidate sets.
6. Recommending fine-tuning before proving the corpus and retrieval path are working.

## Reflection

After each drill, write:
- what artifact would most quickly confirm your diagnosis,
- what nearby failure family you almost confused it with,
- and what rollback-safe experiment you would run next.

## Next drill

- [[Practice - Benchmarking and Failure Analysis Drills]]
