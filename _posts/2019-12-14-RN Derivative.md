---
layout:     post
title:      Stochastic Calculus Cookbook - 2
subtitle:   Radon-Nikodym Derivative 
date:       2019-12-14
author:     William
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Stochastic Calculus
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: { 
      inlineMath: [['$','$'], ['\\(','\\)']],
      processEscapes: true
    }
  });
  </script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Radon-Nikodym Derivative
## Definition
RN derivative $Z$ is the link between two equavalent probability measure $P$ and $\tilde{P}$. $Z$ can be defined as below:

$$Z = \frac{d\tilde{P}}{dP}$$

> **equavalent probability measure** means these two probability measure must agree on what is possible and what is impossible

> $$P(\omega) = \tilde{P}(\omega)= 0$$

It can be also written as:

$$\tilde P(A) = \int_{\omega \in A}Z(\omega)dP(\omega)$$

Furthermore:

$$\tilde E(X) = \int_{\omega \in \Omega}X(\omega)d\tilde P(\omega) = E(XZ) = \int_{\omega \in \Omega}X(\omega)Z(\omega)dP(\omega)$$

## Properties of RN Derivative
RN derivative must satisfy the following two properties:

- $E(Z) = \int_{\omega \in \Omega}Z(\omega)dP(\omega) = 1$
- $Z(\omega) > 0$ for $\omega \in \Omega$ almost surely

The first propertity is to make sure that $\tilde P(\Omega) = 1$. This is one of the requirement that make $\tilde P$ to be a probability measure.

The second propertity is to make sure there is no negative probability.

## Change Measure for Standard Normal Variable
Suppose that X is a normal distribution $N(\theta, \sigma^2)$ under measure $P$.

We can construct a RN derivative as below:

$$Z(\omega) = e^{-\theta X(\omega) - \frac{1}{2}\theta^2}$$

The expectation under $\tilde P$ is calculated as below:

$$\tilde E(X) = \int x * \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{x^2}{2\sigma^2}}*e^{-\theta x - \frac{1}{2}\theta^2}dx\\
=\int x * \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x+\theta)^2}{2\sigma^2}}dx = 0$$

Under the new measure $\tilde P$, X is still normal distributed. But with 0 mean.

# Radon-Nikodym Derivative Process
In previous discussion, it's based on a single random variable. This scetion will talk about changing the measure for a whole process. 

For $0 \le t \le T$, where $T$ is a fixed final time. Suppose $Z$ is an almost surely postive random variable satisfying $E(Z) = 1$. RN Derivative process can be defiend as:

$$Z(t) = E[Z|\mathcal{F}(t)]$$

# Girsanov Theorem

Define 

$$Z(t) = exp(-\int_0^t\theta(u)dW(u)-\frac{1}{2}\int_0^t\theta^2(u)du)$$

$$\tilde W(t) = W(t) + \int_0^t\theta(u)du$$

It's clearly in  differential form :

$$d\tilde W(t) = dW(t) + \theta(t)dt$$

Set $Z = Z(T)$. Using $Z(t)$ to change the measure, $\tilde W(t)$ is a brownian motion under new measure $\tilde P$.

