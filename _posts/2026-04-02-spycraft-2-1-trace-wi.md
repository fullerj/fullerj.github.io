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

I read *Tracers in the Dark*[^tracers] while working on [VADER]({{ "/publications/vader-dead-drop-resolver/" | relative_url }}) (also highlighted in [Spycraft 2.0]({{ "/blog/spycraft-2-0/" | relative_url }})), and kept coming back to the Bitcoin transaction data behind that work. Well, I carved out some time to do it.

The first pass focused on collecting blockchain indicators tied to malware activity. That included wallet IDs and transaction IDs.

This writeup is the second pass.

Instead of treating those indicators as static artifacts, I asked a different question: what does the system look like if those indicators are treated as a network?

Baseline context: the VADER collection identified 273 dead drops across 7 web applications. Pastebin accounted for 68 percent of the abuse. Blockchain explorers made up 25 percent, including 23 transaction IDs and 14 wallet IDs.

## Scope and Data

This run starts from that blockchain-explorer slice: 14 wallet IDs and 23 transaction IDs. After enrichment and a single round of neighborhood expansion, the dataset grew to 41 wallets. The resulting graph contains 993 nodes and 1,579 edges. From the original seeds, hop-based expansion produced 540 related-wallet links, and 10 seed wallets generated decoded signal rows.

The workflow is intentionally simple: ingest local wallet and transaction seeds, pull Bitcoin data from multiple providers, build a depth-1 graph, label wallet roles, decode value sequences into candidate IPv4 outputs (see [VADER]({{ "/publications/vader-dead-drop-resolver/" | relative_url }})), then expand related-wallet neighborhoods from the seeds.

Role labels are used literally. A transit wallet passes value between peers. A collector accumulates value. A distributor pushes value outward to many peers. An isolated wallet shows little connected activity within this slice.

The graph depth is shallow by design. The goal is fast pattern extraction from known footholds.

## How the Picture Changed

At the start, the data looked like a loose set of wallet references. But, after graphing and decoding, it stopped looking loose. The same wallets reappear. The same patterns repeat. The neighborhoods around those anchors expand in ways that do not look random. Teasing out the structure is the main result.

Figure 1 shows the full graph view. Even with minimal styling, dense cores and spoke-like neighborhoods stand out.

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

Figure 2 isolates the hub-focused view used for analysis. It makes the central wallets and their immediate neighborhoods easier to see.

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

These figures are a bit dense but it's the shape matters more than the details.

In Figure 1, several nodes sit far from the central mass. Given that the initial wallets and transactions are malware-linked access points, those outliers may represent short-lived paths or routes that shifted quickly under pressure. That remains a working hypothesis, but the pattern suggests more than incidental contact. They matter because they can become future pivots if activity moves away from current hubs.

Figure 1 also shows detached mini-clusters and long, thin tails connected by very few edges. That pattern is consistent with compartmentalized movement. Value moves through narrow paths without broad connectivity.

Now, in Figure 2, the hub-and-spoke structure is a bit clearer. A small number of nodes absorb most of the activity, while many peripheral nodes appear once or a few times and then drop out. That asymmetry drives the concentration metrics.

Another signal in Figure 2 is the presence of self-loops and short cycles around active nodes. A loop does not prove signaling on its own, but repeated loopback behavior can indicate reuse, staged consolidation, or test transfers before outward movement.

Now we can see that it's not just random payment traffic. It more organized than that, with core coordination points, disposable edge wallets, and bridge paths that allow rotation without rebuilding the system.

## The Most Interesting Signals

The first signal is concentration. Activity accumulates around a small set of hubs:

- 17gd1msp5FnMcEMF1MitTNSsYs7w7AQyCt: 530 interactions  
- 1CeLgFDu917tgtunhJZ6BA2YdR559Boy9Y: 213 interactions  
- 1HTDy9SkfhwaNCXFA8wFCvN53f3iGpm8kb: 29 interactions  

If this were random contamination, the distribution would be flatter. Even at depth 1, the structure remains hierarchical. That centralization is apparent in Figure 2.

The second signal is repetition in decoded values. The busiest wallets repeatedly map to the same candidate outputs:

- 17gd1msp5FnMcEMF1MitTNSsYs7w7AQyCt → 96.69.184.42 (269 events)  
- 1CeLgFDu917tgtunhJZ6BA2YdR559Boy9Y → 195.123.220.180 (215 events)  
- 1CpTCVckjajNKDd7PsApV3cAkunVd4Mcmt → 128.247.64.234 (14 events)  

A single decode could be noise. Repetition at this volume is harder to dismiss.

### Figure 7: Recurrence Timeline for Decoded Candidates

![Figure 7 - decoded recurrence timeline](/assets/blogs/trace-wi/decoded_recurrence_timeline.png)

The timeline places recurrence in time, not just counts. In this run, decoded events span from mid-2017 through early 2021. All 10 wallets with decoded activity retain enough timestamp context to measure intervals between observations.

These wallet IDs come from the [VADER]({{ "/publications/vader-dead-drop-resolver/" | relative_url }}) dataset, which includes malware samples from 2012 through 2022. The timeline reflects when decoded on-chain events were observed, not the full range of the source corpus.

Bubble size tracks recurrence volume. The same wallets that dominate counts also persist over multiple years.

The role split reinforces this:

- transit: 31  
- collector: 7  
- distributor: 1  
- isolated: 2  

Most nodes act as pass-through points, with very few clear fan-out endpoints. That profile aligns with staged relay movement rather than simple one-direction payments.Seed expansion is also large relative to the initial footprint. From a small seed set, the graph produced 540 related-wallet links. That suggests stable adjacency around known anchors.

## What This May Lead To

At this point, I'm not too concerned about labelling individual nodes. I'm trying to understand the operating patterns over time.

One practical read: public rails are cheap, durable, and always available, which makes them useful for signaling. A few hubs stay consistently active. Transit-heavy paths segment movement between stages. Related-wallet neighborhoods look like rotation routes before they become obvious elsewhere.

However, I want to be clear that I'm not trying to make any attribution claims. If the same shape keeps showing up in additional windows, it points to coordination logic rather than isolated traces.

## Open Questions Worth Chasing

The next questions focus on stability and timing. Do the same hubs remain dominant across reruns and time windows, or does control rotate? Do decoded candidates cluster around campaign periods? How often do they recur across unrelated seed sets? When a hub goes quiet, which adjacent wallets take its place? Do decoded candidates overlap with known hosting providers or prior infrastructure clusters?

Answering those moves this from pattern observation toward lifecycle understanding.

## What Changed in My Understanding

Before this pass, these blockchain indicators were just dead drops.

After this pass, part of the wallet set seems to be signaling infrastructure with visible structure, i.e, concentrated hubs, repeated value-encoded candidates, transit-heavy movement, and expandable neighborhoods.

So, this is more like a partial blueprint instead of just a list of clues.

The full system is still incomplete, but the shape, rhythm, and likely handoff points are visible. 

I may keep just keep pulling this thread...

[^tracers]: Greenberg, Andy. *Tracers in the Dark: The Global Hunt for the Crime Lords of Cryptocurrency*. Doubleday, 2022.