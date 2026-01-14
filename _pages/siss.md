---
title: "SISS Study"
permalink: /siss/
layout: archive
entries_layout: list
author_profile: true
sidebar:
  nav: "siss"
---
{% assign docs = site.siss | sort: "date" | reverse %}
{% assign weeks = docs | map: "week" | uniq | sort | reverse %}

{% for w in weeks %}
<h2 class="archive__subtitle">Week {{ w }}</h2>

<div class="entries-{{ page.entries_layout | default: 'list' }}">
  {% assign items = docs | where: "week", w | sort: "part" %}
  {% for post in items %}
    {% include archive-single.html type=page.entries_layout %}
  {% endfor %}
</div>

{% endfor %}
