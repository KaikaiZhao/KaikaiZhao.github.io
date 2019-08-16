---
layout: post
title: Sparse Variational Inference for Generalized Gaussian Process Models - Tutorial 2
description: This is a tutorial for this ICML 2015 paper 'Sparse Variational Inference for Generalized Gaussian Process Models'. It covers fixed point method, stochastic variational inference and some experiments.
date: 2019-08-10
---
<p>
Since the canonical link function $\lambda=e^{f}$ may bring numerical issues, we introduce a good alternative link function $\lambda=\ln(1+e^{f})$ which could be stabler. In this continued blog, we will talk about calculating the first derivatives and the second derivatives for this link, including the corresponding expectations. Also, fixed point method will be introduced.
</p>

