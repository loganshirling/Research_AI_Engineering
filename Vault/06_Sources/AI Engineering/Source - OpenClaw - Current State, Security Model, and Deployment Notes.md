---
title: Source - OpenClaw - Current State, Security Model, and Deployment Notes
type: source
status: active
topic: AI Agents
course: AI Engineering
tags: [source, openclaw, self-hosted, gateway, security, deployment]
related:
  - "[[Topic - OpenClaw and Self-Hosted Personal Agents]]"
  - "[[Topic - AI Agents: Design, Boundaries, and Operations]]"
sources:
  - "https://docs.openclaw.ai/"
  - "https://docs.openclaw.ai/gateway/security"
  - "https://docs.openclaw.ai/gateway/remote"
  - "https://docs.openclaw.ai/gateway/configuration-reference"
  - "https://docs.openclaw.ai/start/getting-started"
  - "https://github.com/openclaw/openclaw"
last_updated: 2026-03-12
review_status: researched
priority: high
---

# Source - OpenClaw - Current State, Security Model, and Deployment Notes

## Why this source bundle matters

OpenClaw is useful here not because it is the only agent project that matters, but because it makes self-hosted personal-agent design unusually concrete:
- gateway architecture,
- multi-channel routing,
- session isolation,
- remote access,
- and trust boundaries.

## What the docs currently make clear

### 1. It is positioned as a self-hosted personal assistant and gateway
The docs describe OpenClaw as self-hosted, multi-channel, and agent-native, while the GitHub repository describes it as a personal AI assistant you run on your own devices.[^openclaw-home][^openclaw-github]

### 2. The Gateway is the control plane
The homepage says the Gateway is the single source of truth for sessions, routing, and channel connections.[^openclaw-home]

### 3. Host/config trust is a core assumption
The security docs explicitly say the host and config boundary are trusted and that operators who can modify host state or config should be treated as trusted operators.[^openclaw-security]

### 4. Mixed-trust sharing is discouraged
The same docs say one Gateway is not the recommended setup for mutually untrusted or adversarial operators, and suggest separate gateways or separate OS users/hosts for split trust boundaries.[^openclaw-security]

### 5. Remote exposure is intentionally conservative
The remote-access docs recommend loopback-only as the safest default and prefer loopback plus SSH or Tailscale-style access over casual wider exposure.[^openclaw-remote]

### 6. Inbound messaging policy is part of the safety model
The configuration reference documents DM policy modes such as pairing, allowlist, open, and disabled.[^openclaw-config]

## Why this is useful for the course

OpenClaw is a strong teaching example for:
- personal-agent trust boundaries,
- channel-aware permissions,
- gateway design,
- remote access choices,
- and the difference between “local feeling” and “safe by default.”

## Caveats

- OpenClaw’s docs are product docs, so they are best used as a current-state operational reference rather than a neutral academic treatment.
- The project is moving quickly, so implementation details should be re-checked before using the note for exact deployment steps.
- “Self-hosted” still leaves large security responsibilities with the operator.

## Related notes

- [[Topic - OpenClaw and Self-Hosted Personal Agents]]
- [[Topic - AI Agents: Design, Boundaries, and Operations]]
- [[Technique - Agent Approval Design]]

[^openclaw-home]: OpenClaw Docs, homepage. Last verified 2026-03-12. https://docs.openclaw.ai/
[^openclaw-security]: OpenClaw Docs, "Security." Last verified 2026-03-12. https://docs.openclaw.ai/gateway/security
[^openclaw-remote]: OpenClaw Docs, "Remote Access." Last verified 2026-03-12. https://docs.openclaw.ai/gateway/remote
[^openclaw-config]: OpenClaw Docs, "Configuration Reference." Last verified 2026-03-12. https://docs.openclaw.ai/gateway/configuration-reference
[^openclaw-github]: openclaw/openclaw GitHub repository. Last verified 2026-03-12. https://github.com/openclaw/openclaw
