---
title: Source - Advanced Systems Expansion - Core Sources
type: source
status: active
topic: Advanced AI Systems
course: AI Engineering
tags: [source, advanced-systems, agents, multimodal, voice, serving]
prerequisites: []
related:
  - "[[Topic - Advanced Systems, Agents, Multimodal, and Voice]]"
  - "[[Map - AI Engineering]]"
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
---

# Source - Advanced Systems Expansion - Core Sources

## Purpose

This note groups the core primary and official sources used to support the advanced systems expansion of the AI Engineering course.

## Source clusters

### Serving and vLLM
- Woosuk Kwon et al., *Efficient Memory Management for Large Language Model Serving with PagedAttention*  
  https://arxiv.org/abs/2309.06180
- vLLM documentation  
  https://docs.vllm.ai/en/latest/

### Context engineering and long context
- OpenAI API guide, prompt caching  
  https://developers.openai.com/api/docs/guides/prompt-caching/
- Anthropic engineering, *Effective context engineering for AI agents*  
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Nelson F. Liu et al., *Lost in the Middle: How Language Models Use Long Contexts*  
  https://arxiv.org/abs/2307.03172

### Agent orchestration and workflows
- Anthropic research, *Building Effective AI Agents*  
  https://www.anthropic.com/research/building-effective-agents
- OpenAI API guide, Agents SDK  
  https://developers.openai.com/api/docs/guides/agents-sdk/
- Google Agent Development Kit docs, workflow agents  
  https://google.github.io/adk-docs/agents/workflow-agents/

### MCP and tool security
- MCP security best practices  
  https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices
- Anthropic engineering, *Writing effective tools for agents*  
  https://www.anthropic.com/engineering/writing-tools-for-agents
- OWASP, *AI Agent Security Cheat Sheet*  
  https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html

### Evals
- OpenAI API guide, evaluation best practices  
  https://developers.openai.com/api/docs/guides/evaluation-best-practices/
- Anthropic engineering, *Demystifying evals for AI agents*  
  https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents

### Multimodal documents
- OpenAI API guide, file inputs  
  https://developers.openai.com/api/docs/guides/file-inputs/
- OpenAI API guide, file search  
  https://developers.openai.com/api/docs/guides/tools-file-search/

### Voice and realtime
- OpenAI API guide, voice agents  
  https://developers.openai.com/api/docs/guides/voice-agents/
- OpenAI API guide, Realtime API  
  https://developers.openai.com/api/docs/guides/realtime/

### End-to-end case studies
- Anthropic engineering, *How we built our multi-agent research system*  
  https://www.anthropic.com/engineering/multi-agent-research-system

## Why these sources matter

This set deliberately favors:
- primary papers where they exist,
- official product or protocol docs for changing implementation details,
- and well-scoped engineering writeups for applied architecture guidance.

That mix is the most durable way to support an AI-engineering note set where both fundamentals and operational details matter.

## Related notes

- [[Topic - Advanced Systems, Agents, Multimodal, and Voice]]
- [[Practice - Advanced Systems Architecture Drills]]
- [[Packet - AI Engineering - Section 02 - Advanced Systems and Agent Operations]]
