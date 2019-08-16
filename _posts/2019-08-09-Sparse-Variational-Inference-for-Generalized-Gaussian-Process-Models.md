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

$$
    p\left(\mathbf{y}, \mathbf{f}, \mathbf{f}_{m}\right)=p(\mathbf{y} | \mathbf{f}) p\left(\mathbf{f} | \mathbf{f}_{m}\right) p\left(\mathbf{f}_{m}\right)
$$

<p>
where the conditional GP prior is $p\left(\mathbf{f} | \mathbf{f}_{m}\right)=N\left(\mathbf{f} | K_{n m} K_{m m}^{-1} \mathbf{f}_{m}, K_{n n}-K_{n m} K_{m m}^{-1} K_{m n}\right)$. Then the true marginal likelihood can be written as
</p>

$$
    \log p(\mathbf{y})=\log \int p(\mathbf{y} | \mathbf{f}) p\left(\mathbf{f} | \mathbf{f}_{m}\right) p\left(\mathbf{f}_{m}\right) d \mathbf{f} d \mathbf{f}_{m}
$$

<p>
The posterior $p(\mathbf{f},\mathbf{f}_{m}|\mathbf{y})$ is approximated by the variational distribution
</p>

$$
q\left(\mathbf{f}, \mathbf{f}_{m}\right)=p\left(\mathbf{f} | \mathbf{f}_{m}\right) \phi\left(\mathbf{f}_{m}\right)
$$

<p>
where $\phi$ is a multivariate Gaussian distribution with mean $\boldsymbol{m}$ and covaraince $\boldsymbol{V}$ and these two parameters are exactly what we are going to optimize.
</p>

<p>
Applying Jensen's inequality we obtain
</p>

$$
\begin{aligned}
    \log p(\mathbf{y})&=\log \int p\left(\mathbf{f} | \mathbf{f}_{m}\right)\phi\left(\mathbf{f}_{m}\right)\cdot \frac{p(\mathbf{y} | \mathbf{f}) p\left(\mathbf{f} | \mathbf{f}_{m}\right) p\left(\mathbf{f}_{m}\right)}{p\left(\mathbf{f} | \mathbf{f}_{m}\right)\phi\left(\mathbf{f}_{m}\right)}  d \mathbf{f} d \mathbf{f}_{m}\\
    &\ge \int p\left(\mathbf{f} | \mathbf{f}_{m}\right) \phi\left(\mathbf{f}_{m}\right) \log \frac{p(\mathbf{y} | \mathbf{f}) p\left(\mathbf{f} | \mathbf{f}_{m}\right) p\left(\mathbf{f}_{m}\right)}{p\left(\mathbf{f} | \mathbf{f}_{m}\right) \phi\left(\mathbf{f}_{m}\right)} d \mathbf{f} d \mathbf{f}_{m}\\
    &= \int p\left(\mathbf{f} | \mathbf{f}_{m}\right) \phi\left(\mathbf{f}_{m}\right) \log \frac{p(\mathbf{y} | \mathbf{f})  p\left(\mathbf{f}_{m}\right)}{ \phi\left(\mathbf{f}_{m}\right)} d \mathbf{f} d \mathbf{f}_{m}\\
    &= \int p\left(\mathbf{f} | \mathbf{f}_{m}\right) \phi\left(\mathbf{f}_{m}\right) \log p(\mathbf{y} | \mathbf{f}) d \mathbf{f} d \mathbf{f}_{m} + \int p\left(\mathbf{f} | \mathbf{f}_{m}\right) \phi\left(\mathbf{f}_{m}\right) \log \frac{p\left(\mathbf{f}_{m}\right)}{ \phi\left(\mathbf{f}_{m}\right)} d \mathbf{f} d \mathbf{f}_{m}\\
    &= \int p\left(\mathbf{f} | \mathbf{f}_{m}\right) \phi\left(\mathbf{f}_{m}\right) \log p(\mathbf{y} | \mathbf{f}) d \mathbf{f} d \mathbf{f}_{m} - \int  \phi\left(\mathbf{f}_{m}\right) \log \frac{\phi\left(\mathbf{f}_{m}\right)}{ p\left(\mathbf{f}_{m}\right)} d \mathbf{f}_{m}\\
    =& \int q\left(\mathbf{f}, \mathbf{f}_{m}\right) \log p(\mathbf{y} | \mathbf{f}) d \mathbf{f} d \mathbf{f}_{m} - \int  \phi\left(\mathbf{f}_{m}\right) \log \frac{\phi\left(\mathbf{f}_{m}\right)}{ p\left(\mathbf{f}_{m}\right)} d \mathbf{f}_{m}\\
    =&\int q\left(\mathbf{f}\right) \log p(\mathbf{y} | \mathbf{f}) d \mathbf{f} - \int  \phi\left(\mathbf{f}_{m}\right) \log \frac{\phi\left(\mathbf{f}_{m}\right)}{ p\left(\mathbf{f}_{m}\right)} d \mathbf{f}_{m}\\
    =&\int q\left(\mathbf{f}\right) \log p(\mathbf{y} | \mathbf{f}) d \mathbf{f} - \mathrm{KL}(\phi(\mathbf{f_m})\|p(\mathbf{f_m}))\\
