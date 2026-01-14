---
title: "Web"
permalink: /siss/web/
layout: single
author_profile: true
sidebar:
  nav: "siss"
---

{% assign docs = site.siss | where: "domain", "web" | sort: "date" | reverse %}

<div class="entries-list">
  {% for post in docs %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>
