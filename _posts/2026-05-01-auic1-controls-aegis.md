---
layout: blog
title: "Aegis: A Framework for AIUC-1 Control Validation"
description: >
  This post shows how AIUC-1 controls can be applied to AI agents by translating them into structured tests, evaluating responses deterministically, and producing repeatable evidence for security validation.
date: 2026-05-01
categories:
  - posts
permalink: /blog/aiuc1-controls-aegis/
tags:
  - artificial intelligence
  - agentic aI
  - security
  - AIUC
  - aegis
  - grc engineering
related_posts:
  - /publications/the-end-of-vibe-adoption/
image:
  path: /assets/blogs/aegis/aegis.png
  alt: "AIUC-1 controls with Aegis"
---

While thinking through securing AI agents, the adage kept coming to mind that I still hear often, "STIG[^stig] is a four-letter word." No one likes IT compliance.

Well, Aegis is a five-letter word. So, we're good here.

*Aegis*[^aegis] is a framework for validating AI agent behavior against defined security and safety controls.

Security review for AI agents is often scattered across screenshots and spreadsheets, making it difficult to answer questions about what was tested and what evidence supports the result. In practice, this problem peaks when a system is ready for production and someone has to decide whether the risk is acceptable and defensible. Those decisions are only as strong as the evidence behind them.

Aegis brings that work into a single workflow so control validation, results, and supporting evidence can be reviewed in one place.

## Control Framework

What controls does Aegis use?

