---
layout: post
title: Sparse Variational Inference for Generalized Gaussian Process Models - Tutorial 2
description: This is a tutorial for this ICML 2015 paper 'Sparse Variational Inference for Generalized Gaussian Process Models'. It covers fixed point method, stochastic variational inference and some experiments.
date: 2019-08-10
---
<p>
In this continued blog, we will talk about VLB optimization, including calculating the gradients of the VLB and the fixed point method. Aditionally, we'll talk about a little stochastic variational inference.
</p>

### Calculating the gradients of the VLB
<p>
Now we have the VLB handy. Our goal is to optimize variational parameters, so we need to calculate the gradients of the VLB w.r.t $\boldsymbol{m}$ and $\boldsymbol{V}$. In the first paper, the chain rule is employed when the gradients of the log likelihood expectation term are calculated. Specifically, 
</p>

$$
\frac{\partial \mathrm{VLB}}{\partial \boldsymbol{m}}=\frac{\partial VLB}{\partial m_{q_{i}}}\frac{\partial m_{q_{i}}}{\partial \boldsymbol{m}}
$$

$$
\frac{\partial \mathrm{VLB}}{\partial \boldsymbol{V}}=\frac{\partial VLB}{\partial \sqrt{v_{q_{i}}}}\frac{\partial(\sqrt{v_{q_{i}}})}{\partial \boldsymbol{V}}
$$

<p>
The paper has put the first step of the chain rule in detail. One $\frac{1}{\sqrt{2\pi}}$ is missing in Eq. (7) of the original paper, but it does not affect the final outcome as the missing term is absorbed into the subscript of the expectation in the last step of Eq. (8). The second step can be obtained easily via Eq. (2a) and (2b) in <a href="https://kaikaizhao.github.io/notes/2019/08/09/Sparse-Variational-Inference-for-Generalized-Gaussian-Process-Models" target="_blank">Tutorial 1</a>.
</p>

<p>
After calculating the derivatives of KL term, we put together the derivatives of the two parts of VLB. Then we get the derivatives of the VLB,
</p>

$$
    \frac{\partial \mathrm{VLB}}{\partial \boldsymbol{m}}=\sum_{i}\left(\rho_{i} K_{M}^{-1} K_{M i}\right)-K_{M}^{-1}\left(\boldsymbol{m}-\boldsymbol{m}_{\mathcal{U}}\right)\tag{1}\label{de-m}
$$

$$
\frac{\partial \mathrm{VLB}}{\partial V}=\frac{1}{2} \sum_{i}\left(\lambda_{i} K_{M}^{-1} K_{M i} K_{i M} K_{M}^{-1}\right)+\frac{1}{2}\left(V^{-1}-K_{M}^{-1}\right)\tag{2}\label{de-V}
$$

<p>where $\rho_i$ and $\lambda_i$ are derived from the first step of chain rule when we calculate the derivatives of log likelihood expectation. They are expectations of the first derivatives and second derivatives of log likelihood. These are available in the Table 1 of the original paper. In the next article we will continue to talk about this topic.</p>

### VLB optimization
<p>
By first-order optimality, the optimal variational parameters can be found via the conditions $\left.\frac{\partial \mathrm{VLB}}{\partial \boldsymbol{m}}\right|_{\boldsymbol{m}=\boldsymbol{m}^{*}}=0$ and $\frac{\partial \mathrm{VLB}}{\partial V} |_{V=V^{\star}}=0$.
</p>

$$
\boldsymbol{m}^{\star}=K_{M N} \boldsymbol{\rho}^{\star}+\boldsymbol{m}_{\mathcal{U}}
\tag{3a}\label{sol-m}
$$

$$
V^{\star}=\left(K_{M}^{-1}-K_{M}^{-1} K_{M N} \operatorname{diag}\left(\lambda^{\star}\right) K_{N M} K_{M}^{-1}\right)^{-1}\tag{3b}\label{sol-V}
$$

<p>
In general, \eqref{sol-m} and \eqref{sol-V} are a set of nonlinear equations coupled through their dependencies on $\boldsymbol{\rho}$ and $\lambda$, except Gaussian likelihood. For the case of count regression, when we calculate $\boldsymbol{m}$, we need to know $\boldsymbol{\rho}$ that is dependent on $m$ and $v$ from $\rho=-e^{m+\frac{1}{2} v}+y$. From (2a) and (2b) in <a href="https://kaikaizhao.github.io/notes/2019/08/09/Sparse-Variational-Inference-for-Generalized-Gaussian-Process-Models" target="_blank">Tutorial 1</a>, $m$ and $v$ are dependent on $\boldsymbol{m}$ and $\boldsymbol{V}$. Therefore, they are coupled, that is to say, if you want to compute $\boldsymbol{m}$, you have to know $\boldsymbol{m}$ and $\boldsymbol{V}$. So gradient ascent is a standard approach to solving this problem.
</p>
<p>
For the covariance, we optimize the Cholesky factor $L$ of $V=LL^T$ instead of $V$ directly, which guarantees that $V$ is positive-definite. In our case the gradient is
</p>

$$
\frac{\partial \mathrm{VLB}}{\partial L}=\sum_{i}\left(\lambda_{i} K_{M}^{-1} K_{M i} K_{i M} K_{M}^{-1} L\right)+\left(L^{-1^{T}}-K_{M}^{-1} L\right)
$$

### Fixed point method

<p>
First of all, we introduce the description of <b>fixed-point theorem</b>.
</p>

> If a function $f$ defined on the real line with real values is Lipschitz continuous with Lipschitz constant $L<1$, then this function has precisely one fixed point, and the fixed-point iteration converges towards that fixed point for any initial guess $x_{0}$. This theorem can be generalized to any metric space.

<p>
The first paper proposed to optimize the covariance via a fixed-point operator, $T$:
</p>

$$
    T(V)=\left(K_{M}^{-1}-K_{M}^{-1} K_{M N} \operatorname{diag}(\boldsymbol{\lambda}) K_{N M} K_{M}^{-1}\right)^{-1}\tag{4}\label{fp-V}
$$

<p>
The above formula exactly applies fixed-point iteration to \eqref{sol-V}. The critical condition is Lipschitz constant $ L<1 $, i.e. for all $U$, $V$
</p>

$$
\|T(V)-T(U)\| \leq L\|V-U\|
$$

