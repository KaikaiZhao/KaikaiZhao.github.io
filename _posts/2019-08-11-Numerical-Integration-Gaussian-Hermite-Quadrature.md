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
In the case of variational inference, when we compute the variational evidence lower bound(ELBO), the first term of ELBO usually represents the goodness of fit for a certain model, that is to say, we need to calculate the expectation of log likelihood w.r.t the variational distribution. Generally, our variational distribution is easy to handle, like Gaussian distribution. In many cases, the closed-form of data fit term is not available because of the non-conjugacy between our likelihood and prior. Then we need approximation for calculating the expectation and Gaussian-Hermite Quadrature is a good tool for this.
</p>

 is  In terms of variational inference experienced the case of calculating the expectation of  can use an inline formula $$\forall x \in R$$ like this one \eqref{eq:sample} \eqref{eq:sample1}

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