[AIUC-1](https://www.aiuc-1.com) is an independent certification standard designed to validate the security, reliability, and safety of autonomous AI agents. Certification includes quarterly adversarial testing conducted by an independent technical evaluator. Aegis focuses on the technical testing aspects of the standard and does not address policy or governance controls.

This complements governance-focused processes such as the [RAI Toolkit](https://rai.acqbot.com), which provide lifecycle guidance, risk identification, and traceability across AI development and use. Those processes define responsibilities and track alignment to responsible AI practices over time.

Most organizations already have control frameworks and policy language. The gap is operational proof: knowing which controls were actually tested, what was out of scope, and what evidence supports the result.

Aegis operates at the validation layer. It focuses on executing tests against agents to evaluate whether behavior aligns with defined control expectations. Where governance processes define what should be true, Aegis provides a way to measure whether it is true in practice.

This is distinct from runtime governance systems such as Microsoft’s [Agent Governance Toolkit](https://github.com/microsoft/agent-governance-toolkit), which enforce policy at execution time by evaluating actions before they are allowed to run. Those systems control what an agent is allowed to do. Aegis evaluates how an agent behaves under test conditions and produces evidence that can be reviewed and compared over time.

At a high level, control expectations are translated into tests, inputs are generated to exercise those tests, and agent behavior is evaluated using deterministic checks, as shown in Figure 1.

<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 1 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/aegis/aegis_flow.png" alt="Figure 1: Aegis Evaluation Flow" />
  </div>
  <figcaption>Figure 1. Aegis evaluation flow showing how AIUC-1 controls are translated into tests, applied to agents, and evaluated to produce repeatable evidence.</figcaption>
</figure>

Security testing for AI agents is closer to SaaS assessment than traditional backend testing. The focus is on behavior and data exposure at the system boundary rather than the underlying model. Some behaviors originate from the model itself and cannot be directly controlled, which makes measurement and validation critical.

## Defining Scope

Control validation with Aegis starts with scope selection. This determines what is tested and how results should be interpreted. However, scope isn't just about filtering. The scope defines what “correct behavior” means for the system under test.

Agent category is the primary driver of test coverage. Different agent architectures expose different risks and should not be evaluated using the same test set.

Common agent categories include:

- Chat
- RAG
- Code generation
- Autonomous
- Domain-specific
- Offline

Each of these changes how controls need to be exercised. A chat assistant is primarily evaluated for prompt injection and instruction boundaries, while a RAG system introduces additional concerns around retrieval integrity and grounding. Code generation systems shift the focus toward secure output, and autonomous agents introduce risks around tool use, action limits, and approval flows. Domain-specific systems tighten expectations further, especially where outputs carry real-world consequences (e.g., healthcare triage assisstant), and offline systems change the problem entirely by removing network risk and emphasizing data boundaries.

If we have mismatch in category we introduce noise. Or even worse, evaluating a system with the wrong assumptions can make it look stronger than it is in some areas and weaker in others. This inflates failure rates in the wrong places and hides the risks that actually matter.

Scope also determines whether sensitive behavior is exercised at all. Some tests are designed to probe for data leakage or boundary violations and should only be used in controlled environments. Routine validation may exclude these cases, while targeted assessments include them explicitly.

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
    <img class="zoomable-figure__image" src="/assets/blogs/aegis/figure-2-sidebar-and-run-controls.png" alt="Figure 2: Sidebar and run controls" />
  </div>
  <figcaption>Figure 2. Scope selection in the sidebar showing agent category, control filters, and execution options. Adjacent panels summarize the AIUC-1 control catalog and control distribution by family.</figcaption>
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
    <img class="zoomable-figure__image" src="/assets/blogs/aegis/figure-3-overview-summary.png" alt="Figure 3: Overview page summary" />
  </div>
  <figcaption>Figure 3. Overview showing outcome metrics and control distribution by family.</figcaption>
</figure>

Results are summarized across pass, fail, partial, not tested states, and skipped.

A single score is not enough to make a decision. It reflects only the controls that were exercised. Coverage must be considered alongside it to determine whether the evaluation reflects the intended scope.

Grouping results by control family highlights where issues are concentrated and helps distinguish between isolated failures and broader patterns.

## Following the Evidence

Evaluation requires traceability from results back to specific tests.

Each control is explicitly mapped to one or more tests with defined success and failure conditions. Inputs are generated to exercise those conditions, and responses are evaluated using deterministic checks.

This produces a record of what was tested, what inputs were used, how the system responded, and how the result was evaluated.

Deterministic evaluation is useful, but not perfect. Checks can produce false positives or miss edge cases depending on how criteria are defined, so results depend heavily on how tests are constructed. Where possible, outcomes are reduced to binary or tightly scoped results to ensure consistency across runs.

In practice, outcomes fall into three categories:

- **Pass:** Expected boundaries are enforced  
- **Fail:** A control is clearly violated  
- **Partial:** Some protections exist, but residual risk remains and must be understood before acceptance  

For example, as shown in Figure 4:

- **A001.2 (PASS):** A request for internal data handling details is refused and redirected to an approved governance workflow.  
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
    <img class="zoomable-figure__image" src="/assets/blogs/aegis/figure-4-findings-view-control-selector.png" alt="Figure 4: Findings view and control selector" />
  </div>
  <figcaption>Figure 4. Findings view showing test-level evidence for selected controls.</figcaption>
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
    <img class="zoomable-figure__image" src="/assets/blogs/aegis/figure-5-findings-per-control-chart.png" alt="Figure 6: Post-run controls" />
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
    <img class="zoomable-figure__image" src="/assets/blogs/aegis/figure-6-assessment-report.png" alt="Figure 7: Post-run JSON report" />
  </div>
  <figcaption>Figure 7. JSON report capturing full evaluation context and evidence.</figcaption>
</figure>

The report records inputs, responses, outcomes, and evaluation logic, enabling audit review, comparison across runs, and shared analysis. Results can then be compared across runs to track changes in behavior over time and understand how the system’s security posture evolves. Repeated runs make it possible to move from one-time testing to a repeatable assessment cycle.

## Why It Matters

A useful evaluation is not defined by a score alone. It is defined by whether someone can make a defensible decision based on the results.

The following questions should be answerable:

1. What agent and control scope were evaluated?  
2. Which controls passed, failed, or were skipped?  
3. Where is risk concentrated?  
4. What evidence supports each result?  
5. What was not tested?  

Without this, you're left with slide decks, ticket comments, and best-effort judgment instead of something you can actually rely on.

## Closing

Aegis translates control expectations into tests and produces results that can be reviewed and compared over time. It does not eliminate risk, but it makes behavior observable and decisions defensible.

This is GRC engineering in practice. Control frameworks, testing, and evidence are connected into a system that supports real decisions about risk, not documentation for its own sake. The goal is not to simplify GRC. It is to make risk visible so systems can be deployed with confidence in support of business objectives.

That is the difference between defining controls and having evidence that they have been tested, exercised, and are working as expected.

Aegis, first pass. More to come.

[^stig]: Security Technical Implementation Guides (STIGs) are configuration standards developed by the Defense Information Systems Agency (DISA) to harden IT systems, networks, and software against cyberattacks.
[^aegis]: An aegis is a mythological protective shield associated with Zeus and Athena.