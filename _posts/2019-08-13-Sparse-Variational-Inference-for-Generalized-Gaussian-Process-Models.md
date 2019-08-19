---
layout: post
title: Sparse Variational Inference for Generalized Gaussian Process Models - Tutorial 5
description: This is a tutorial for this ICML 2015 paper 'Sparse Variational Inference for Generalized Gaussian Process Models'. This article covers the details of implementation and experiments.
date: 2019-08-13
---

<p>
In this continued blog, we will talk about the details of implementation and experiments. Our python code is available <a href="https://github.com/KaikaiZhao/Sparse-Variational-Inference-for-Generalized-Gaussian-Process-Models---Tutorial" target="_blank">here</a>. It follows the formulas presented in the preceding blogs closely. Besides, it is by no-means optimized, rather, it is supposed to be easy to read and easy to understand. If you have any questions with reading the code, please feel free to email me:zhaokai@iu.edu.
</p>

### Experiments

We will present five sets of experimental results. Our code is written from scratch, except that we use <a href="https://sheffieldml.github.io/GPy/" target="_blank">GPy</a> to initialize kernel hyperparameters with Laplace approximation.

<p>
The first experiment is to compare gradient descent(GD), FPb, FPi, and FPi-mean. FPi-mean denotes FPi with fixed-point update for the mean. As can be seen from the following figure, the performance of the FPi-mean method outperforms other methods but its drawback is instability, which has been mentioned in the second paper.
</p>

<img class="img-responsive" src="/img/count-p2-segment-se.png" alt="GD-FP"/>

<p>
The second experiment is to compare GD and S-DSVI. S-DSVI refers to the analogous approach using a structured variational approximation but using standard gradients in the standard parameter space.
</p>

<img class="img-responsive" src="/img/class-SDSVI-VLB-err-musk-500.png" alt="GD-SDSVI"/>

<p>
The second experiment is to compare GD and S-DSVI. S-DSVI refers to the analogous approach using a structured variational approximation but using standard gradients in the standard parameter space.
</p>

<img class="img-responsive" src="/img/count-HMC-VLB-err-epid.png" alt="GD-HMC"/>

<p>
The second experiment is to compare GD and S-DSVI. S-DSVI refers to the analogous approach using a structured variational approximation but using standard gradients in the standard parameter space.
</p>

<img class="img-responsive" src="/img/class-MC-VLB-err-musk-500.png" alt="MC"/>

<p>
The second experiment is to compare GD and S-DSVI. S-DSVI refers to the analogous approach using a structured variational approximation but using standard gradients in the standard parameter space.
</p>

<img class="img-responsive" src="/img/class-3SVI-err-musk-500.png" alt="SVI"/>

