---
layout: default
title: Work
permalink: /work/
description: "Applied work on pricing, policy evaluation, allocation, and AI decisions, with methods, findings, and available papers and demonstrations."
---

<main class="wrap">

<h1>Work</h1>
<p>
I use economic models, causal inference, and machine learning to answer
questions about pricing, policy, and AI behavior. These examples describe
what I worked on, how I approached it, and what the evidence supports.
The <a href="{{ '/research/' | relative_url }}">Research page</a> has the
full academic record.
</p>
<p class="paper-links">
  <a href="#pricing">Pricing</a>
  <a href="#policy-allocation">Policy &amp; allocation</a>
  <a href="#ai-decisions">AI decisions</a>
  <a href="#agent-collusion">Agent collusion</a>
</p>

<section class="section" id="pricing">
<h2>Demand modeling for pricing decisions</h2>
<p class="paper-meta">Amazon Retail Pricing &middot; Economist Intern &middot; May&ndash;August 2023</p>
<p><strong>Question.</strong> How can product-level demand estimates help a retailer evaluate discounts and bundle prices?</p>
<p><strong>My contribution.</strong> I combined product-text embeddings with gradient boosting to forecast demand, evaluated pricing alternatives with demand models, and presented recommendations internally.</p>
<p><strong>Data and methods.</strong> Product-level demand and text features; machine learning and comparisons with standard logit demand models.</p>
<p><strong>Result.</strong> Forecast RMSE fell by 5% relative to standard logit models. Pricing recommendations were presented to inform future A/B tests; the reported improvement measures forecast accuracy.</p>
<p><strong>Practical relevance.</strong> Better demand forecasts provide an input to comparing pricing choices. The internship work evaluated those choices with the objective of improving long-term customer revenue.</p>
<p class="paper-links"><a href="{{ site.resume | relative_url }}" aria-label="View tech resume PDF">Resume</a></p>
</section>

<section class="section" id="policy-allocation">
<h2>Measuring policy effects and informing allocation</h2>
<p class="paper-title">Pennies for Pads: Subsidized Menstrual Health Products and Adoption in India</p>
<p class="paper-meta">With <a href="https://ritikasethi.com/">Ritika Sethi</a> &middot; Under review</p>
<p><strong>Question.</strong> Does making low-cost menstrual products available through subsidized pharmacies increase adoption, and how might pharmacy locations affect access?</p>
<p><strong>My contribution.</strong> With Ritika Sethi, I study the effects of the Suvidha program. The interactive companion lets users explore pharmacy-siting choices alongside the paper's estimates.</p>
<p><strong>Data and methods.</strong> Linked administrative, survey, and geospatial data covering nearly 500,000 women aged 15&ndash;24. We use doubly robust difference-in-differences to study the staggered pharmacy rollout.</p>
<p><strong>Finding and status.</strong> The paper estimates a 9.6 percentage-point increase in use of locally prepared or sanitary napkins. The manuscript is under review.</p>
<p><strong>Practical relevance.</strong> The planner illustrates the tradeoff between predicted adoption gains and reaching underserved areas. Its default candidate locations are illustrative; users can supply subdistrict estimates through a CSV.</p>
<p class="paper-links"><a href="{{ '/assets/files/RD_PMBJP.pdf' | relative_url }}">Paper</a> <a href="{{ '/pmbjp/' | relative_url }}">Interactive planner</a> <a href="{{ '/research/' | relative_url }}#pennies-for-pads">Research entry</a></p>
</section>

<section class="section" id="ai-decisions">
<h2>Understanding and evaluating AI decisions</h2>
<p class="paper-title">Decodable but Unused: What Language Models Represent (and Ignore) When Choosing Between Two Options</p>
<p class="paper-meta">Dibya Deepta Mishra &middot; Accepted, ALTA 2026</p>
<p><strong>Question.</strong> Does information that can be recovered from a language model's hidden states actually affect its choices?</p>
<p><strong>My contribution.</strong> I designed controlled insurance and coffee-choice tasks and used probes and activation-based tests to examine decision-relevant representations.</p>
<p><strong>Data and methods.</strong> Two-option choice prompts with experimenter-defined preferences, hidden-state probes, control tasks, and activation interventions.</p>
<p><strong>Finding and status.</strong> Probes can recover the utility-maximising option even when the tested models' choices remain near chance. The paper is accepted for archival publication at the Australasian Language Technology Association Workshop (ALTA), 2026.</p>
<p><strong>Practical relevance.</strong> A recoverable signal alone is insufficient evidence that a model uses that information when making a decision. Evaluation needs to examine behavior as well as representations.</p>
<p class="paper-links"><a href="{{ '/assets/files/decodable-but-unused.pdf' | relative_url }}" aria-label="View Decodable but Unused draft PDF">Draft</a> <a href="{{ '/research/' | relative_url }}#utility-maximizing-choice">Research entry</a></p>
</section>

<section class="section" id="agent-collusion">
<h2>Detecting collusion between AI agents</h2>
<p class="paper-meta">With Amit Balter, Chengxiao Wang, Fan Wu, and Michael Ball &middot; Apart Research sprint report &middot; October 2026</p>
<p><strong>Question.</strong> How can evaluators identify collusion when AI agents interact repeatedly and review each other's work?</p>
<p><strong>My contribution.</strong> I contributed to experiment design, the Python labeling pipeline using Modal, and writing.</p>
<p><strong>Data and methods.</strong> The team studied behavioral and conversational evidence across 1,800 agent runs and 18,000 episodes, evaluating a rule-plus-LLM classifier against 342 annotated episodes.</p>
<p><strong>Result and status.</strong> The coauthored sprint report examines collusion intensity, onset, and reciprocity. It is a separate project from the ALTA paper.</p>
<p><strong>Practical relevance.</strong> The analysis helps examine how evidence from agents' conversations compares with evidence from their behavior in repeated interactions.</p>
<p class="paper-links"><a href="https://github.com/fw2274/detecting-collusion-btw-ai-agents">Code &amp; project documentation</a> <a href="{{ '/journal/2026/10/25/detecting-collusion/' | relative_url }}">Sprint notes</a> <a href="{{ '/research/' | relative_url }}#detecting-collusion">Research entry</a></p>
</section>

</main>