\end{aligned}
$$

<p>
After we substitute the integral sign and $m$ with a sum sign and $\mathcal{U}$ respectively, we obtain the Eq. (2) in the first paper, which is
</p>

$$
    \log p(\boldsymbol{y}) \geq \sum_{i=1}^{N} \mathbb{E}_{q\left(f\left(\boldsymbol{x}_{i}\right)\right)}\left[\log p\left(y_{i} | f\left(\boldsymbol{x}_{i}\right)\right)\right] -\mathrm{KL}\left(\phi\left(\boldsymbol{f}_{\mathcal{U}}\right) \| p\left(\boldsymbol{f}_{\mathcal{U}}\right)\right)
    \tag{1}\label{VLB}
$$
<p>
where $q(f(x_i))$ is the marginal distribution of latent function values $f(x_i)$ w.r.t the approximate posterior $q(f_{\mathcal{X}},f_{\mathcal{U}})$. The right hand side of Eq.(\ref{VLB}) is the so-called <b>variational lower bound(VLB)</b>.
</p>
<p>
As $q(f_{\mathcal{X}},f_{\mathcal{U}})$ is a joint Gaussian distribution, the marginal distribution of $f(x_i)$ is specified by a univariate Gaussian with mean $m_q(x_i)$ and variance $v_q(x_i)$ where
</p>

$$
    m_{q}(\boldsymbol{x})=m(\boldsymbol{x})+K_{x M} K_{M}^{-1}\left(\boldsymbol{m}-\boldsymbol{m}_{\mathcal{U}}\right)
$$
$$
v_{q}(\boldsymbol{x})=k(\boldsymbol{x}, \boldsymbol{x})+K_{x M} K_{M}^{-1}\left(V-K_{M}\right) K_{M}^{-1} K_{M x}
$$

### Calculating VLB
In this section we first calculate the VLB and then we derive the gradients for variational parameters in next section. 
<p>
The VLB consists of two parts: the expectation of log likelihood and the KL divergence. Actually, we don't need to compute the VLB, because we can stop iterations when there are no changes for the variational parameters $\boldsymbol{m}$ and $\boldsymbol{V}$. However, we would like to observe the changes for the VLB, hence we will calculate it explicitly.
</p>

#### Expectation of log likelihood
<p>
We use a change of variable to simply the calculation, that is, $f_i = z_{i} \sqrt{v_{q_{i}}}+m_{q_{i}}$. Then the expectation of log likelihood has the following form,
</p>

$$\label{VLB-1}
    \mathbb{E}_{q_{i}\left(f_{i}\right)}\left[\log p\left(y_{i} | f_{i}\right)\right]=\frac{1}{\sqrt{2 \pi}} \int \log p\left(y_{i} | z_{i} \sqrt{v_{q_{i}}}+m_{q_{i}}\right) e^{-\frac{1}{2} z_{i}^{2}} d z_{i}
$$


Generally, we employ gadient ascent to optimize variational parameters.