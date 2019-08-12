---
layout: post
title: Numerical Integration - Gaussian-Hermite Quadrature
description: How to calculate the expectation of a function w.r.t a normal distribution when its closed form is not available
date: 2019-08-11
---

## Why do we need Gaussian-Hermite Quadrature?
<p>
Speaking of Bayesian machine learning, you may have to deal with non-Gaussian likelihoods which require some approximation of the posterior as the prior is non-conjugate. 
</p>
<p>
In the case of variational inference, when we compute the variational evidence lower bound(ELBO), the first term of ELBO usually represents the goodness of fit for a certain model, that is to say, we need to calculate the expectation of log likelihood w.r.t the variational distribution. Generally, our variational distribution is easy to handle, like Gaussian distribution. 
</p>
<p>
What I'm trying to say is how to calculate
</p>

\begin{equation}
\mathrm{E}[h(x)] \text{ with }  x \sim N\left(\mu, \sigma^{2}\right)
\end{equation}

The above is equivalent to calculate

\begin{equation}
\int_{-\infty}^{\infty} \frac{1}{\sigma \sqrt{2\pi}} h(x) \exp \left(-\frac{(x-\mu)^{2}}{2 \sigma^{2}}\right) dx
\label{eq:expect}
\end{equation}

## The mathematical formulations

<p>
In many cases, the closed-form of data fit term, e.g. \eqref{eq:expect}, is not available because of the non-conjugacy between our likelihood and prior. Then we need approximation for calculating the expectation and Gaussian-Hermite Quadrature is a good tool for this.
</p>

<p>
In numerical analysis, <a href="https://en.wikipedia.org/wiki/Gauss%E2%80%93Hermite_quadrature" target="_blank">Gaussian-Hermite Quadrature</a> is used to approximate the value of integrals of the following kind:
</p>

\begin{equation}
\int_{-\infty}^{+\infty} e^{-x^{2}} f(x) dx
\end{equation}

The basic idea is

\begin{equation}
\int_{-\infty}^{+\infty} e^{-x^{2}} f(x) d x \approx \sum_{i=1}^{n} w_{i} f\left(x_{i}\right)
\end{equation}

where $$n$$ is the number of sample points used. The $$x_i$$ are the roots of the physicists' version of the Hermite polynomial $$H_n(x) (i = 1,2,\ldots,n)$$, and the associated weights $$w_i$$ are given by
\begin{equation}
w_{i}=\frac{2^{n-1} n ! \sqrt{\pi}}{n^{2}\left[H_{n-1}\left(x_{i}\right)\right]^{2}}
\end{equation}

