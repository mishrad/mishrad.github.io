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

<section class="section">
<h2>Alignment &times; Econ</h2>

<p style="color: var(--ink-soft);">
Essays and experiments connecting classic ideas from economics &mdash;
incentives, information, contracts, mechanism design, and strategic
behavior &mdash; to the problem of building and evaluating AI systems.
</p>

<div class="paper">
  <p class="paper-title"><a href="{{ '/journal/2026/09/02/alignment-econ1/' | relative_url }}">Part 1: You Can't Reward What You Can't See</a></p>
  <p class="paper-abstract">
    What a classic 1979 economics paper can teach us about monitoring and
    aligning modern AI systems.
  </p>
</div>

<div class="paper">
  <p class="paper-title" style="color: var(--ink-soft);">Part 2: Coming soon</p>
</div>

</section>

<section class="section">
<h2>Notes</h2>

<ul class="post-list">
{% for post in site.posts %}
  {% unless post.url contains 'alignment-econ1' %}
  <li>
    <span class="post-date">{{ post.date | date: "%B %-d, %Y" }}</span>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    {% if post.summary %}<p class="post-summary">{{ post.summary }}</p>{% endif %}
  </li>
  {% endunless %}
{% endfor %}
</ul>

</section>

</main>
