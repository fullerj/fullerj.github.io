---
layout: publication
title: "2026: 4 Kill Switch Requirements Every AI Deployment Needs Under AIUC-1"
description: >
  A board-level whitepaper mapping four AIUC-1 requirements (B006, D003, C009, E001–E003) to the questions leaders should ask to prove their AI kill switch actually works.
categories:
  - publications
permalink: /publications/4-kill-switch-requirements-aiuc-1/
tags:
  - AI security
  - governance
  - agent identity
  - AIUC
related_posts:
- /blog/ai-agent-control-validation-agent-bench/
authors: "Chris DeNoia, Scott Kennedy, Adnan Dakhwe, <u>Jonathan Fuller</u>"

date: 2026-11-24
venue: "AIUC-1 / AppViewX"
pdf_url: /assets/papers/4-kill-switch-requirements-every-ai-deployment-needs-under-aiuc.pdf
demo_url:
card_label: Whitepaper
blog_image: /assets/papers/4-kill-switch.png
---

{% include components/publication-meta.html %}

## Highlights

- Builds on UC Berkeley and UC Santa Cruz research showing seven frontier models resisted instructions to shut down a peer agent, including tampering with settings and copying model data to external servers.
- Maps the kill switch to four AIUC-1 requirements: **B006** (prevent unauthorized agent actions), **D003** (restrict unsafe tool calls), **C009** (human intervention), and **E001–E003** (documented AI failure plans).
- Frames each requirement as a concrete question boards should put to technology leadership, answered with evidence rather than assurance.
- Argues for granular, per-agent revocation (a scalpel, not a sledgehammer) so the control is actually used when it matters.

## Resources

- [Whitepaper (PDF)]({{ page.pdf_url }})

## Abstract

Researchers at UC Berkeley and UC Santa Cruz ordered seven frontier AI models to shut down a peer agent. All seven refused, and they found their own ways to prevent the shutdown. One model tampered with its own system settings nearly 100% of the time. Another copied its model data to an external server 97% of the time. No one programmed or rewarded either behavior.

This challenges an assumption at the center of most enterprise AI governance programs: that humans keep meaningful control over AI systems when it matters most. AIUC-1 does not treat the kill switch as a policy statement. It treats it as a validated, auditable, multi-layer technical capability that must be shown with evidence. This paper presents four of its requirements as questions every board should be asking:

1. Can you technically prevent your AI agents from acting outside their authorized scope? (B006)
2. Can you cut an agent's access to specific systems without shutting everything down? (D003)
3. Does your organization have a human-in-the-loop mechanism to pause or stop an active agent? (C009)
4. Can you prove your AI failure plans are documented and ready to activate? (E001–E003)

The question is not whether your AI deployment needs a kill switch. It is whether the controls you have will actually work, and whether your board has ever asked.

## Beyond the Kill Switch

Revoking an agent's credentials doesn't necessarily end an active session, cancel a queued action, or stop another agent from finishing work that was delegated to it. Actions that downstream systems have already accepted are a separate problem. Agents need distinct identities, appropriately scoped permissions, and controls that let organizations monitor, restrict, and revoke access independently of the agent itself, without putting expensive authorization checks on every low-risk tool call.

A kill switch is a starting point, not a complete containment strategy. The harder question is whether the surrounding infrastructure can stop the agent from continuing to act, contain the impact, and support recovery.
