---
layout: blog
title: "Unifying PingCastle, Microsoft Zero Trust, and ScubaGear Through the DoW Zero Trust Activity Model"
description: >
  This post explores how PingCastle, Microsoft Zero Trust Assessment, and ScubaGear findings can be normalized into a shared DoW Zero Trust activity model to improve prioritization, detection engineering, and operational decision-making.
abstract: >
  By treating DoW Zero Trust activities as the control layer, security teams can unify fragmented findings into a coherent operational model.
date: 2026-05-22
categories:
  - posts
permalink: /blog/zta-control-convergence/
tags:
  - zero trust
  - ICAM
  - PingCastle
  - ScubaGear
  - Microsoft Zero Trust
  - threat modeling
  - detection engineering
  - security operations
  - identity security
  - control convergence
related_posts:

image:
  path: /assets/blogs/zta-control-convergence/zta.png
  alt: "Control convergence across PingCastle, Microsoft Zero Trust, and ScubaGear mapped into DoW Zero Trust activities"
---

#### The Reality of the DoW Zero Trust Roadmap

The DoW Zero Trust Architecture roadmap has become one of the most influential cybersecurity modernization frameworks in government. With 152 activities spanning seven pillars, it represents an ambitious attempt to operationalize Zero Trust at enterprise scale. For those of us inside the Department of War (DoW) ecosystem, it is a strategic mandate that will shape architecture, procurement, operations, and governance for years to come.

The roadmap is comprehensive, and I have mixed feelings about having to focus on 152 activities. However, its approach to identity-centric security, microsegmentation, device trust, behavioral analytics, continuous monitoring, and automation are all areas that unquestionably improve defensive posture when implemented thoughtfully. The framework also forces organizations to confront uncomfortable realities that many enterprises have delayed addressing for years: sprawling implicit trust relationships, fragmented visibility, weak access governance, and limited operational automation.


<br>

| ID | Activity | Operational Focus | Intended Outcome |
|:--|:--|:--|:--|
| **1.1.1** | **Inventory User** | Establish authoritative identity sources for privileged and non-privileged users. Connect identities to lifecycle management processes. | Organizations maintain accurate visibility and control over authenticated users and privileged accounts. |
| **1.2.1** | **Implement App Based Permissions per Enterprise** | Standardize enterprise attributes, roles, and permissions across ICAM implementations and applications. | Conditional access and PAM workflows can enforce authorization consistently across enterprise applications. |
| **1.2.2** | **Rule Based Dynamic Access** | Use dynamic rules, JIT, and JEA to control administrative access in real time. | Access decisions become continuously evaluated based on identity, context, and authorization state. |
| **1.2.4** | **Enterprise Gov't Roles and Permissions** | Federate identity attributes and standardize enterprise roles through centralized ICAM services. | Enterprise-wide role consistency improves resilience and authorization management. |
| **1.3.1** | **Organizational MFA/IDP** | Centralize MFA and identity provider integration for critical applications and services. | Critical applications consistently enforce MFA using approved identity and PKI solutions. |
| **1.3.2** | **Alternative Flexible MFA** | Support alternative MFA mechanisms including tokens, biometrics, and self-service capabilities. | Users gain flexible MFA options while maintaining approved security requirements. |
| **1.4.1** | **Implement System and Migrate Privileged Users** | Deploy PAM tooling and migrate supported privileged workflows into managed access systems. | Privileged access becomes centrally governed through PAM-integrated applications and systems. |

<br>
<sub><i>Example DoW Zero Trust activities used as the canonical normalization layer for mapping findings across PingCastle, Microsoft Zero Trust Assessment, and ScubaGear.</i></sub>

#### The Challenge of Implementing Zero Trust in Brownfield Environments

More on my mixed feelings…

It is fair to acknowledge that the roadmap can feel overwhelming, especially for organizations operating in complex brownfield environments. Much of the Zero Trust conversation assumes a level of architectural cleanliness that simply does not exist across large enterprises, particularly within operational technology, legacy mission systems, and hybrid environments built over decades. It also often assumes a level of fiscal flexibility that many organizations simply do not have.

