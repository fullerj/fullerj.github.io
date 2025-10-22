---
layout: publication
title: "Invited Tutorial: Counteracting Web Application Abuse in Malware"
description: >
  Hands-on SecDev 2023 tutorial that teaches incident responders how to triage and remediate Web App-Engaged malware using the Marsea toolchain.
categories:
  - publications
permalink: /publications/secdev-tutorial-web-application-abuse/
tags:
  - malware
  - incident-response
  - tutorials
conference: "2023 IEEE Secure Development Conference (SecDev) — Invited Tutorial"
authors: "M. Yao, <u>Jonathan Fuller</u>, R. Pai Sridhar, S. Agarwal, A. K. Sikder, B. Saltaformaggio"
venue: "IEEE SecDev"
pdf_url:
slides_url: 
talk_url: 
code_url: https://github.com/CyFI-Lab-Public/MARSEA
dataset_url: 
media_links:
  - title: "SecDev Conference Preview — Tackling Web App Abuse"
    url: https://example.com/media/secdev-tutorial-preview
---

**Conference**: {{ page.conference }}  
**Authors**: {{ page.authors }}

## Highlights

- Distills forensic best practices for identifying and containing Web App-Engaged malware across popular web platforms.
- Showcases the Marsea pipeline, enabling responders to document proof-of-abuse and coordinate takedowns with service providers.
- Features a 90-minute mix of tool walkthroughs and labs, giving participants practical experience handling WAE incidents end-to-end.

## Resources

- [PDF]({{ page.pdr_url }})  
- [Source code]({{ page.code_url }})  


## Abstract

Web applications offer a broad range of functionalities that are exploited by malware to serve as alternatives to traditional attacker-controlled servers. To effectively combat these Web App-Engaged (WAE) malware, prompt coordination between incident responders and web application providers is crucial. Any delay in this collaboration can allow WAE malware to flourish, posing serious security risks to the compromised web applications.  
  
Our tutorial is designed to enlighten attendees on the best practices for forensic analysis of malware, while also helping to assess the scope of infection caused by WAE malware. Additionally, we will discuss the essential “proof-of-abuse” documentation required to facilitate groundbreaking collaboration between incident responders and service providers.  
  
To enhance the security awareness of incident responders and to streamline forensic analysis for quicker response times, we are offering a 90-minute tutorial. This educational session will address the unique challenges posed by WAE malware and explore specialized remediation techniques available through web application platforms. We will also unveil a recently-developed forensic tool designed specifically for handling WAE malware. The tutorial will be a hybrid experience, featuring both a walkthrough of this innovative tool and a hands-on exercise, enabling participants to use the tool for analyzing WAE malware effectively.

## BibTeX

```bibtex
@inproceedings{yao2023secdevtutorial,
  title     = {Invited Tutorial: Counteracting Web Application Abuse in Malware},
  author    = {Yao, Mingxuan and Fuller, Jonathan and Pai Sridhar, Ranjita and Agarwal, Saumya and Sikder, Amit K. and Saltaformaggio, Brendan},
  booktitle = {Proceedings of the IEEE Secure Development Conference (SecDev)},
  year      = {2023},
  note      = {Invited Tutorial}
}
```
