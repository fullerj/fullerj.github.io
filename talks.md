---
layout: page
title: Talks
permalink: /talks/
description: >
  This section features a selection of conference presentations, invited talks, and panel discussions delivered at academic, professional, and public forums. 
---

{% assign talks = site.data.talks | sort: "date" | reverse %}

{% if talks and talks.size > 0 %}
{% for talk in talks %}
### {{ talk.title }}
**Event:** {{ talk.event }}{% if talk.location %}, {{ talk.location }}{% endif %}  
**Date:** {% if talk.date %}{{ talk.date | date: "%B %-d, %Y" }}{% else %}TBA{% endif %}

{% if talk.description %}
{{ talk.description }}
{% endif %}

{% if talk.resources %}
**Resources:**{% for resource in talk.resources %}
- [{{ resource.label }}]({{ resource.url }})
{% endfor %}
{% endif %}

---
{% endfor %}
{% else %}
No talks have been published yet. Check back soon.
{% endif %}
