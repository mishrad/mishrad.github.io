---
title: "You Can't Reward What You Can't See"
date: 2026-09-02
summary: "What a classic 1979 economics paper can teach us about monitoring and aligning modern AI systems."
---

Consider the following situation, a manager asks his reportee to do something. They only observe the outcome but not everything that the reportee does to get to the outcome. The manager might see some see signals that provide some intuition into what their reportee does such as some intermediate steps, progress reports etc. 

Sounds familiar! 

This is remarkably close to what an AI interaction looks like today. A human asks an AI model to do something, but cannot directly observe everything that matters about how the model arrives at the answer. Instead, we see signals. A tool trace, some benchmark scores, human rating, tokens used, the final answers etc. 

An interesting question arises. How should these signals be used in training or evaluating a model? Which signals actually tell us something useful about whether the model behaves in a way that we want?

A version of this question was asked almost half a century ago by Bengt Holmstrom in 1979 in his seminal paper: *Moral Hazard and Observability*. How should a principal (human) motivate (train) an agent (model) if they cannot observe the agent's actions (model behavior) directly? 

In the language of AI alignment, we can tentatively (not exactly) translate the principal into the human and the agent into the model. So let's try to distill (pun intended) what a paper on contracts half a century ago can teach us about aligning modern AI systems.


## The Core Economics
Start with a simple principal-agent problem. 

A principal cares about a desirable outcome $x$. The outcome is dependent on two things $x=f(a,\theta)$ where $a$ is an action chosen by the agent and $\theta$ represents factors outside the agent's control. 

The agent, meanwhile, chooses $a$ to maximize their own utility. While a higher effort may improve the expected outcome, it is costly. The principal therefore tries to design a payment rule that rewards observable performance in a way that induces the agent to make effort. The problem is that a good outcome does not necessarily imply a good action. 

### A simple example
Suppose I manage a salesperson.

I would like the salesperson to work hard, but I cannot follow them around all day observing how many potential customers they contact, how carefully they prepare for meetings, or how diligently they follow up on leads.

What I can observe is their sales.

Suppose I observe that they generated $100,000 in sales this quarter. Did they work hard?

Maybe.

But sales depend on more than effort. Perhaps demand happened to be unusually strong. A salesperson who barely worked in a booming market might generate \$100,000, while someone who worked extremely hard during a terrible quarter might generate only \$80,000.

The manager sees sales, but what they really want to incentivize is effort. 

Now suppose the manager observes another signal: overall demand in the salesperson's region.

That signal does not directly tell us how hard the salesperson worked. But it changes how we interpret their sales.

\$100,000 of sales during an enormous market boom is one thing.
\$100,000 of sales while the entire regional market is collapsing is quite another.

Market conditions therefore provide information about the employee's hidden action over and above the information contained in sales alone.

This brings us to the crux of Holmstrom's question

*When does an additional signal tell us something about the agent's action that we could not already infer from the outcome?*

### The Informativeness Principle

Suppose the principal already observes the output $x$, and is considering whether to use an additional signal $y$. It all boils down to whether $y$ tells us something about the agent's action that $x$ does not tell us. Mathematically speaking, if the conditional distribution of $y$ given $x$ changes with $a$, then Holmstrom shows that $y$ is a useful indicator about the agent's effort. In the example above, market conditions provide information about the employee's hidden action. 

Concretely, let $f(y\mid x,a)$ be the conditional distribution of $y$ given $x$ and $a$. If $f(y\mid x,a)=f(y\mid x)$, then learning $y$ tells us nothing about $a$ beyond what is implied by $x$ and hence there is no benefit from conditioning the incentive contract on $y$. Conversely if $f(y\mid x,a)$ depends on $a$, then $y$ provides additional information about the hidden action and can potentially improve the contract. 

This result is often summarized as the informativeness principle: a performance measure is useful for incentives when it provides information about the agent's action beyond what is already contained in other measures. Holmstrom later summarized the intuition as using information that is informative about the agent's choice of action and excluding information that is not.

Hence, market conditions are a good signal but something unrelated such as rain in Tokyo might not be useful (although proponents of the butterfly effect would disagree). This is obvious but the distinction between useful and not so useful information becomes very interesting when the agent is an AI system. 

## That is cool! But what does this have to do with AI alignment?

Suppose an AI system gives the correct final answer. Does that tell us everything we need to know about how it behaved?

Probably not.

Two models might produce exactly the same final answer while taking very different paths to get there. One might use a robust strategy; another might exploit a shortcut that happens to work on this particular task. If we can observe additional signals: a tool trace, intermediate verification results, or some other piece of observable behavior, those signals may help us distinguish between the two.

Hence, the analogous question becomes *Given that the action we ultimately care about is hidden, what observable information should we use to evaluate and incentivize the agent?*

I do not claim that that an AI model literally behaves like an employee maximizing wages. Nor should we interpret the hidden action $a$ as literally corresponding to every neural activation inside the model. 

We have some behavior we want to induce. We cannot observe all of it directly. Instead, we construct training and evaluation systems using observable signals. Holmstrom gives us a way of asking which signals might actually be useful.

### Case Study 1: Outcome Supervision v. Process Supervision

