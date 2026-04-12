---
layout: blog
title: "Spycraft 2.1: From Wallets to Structure in Bitcoin Transaction Graphs"
description: >
  This post extends Spycraft by analyzing Bitcoin wallet and transaction identifiers as a graph, revealing concentrated hubs, repeated value patterns, and structured behavior from malware-derived data.
date: 2026-04-02
categories:
  - posts
permalink: /blog/spycraft-2-1-bitcoin-transaction-graph-analysis/
tags:
  - blockchain
  - bitcoin
  - malware
  - graph analysis
related_posts:
  - /blog/spycraft-2-0/
image:
  path: /assets/blogs/trace-wi/trace-wi.png
  alt: "Bitcoin transaction graph showing wallet clusters and flow patterns"
---

# Spycraft 2.1: From Wallets to Structure in Bitcoin Transaction Graphs

I read *Tracers in the Dark*[^tracers] while working on [VADER]({{ "/publications/vader-dead-drop-resolver/" | relative_url }}) (and later wrote about that in [Spycraft 2.0]({{ "/blog/spycraft-2-0/" | relative_url }})), and I kept coming back to the same question: what actually sits underneath those Bitcoin indicators. Eventually I just dug in.

At first it was just collecting blockchain indicators tied to malware, wallet IDs, transaction IDs, to identify the full scope of dead drop resolvoers, but this is where things started to shift. Instead of treating those indicators like static artifacts, I started wondering what they look like as a network, not just a list, but something with structure.

For context, the VADER collection surfaced 273 dead drops across 7 web applications, with Pastebin dominating at 68 percent, and blockchain explorers making up another 25 percent, including 23 transaction IDs and 14 wallet IDs.

## Scope and Data

This run starts from that slice, 14 wallets and 23 transactions, which after enrichment and a single round of expansion grew to 41 wallets. The resulting graph contains 993 nodes and 1,579 edges, with hop-based expansion producing 540 related-wallet links, and 10 seed wallets generating decoded signal rows.

I kept the workflow simple, pulling local seeds, querying multiple Bitcoin data providers, building a depth-1 graph, labeling wallet roles, decoding value sequences into candidate IPv4 outputs (see [VADER]({{ "/publications/vader-dead-drop-resolver/" | relative_url }})), and then expanding outward once. The goal here is not completeness, but speed, pulling structure from known footholds and seeing what holds up.

Role labels are literal, transit wallets pass value along, collectors accumulate, distributors push outward, and isolated wallets barely connect. The shallow depth is deliberate, because if structure shows up here, it tends to be real.

## How the Picture Changed

This started out looking like a loose collection of references, just wallets tied to activity, but once everything was graphed and decoded, the same wallets kept showing up, the same patterns repeated, and the same neighborhoods expanded outward in ways that did not feel random. That shift, from scattered indicators to something with shape, is really the main result.

Figure 1 shows the full graph, which is dense and a bit messy, but even without heavy styling you can see cores forming and spokes radiating outward.

<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 1 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/trace-wi/graph_full.png" alt="Figure 1 - Full graph overview (993 nodes, 1,579 edges)" />
  </div>
  <figcaption>Figure 1 - Full graph overview (993 nodes, 1,579 edges)</figcaption>
</figure>

Figure 2 narrows that view to the hubs, where the structure becomes easier to read, and where most of the interesting behavior sits.

<figure class="zoomable-figure" data-zoom-min="1" data-zoom-max="4" data-zoom-step="0.2">
  <div class="zoomable-figure__toolbar" aria-label="Figure 2 zoom controls">
    <button type="button" class="zoomable-figure__button" data-zoom-action="out" aria-label="Zoom out">-</button>
    <span class="zoomable-figure__level" aria-live="polite">100%</span>
    <button type="button" class="zoomable-figure__button" data-zoom-action="in" aria-label="Zoom in">+</button>
    <button type="button" class="zoomable-figure__button zoomable-figure__button--reset" data-zoom-action="reset">Reset</button>
  </div>
  <div class="zoomable-figure__viewport">
    <img class="zoomable-figure__image" src="/assets/blogs/trace-wi/graph_hubs.png" alt="Figure 2 - Hub-focused graph view" />
  </div>
  <figcaption>Figure 2 - Hub-focused graph view</figcaption>
