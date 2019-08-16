---
layout: post
title: Sparse Variational Inference for Generalized Gaussian Process Models - Tutorial 2
description: This is a tutorial for this ICML 2015 paper 'Sparse Variational Inference for Generalized Gaussian Process Models'. It covers fixed point method, stochastic variational inference and some experiments.
date: 2019-08-10
---
<p>
Since the canonical link function $\lambda=e^{f}$ may bring numerical issues, we introduce a good alternative link function $\lambda=\ln(1+e^{f})$ which could be stabler. In this continued blog, we will talk about calculating the first derivatives and the second derivatives for this link, including the corresponding expectations. Also, fixed point method will be introduced. Aditionally, we'll talk about a little stochastic variational inference.
</p>

### Deriving the formulae related to $\lambda=\ln(1+e^{f})$
<p>
We still use the chain rule. Specifically, we first calculate derivatives of log Poisson likelihood w.r.t $\lambda$ and calculate derivatives of link function $\lambda=\ln(1+e^{f})$ w.r.t $f$.
</p>

$$\label{log-lik}
\begin{aligned}
    \log p(y|\lambda)&=\log \frac{1}{y!}e^{-\lambda}\lambda^y\\
    &=-\ln y! -\lambda + y\ln\lambda\\
    &=-\ln\Gamma(y+1) -\lambda + y\ln\lambda
\end{aligned}
$$

$$\label{d-log-lik}

    \frac{\partial \log p(y|\lambda)}{\partial \lambda}&=-1 + \frac{y}{\lambda} \quad \frac{\partial^2 \log p(y|\lambda)}{\partial \lambda^2}&=-\frac{y}{\lambda^2}

$$

$$\label{d2-log-lik}
\begin{aligned}
    \frac{\partial^2 \log p(y|\lambda)}{\partial \lambda^2}&=-\frac{y}{\lambda^2}
\end{aligned}
$$