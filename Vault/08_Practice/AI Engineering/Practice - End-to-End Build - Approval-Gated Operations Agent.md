---
title: Practice - End-to-End Build - Approval-Gated Operations Agent
type: practice
status: active
topic: AI Engineering
course: AI Engineering
tags: [build, capstone, agent, approvals, operations, control-flows]
prerequisites:
  - "[[Topic - AI Agents: Design, Boundaries, and Operations]]"
  - "[[Technique - Agent Approval Design]]"
  - "[[Technique - Eval Operations]]"
related:
  - "[[Topic - End-to-End AI System Case Studies and Build Patterns]]"
  - "[[Topic - OpenClaw and Self-Hosted Personal Agents]]"
sources: []
last_updated: 2026-03-12
review_status: researched
priority: high
---

# Practice - End-to-End Build - Approval-Gated Operations Agent

## Purpose

Use this note as the third capstone-style build doc in the AI Engineering build track.

This build is for systems that move beyond research and review into **bounded action**:
- updating records,
- drafting or sending messages,
- triggering workflows,
- or proposing operations steps that may require approval.

The goal is not maximal autonomy.
The goal is controlled execution with visible permissions, approvals, traces, and rollback paths.

## Product goal

Build an approval-gated operations agent that can:
- read tickets, notes, or requests,
- propose the next operational action,
- gather the required evidence,
- draft the exact action payload,
- request approval when blast radius is meaningful,
- execute only after approval and policy checks,
- and log the full path from request to outcome.

## Dominant failure cost

The most important failure is **performing or preparing the wrong side effect with too much confidence or too little control**.

An operations agent can be slower and still be useful.
It cannot be casually destructive, ambiguous about what it did, or impossible to audit after the fact.

Secondary failure costs:
- vague approval prompts,
- missing authorization checks,
- repeated ineffective tool loops,
- poor escalation behavior,
- and missing logs for what changed.

## Minimum viable architecture

1. **Request intake**
- classify the request,
- determine whether this is read-only, draft-only, or action-taking work,
- identify the target systems and risk level.

2. **Planning step**
- produce a short structured plan:
  - goal,
  - target resource,
  - proposed action,
  - required tools,
  - approval requirement,
  - rollback note.

3. **Evidence gathering**
- retrieve the ticket, record, or supporting context,
- verify current state before any side effect,
- collect the exact identifiers and parameters needed for execution.

4. **Approval checkpoint**
- present the exact intended action,
- the exact target,
- the important parameters,
- the expected outcome,
- and the request ID / payload binding.

5. **Execution step**
- re-check authorization and current state,
- execute the bounded action,
- capture result and final status.

6. **Post-action logging**
- request ID,
- approvals,
- tool calls,
- before/after state if available,
- failure category if execution was blocked or denied.

## What should stay deterministic

- permission checks,
- approval gating,
- request and payload IDs,
- retry limits,
- timeout rules,
- rollback triggers,
- and execution policy.

## Where the model gets judgment

- summarizing the request,
- proposing the plan,
- determining whether more evidence is needed,
- drafting user-facing or operator-facing language,
- and selecting among allowed next steps inside the harness.

## Tool and permission boundaries

Use narrowly scoped tools for:
- reading tickets and records,
- drafting updates,
- staging changes,
- and executing bounded operations.

Keep the first version away from:
- broad shell access,
- unconstrained database writes,
- multi-system cascading actions,
- or any “trust me” tool with vague parameters.

## Approval design requirements

Every approval screen should show:
- exact action type,
- exact target,
- meaningful parameters,
- whether the action is reversible,
- request ID,
- and execution window.

Approvals should be:
- short-lived,
- bound to a specific payload,
- logged,
- and re-checked at execution time.

## Eval plan

Gold sets should include:
- harmless read-only cases,
- draft-only cases,
- correct approval-required cases,
- denied-approval cases,
- stale-state cases where execution should stop,
- and cases where the right answer is escalation instead of action.

Metrics:
- approval-routing correctness,
- action correctness,
- payload correctness,
- execution success / safe refusal rate,
- trace completeness,
- and regression on known risky workflows.

## Rollback plan

Restrict or roll back if:
- action logs become incomplete,
- approval-routing accuracy drops,
- the system executes stale or mismatched payloads,
- operator trust declines because actions are hard to inspect,
- or incidents show repeated tool or permission misuse.

## What would make you redesign

- if users mainly need explanation and retrieval, not side effects,
- if the action space is too broad for narrow tools,
- if approvals are so frequent that the product becomes unusable,
- or if policy and auth logic are still too immature to support safe execution.

## Recommended next step

Write a one-page architecture memo for this build with:
- action taxonomy,
- risk tiers,
- approval boundaries,
- tool contracts,
- eval suite,
- logging model,
- and rollback triggers.
