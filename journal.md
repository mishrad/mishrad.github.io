---
layout: default
title: Journal
permalink: /journal/
---

<main class="wrap">

<h1>Journal</h1>

<p style="color: var(--ink-soft);">
Notes on research in progress, papers I am reading, and things I am learning.
Written to think, not to conclude.
</p>

<ul class="post-list">
{% for post in site.posts %}
  <li>
    <span class="post-date">{{ post.date | date: "%B %-d, %Y" }}</span>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    {% if post.summary %}<p class="post-summary">{{ post.summary }}</p>{% endif %}
  </li>
{% endfor %}
</ul>

</main>
