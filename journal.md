---
layout: default
title: Journal
permalink: /journal/
---

<main class="wrap">

<h1>Journal</h1>

<p style="color: var(--ink-soft);">
Notes on economics, AI, and things I am working through.
</p>

<section class="section">
<h2>Alignment &times; Econ</h2>

<div class="paper">
  <p class="paper-title"><a href="{{ '/journal/2026/09/02/alignment-econ1/' | relative_url }}">Part 1: You Can't Reward What You Can't See</a></p>
  <p class="paper-abstract">
    What a classic 1979 economics paper can teach us about monitoring and
    aligning modern AI systems.
  </p>
</div>

</section>

<section class="section">
<h2 id="hackathons">Hackathons</h2>

<ul class="post-list">
{% for post in site.posts %}
  {% if post.categories contains 'hackathons' %}
  <li>
    <span class="post-date">{{ post.date | date: "%B %-d, %Y" }}</span>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    {% if post.summary %}<p class="post-summary">{{ post.summary }}</p>{% endif %}
  </li>
  {% endif %}
{% endfor %}
</ul>

</section>

<section class="section">
<h2>Notes</h2>

<ul class="post-list">
{% for post in site.posts %}
  {% unless post.url contains 'alignment-econ1' %}
  {% unless post.categories contains 'hackathons' %}
  <li>
    <span class="post-date">{{ post.date | date: "%B %-d, %Y" }}</span>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    {% if post.summary %}<p class="post-summary">{{ post.summary }}</p>{% endif %}
  </li>
  {% endunless %}
  {% endunless %}
{% endfor %}
</ul>

</section>

</main>