Building a mature Zero Trust architecture is not cheap. Efforts like [DISA Thunderdome](https://ironbow.com/techsource/thunderdome-shifting-dod-cybersecurity-to-zero-trust) demonstrate what large-scale Zero Trust engineering can look like in practice, but they also highlight the reality that significant architectural transformation requires significant investment. Some portions of the framework are naturally more achievable in greenfield cloud-native environments than in operational environments where uptime, interoperability, and mission continuity outweigh the luxury of architectural purity unless organizations are willing and able to fund major structural changes.

Some of us are implementing Zero Trust while also quietly touching our index finger to our nose anytime someone mentions “strategic modernization funding.”

#### When Zero Trust Becomes Compliance Theater

There is also a legitimate concern that Zero Trust can become overly compliance-driven. When a framework expands into 152 distinct activities, organizations naturally begin measuring progress through checklists. I’ve spent more time than I care to admit trying to demonstrate alignment to the framework rather than materially reducing attack paths or operational risk. Complexity itself can become a vulnerability.

#### What Happened to ICAM?

At times, it is worth asking a simple question: what happened to thinking about Zero Trust primarily through the lens of ICAM, Identity, Credential, and Access Management (ICAM)? That question was posed to me last year during a visit to the U.S. Military Academy by the Deputy Chief of Staff, G-6 (the ultimate logistics and implementation boss for the U.S. Army, turning the Army policies into real-world networks used on the move). The question stuck with me because it cut through much of the architectural complexity and forced the conversation back toward fundamentals. Plus, he was pushing back on my definition of Zero Trust since I had the 152 activities in mind. 

#### Identity Still Matters Most

For many practitioners, Zero Trust originally centered on a relatively straightforward principle: strongly verify identities, continuously validate trust, enforce least privilege, and tightly control access to resources. In many ways, identity was the foundation that everything else was built upon.

Today, however, the conversation can sometimes feel as though Zero Trust has evolved into an almost all-encompassing architecture doctrine touching every domain, platform, telemetry stream, and orchestration layer simultaneously. While that expansion reflects the growing complexity of modern environments, it can also dilute focus. There is a risk that organizations pursue sprawling Zero Trust transformation efforts while still struggling with foundational identity governance, privileged access management, asset inventory, or consistent MFA enforcement. In practice, many successful security programs still derive the majority of their defensive value from getting the ICAM fundamentals right. Yes, I’m looking at you! 

#### The Benefits I Still Believe In

And yet, despite the sometime overly complex DoW process, there are important positives that I wont dismiss. The framework provides a common language for modernization across the enterprise. It creates alignment between security, infrastructure, cloud, identity, and operational technology teams that have historically operated in silos. It also moves us away from perimeter-centric assumptions toward continuous verification, visibility, and least privilege, all of which reflect the realities of modern threat activity. Is this not what we need more than ever with the proliferation of AI solutions in our environment?

#### Strategic Alignment vs. Architectural Complexity

Perhaps most importantly, the roadmap forces strategic prioritization. Many organizations have accumulated years of disconnected security tooling without a coherent architectural direction. The DoW ZTA model, despite its complexity, offers a structured framework for thinking about modernization in phases rather than as isolated technology purchases even though organizations that have “successfully achieved” all 152 activities have taken this approach. 

Ultimately, reasonable people can debate whether every activity within the roadmap delivers equal operational value, whether portions of the framework overcomplicate security, or whether some advanced capabilities are realistically attainable at scale. But for those of us operating within the DoW environment, the practical reality is straightforward: this is the framework we are implementing…and no, I’m not saluting as I write this. My hands are on the keyboard. 

Now, I’m forced to find how to implement it pragmatically while balancing mission needs, operational realities, legacy constraints, and measurable risk reduction without losing sight of the reason the framework exists in the first place.

#### The Operational Problem I Am Trying To Solve

Most security teams already run multiple assessments. You might use [PingCastle](https://www.pingcastle.com) to assess Active Directory (AD) exposure and privilege structure. If you’ve made it this far, why do you still have AD? [Microsoft Zero Trust Assessment](https://github.com/microsoft/zerotrustassessment) evaluates cloud identity posture and conditional access maturity. CISA's [ScubaGear](https://github.com/cisagov/ScubaGear) measures policy alignment and configuration drift against secure baselines.

Individually, each tool is valuable, but they create a different problem when all together. Each assessment sees a different slice of the environment and reports independently. The result is fragmented prioritization and teams that spend more time managing reports than understanding system behavior under attack.

For smaller security teams (I can't forget my vCISO colleagues who often use tools like these), this becomes operationally painful very quickly. One engineer understands AD inheritance. Another owns cloud policy. Nobody owns cross-domain attack modeling. But I still need to be able to communicate a coherent risk narrative and measurable progress.

#### Why The DoW ZTA Activity Model Becomes Useful

How do I make lemonade out of mixed feelings? Originally, the instinct was to correlate overlapping findings directly between tools.

PingCastle might flag excessive privilege inheritance. Microsoft Zero Trust might report weak privileged access governance. ScubaGear might surface policy drift affecting administrative controls. However, many duplicate findings were not actually duplicates. They are multiple observations of the same operational weakness appearing across different control planes.

So, instead of correlating findings directly between PingCastle, Microsoft Zero Trust Assessment, and ScubaGear, all findings are normalized into a shared DoW Zero Trust activity model. The ZTA activities become the canonical layer. The tools become evidence sources mapped into that structure.
Now, we’re not asking which tool is correct or which severity score matters most or whether findings should be deduplicated. Instead, we’re focusing on which ZTA activities are weak, how many independent systems surface evidence for that weakness, and what adversary behaviors become possible because of it. We find this to be an operationally useful model.

#### From Tool Overlap to Control Convergence

Once findings are mapped into shared ZTA activities, overlap stops being the important concept. Convergence becomes the important concept. A single finding tied to one ZTA activity may not mean much by itself. But when identity assessments, access-control assessments, and policy assessments independently map into the same operational capability, confidence increases dramatically that the weakness is both real and exploitable. Anchoring findings to ZTA activities instead of directly to vendor outputs creates a more durable prioritization model that can survive tooling evolution over time.

#### What The Data Actually Looks Like

The implementation itself is intentionally simple. Each assessment source maps findings into shared DoW Zero Trust activities. The activity becomes the canonical reference point, while the individual tools become evidence sources attached to that activity.

A simplified example looks like this:

```json
{
  "ztaCapabilityId": "1.1.1",
  "msZeroTrust": [],
  "scubaGear": [
    {
      "match": {
        "testId": "MS.AAD.6.1v1",
        "title": "User passwords SHALL NOT expire."
      },
      "ztaCapabilityId": "1.1.1"
    },
    {
      "match": {
        "testId": "MS.EXO.5.1v1",
        "title": "SMTP AUTH SHALL be disabled."
      },
      "ztaCapabilityId": "1.1.1"
    }
  ],
  "pingCastle": []
}
```
<sub><i>Example of a real ZTA activity mapping. Activity <code>1.1.1</code> currently maps to multiple ScubaGear findings, while no corresponding mappings exist from Microsoft Zero Trust Assessment or PingCastle for this activity. Not all ZTA activities receive equal coverage across assessment sources.</i></sub>

Normalization leads to simplification. Instead of treating PingCastle, Microsoft Zero Trust Assessment, and ScubaGear as disconnected reports, all findings now resolve into a shared operational capability model. The ZTA activity becomes the common language between identity, access, policy, and configuration assessments.This is not meant to replace threat modeling or detection engineering. It creates a cleaner foundation for those activities later. 

It is also important to acknowledge what this model does not do. Mapping findings into ZTA activities does not mean every activity within the DoW Zero Trust Architecture is fully represented or validated. These mappings only reflect what the underlying assessment sources are capable of observing. Some ZTA activities naturally align well with identity, access, and policy assessment tooling. Others do not. Entire portions of the framework may have little or no visibility through PingCastle, Microsoft Zero Trust Assessment, or ScubaGear because those tools were never designed to measure every operational capability inside the roadmap.

This means the model is best understood as a normalization and visibility layer rather than a complete representation of Zero Trust maturity. It helps organizations understand where multiple assessment sources converge, where operational weaknesses repeatedly surface, and where additional visibility may still need to be engineered.

#### Why This Matters Operationally

This simplifies remediation. Instead of chasing isolated findings, teams work against capability failures. Ownership becomes clearer. We actually assign findings to teams who then assign them to engineers within their teams. Prioritization now becomes defensible. It also makes it easier to explain when illustrating ***one way*** we are tackling the DoW ZTA mandate.

#### What Small Teams Gain From This

Smaller teams rarely win through scale. Clarity is key to succeed. The aim of this model is to reduce fragmentation (and fatigue). It reduces duplicated effort. It creates a shared operational language between our teams. Most importantly, it gives us a way to prioritize realistically. Not every Zero Trust activity matters equally in practice. Not every assessment finding deserves equal urgency. But when multiple independent systems converge on the same operational weakness, that is usually where attention belongs first.

#### How This Works in Practice

The implementation behind this model is intentionally lightweight.

At a high level, the workflow looks like this:

![DoW ZTA normalization and convergence workflow](/assets/blogs/zta-control-convergence/workflow.png)

<sub><i>Figure 1. PingCastle, Microsoft Zero Trust Assessment, and ScubaGear findings normalize into shared DoW Zero Trust activities, creating a common operational reference layer across identity, access, and policy assessment domains.</i></sub>

1. Run the native assessment tools normally.
2. Export the resulting HTML reports.
3. Run the post-processing script against the reports.
4. Normalize findings into shared ZTA activity mappings.
5. Review where multiple assessment sources converge on the same operational capability.


#### Closing

PingCastle, Microsoft Zero Trust Assessment, and ScubaGear are not competing viewpoints. They are partial observations of the same environment under stress.The DoW Zero Trust activity model provides something these tools cannot provide independently: a stable operational structure capable of normalizing fragmented findings into a coherent capability model.

That does not solve every problem inside Zero Trust modernization. The framework is still large. The operational realities are still messy. Legacy systems still exist. ICAM fundamentals still matter more than many organizations are willing to admit. But if the framework is going to succeed operationally, organizations need a way to translate fragmented assessment data into actionable defensive understanding.

---

For the source, see the [GitHub repo](https://github.com/Empirical-Defense/zta-convergence)