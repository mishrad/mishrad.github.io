---
title: "Learning PIE Through Headline Experiments"
date: 2026-11-04
category: thoughts-on-papers
summary: "Working through PIE with Upworthy experiments to see whether headline wording helps predict differences in click-through rates."
---

[Code on GitHub](https://github.com/mishrad/pie-headlines)

[Predicted Incrementality by Experimentation (PIE)](https://doi.org/10.3386/w35044), by Brett Gordon, Robert Moakler, and Florian Zettelmeyer, is a nice combination of economics, marketing, and computer science. It starts with a question advertisers face: did the ad change anyone’s behavior? Someone who saw an ad and bought the product might have bought it anyway. *Incrementality* is the extra behavior the ad caused, such as purchases that would not have happened without it. This is hard to untangle when ads target people who are already more likely to buy. Randomized experiments can help, but running one for every campaign is costly.

The authors ask a simple question: can we learn from the experiments we already ran to estimate effects for campaigns where we didn’t run one? They use randomized experiments to learn how campaign characteristics relate to the effects the ads caused. They then use that relationship to estimate effects for other campaigns, using information that can include metrics collected after a campaign ends.

I’ve worked with language model embeddings in my research and as an intern at Amazon. An embedding is a list of numbers a model uses to represent text. That made me curious about the ad *creative*, meaning the words and images people see. Could the wording of an ad, or eventually its text and images together, help predict its effect? I wanted to try a small exercise before committing to a larger research project.

I found some useful public data in Upworthy’s headline experiments. Given two headlines for the same article, could their wording help predict the difference in click-through rates? I used the headline data to explore that idea. A change in headline clicks measures something different from the extra purchases or other actions an ad campaign causes.

## Building the sample

The [exploratory file](https://osf.io/download/3vqmp/) contains 4,873 experiments. I excluded any experiment with a variant (a version shown to readers) dated between June 25, 2013 and January 10, 2014, following the [archive authors&#39; warning](https://upworthy.natematias.com/2024-06-upworthy-archive-update.html) about possible randomization problems during that period.

Within each experiment, I looked for distinct headlines with the same image and article excerpt. I chose one pair per experiment using a rule based on a deterministic hash. Running the rule again on the same data gives the same pair. The rule did not use clicks, impressions (how many times a headline was shown), or the recorded winner. That left me with about 1,900 pairs.

I called the headlines A and B according to their order, without using which one won. The click-through rate (CTR) is clicks divided by impressions. With the rates written as fractions, the difference I wanted to predict was:

`100 × (CTR(B) − CTR(A))`

Suppose A gets a 3% click-through rate and B gets 3.4%. The observed lift is 0.4 percentage points. Randomly assigning which headline people see helps us interpret the difference as an effect of showing one headline instead of the other. The model does not tell us the causal effect of any individual word.

Splitting the data also needed some care. Different experiments can contain the same headline or article. If related content appears in both training and testing, a model might get credit for predicting something it has effectively already seen. This is called *content leakage*.

I linked experiments that shared a headline after basic text cleanup or had the same article identifier in their URLs. I checked all the cleaned versions in each experiment and kept each group of linked experiments together. That gave me 1,172 experiments to fit the models (training), 397 to compare models before the final check (validation), and 361 set aside for that final check (testing). Grouping reduces leakage, although it cannot catch every related article. All three sets come from the public exploratory file.

## What I fitted

I compared two ways to represent each headline. TF-IDF (term frequency–inverse document frequency) counts words and two-word combinations, giving less weight to terms that are common across headlines. I used only the training headlines to choose the vocabulary and calculate the weights. The other method, [all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2), turns the whole headline into an embedding of 384 numbers, scaled so every embedding has the same length. I kept MiniLM frozen, meaning I used it as it came and trained a separate model to predict lift.

In both cases, I subtracted A’s features from B’s and fitted a ridge regression, a linear model that discourages large weights. I left out the intercept, the fixed amount a model would otherwise add to each prediction. That way, switching A and B reverses the predicted lift.

MiniLM had the lower error on the validation set. I then evaluated both models on the test set using the original training fits, without retraining on the validation data. The table also shows two simple benchmarks: always predict zero lift, or always predict the average lift from the training set. RMSE (root mean squared error) measures prediction error in percentage points and gives larger mistakes more weight. Lower is better.

| Model             | Test RMSE, percentage points |
| ----------------- | ---------------------------: |
| Zero lift         |                       0.5701 |
| Training mean     |                       0.5701 |
| TF-IDF plus ridge |                       0.5635 |
| MiniLM plus ridge |             **0.5447** |

MiniLM reduced prediction error relative to TF-IDF by 0.01878 percentage points, or **3.33%**. That is a reduction in error, not an increase in clicks. To see how sensitive the comparison was to which groups were in the test set, I used a *paired bootstrap*. I drew 2,000 resamples from the 331 test content groups, allowing a group to be drawn more than once. I compared both models on the same groups in each resample. The approximate 95% interval for the relative reduction was **1.35% to 5.41%**.

The improvement was modest, but it made me want to try the comparison on more data. The observed click gaps can fluctuate because each experiment has a finite number of views. And because the bootstrap holds the fitted models fixed, the interval doesn’t capture how the comparison might change if I retrained the models.

I prepared the data with Spark on one AWS virtual machine (EC2) and trained the models in a SageMaker notebook using a CPU. I moved the files between the two by uploading and downloading them. Part of the exercise was to get familiar with that setup before trying it on more data.

## What I want to try next

I am still unsure how the comparison would behave with a larger and more varied dataset. My next step is to evaluate the models as they are on more Upworthy experiments, after removing examples that overlap with content I’ve already used. If you know of other datasets, feel free to email me.

The question that brings me back to PIE is whether the ad itself adds useful information beyond the campaign metrics already available. Do the words and images help predict what the ad caused? I’d like to test that with the text first and then with text and images together.

Embeddings improved predictions of the observed gap in click-through rates on the test set. Whether that carries over to larger datasets and ads is something I’d like to find out.

---

*Data: [The Upworthy Research Archive](https://doi.org/10.1038/s41597-021-00934-7), Matias, Munger, Aubin Le Quéré and Ebersole (2021), licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). This project filters the data and constructs paired comparisons and predictions.*
