---
title: Topic - Advanced Systems, Agents, Multimodal, and Voice
type: topic
status: active
topic: Advanced AI Systems
course: AI Engineering
tags: [advanced-systems, agents, multimodal, voice, serving, evals]
prerequisites:
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
  - "[[Topic - Tool Calling, Structured Outputs, and Agent Loops]]"
  - "[[Topic - Retrieval System Design for RAG]]"
  - "[[Topic - LLM Observability and Tracing]]"
related:
  - "[[Topic - Latency Engineering for LLM Systems]]"
  - "[[Topic - Guardrails, Prompt Injection, and Output Validation]]"
  - "[[Topic - Eval Design for Serving Changes]]"
  - "[[Practice - Advanced Systems Architecture Drills]]"
  - "[[Source - Advanced Systems Expansion - Core Sources]]"
sources:
  - "https://arxiv.org/abs/2309.06180"
  - "https://docs.vllm.ai/en/latest/"
  - "https://developers.openai.com/api/docs/guides/prompt-caching/"
  - "https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents"
  - "https://arxiv.org/abs/2307.03172"
  - "https://www.anthropic.com/research/building-effective-agents"
  - "https://developers.openai.com/api/docs/guides/agents-sdk/"
  - "https://google.github.io/adk-docs/agents/workflow-agents/"
  - "https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices"
  - "https://www.anthropic.com/engineering/writing-tools-for-agents"
  - "https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html"
  - "https://developers.openai.com/api/docs/guides/evaluation-best-practices/"
  - "https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents"
  - "https://developers.openai.com/api/docs/guides/file-inputs/"
  - "https://developers.openai.com/api/docs/guides/tools-file-search/"
  - "https://developers.openai.com/api/docs/guides/voice-agents/"
  - "https://developers.openai.com/api/docs/guides/realtime/"
  - "https://www.anthropic.com/engineering/multi-agent-research-system"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: hard
must_know: true
---

# Topic - Advanced Systems, Agents, Multimodal, and Voice

## Purpose

This note adds the advanced systems layer that the course was missing: real serving stacks, context engineering, orchestration, tool security, eval operations, multimodal documents, voice, and end-to-end build patterns.[^1][^3][^6][^12]

## 80/20 summary

Advanced AI engineering is mostly about controlling **state, tools, and failure modes**.

The key questions are:
1. what context should the model see,
2. what tools should it be allowed to use,
3. what should stay deterministic,
4. how will quality be measured,
5. and what happens when the system is wrong, slow, or unsafe.

## The eight next topics

### 1. vLLM, PagedAttention, prefix caching, and continuous batching

Serving is not just "run the model faster." It is a problem of KV-cache pressure, scheduler policy, queueing, and prompt reuse. PagedAttention matters because block-based cache management reduces waste, while continuous batching and prefix caching raise throughput and lower repeated prefill work in real traffic.[^1][^2]

The practical lesson is to diagnose whether latency is coming from queueing, prefill, decode bandwidth, or cache pressure before scaling hardware.

### 2. Context engineering, prompt caching, and long-context tradeoffs

Long context changes cost and behavior, but it does not remove the need for curation. Context engineering includes memory policy, trimming, compression, and evidence ordering, not just prompt writing.[^3][^4]

"Lost in the Middle" is the clearest reminder that long context still suffers from placement problems, so retrieval and arrangement still matter.[^5]

### 3. Agent orchestration, handoffs, and deterministic workflow design

The best agent systems are usually simple. Start with one agent plus tools. Add handoffs only when a real specialization boundary appears. Use deterministic workflow controllers when the sequence should not be left to model judgment.[^6][^7][^8]

The main lesson is that "agentic" should not mean the model decides every transition.

### 4. MCP, tool design, and secure agent execution

Once agents can call tools, the system can take actions, not just generate text. That makes tool descriptions, permissions, approvals, and provenance control first-class engineering concerns.[^9][^10][^11]

Treat MCP as an interoperability layer, not a security boundary by itself.

### 5. Eval flywheels, human review, and regression gates

Without evals, teams operate on anecdotes and reintroduce solved failures. A strong eval flywheel turns incidents into regression tests, keeps humans in the loop where judgment matters, and blocks risky releases.[^12][^13]

The important shift is from one-off debugging to cumulative learning.

### 6. Multimodal document and image pipelines

Many valuable systems work on PDFs, scans, screenshots, charts, and mixed-layout pages. Those are not just text-retrieval problems. Strong designs separate page understanding, corpus search, structured extraction, and source grounding.[^14][^15]

### 7. Realtime voice agents and speech-to-speech systems

Voice systems differ from chat because latency, interruption, pacing, and recovery directly shape product quality. Speech-to-speech and realtime sessions change the architecture and the eval set.[^16][^17]

### 8. End-to-end AI system case studies and build patterns

Case studies matter because they show how the above tradeoffs combine in real builds. The durable pattern is to design around the dominant failure cost, then document model choices, retrieval policy, approvals, traces, evals, and rollback logic.[^18]

## How these topics connect

These are not separate silos:
- serving changes latency budgets,
- context design changes retrieval and caching,
- orchestration changes tool and approval boundaries,
- evals determine whether any change was actually an improvement,
- and multimodal or voice systems make all of those constraints more visible.

That is why this expansion belongs together as one module instead of isolated notes.

## What to read next

- [[Practice - Advanced Systems Architecture Drills]]
- [[Packet - AI Engineering - Section 02 - Advanced Systems and Agent Operations]]
- [[Source - Advanced Systems Expansion - Core Sources]]

## Sources and citations

[^1]: Woosuk Kwon et al., *Efficient Memory Management for Large Language Model Serving with PagedAttention*. https://arxiv.org/abs/2309.06180  
[^2]: vLLM documentation. https://docs.vllm.ai/en/latest/  
[^3]: OpenAI API guide, prompt caching. https://developers.openai.com/api/docs/guides/prompt-caching/  
[^4]: Anthropic engineering, *Effective context engineering for AI agents*. https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents  
[^5]: Nelson F. Liu et al., *Lost in the Middle: How Language Models Use Long Contexts*. https://arxiv.org/abs/2307.03172  
[^6]: Anthropic research, *Building Effective AI Agents*. https://www.anthropic.com/research/building-effective-agents  
[^7]: OpenAI API guide, Agents SDK. https://developers.openai.com/api/docs/guides/agents-sdk/  
[^8]: Google Agent Development Kit docs, workflow agents. https://google.github.io/adk-docs/agents/workflow-agents/  
[^9]: MCP security best practices. https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices  
[^10]: Anthropic engineering, *Writing effective tools for agents*. https://www.anthropic.com/engineering/writing-tools-for-agents  
[^11]: OWASP, *AI Agent Security Cheat Sheet*. https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html  
[^12]: OpenAI API guide, evaluation best practices. https://developers.openai.com/api/docs/guides/evaluation-best-practices/  
[^13]: Anthropic engineering, *Demystifying evals for AI agents*. https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents  
[^14]: OpenAI API guide, file inputs. https://developers.openai.com/api/docs/guides/file-inputs/  
[^15]: OpenAI API guide, file search. https://developers.openai.com/api/docs/guides/tools-file-search/  
[^16]: OpenAI API guide, voice agents. https://developers.openai.com/api/docs/guides/voice-agents/  
[^17]: OpenAI API guide, Realtime API. https://developers.openai.com/api/docs/guides/realtime/  
[^18]: Anthropic engineering, *How we built our multi-agent research system*. https://www.anthropic.com/engineering/multi-agent-research-system
