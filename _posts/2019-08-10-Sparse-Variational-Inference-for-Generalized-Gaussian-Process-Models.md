---
layout: post
title: Sparse Variational Inference for Generalized Gaussian Process Models - Tutorial 2
description: This is a tutorial for this ICML 2015 paper 'Sparse Variational Inference for Generalized Gaussian Process Models'. It covers fixed point method, stochastic variational inference and some experiments.
date: 2019-08-10
---
<p>
Since the canonical link function $\lambda=e^{f}$ may bring numerical issues, we introduce a good alternative link function, that is, $\lambda=\ln(1+e^{f})$ which could be stabler. In this continued blog, we will talk about calculating the first derivatives and the second derivatives for this link, including the corresponding expectations. Also, fixed point method will be introduced. Aditionally, we'll talk about a little stochastic variational inference.
</p>

### Deriving the formulae related to $\lambda=\ln(1+e^{f})$
<p>
We still use the chain rule. Specifically, we first calculate derivatives of log Poisson likelihood w.r.t $\lambda$ and then calculate derivatives of link function $\lambda=\ln(1+e^{f})$ w.r.t $f$.
</p>

$$
\begin{aligned}
\log p(y|f)&=\ln(\frac{1}{y!}\frac{1}{1+e^f}[\ln(1+e^f)]^y)\\
&=-\ln\Gamma(y+1) -\ln(1+e^f) + y\ln\ln(1+e^f)
\end{aligned}
$$

$$
\begin{aligned}
    \frac{\partial}{\partial f} \log p(y|f)&=\frac{\partial \log p(y|f)}{\partial \lambda} \frac{\partial \lambda}{\partial f}\\
    &\overset{\lambda=\ln(1+e^{f})}{=}(-1 + \frac{y}{\lambda})\frac{e^{f}}{1+e^{f}}\\
    &=\left(\frac{y}{\ln(1+e^{f})}-1\right)\frac{1}{1+e^{-f}}\\
    &=\frac{y}{(1+e^{-f})\ln(1+e^{f})}-\frac{1}{1+e^{-f}}\\
    &=\left(\frac{y}{\ln(1+e^{f})}-1\right)\frac{1}{1+e^{-f}}\\
\end{aligned}
$$

$$
\begin{aligned}
    \frac{\partial^2}{\partial f^2} \log p(y|f)&=\frac{\partial^2 \log p(y|f)}{\partial \lambda^2} \frac{\partial \lambda}{\partial f}\frac{\partial \lambda}{\partial f}+\frac{\partial \log p(y|f)}{\partial \lambda} \frac{\partial^2 \lambda}{\partial f^2}\\
    &\overset{\lambda=\ln(1+e^{f})}{=}(-\frac{y}{\lambda^2})\frac{e^{f}}{1+e^{f}}\frac{e^{f}}{1+e^{f}}+(-1 + \frac{y}{\lambda})\frac{e^{-f}}{(1+e^{-f})^2}\\
    &=(-\frac{y}{\lambda^2})\frac{1}{(1+e^{-f})^2}+(-1 + \frac{y}{\lambda})\frac{e^{-f}}{(1+e^{-f})^2}\\
    &=\left( (\frac{y}{\ln(1+e^f)}-1)e^{-f}-\frac{y}{\ln^2(1+e^f)} \right)\frac{1}{(1+e^{-f})^2}\\
\end{aligned}
$$

<p>
Once we obtain the above formulae, we can get expectations of the deravatives w.r.t $\mathcal{N}(f | m, v)$, i.e. $\rho_i$ and $\lambda_i$ in Eq. (4) and (5) of <a href="https://kaikaizhao.github.io/notes/2019/08/09/Sparse-Variational-Inference-for-Generalized-Gaussian-Process-Models" target="_blank">Tutorial 1</a>,  through Gaussian-Hermite quadrature since the closed form expressions are not available.
</p>

