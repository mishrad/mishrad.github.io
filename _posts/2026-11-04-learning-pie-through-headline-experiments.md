---
title: "Learning PIE Through Headline Experiments"
date: 2026-11-04
category: paper-replications
summary: "Working through PIE with Upworthy experiments, and testing whether headline embeddings help predict click lift."
---

*Paper replication notes.* [Code on GitHub](https://github.com/dibyadeeptamishra/pie-headlines)

[PIE, by Brett Gordon, Robert Moakler and Florian Zettelmeyer](https://doi.org/10.3386/w35044) is a nice combination of econ, marketing, and CS. Specifically, it looks at at a problem advertisers deal with - did an ad change someone's behavior? It is not ex-ante clear. Somebody who saw an ad and bought the product might have bought it anyway. This is particularly difficult to untangle when ads target people more likely to buy. While experiments can be very helpful here, running one for every campaign is costly.

The authors ask a simple question- Can we reuse the experiments we have already run to learn about campaigns we want to run. They use experimental results to learn a relationship between campaign characteristics and incremental effects, then use that relationship to make predictions for other campaigns.

I have previously worked with language-model embeddings for my research as well as an intern at Amazon, so this made me curious about whether information in the creative itself could add something to that approach. Could the wording of an ad, or eventually its text and images together, help predict its effect? I wanted to try a small exercise to work through that question before commiting to a research project. 

I found some nice public data in Upworthy's headline experiments. Given two headlines for the same article, could their wording help predict the difference in click-through rates? While, that is a different outcome from advertising incrementality,I want to understand the paper better and explore an ingredient that might be useful in a broader extension.

## Building the sample

The [exploratory file](https://osf.io/download/3vqmp/) contains 4,873 experiments. I excluded any experiment with a variant dated between June 25, 2013 and January 10, 2014, following the [archive authors&#39; warning](https://upworthy.natematias.com/2024-06-upworthy-archive-update.html) about possible randomization problems during that period.

Within each experiment, I looked for distinct headlines with the same image ID and excerpt. I then used a deterministic hash to choose one pair per experiment. I made sure that the outcomes - Clicks, impressions and the recorded winner had no role in choosing the pair which left me with ~1900 observations.

I called the headlines A and B according to their order, without using which one won. The outcome I wanted to predict was:

`100 × (CTR(B) − CTR(A))`

Suppose A gets a 3% click-through rate and B gets 3.4%. The observed lift is 0.4 percentage points. Random assignment supports interpreting the difference as an experimental effect but the model does not tell us the causal effect of an individual word.

Splitting the data also needed some care. Different experiments can contain the same headline or article. If related content appears in both training and testing, a model might look better than it actually is. 

I linked experiments that shared normalized headlines or article slugs, checking all their cleaned variants, and kept each connected group in the same split. That produced 1,172 training experiments, 397 for validation and 361 for testing. These links reduce content leakage, although they cannot catch every related article. All three splits come from the public exploratory file.

## What I fitted

I compared TF-IDF with frozen MiniLM embeddings. TF-IDF represents words and two-word combinations, with the vocabulary and weights fitted only on the training headlines. The embedding model, [all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2), represents each headline as a normalized vector of 384 numbers. I used it without fine-tuning (would not suggest for actual work).

In both cases, I subtracted A's features from B's and fitted ridge regression without an intercept. This gives the prediction a useful property: switching the two headlines reverses the predicted lift. During validation, I found MiniLM to have the lower error. I then evaluated the original training fits on the test set, without refitting them on the combined training and validation data.

| Model             | Test RMSE, percentage points |
| ----------------- | ---------------------------: |
| Zero lift         |                       0.5701 |
| Training mean     |                       0.5701 |
| TF-IDF plus ridge |                       0.5635 |
| MiniLM plus ridge |             **0.5447** |

MiniLM reduced prediction error relative to TF-IDF by 0.01878 percentage points, or **3.33%**. Across 2,000 paired bootstrap resamples of the test set's 331 content groups, the approximate 95% interval for the relative reduction was **1.35% to 5.41%**.

While this was a modest improvement, it was a useful pilot to give me confidence that we could do something more with it. However, there are limits to what this comparison tells us. The labels are noisy experimental estimates. The bootstrap holds the fitted models fixed, so it leaves out uncertainty from retraining them. 

I used Spark on one EC2 machine for preparation and a SageMaker notebook on CPU for modeling, moving files through uploads and downloads. I did this so that when I run this experiment on larger data, I am prepared. 

## What I want to try next

I am still unsure how the comparison would behave with a larger and more varied dataset. My next step is to evaluate the frozen models on more Upworthy experiments, removing related-content overlap first. I am also thinking about more datasets so feel free to suggest on my email. 

The question that brings me back to PIE is whether creative features add useful information beyond the campaign metrics already available. Do the words and images in an ad tell us something about its incremental effect? My earlier experience with embeddings makes me interested in testing that, first with creative text and then with multimodal inputs that combine text and images.

Embeddings improved prediction of observed headline click lift in this held-out sample. Whether that carries over to larger datasets and advertising creative is something I would like to find out.

---

*Data: [The Upworthy Research Archive](https://doi.org/10.1038/s41597-021-00934-7), Matias, Munger, Aubin Le Quéré and Ebersole (2021), licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). This project filters the data and constructs paired comparisons and predictions.*
