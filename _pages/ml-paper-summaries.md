---
layout: default
permalink: /ml-paper-summaries/
title: ml paper summaries
nav: true
nav_order: 2
---

<div class="post">
  <h1>ML paper summaries</h1>

  <p>
    Summaries and personal thoughts of papers (mostly in machine learning) I have read in detail.
  </p>

  <!-- <p>
    This page is both a catalog of my reading progress and a place for others to find
    interesting papers to check out.
  </p> -->

  {% comment %}
    Match tag/category name even when front matter uses semicolon-separated strings
    (one YAML string) — join first, then substring check.
  {% endcomment %}
  {% assign sorted_posts = site.posts | sort: "date" | reverse %}
  {% assign paper_count = 0 %}

  <hr>
  <ol>
    {% for post in sorted_posts %}
      {% assign tag_str = post.tags | join: " " %}
      {% assign cat_str = post.categories | join: " " %}
      {% if tag_str contains 'ml-paper-summary' or cat_str contains 'ml-paper-summary' %}
        {% assign paper_count = paper_count | plus: 1 %}
        <li>
          {% if post.redirect == blank %}
            <a href="{{ post.url | relative_url }}">({{ post.date | date: '%b %-d, %Y' }}) {{ post.title }}</a>
          {% elsif post.redirect contains '://' %}
            <a href="{{ post.redirect }}" target="_blank">({{ post.date | date: '%b %-d, %Y' }}) {{ post.title }}</a>
          {% else %}
            <a href="{{ post.redirect | relative_url }}">({{ post.date | date: '%b %-d, %Y' }}) {{ post.title }}</a>
          {% endif %}
        </li>
      {% endif %}
    {% endfor %}
  </ol>

  {% if paper_count == 0 %}
    <p>No ML paper summaries yet. Add <code>ml-paper-summary</code> to a post tag/category to include it here.</p>
  {% endif %}
</div>
