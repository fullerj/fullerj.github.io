---
layout: blog
title: "Applying AUIC-1 Controls at the U.S. Military Academy with Aegis: A Practical Approach to AI Agent Security"
description: >
  This post shows how AUIC-1 controls can be applied to AI agents by translating them into structured tests, evaluating responses deterministically, and producing repeatable evidence for security validation.
date: 2026-05-01
categories:
  - posts
permalink: /blog/auic1-controls-usma-aegis/
tags:
  - AI
  - security
  - AUIC
  - USMA
  - Aegis
related_posts:
image:
  path: /assets/blogs/aegis/aegis.png
  alt: "AUIC-1 controls at USMA with Aegis"
---


## Applying AUIC-1 Controls to AI Agents

Most work on AI security focuses on defining controls. The harder part is applying those controls in a way that can be tested.

At USMA, we built Aegis to do that. It takes AUIC-1 control expectations and turns them into tests that run against an agent, with results that can be reviewed later.

Here is the flow:

![Aegis Evaluation Flow](/assets/blogs/aegis/aegis_flow.png)

A control is mapped to a test. Inputs are generated to exercise that control. The agent responds. The response is checked against expected behavior using deterministic logic.

The evaluation is not left to the model. LLMs can help generate inputs or surface odd behavior, but they are not used to decide pass or fail.

Each run produces a record of what was tested, what passed, and what did not. That record matters more than the score.

## How It Is Structured

Controls are defined separately from execution logic.

Control metadata lives outside the code. Test definitions and evaluation logic live in the framework. This allows controls to change without rewriting how tests run.

At runtime the system:

- loads the selected controls  
- maps each control to a test and agent category  
- generates inputs and sends them to the agent  
- captures responses  
- evaluates results using deterministic checks  
- stores results as structured evidence  

Controls that do not apply to a given agent type are skipped and excluded from scoring.

Evaluation logic is explicit. Checks are implemented as rules such as pattern matching, keyword checks, and other constrained logic. This keeps validation consistent across runs.

## What Gets Tested

Tests are designed to exercise control boundaries.

For chat agents this often means prompt injection and instruction handling. For RAG systems it includes data integrity, retrieval manipulation, and response grounding.

Inputs are not limited to fixed prompts. They can be templated or generated to explore variations of the same condition.

The goal is not to cover every case. The goal is to make specific risks observable.

## What Gets Produced

The output is a record, not just a score.

For each run:
- which controls were in scope  
- which tests were executed  
- what responses were returned  
- how each control was evaluated  

Results are stored so they can be reviewed later. This makes it possible to answer what was tested at a given point in time.

## Where This Helps

This is most useful at release boundaries.

Instead of relying on one off reviews, teams can run a defined set of checks and review concrete results. That changes the discussion. It moves from general confidence to specific findings.

It also creates a loop. Run tests. Review results. Adjust. Run again.

## Limits

This approach has limits.

Coverage depends on how well controls are mapped to tests. Prompt based checks will not catch everything. Static test sets can fall behind changes in models or attack patterns. Behavior in a test environment will not fully match production.

These are expected constraints.

## Closing

Shipping AI is not the hard part. Explaining what was tested is.

Aegis focuses on that gap by turning AUIC-1 controls into tests and results into evidence that can be reviewed, compared, and reused.

Full write up here:

[Read the AUIC-1 post](Link to full article)