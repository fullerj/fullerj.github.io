---
layout: post
title: "Misuse-Based Detection of Z-Wave Network Attacks"
description: >
  Extends Z-Wave misuse-based intrusion detection to achieve 99% detection accuracy against emerging wireless sensor attacks.
categories:
  - publications
permalink: /publications/misuse-detection-zwave-attacks/
tags:
  - intrusion-detection
  - wireless-security
  - z-wave
journal: "Computers & Security, Volume 64, January 2017, Pages 44–58"
venue: "COSE"
authors: "<u>Jonathan Fuller</u>, B. Ramsey, M. Rice, J. Pecarina"
pdf_url: /assets/papers/cose17.pdf
code_url: 
dataset_url: 
media_links:
  - title: "Smart Home Watch — Detecting Z-Wave Intrusions"
    url: https://example.com/media/misuse-zwave-smart-home-watch
---

**Journal**: {{ page.journal }}  
**Authors**: {{ page.authors }}

## Highlights

- Addresses the lack of defensive tooling for proprietary Z-Wave networks by extending a misuse-based intrusion detection system (MBIDS).
- Benchmarks baseline and extended MBIDS implementations, demonstrating a mean misuse detection rate of 99%.
- Provides actionable guidance for defenders of wireless sensor networks deployed in critical infrastructure, smart metering, and home automation.

## Resources

- [PDF]({{ page.pdf_url }})  


## Abstract

Wireless Sensor Networks (WSNs) are becoming ubiquitous, providing low-cost, low-power, and low-complexity systems in which communication and control are tightly integrated. Although much security research into WSNs has been accomplished, researchers struggle to conduct thorough analyses of closed-source proprietary protocols. Of the numerous available and underanalyzed proprietary protocols, those based on the ITU-T G.9959 recommendation specifying narrow-band sub-GHz communications have recently experienced significant growth. The Z-Wave protocol is the most common implementation of this recommendation. Z-Wave developers are required to sign nondisclosure and confidentiality agreements, limiting the availability of tools to perform open source research. Given recently demonstrated attacks against Z-Wave networks, defensive countermeasures are needed. This work extends an existing implementation of a Z-Wave Misuse-Based Intrusion Detection System (MBIDS). A side-by-side comparison is performed through experimentation to measure misuse detection accuracy of the baseline and extended MBIDS implementations. Experiment results determine the extended MBIDS achieves a mean misuse detection rate of 99%, significantly improving the security posture in MBIDS-monitored Z-Wave networks.



## BibTeX

```bibtex
@article{fuller2017misusezwave,
  title   = {Misuse-Based Detection of Z-Wave Network Attacks},
  author  = {Fuller, Jonathan and Ramsey, Benjamin and Rice, Mason and Pecarina, John},
  journal = {Computers & Security},
  volume  = {64},
  pages   = {44--58},
  month   = {January},
  year    = {2017}
}
```
