---
layout: default
title: "Economics, Decision-Making and AI"
permalink: /research-agenda/
---

<main class="wrap">

<h1>Economics, Decision-Making &amp; AI</h1>

<p>
Intelligent agents &mdash; people, firms, institutions, and increasingly AI
systems &mdash; make decisions using incomplete information and imperfect
representations of the world. At the same time, the institutions around
them try to shape those decisions: through the information they provide,
the incentives they set, the monitoring they impose, and the constraints
they enforce. My research asks how these two forces interact. I come to
this question as an economist, using tools from structural econometrics,
causal inference, and economic theory, and increasingly from machine
learning. The same question that has motivated my work on consumer choice
and healthcare markets &mdash; what does a decision-maker actually see and
use, as opposed to what it could in principle optimize &mdash; turns out to
be just as central to understanding AI systems.
</p>

<section class="section">
<h2>1. Human and machine decision-making</h2>
<p>
A decision-maker rarely optimizes over the full choice set or the full
information available to it. In my work on hospital choice, patients
choose from a limited consideration set rather than every available
option, shaped by what they have seen, heard, or been told. Modeling this
limited consideration, and estimating how it responds to information and
incentives, has been a central part of my empirical work on healthcare
markets.
</p>
<p>
The same question arises, in a different form, for AI systems. A language
model may encode information relevant to a decision &mdash; a comparison,
a preference, a risk estimate &mdash; without that information actually
driving its output. Interpretability research often asks whether a
variable is represented inside a model; the question I am most interested
in is whether that representation is behaviorally relevant, in the same
sense that a piece of information is only economically relevant to a
consumer if it changes what they choose. Bringing consideration-set and
limited-attention models from economics into contact with interpretability
tools is one direction I am pursuing.
</p>
</section>

<section class="section">
<h2>2. Incentives, monitoring, and AI agents</h2>
<p>
Once a decision-maker's actions cannot be observed directly, whoever is
training or evaluating it faces a classic principal-agent problem: which
observable signals actually tell you something about the hidden behavior
you care about, and which do not. This question has a long history in
information economics &mdash; Holmstrom's informativeness principle,
multitask contracting, and the broader literature on mechanism design
under limited observability.
</p>
<p>
I think this literature has a lot to offer AI evaluation and monitoring,
where similar structure recurs: models are trained and graded on
observable outcomes and traces, evaluators face the same question about
which signals are informative, and, under enough optimization pressure,
models can learn to make their behavior look better on the measured signal
without actually improving on the dimension the signal was meant to proxy
for. I work through some of these connections informally in a personal
essay series in my journal, <a href="{{ '/journal/' | relative_url }}">Alignment
&times; Econ</a> &mdash; for example, using the informativeness principle
to think about outcome versus process supervision, and using ideas from
multitasking contracts to think about reward hacking and Goodhart's law.
This is a research direction rather than a settled result: the aim is to
see how far these classic contracting ideas travel, and where they break
down, when the agent is a model rather than a person.
</p>
</section>

<section class="section">
<h2>3. Markets, allocation, and distribution shift</h2>
<p>
A second strand of my work studies how markets and institutions allocate
scarce resources &mdash; hospital capacity, subsidized health products,
spectrum, spots in a matching market &mdash; and how the design of prices,
subsidies, and rules shapes who gets what. This has mostly been empirical
work in healthcare and Indian development contexts, using causal inference
and structural methods to evaluate specific policies.
</p>
<p>
I am increasingly interested in optimal transport as a tool for this kind
of question, because it gives a natural language for describing how a
distribution of outcomes shifts in response to a policy, a price, or a
change in the environment. That connects naturally to allocation problems
in more classical economic settings, but also to AI settings, where
systems increasingly participate in markets and decision pipelines and
where the distributions they act on shift over time. Matching, allocation,
and distribution shift are, at some level of abstraction, the same
problem.
</p>
</section>

<section class="section">
<h2>A common thread</h2>
<p>
The common thread across these three directions is a distinction I keep
returning to: the objective a decision-maker could in principle optimize,
the information or representation it actually uses, the incentives
shaping its behavior, and the allocation or outcome that results.
Economics has spent decades building tools for reasoning about the gaps
between these &mdash; limited information, moral hazard, mechanism design,
market design. My work asks how far those tools extend to a world where
the decision-maker is sometimes a person, sometimes a firm or institution,
and increasingly an AI system, and where the gaps between what could be
optimized and what is actually used, observed, or rewarded matter more
than ever.
</p>
</section>

</main>
