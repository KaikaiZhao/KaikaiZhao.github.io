---
layout: post
title: Numerical Integration - Gaussian-Hermite Quadrature
description: How to calculate the expectation of a function w.r.t a normal distribution when its closed form is not available
date: 2019-08-11
---

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



<p>
In many cases, the closed-form of data fit term, e.g. \eqref{eq:expect}, is not available because of the non-conjugacy between our likelihood and prior. Then we need approximation for calculating the expectation and Gaussian-Hermite Quadrature is a good tool for this.
</p>

<p>
In numerical analysis, Gaussian-Hermite Quadrature is used to approximate the value of integrals of the following kind:
</p>

\begin{equation}
\int_{-\infty}^{+\infty} e^{-x^{2}} f(x) dx
\end{equation}

The basic idea is

\begin{equation}
\int_{-\infty}^{+\infty} e^{-x^{2}} f(x) d x \approx \sum_{i=1}^{n} w_{i} f\left(x_{i}\right)
\end{equation}



\begin{equation}
  \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15}
  \label{eq:sample}
\end{equation}

\begin{equation}
M = \left( \begin{array}{ccc}
x_{11} & x_{12} & \ldots \\
x_{21} & x_{22} & \ldots \\
\vdots & \vdots & \ldots \\
\end{array} \right)
\label{eq:sample1}
\end{equation}