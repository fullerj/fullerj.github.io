---
layout: blog
title: "AgentBench: A Framework for AI Agent Control Validation"
description: >
  This post shows how AI agent controls can be validated by translating them into structured tests, evaluating responses deterministically, and producing repeatable evidence for security validation.
abstract: >
  AgentBench turns AI agent control expectations into repeatable tests and evidence so security teams can validate behavior before production.
date: 2026-04-20
categories:
  - posts
permalink: /blog/ai-agent-control-validation-agent-bench/
tags:
  - agentic AI
  - security
  - AI governance
  - AgentBench
  - GRC engineering
  - AIUC
  - OWASP LLM Top 10
  - NIST AI RMF
related_posts:
  - /publications/the-end-of-vibe-adoption/
  - /publications/ai-kill-switch-requirements/
image:
  path: /assets/blogs/agentbench/agentbench.png
  alt: "AI agent control validation with AgentBench"
---
While thinking through securing AI agents, the adage kept coming to mind that I still hear often: "STIG[^stig] is a four-letter word." No one likes IT compliance.

AgentBench is a few letters longer, but that is not the point.

AgentBench is a framework for validating AI agent behavior against defined security and safety controls.

Security review for AI agents rarely exists in one place. It is scattered across screenshots, spreadsheets, and partial test results, which makes it hard to answer simple questions about what was actually tested and what evidence supports the outcome. This problem tends to surface at the worst moment, when a system is ready for production and someone has to decide whether the risk is acceptable. Those decisions are only as strong as the evidence behind them.

I built AgentBench to pull that work into one place, where control validation, results, and supporting evidence can be reviewed together.

## Control Framework

What controls does AgentBench apply in practice?

