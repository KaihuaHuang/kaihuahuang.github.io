---
layout:     post
title:      Multiple Asset Option
subtitle:   Backpropagation
date:       2018-12-21
author:     William
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Stochastic Calculus
    - Derivative Pricing
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

# Multiple Asset Option
## Definition
Multi asset option's value is depending on the multiple asset value. For example, there are two assets denoted as  $S_1$ and $S_2$, the payoff of one possible two-asset option is $max(S_1,S_2)$. 

## Two-Asset General Example

Suppose there are two assets follow belowing SDE:

$$dS_1 = \mu S_1dt + \sigma_1S_1dW_1$$

$$dS_2 = \mu S_2dt + \sigma_1S_1dW_2$$

And $dW$ is normal distributed random variable with mean $0$ and variance $t$.

There  is a correlation between $dW_1$ and $dW_2$, defined as below:

$$Cov(dW_1,dW_2) = E(dW_1dW_2) - E(dW_1)E(dW_2) = E(dW_1dW_2) = \rho dt$$

$$Corr(dW_1,dW_2) = \frac{Cov(dW_1,dW_2)}{\sigma_{dW_1}\sigma_{dW_2}} = \rho$$

Denote the value of two-asset option as $V$ and it's a function of $S_1$, $S_2$ and $t$.

$$V(S_1,S_2,t)$$

Take derivative on $V$:

$$dV = \frac{\partial V}{\partial S_1}dS_1 + \frac{1}{2}\frac{\partial^2 V}{\partial S_1^2}dS_1dS_1+\frac{\partial V}{\partial S_2}dS_2 + \frac{1}{2}\frac{\partial^2 V}{\partial S_2^2}dS_2dS_2 +\frac{1}{2}\frac{\partial^2 V}{\partial S_1S_2}dS_1dS_2+\frac{\partial V}{\partial t}dt$$

>- $dS_1dS_1 = \sigma^2_1S_1^2dt$
>- $dS_2dS_2 = \sigma^2_2S_2^2dt$
>- $dS_1dS_2 = \sigma_1\sigma_2S_1S_2\rho dt$


