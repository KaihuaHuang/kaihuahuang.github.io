---
layout:     post
title:      Stochastic Calculus Cookbook
subtitle:   Brownian Motion and Ito Integral 
date:       2019-7-26
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

# Brownian Motion
## 1. Definition

- Continuous sample path but nondifferentiable
- Independent increment

> $W_{t_1}-W_0$, $W_{t_2} - W_{t_1}$ ... $W_{t_n} - W_{t_{n-1}}$ are i.i.d

- Each of the increments is normally distributed

> $$W_{t_m} - W_{t_n} \sim N(0,t_m-t_n)$$

## 2. Properties

1. $E(W_t\vert F_s) = W_s $
2. $E(W_t^2-t\vert F_s) = W_s^2-s$
3. $E[exp(\lambda W_t - \frac{1}{2}\lambda^2t)\vert F_s] = exp(\lambda W_s-\frac{1}{2}\lambda^2s)$
4. $cov(W_t,W_s) = s$

> **Note:** 
> The above proof can be done by replace $W_t = W_t-W_s+W_s$

# Ito Integral
## 1. Definition
The Ito Intergral is defined as below:

$$I \equiv \int_0^TH(t)dW$$

$I$ is a random variable.

## 2. Probabilistic Properties

Since $I$ is a random number, so we would care about its expectation and variance.

**Expectation**

$$E(I_t) = 0$$

since $I$ is martingale.

**Variance**

$$E(I_t^2) = E[\int_0^tH(u)^2du]$$

This is also called Ito Isometry.

**Linearity**

If $I_1(t) = \int_0^TH_1(t)dW$ and $I_2(t) = \int_0^TH_2(t)dW$

$$I_1(t)+I_2(t) = \int_0^T[H_1(t)+H_2(t)]dW$$

$$cI_1(t) = c\int_0^TH_1(t)dW \ for \ constant\ c$$

> **Example - $\int_0^TW_tdW_t$**
> $$Var(I) = E[\int_0^TW_t^2dt]\\=\int_0^TE(W_t^2)dt = \int_0^Ttdt = \frac{1}{2}T^2$$

# Ito Formula

$$df(t,X_t)= f'_xdX_t+f'_tdt+\frac{1}{2}f''_{xx}dX_tdX_t$$

> **Example - Geometric Brownian Motion**
> Suppose the S follows: $dS_t = \alpha _tS_tdt + \sigma_tS_tdW_t$
> Consider a function $f(S_t) = log(S_t)$
> $$dlog(S_t) = \frac{1}{S_t}dS_t - \frac{1}{2}*\frac{1}{S_t^2}dS_tdS_t \\
    = \alpha_tdt + \sigma_tdW_t - \frac{1}{2S_t^2}*(\sigma_t^2S_t^2dt) \\
    = (\alpha_t-\frac{1}{2}\sigma_t^2)dt + \sigma_tdW_t$$
> As a result
> $$log(S_T)-log(S_0) = \int_0^T(\alpha_t-\frac{1}{2}\sigma_t^2)dt + \int_0^T\sigma_tdW_t\\
  = (\alpha_t-\frac{1}{2}\sigma_t^2)T + \sigma_tW_t$$
> The $S_T$ can be expressed as below:
> $$S_T = S_0exp[(\alpha_t-\frac{1}{2}\sigma_t^2)T + \sigma_tW_t]$$

> $S_T$ is lognormal distributed.

> **Example - OU Process**
> Suppose the X follows: $dX_t = (\alpha-\beta X_t)dt + \sigma_tdW_t$
> $$d(e^{\beta t}X_t) = \alpha e^{\beta t}dt + e^{\beta t} \sigma dW_t$$
> Intergrate it
> $$e^{\beta T}X_T - X_0 = \int_0^T\alpha e^{\beta t}dt + \int_0^Te^{\beta t} \sigma dW_t \\
  = \frac{\alpha}{\beta}(e^{\beta T} - 1) + \sigma \int_0^Te^{\beta t} dW_t$$
> The $X_T$ can be expressed as below:
> $$X_T = e^{-\beta T}X_0 + \frac{\alpha}{\beta}(1 - e^{-\beta T}) + \sigma e^{-\beta T}\int_0^Te^{\beta t} dW_t$$




# References



  [1]: http://static.zybuluo.com/williamhkh/sokkdt6ivb1ag7iw4e8yobib/image_1df1n21nu13ps9t2uo41vku1199.png
  [2]: http://static.zybuluo.com/williamhkh/0tplxy5ef7hu7w2j6km6n9d6/image_1df1n5iupjcd9qi1b7vo6g1rr5m.png




