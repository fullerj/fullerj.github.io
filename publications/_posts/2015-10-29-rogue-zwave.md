---
layout: post
title: "Rogue Z-Wave Controllers: A Persistent Attack Channel"
description: >
  Early warning on covert Z-Wave gateway compromises that enable persistent control over wireless sensor networks.
categories:
  - publications
permalink: /publications/rogue-zwave-controllers/
tags:
  - wireless-security
  - IoT
  - ZWave
conference: "2015 IEEE 40th Local Computer Networks Conference Workshops (LCN Workshops), Clearwater Beach, FL, 2015"
authors: "<u>Jonathan Fuller</u>; B. Ramsey"
venue: "IEEE LCN Workshops"
pdf_url: /assets/papers/senseapp15.pdf
talk_url: 
code_url: 
dataset_url: 
media_links:
  - title: "Industry IoT Watch — Rogue Controllers in Z-Wave Networks"
    url: https://example.com/media/rogue-zwave-industry
---

**Conference**: {{ page.conference }}  
**Authors**: {{ page.authors }}

## Highlights

- Demonstrates a novel technique to inject rogue gateway controllers into Z-Wave wireless sensor networks.
- Shows the injected controller can persistently communicate with vulnerable devices even without constant Internet connectivity.
- Outlines mitigation strategies for infrastructure operators deploying Z-Wave in smart metering, home automation, and industrial environments.

## Resources

- [PDF]({{ page.pdf_url }})  


## Abstract

The popularity of Wireless Sensor Networks (WSN) is increasing in critical infrastructure, smart metering, and home automation. Of the numerous protocols available, Z-Wave has significant potential for growth in WSNs. As a proprietary protocol, there are few research publications concerning Z-Wave, and thus little is known about the security implications of its use. Z-Wave networks use a gateway controller to manage and control all devices. Vulnerabilities have been discovered in Z-Wave gateways, all of which rely on the gateway to be consistently connected to the Internet. The work herein introduces a new vulnerability that allows the injection of a rogue controller into the network. Once injected, the rogue controller maintains a stealthy, persistent communication channel with all inadequately defended devices. The severity of this type of attack warrants mitigation steps, presented herein.



## BibTeX

```bibtex
@inproceedings{fuller2015roguezwave,
  title     = {Rogue Z-Wave Controllers: A Persistent Attack Channel},
  author    = {Fuller, Jonathan and Ramsey, Benjamin},
  booktitle = {Proceedings of the 2015 IEEE 40th Local Computer Networks Conference Workshops (LCN Workshops)},
  year      = {2015},
  month     = {October},
  address   = {Clearwater Beach, FL},
  publisher = {IEEE}
}
```
