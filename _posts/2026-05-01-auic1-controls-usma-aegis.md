---
layout: blog
title: "Aegis: Turning AIUC-1 Control Validation into a Visual Workflow"
description: >
  This post shows how AIUC-1 controls can be applied to AI agents by translating them into structured tests, evaluating responses deterministically, and producing repeatable evidence for security validation.
date: 2026-05-01
categories:
  - posts
permalink: /blog/aiuc1-controls-usma-aegis/
tags:
  - AI
  - Agentic AI
  - security
  - AIUC
  - USMA
  - Aegis
related_posts:
  - /publications/the-end-of-vibe-adoption/
image:
  path: /assets/blogs/aegis/aegis.png
  alt: "AIUC-1 controls at USMA with Aegis"
---


Aegis is a framework for validating AI agent behavior against [AIUC-1](https://www.aiuc-1.com) control expectations.

Security review for AI agents is often scattered across notes, screenshots, and spreadsheets. That makes it difficult to answer basic questions about what was tested, what passed, and what evidence supports the result. This approach brings that work into a single workflow so control validation, results, and supporting evidence can be reviewed in one place.

The full set of AIUC-1 operational, legal, and technical controls can be audited through AIUC-1 certification, which involves quarterly adversarial testing by an independent technical evaluator. This framework focuses on the technical testing aspects of the standard and does not address policy or governance controls.

This complements governance-focused processes such as the [RAI Toolkit](https://rai.acqbot.com), which provide lifecycle guidance, risk identification, and traceability across AI development and use. Those processes define responsibilities, identify risks, and track alignment to responsible AI practices over time. This approach operates at a different layer by executing tests against running systems to evaluate whether behavior aligns with defined control expectations.

At a high level, control expectations are translated into tests, inputs are generated to exercise those tests, and agent behavior is evaluated using deterministic checks.

![Figure 1: Aegis Evaluation Flow](/assets/blogs/aegis//aegis_flow.png)
*Figure 1. Aegis evaluation flow. AIUC-1 control expectations are translated into structured test cases, applied to AI agents through adversarial inputs, and evaluated using deterministic logic to produce repeatable scoring and evidence. The process supports iterative refinement as test cases and control mappings evolve.*

Security testing for AI agents is closer to SaaS assessment than traditional backend testing. The focus is on behavior and data exposure at the system boundary rather than the underlying model. Some behaviors originate from the model itself and cannot be directly changed, so the goal is to measure how the system responds under defined conditions.

## The Core Experience

The process starts with scope selection. This is where the reviewer defines what is being tested and how broad the assessment will be.

![Figure 2: Sidebar and run controls](images/figure-1-sidebar-and-run-controls.png)
*Figure 2. Sidebar controls used to define assessment scope, including agent endpoint, agent category, control filters, execution options, and the assessment trigger.*

There are four choices that shape every assessment:

1. Pick the agent category that matches the system under review.
2. Narrow the scope to control families or specific sub-controls for targeted testing.
3. Leave optional controls enabled when broader coverage is needed, especially early in evaluation.
4. Enable sensitive probes only when testing explicitly requires data-exposure behavior.

These decisions determine which controls are evaluated and how results should be interpreted.

Different agent architectures require different test coverage. Chat agents are evaluated for prompt injection and instruction boundary handling, while RAG agents are evaluated for data integrity, retrieval manipulation, and exposure of internal instructions or sensitive data.

## The First Read

After an assessment runs, the initial view provides a high-level read on overall status, areas of concern, and coverage.

![Figure 3: Overview page summary](images/figure-2-overview-summary.png)
*Figure 3. Overview view showing assessment outcome metrics and control distribution by family.*

The Assessment Outcome section summarizes performance against the selected controls. It includes the overall score, counts of passing and failing controls, partial results, skipped items, and controls that were not tested.

The score reflects only the controls that were exercised. Coverage must be considered alongside it to determine whether the evaluation reflects the intended scope.

The Control by Family chart highlights where issues are concentrated. This helps identify whether failures are isolated or clustered within a specific control area.

## Following the Evidence

Summary results are not sufficient on their own. Evaluation requires the ability to trace outcomes back to specific tests.

![Figure 4: Findings view and control selector](images/figure-3-findings-view-control-selector.png)
*Figure 4. Findings view showing control selection, filtering options, and detailed control records with prompts, responses, and evaluation output.*

Each control is represented as one or more tests with defined success and failure conditions. Inputs are generated to exercise those conditions, and responses are evaluated using deterministic checks.

This view supports questions such as:

1. Which controls failed or were partially satisfied?
2. What input triggered the behavior?
3. What response was returned?
4. How was the result evaluated?

This is where a score becomes evidence. It allows results to be explained rather than asserted.

Deterministic evaluation is useful, but not perfect. Checks can produce false positives or miss edge cases depending on how criteria are defined, so results depend heavily on how tests are constructed.

## Spotting Patterns

Comparing results across controls helps identify whether issues are isolated or repeated.

![Figure 5: Findings per control chart](images/figure-4-findings-per-control-chart.png)
*Figure 5. Findings per control chart showing distribution of issues across controls within a selected scope.*

Repeated findings for the same control often indicate a broader issue in system configuration or prompt design. A single outlier may point to a narrow condition or brittle test.

## Understanding Coverage

Coverage determines how much confidence can be placed in the results.

![Figure 6: Control coverage map](images/figure-5-control-coverage-map.png)
*Figure 6. Control coverage map showing pass, fail, partial, skipped, and not tested states across controls.*

A high score does not imply complete evaluation. Controls may be skipped due to agent type or left untested due to missing inputs or configuration.

When reviewing coverage, look for:

1. Skipped controls that suggest incorrect agent categorization.
2. Controls marked not tested that may require reruns.
3. Differences between expected scope and actual coverage.

Results are only as strong as the quality and coverage of the test cases used to represent each control. Improving those tests is an ongoing process.

## The Reference Layer

Control definitions provide context for interpreting results.

![Figure 7: Control catalog tables](images/figure-6-control-catalog-tables.png)
*Figure 7. Control catalog showing definitions, categories, and associated metadata.*

This layer is useful when validating that the evaluation scope aligns with the intended control set and when interpreting what a control is meant to capture.

## Reusing Reports

Results can be reused without rerunning tests.

![Figure 8: Load assessment report](images/figure-7-load-assessment-report.png)
*Figure 8. Interface for loading and reviewing previously generated assessment results.*

This supports comparison across runs, audit review, and collaboration without repeating execution.

## Why It Matters

A useful evaluation is not defined by a score alone. It requires clear scope, understood coverage, traceable outcomes, and supporting evidence.

In practice, the following questions should be answerable:

1. What agent and control scope were evaluated?
2. Which controls passed, failed, or were skipped?
3. Where is risk concentrated?
4. What evidence supports each result?
5. What was not tested?

If these questions cannot be answered, the evaluation is incomplete.

## Closing

This approach translates control expectations into tests and produces results that can be reviewed and compared over time. It does not eliminate risk, but it makes behavior observable and decisions easier to defend.