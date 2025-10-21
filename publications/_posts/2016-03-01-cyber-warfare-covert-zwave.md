---
layout: post
title: "Wireless Intrusion Detection of Covert Channel Attacks in ITU-T G.9959-Based Networks"
description: >
  Shows how covert channels over ITU-T G.9959 wireless links enable Z-Wave compromise and introduces an MBIDS countermeasure with 92% accuracy.
categories:
  - publications
permalink: /publications/wireless-intrusion-detection-g9959-covert-channels/
tags:
  - intrusion-detection
  - covert-channels

conference: "11th International Conference on Cyber Warfare and Security (ICCWS), 2016"
authors: "<u>Jonathan Fuller</u>, B. Ramsey, J. Pecarina, M. Rice"
venue: "ICCWS"
pdf_url: /assets/papers/iccws16.pdf
code_url: 
dataset_url: 
talk_url: h
media_links:
  - title: "Cyber Defense Digest — Covert Channels in Z-Wave Networks"
    url: https://example.com/media/cyber-defense-digest-zwave
---

**Conference**: {{ page.conference }}  
**Authors**: {{ page.authors }}

## Highlights

- Reveals a covert channel technique for injecting manipulated packets into ITU-T G.9959/Z-Wave wireless sensor networks.
- Demonstrates full compromise after WLAN access by embedding hidden instructions inside Z-Wave MAC frames.
- Develops a USRP-backed misuse-based intrusion detection system (MBIDS) achieving 92% mean accuracy against the covert channel attacks.

## Resources

- [PDF]({{ page.pdf_url }})  

## Abstract

We introduce herein an information hiding technique for injecting manipulated packets into wireless sensor networks (WSNs). We exhibit how an attacker can apply information hiding as a type of covert channel attack over radio frequency transmissions into the WSN. The feasibility of our injection method is demonstrated through an attack on the most common implementation of the ITU-T G.9959 recommendation, commercially known as Z-Wave. More specifically, we illustrate that after accessing a Z-Wave gateway controller through compromising the WLAN backbone, the attacker has the ability to install malware. The malware scans incoming Z-Wave packets for information hidden in Media Access Control (MAC) frames received by the Z-Wave controller. Upon identification of hidden information, a Reverse Secure Shell is initiated through the WLAN back to the attacker. The outcomes of this attack include control of the Z-Wave network and access to the networked devices on the target WLAN from any Internet connected device. Given this new application of information hiding techniques to Z-Wave networks, we recognize the need for countermeasures. We therefore offer an effective Misuse-based Intrusion Detection System (MBIDS) capable of distinguishing between manipulated and correctly formed packets. A Universal Software Radio Peripheral (USRP) Software-Defined Radio (SDR) is used in conjunction with a packet monitoring tool capturing incoming transmissions and inspecting them for any violations of the ITU-T G.9959 MAC specification. We then analytically and experimentally estimate the efficacy of the USRP as a packet capture device in a realistic test setup, and then evaluate the total efficiency of our MBIDS solution. By employing the MBIDS in the Z-Wave network, we show the MBIDS is capable of detecting packet manipulation attacks with 92% mean accuracy.



## BibTeX

```bibtex
@inproceedings{fuller2016g9959covert,
  title     = {Wireless Intrusion Detection of Covert Channel Attacks in ITU-T G.9959-Based Networks},
  author    = {Fuller, Jonathan and Ramsey, Benjamin and Pecarina, John and Rice, Mason},
  booktitle = {Proceedings of the 11th International Conference on Cyber Warfare and Security (ICCWS)},
  year      = {2016}
}
```
