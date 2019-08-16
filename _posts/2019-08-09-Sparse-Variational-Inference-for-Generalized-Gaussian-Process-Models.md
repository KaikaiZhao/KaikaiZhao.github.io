---
layout: post
title: Sparse Variational Inference for Generalized Gaussian Process Models - a Tutorial
description: This is a tutorial for this ICML 2015 paper 'Sparse Variational Inference for Generalized Gaussian Process Models'. It covers fixed point method, stochastic variational inference and some experiments.
date: 2019-08-09
---

<p>In Summer 2019 semester I am honored to work with Professor Roni Khardon for my independent study. Thank Lijiang Guo, Weizhe Chen and Yadi Wei for helpful discussions.</p>

<p>
During this past summer I worked on the following papers. I spent some time understanding the algorithms proposed in the papers and implementing some of them. However, due to page limit the derivations in these papers are brief, hence we had to complete the unpublished derivation before implementation. Therefore, we will try to present a full picture of the first paper.
</p>

<ol>
    <li>Rishit Sheth, Yuyang Wang, and Roni Khardon. <a href="http://homes.sice.indiana.edu/rkhardon/PUB/icml15sparseFPGP.pdf" target="_blank">Sparse variational inference for generalized gaussian process</a>. ICML 2015.</li>
    <li>Rishit Sheth and Roni Khardon. <a href="http://proceedings.mlr.press/v51/sheth16.pdf" target="_blank">A Fixed-Point Operator for Inference in Variational Bayesian Latent Gaussian Models</a>. AISTATS 2016.</li>
    <li>Matthew D. Hoffman, David M. Blei, Chong Wang and John Paisley. <a href="http://www.columbia.edu/~jwp2128/Papers/HoffmanBleiWangPaisley2013.pdf" target="_blank">Stochastic Variational Inference</a>. Journal of Machine Learning Research 14(2013).</li>
    <li>Rishit Sheth and Roni Khardon. <a href="https://arxiv.org/abs/1612.03957" target="_blank">Monte Carlo Structured SVI for Two-Level Non-Conjugate Models</a>. arXiv:1612.03957.</li>
</ol>
<!-- , but we will exactly explain the ideas in the fist paper, fixed point method, and the connection between FP method and SVI -->
<p>
In this tutorial we cannot cover all the ideas in the above papers. The main topic is sparse variational inference for generalized gaussian process models. To explain this main topic better, we will also include some ideas from other papers where appropriate, such as the fixed point method, SVI and their connection as well.
</p>

<p>
We will assume prior knowledge of (sparse) Gaussian process and variational inference.
</p>

### Variational Lower Bound for Sparse Gaussian Processes

<p>
For sparse variational Gaussian Processes, we follow <a href="http://proceedings.mlr.press/v5/titsias09a/titsias09a.pdf" target="_blank">Titsias 2009</a>. We first need to select the inducing inputs $X_m$, and $f$ and $f_m$ denote the training latent function values and the inducing variables, respectively. Then, the initial joint model is
</p>