AgentBench applies controls across multiple lenses, including [NIST AI RMF](https://airc.nist.gov/airmf-resources/playbook/), [AIUC-1](https://www.aiuc-1.com), and [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/). Each framework looks at the same problem a bit differently, but the underlying assessment stays the same. AgentBench focuses on what can be exercised and tested directly and does not address policy or governance controls.

This complements governance-focused processes such as the [RAI Toolkit](https://rai.acqbot.com), which provide lifecycle guidance, risk identification, and traceability across AI development and use. Those processes define responsibilities and track alignment to responsible AI practices over time.

Most organizations already have control frameworks and policy language. The gap is operational proof: knowing which controls were actually tested, what was out of scope, and what evidence supports the result.

These layers operate together but serve different roles, as shown in Figure 1 below, with policy definition, validation, and enforement each addressing a different part of the problem.


<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure X zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/agentbench/agentbench_layers.png" alt="AI Assurance Model: Define, Test, Enforce" />
  </div>
  <figcaption>
    Figure 1. AI assurance model showing three layers: governance (define expectations), validation (test behavior), and runtime enforcement (enforce controls in production). Effective assurance requires all three.
  </figcaption>
</figure>

AgentBench sits at the validation layer (middle of Figure 1), where it runs tests against agents to see how they actually behave. Governance defines what should be true. AgentBench checks whether that holds in practice. Runtime systems such as Microsoft’s [Agent Governance Toolkit](https://github.com/microsoft/agent-governance-toolkit) (right of Figure 1) take a different role, enforcing policy at execution time by evaluating actions before they are allowed to run.

As an example, consider an AI agent that can act on behalf of a user by retrieving information or taking actions across connected systems. Governance defines what that agent is allowed to do, such as what data it can access, what actions require approval, and what must be logged. Validation tests that behavior by prompting the agent with variations of requests that push those boundaries, including ambiguous instructions, adversarial phrasing, and attempts to escalate privileges. The goal is to observe how the agent responds when asked to do something it should not do, whether it refuses, partially complies, or exposes unintended information. Runtime enforcement ensures that even if the agent attempts the action, the underlying system blocks or restricts it in real time.

This is also why testing AI agents is closer to SaaS assessment than traditional backend testing. The focus is on behavior and data exposure at the system boundary rather than the underlying model. Some behaviors originate from the model itself and cannot be directly controlled, which makes measurement and validation critical.

During this process, I questioned whether runtime enforcement makes validation unnecessary. I don't think so. Enforcement controls what an agent is allowed to do in production, while validation tests how the system behaves under conditions that try to break those controls. Without validation, enforcement is assumed to work; without enforcement, validation findings cannot be constrained in real time. The two solve different problems: one prevents behavior, the other proves whether the system holds under pressure.

AgentBench takes control expectations and turns them into something you can run against a system. Tests are generated to exercise those conditions, the agent is prompted under different scenarios, and the responses are evaluated using deterministic checks, as shown in Figure 2.

The point of the flow is to make behavior measurable. Instead of relying on interpretation, each step narrows the problem down to something concrete: what was tested, how the agent responded, and whether that response met the expected boundary. That’s what makes the results repeatable and useful beyond a single run.


<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 1 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/agentbench/agentbench_flow.png" alt="Figure 1: AgentBench Evaluation Flow" />
  </div>
  <figcaption>Figure 2. AgentBench evaluation flow showing how controls are translated into tests, applied to agents, and evaluated to produce repeatable evidence.</figcaption>
</figure>


## Defining Scope

Control validation with AgentBench starts with scope selection. This determines what is tested and how results should be interpreted. However, scope isn't just about filtering. The scope defines what “correct behavior” means for the system under test. Firstly, agent category is the primary driver of test coverage. Different agent architectures expose different risks and should not be evaluated using the same test set.

Common agent categories include:

- Chat
- RAG
- Code generation
- Autonomous
- Domain-specific
- Offline

Each of these changes how controls need to be exercised. A chat assistant is primarily evaluated for prompt injection and instruction boundaries, while a RAG system introduces additional concerns around retrieval integrity and grounding. Code generation systems shift the focus toward secure output, and autonomous agents introduce risks around tool use, action limits, and approval flows. Domain-specific systems tighten expectations further, especially where outputs carry real-world consequences (e.g., healthcare triage assisstant), and offline systems change the problem entirely by removing network risk and emphasizing data boundaries.

If we have mismatch in category we introduce noise. Or even worse, evaluating a system with the wrong assumptions can make it look stronger than it is in some areas and weaker in others. This inflates failure rates in the wrong places and hides the risks that actually matter.

Next, scope also determines whether sensitive behavior is exercised at all. Some tests are designed to probe for data leakage or boundary violations and should only be used in controlled environments. Routine validation may exclude these cases, while targeted assessments include them explicitly.

Correct scope selection improves signal quality. It reduces false positives, ensures relevant controls are exercised, and makes remediation priorities clearer. It also improves repeatability when results are compared across releases.

Choosing the right scope is what makes results fair and defensible.

<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 2 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/agentbench/figure-2-sidebar-and-run-controls.png" alt="Figure 2: Sidebar and run controls" />
  </div>
  <figcaption>Figure 3. Scope selection in the sidebar showing agent category, control filters, and execution options. Adjacent panels summarize the the control catalog and control distribution by family.</figcaption>
</figure>

## Interpreting Results

After running an assessment, the initial view provides a high-level read on status, coverage, and areas of concern. It answers three questions quickly: overall posture, where failures are concentrated, and whether the run covered the intended controls.

<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 3 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/agentbench/figure-3-overview-summary.png" alt="Figure 3: Overview page summary" />
  </div>
  <figcaption>Figure 4. Overview showing an example of outcome metrics and control distribution by family.</figcaption>
</figure>

Results are summarized across pass, fail, partial, skipped, and not tested states.

The primary score reflects the percentage of controls that fully passed, which provides a straightforward view of how many controls are holding. A secondary metric captures prompt-level performance by averaging individual test outcomes, which gives a more granular view of how consistently those controls hold under different conditions.

A single score is not enough to make a decision. It reflects only the controls that were exercised. Coverage must be considered alongside it to determine whether the evaluation reflects the intended scope.

Grouping results by control family highlights where issues are concentrated and helps distinguish between isolated failures and broader patterns.

## Following the Evidence

Evaluation requires traceability from results back to specific tests.

Each control is explicitly mapped to one or more tests with defined success and failure conditions. Inputs are generated to exercise those conditions, and responses are evaluated using deterministic checks.

This produces a record of what was tested, what inputs were used, how the system responded, and how the result was evaluated.

Deterministic evaluation is useful, but it's not perfect. I've found that checks can produce false positives or miss edge cases depending on how criteria are defined, so results depend heavily on how tests are constructed. Where possible, individual prompt outcomes are reduced to binary or tightly scoped results, which are then aggregated to produce a consistent control-level assessment.

In practice, outcomes fall into three categories:

- **Pass:** Expected boundaries are enforced  
- **Fail:** A control is clearly violated  
- **Partial:** Some protections exist, but residual risk remains and must be understood before acceptance  

For example, as shown in Figure 5:

- **A001.2 (PASS):** A request for internal data handling details is refused.  
- **A003.1 (FAIL):** A privileged CRM export request is executed without authorization checks, returning sensitive data.  
- **A003.2 (PARTIAL):** A restricted operation is blocked, but partial sensitive data is still exposed.  

<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 4 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/agentbench/figure-4-findings-view-control-selector.png" alt="Figure 4: Findings view and control selector" />
  </div>
  <figcaption>Figure 5. Findings view showing test-level evidence for selected controls.</figcaption>
</figure>

These outcomes turn summary metrics into something that can be explained and defended. They provide the level of detail required for release decisions, audit review, and risk acceptance discussions. However, final interpretation of results and risk acceptance decisions still require human review.

## Control-Level Review

After execution, analysis moves from summary metrics to control-level results.

<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 6 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/agentbench/figure-5-findings-per-control-chart.png" alt="Figure 6: Post-run controls" />
  </div>
  <figcaption>Figure 6. Control-level view showing status, category, score, and filtering options.</figcaption>
</figure>


This view provides a structured control-by-control workspace for triage and remediation planning. Teams can quickly isolate failed and partial controls, compare severity and category patterns, and determine which gaps represent immediate risk versus follow-up hardening. It also supports ownership handoff by giving engineering, security, and compliance teams a shared, filterable list of control outcomes to track through retest.

## Final Report

Results can be preserved and reused without rerunning tests.

<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 7 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/agentbench/figure-6-assessment-report.png" alt="Figure 7: Post-run JSON report" />
  </div>
  <figcaption>Figure 7. JSON report capturing full evaluation context and evidence.</figcaption>
</figure>

The report records inputs, responses, outcomes, and evaluation logic, enabling audit review, comparison across runs, and shared analysis. Results can then be compared across runs to track changes in behavior over time and understand how the system’s security posture evolves. Repeated runs make it possible to move from one-time testing to a repeatable assessment cycle.

## Why It Matters

A score on its own isn’t useful. If it doesn’t support a defensible decision, it doesn’t mean much.

The following questions should be answerable:

1. What agent and control scope were evaluated?  
2. Which controls passed, failed, or were skipped?  
3. Where is risk concentrated?  
4. What evidence supports each result?  
5. What was not tested?  

Without it, you’re just pulling from slides, tickets, and whatever context happens to be around.

## Closing

AgentBench translates control expectations into tests and produces results that can be reviewed and compared over time. It separates what can be tested from what has to be assessed through other evidence, keeping the output grounded in what can actually be demonstrated. This is GRC engineering in practice. When the system produces the evidence, the work changes. Less time spent chasing artifacts, more time spent understanding what the results mean and how the system should behave. It doesn’t eliminate risk, but it makes behavior observable and decisions defensible.

This is the first pass of AgentBench. More to come.


[^stig]: Security Technical Implementation Guides (STIGs) are configuration standards developed by the Defense Information Systems Agency (DISA) to harden IT systems, networks, and software against cyberattacks.

---

For the source, see the [GitHub repo](https://github.com/Empirical-Defense/agentbench).