</figure>

Some nodes sit well outside the main cluster, and given that the starting points are malware-linked, those outliers may represent short-lived paths or routes that shifted under pressure. That remains a working assumption, but they do not look accidental, more like fragments of activity that moved.

There are also detached clusters and long, thin chains with very few connections, which is consistent with compartmentalized movement, where value moves through narrow corridors rather than broadly across the network.

In the hub-focused view, a small number of nodes carry most of the activity, while many peripheral nodes appear briefly and then disappear, creating a clear imbalance. Around those active nodes, self-loops and short cycles show up repeatedly, which on their own do not prove signaling, but taken together start to suggest reuse, staging, or test transfers before outward movement.

At that point, it stops looking like random transaction flow and starts to look organized, with coordination points, disposable edge wallets, and bridge paths that allow rotation without rebuilding the system.

## The Most Interesting Signals

This gets concentrated fast, with a small set of hubs absorbing a disproportionate amount of activity:

- 17gd1msp5FnMcEMF1MitTNSsYs7w7AQyCt: 530 interactions  
- 1CeLgFDu917tgtunhJZ6BA2YdR559Boy9Y: 213 interactions  
- 1HTDy9SkfhwaNCXFA8wFCvN53f3iGpm8kb: 29 interactions  

If this were random contamination, the distribution would be flatter, but even at shallow depth the structure remains hierarchical.

The other signal is repetition in decoded values, and it shows up consistently enough to be difficult to dismiss, with the busiest wallets mapping to the same outputs, over and over:

- 17gd1msp5FnMcEMF1MitTNSsYs7w7AQyCt → 96.69.184.42 (269 events)  
- 1CeLgFDu917tgtunhJZ6BA2YdR559Boy9Y → 195.123.220.180 (215 events)  
- 1CpTCVckjajNKDd7PsApV3cAkunVd4Mcmt → 128.247.64.234 (14 events)  

### Figure 7: Recurrence Timeline for Decoded Candidates

![Figure 7 - decoded recurrence timeline](/assets/blogs/trace-wi/decoded_recurrence_timeline.png)

Looking at this over time adds another layer, with decoded events spanning from mid-2017 through early 2021, and all 10 wallets with decoded activity retaining enough timestamp context to track recurrence intervals. These wallets come from the VADER dataset, which spans malware samples from 2012 through 2022, so what shows up here reflects the on-chain portion of that broader activity.

Bubble size tracks recurrence, and the same wallets that dominate counts also persist across multiple years. The role distribution lines up with that behavior:

- transit: 31  
- collector: 7  
- distributor: 1  
- isolated: 2  

Most nodes act as pass-through points, with very few clear endpoints, which aligns more with staged relay movement than simple one-direction transfers.

Expansion is also telling, because starting from a small seed set, the graph produced 540 related-wallet links, which suggests stable adjacency around those anchors rather than incidental connections.

## What This May Lead To

At this point I am less concerned with labeling individual nodes and more focused on how this behaves over time. Public blockchains are cheap, durable, and always available, which makes them useful not just for moving value, but for signaling.

A few hubs remain consistently active, transit-heavy paths segment movement, and related-wallet neighborhoods look like rotation space where activity can shift before becoming obvious elsewhere. This is not an attribution claim, but if the same structure continues to appear across different runs, it starts to point toward coordination logic rather than isolated traces.

## Open Questions Worth Chasing

What matters next is whether any of this is stable over time, whether the same hubs remain dominant or rotate, whether decoded values cluster around campaign periods, how often these patterns recur across unrelated seed sets, what replaces a hub when it goes quiet, and whether decoded outputs overlap with known infrastructure.

Answering those questions moves this from pattern observation toward lifecycle understanding.

## What Changed in My Understanding

Before this, these blockchain indicators felt like dead drops, static and isolated, but after this pass they look more like infrastructure, not complete, but structured enough to expose concentration, repetition, transit-heavy flow, and expandable neighborhoods.

It reads more like a partial blueprint than a list of clues, with enough visible shape and continuity to suggest underlying coordination, even if the full system is still incomplete. I will probably keep pulling on this thread.

[^tracers]: Greenberg, Andy. *Tracers in the Dark: The Global Hunt for the Crime Lords of Cryptocurrency*. Doubleday, 2022.