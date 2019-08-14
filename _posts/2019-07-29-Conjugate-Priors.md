---
layout: post
title: Conjugate priors
tags: SVI
description: If a conjugate prior is available for a likelihood function, the posterior distribution has the same form as the prior.
date: 2019-07-29
---

<p>When I was learning variational inference, conjugacy ever made me confused. In this blog, I would like to write down what I have learned and my thoughts as well. I hope it could be helpful for the friends who are struggling with this concept.</p>

### Definition

<p>First of all, let's see what's the definition of conjugate priors in the Section 2.4.2 of Bishop's PRML book</p>
<blockquote>
    <p>
        In general, for a given probability distribution $p(x|\eta)$, we can seek a prior $p(\eta)$ that is conjugate to the likelihood function, so that the posterior distribution has the same functional form as the prior.
    </p>
</blockquote>
