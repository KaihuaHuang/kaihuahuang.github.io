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

# Quadratic Variation
## First-order Variation
Suppose $f(t)$ is a function of time. The first-order of variation of $f$ is defined as below:

$$FV_T(f) = \lim_{||\Pi|| = 0}\sum_{j=0}^{n-1}|f(t_{j+1})-f(t_j)|$$

Where $||\Pi|| = max_{j=0,...,n-1}(t_{j+1}-t_j)$.

If $f$ has derivative on $t$ at everywhere, the **Mean Value Theorem** can be applied. 

$$f(t_{j+1})-f(t_j) = f'(t^*)(t_{j+1}-t_j)$$

Where $t^*$ is a value between $t_{j+1}$ and $t_j$.

Bring this back to first-order Variation, we can get that:
$$FV_T(f) = \int_0^T|f'(t)|dt$$

**But Brownina motion doesn't have derivative on t, so this result is not applicable to brownian motion.**

## Quadratic Variation
Quadratic variation is defined as:

$$$$

# Ito Integral
## 1. Definition
The Ito Intergral is defined as below:

$$I \equiv \int_0^TH(t)dW$$

$I$ is a random variable. **If $H(t)$ is nonrandom function of time, $I$ is normal distributed.**

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
> So $X_T$ is normal distributed with mean:
> $$e^{-\beta T}X_0 + \frac{\alpha}{\beta}(1 - e^{-\beta T})$$
> with variance
>$$\sigma^2e^{-2\beta T}*\int_0^Te^{2\beta t} dt = \frac{\sigma^2}{2\beta}(1-e^{-2\beta T})$$
> As $\lim_{T \to\infty}$,the mean of $X_T$ becomes $\frac{\alpha}{\beta}$

> **Example - Hard - Not Finished**
>Solve the following equation:
$$dS_t = hdt + \sigma S_tdt$$

> Assume $X_t$ is the $S_t$ without divident and under the same probability measure
>$$dX_t     = \sigma X_tdw_t$$
>$$ dlogX_t = \frac{1}{X_t}dX_t+\frac{1}{2}*-\frac{1}{X_t^2}dX_tdX_t$$
>$$ dlogX_t = \sigma dw_t - \frac{1}{2}\sigma^2dt$$
>$$ X_T = X_t*exp[\sigma W_{T-t}-\frac{1}{2}\sigma^2(T-t)]$$

>$$ d(\frac{S_t}{X_t}) = \frac{1}{X_t}dS_t - \frac{1}{2}*\frac{1}{X_t^2}dS_tdX_t -\frac{S_t}{X_t^2}dX_t-\frac{1}{2}*\frac{1}{X_t^2}dX_tdS_t+\frac{S_t}{X_t^3}dX_tdX_t$$
>$$=\sigma\frac{S_t}{X_t}dW_t+\frac{h}{X_t}dt-\frac{1}{2}*\frac{\sigma^2S_t}{X_t}dt-\frac{\sigma S_t}{X_t}dW_t-\frac{1}{2}*\frac{\sigma^2S_t}{X_t}dt+\frac{\sigma^2S_t}{X_t}dt$$
$$=\frac{h}{X_t}dt$$

>$$ d(\frac{S_t}{X_t}) = \frac{h}{X_t}dt$$
$$ \frac{S_T}{X_T} - \frac{S_t}{X_t} = \int_t^T \frac{h}{X_t}dt$$
$$S_T = (\frac{S_t}{X_t} + \int_t^T \frac{h}{X_t}dt)*X_t*exp[\sigma W_{T-t}-\frac{1}{2}\sigma^2(T-t)]$$
To make things clear:

>$$S_T = (\frac{S_t}{X_t} + \int_t^T \frac{h}{X_u}du)*X_t*exp[\sigma W_{T-t}-\frac{1}{2}\sigma^2(T-t)]$$

>$$S_T = (\frac{S_t}{X_t} + \int_t^T \frac{h}{X_u}du)*X_t*exp[\sigma W_{T-t}-\frac{1}{2}\sigma^2(T-t)]$$

>$$S_T = (\frac{S_t}{X_t} + \int_t^T \frac{h}{X_t*exp[\sigma W_{T-t}-\frac{1}{2}\sigma^2(u-t)]}du)*X_t*exp[\sigma W_{T-t}-\frac{1}{2}\sigma^2(T-t)]$$

>$$S_T = (S_t + h\int_t^T exp[\frac{1}{2}\sigma^2(u-t)-\sigma W_{u-t}]du)*exp[\sigma W_{T-t}-\frac{1}{2}\sigma^2(T-t)]$$

>Replace z=u-t

>$$S_T = (S_t + h\int_0^{T-t} exp[\frac{1}{2}\sigma^2z-\sigma W_{z}]dz)*exp[\sigma W_{T-t}-\frac{1}{2}\sigma^2(T-t)]$$


# References



  [1]: http://static.zybuluo.com/williamhkh/sokkdt6ivb1ag7iw4e8yobib/image_1df1n21nu13ps9t2uo41vku1199.png
  [2]: http://static.zybuluo.com/williamhkh/0tplxy5ef7hu7w2j6km6n9d6/image_1df1n5iupjcd9qi1b7vo6g1rr5m.png




