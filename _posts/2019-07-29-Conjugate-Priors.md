---
layout: post
title: Conjugate priors
tags: SVI
description: If a conjugate prior is available for a likelihood function, the posterior distribution has the same form as the prior.
date: 2019-07-29
---

<p>When I was studying variational inference, conjugacy ever made me confused. I spent some time figuring it out and found it is important for us to read some top-conference papers. So I would like to write down what I have learned and my thoughts as well. I hope it could be helpful for the friends who are struggling with this concept.</p>

### Definition

<p>First of all, let's see what's the definition of conjugate priors in the Section 2.4.2 of Bishop's PRML book</p>
<blockquote>
    <p>
        In general, for a given probability distribution $p(x|\eta)$, we can seek a prior $p(\eta)$ that is conjugate to the likelihood function, so that the posterior distribution has the same functional form as the prior.
    </p>
</blockquote>

<p>The critical point in the above definition is the final posterior distribution and the prior share the same form. Note that the exponential family will play an important role in the following explanation for the conjugacy concept. If you are not comfortable with it, welcome to check out my <a href="https://kaikaizhao.github.io/notes/2019/07/26/Exponential-Family" target="_blank">exponential family</a> blog.</p>

### Mathematical formulation

Recall the Bayes formula

\begin{equation}
p(\beta | x)=\frac{p(x | \beta) p(\beta)}{\int p(x | \beta) p(\beta) d \beta}
\end{equation}

<p><b>Note:</b> the denominator is not a function w.r.t $\beta$ and it can be considered as a normalization term.</p>

$$
\underbrace{p(\beta | x)}_{\text{posterior}} \propto \underbrace{p(x | \beta)}_{\text {likelihood}}\underbrace{p(\beta) }_{\text{prior}}
$$

#### Given a certain likelihood function, how can we find its conjugate prior?

The following derivation will end up a very useful and interesting result. Here we employ the exponential family form of probability distributions because it simplifies the derivation process significantly.

* Likelihood density:

$$
p\left(x | \beta\right)=h\left(x\right) \exp \left\{t\left(x\right) \beta -\color{red}{A_l(\beta)}\right\}
$$

