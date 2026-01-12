---
title: "Archiving Posts"
permalink: /archiving/
layout: single
author_profile: true
sidebar:
  nav: archiving
---

 
{% assign posts = site.posts | where_exp: "post", "post.categories contains 'archiving'" %}
{% assign all_tags = posts | map: "tags" | join: "," | split: "," | uniq | sort %}

{% for tag in all_tags %}
  <h2 id="{{ tag | slugify }}">📚 {{ tag }}</h2>
  <div class="entries-list">
    {% for post in posts %}
      {% if post.tags contains tag %}
        {% include archive-single.html type="list" %}
      {% endif %}
    {% endfor %}
  </div>
  <hr>
{% endfor %}