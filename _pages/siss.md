---
title: "SISS Study"
permalink: /siss/
layout: archive
entries_layout: list
author_profile: true
sidebar:
  nav: "siss"
---
*본 페이지는 주차별로 정렬된 페이지입니다. 도메인 별 정렬을 원하신다면 좌측의 태그를 클릭해주세요.*

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