In a 2023 paper [*Lets Verify Step by Step*](https://arxiv.org/pdf/2305.20050), Lightman et al. compared two different approaches to improve reliability of state-of-the-art models. The authors evaluate performance on the MATH dataset which is a set of mathematical problems used as a benchmark. They compared outcome supervision which provides feedback for a final result with process supervision that provides feedback for each intermediate reasoning step. In the former case, the supervisor only checks if the model is correct, while in the latter, a human provides feedback on intermediate steps required to solve the problem. 

Through the Holmstrom lens, process supervision provides additional signals. The intermediate steps are useful if they contain information about the quality of the model's behavior that is already not contained in the final answer. Intuitively, it makes sense that the intermediate steps do indeed help. This is because incorrect approach might sometimes lead to a correct solution and hence observing the steps taken to solve the problem might provide some useful insight into the model's behavioral process and help in model evaluation. However, this does not mean that process supervision is valuable simply because it gives us more data.

Suppose the intermediate signal is meaningless, extremely noisy, or unrelated to the behavior we ultimately care about. Then it may provide little additional information.

Or suppose models learn to generate intermediate explanations that look convincing without faithfully reflecting the process that produced the answer. In that case, the signal may become much less informative than we hoped.

Hence, we have a criterion to judge whether a signal is good but not a guarantee that it would be useful.

Similarly in [*Monitoring Reasoning Models for Misbehavior and the Risks of Promoting Obfuscation*](https://arxiv.org/abs/2503.11926), Baker et al. studied reward hacking by reasoning models in agentic coding environments. They used another language model to monitor the reasoning model's chain-of-thought. The authors find that monitoring the trace could detect misbehavior substantially better than monitoring actions and final outputs alone. Hence, a weaker model as a supervisor could detect problematic behavior in a stronger model. 

In Holmstrom's language, the reasoning trace is an additional signal $y$. Observing only the model's actions and outputs gives the evaluator some information about its behavior. 

#### The system games you when you try to game it

If the reasoning trace is useful for detecting reward hacking, one natural response is to incorporate the chain-of-thought monitor directly into the model's training reward. Baker et al. tried this and it worked well initially. But under stronger optimization pressure, models increasingly learned to hide evidence of reward hacking from the monitored reasoning trace while continuing to reward hack. In other words, the signal itself became less informative.

While initially, the reasoning trace $y$ might give us useful information, once the agent got to know the monitoring procedure, it could have strategically reduced the informativeness of $y$ and hence weakening its usefulness. Baker et al. describe the resulting tradeoff as potentially requiring a monitorability tax: avoiding excessive optimization pressure on chain-of-thought so that it remains useful as a monitoring signal.

Hence intermediate information can be enormously useful, but only insofar as it remains informative about the behavior we actually care about.

###  Case Study 2: AI-Assisted Evaluation
Sometimes an informative signal may exist but the problem is that the human evaluator cannot easily extract it from a complicated output. Imagine reviewing a long legal report produced by an AI system. Everything looks great but somewhere there is a factual error that a human evaluator might miss. 

[Saunders et al.](https://arxiv.org/abs/2206.05802) studied a version of this  by training language models to produce critiques of summaries. This helped human evaluators to better identify flaws in reports that they otherwise missed. 

The value of the critique/signal $y$ is not just more text but also the fact it helps in distinguishing desirable from undesirable behavior **conditional on already having seen the output**.

Hence, as AI systems become capable of producing work that is harder for humans to evaluate directly, part of the alignment problem becomes a problem of constructing informative auxiliary signals such as critiques, tool traces, external verification etc. Its not just producing more text, what helps in producing informative text. 

### Other Papers to think about

Once viewed through the lens of finding useful information, a surprising amount of the alignment literature can be interpreted as searching for better signals about behavior. 

[InstructGPT](https://arxiv.org/abs/2203.02155) used human demonstrations and rankings of model outputs to create training signals for language models. Human preferences over observable outputs became part of the information used to shape model behavior.

[Christiano et al. (2017)](https://arxiv.org/abs/1706.03741), trained reinforcement-learning agents from human comparisons between trajectory segments rather than requiring humans to specify a complete reward function in advance.

I do not claim that all these papers are applications of Holmstrom. However, they are related from the POV of a principal not being able to observe the effort made by an agent. 


## Conclusion

The obvious lesson from Moral Hazard and Observability is that incentives matter. However, it also tells us that when the behavior we care about cannot be observed directly, supervision becomes a problem of information. Holmstrom's insight is that these signals should not be valued merely because they are observable or because they correlate with good outcomes. What matters is whether they tell us something new about the hidden behavior we are trying to induce. 

That gives us a useful way to think about AI oversight: What does this signal tell us about the model's behavior that we did not already know from the outcome?

However, things do not end here. What happens when a manager induces an employee to increase sales in the next quarter but leaves the company's reputation in tatters by using unscrupulous but legal means? Specifically, what happens when the observable thing we reward captures only part of what we care about? 

Suppose a model can improve the measured objective while making everything else worse. That takes us from Holmstrom (1979) to Holmstrom and Milgrom (1991), and from imperfect observability to reward hacking, specification gaming, and Goodhart's law.

That will be Part II.

