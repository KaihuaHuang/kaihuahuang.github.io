---
layout:     post
title:      Equivalent Martingale Measure Result 
subtitle:   Numeraire
date:       2019-11-22
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

# Market Price of Risk
## One Factor
Suppose the derivatives only dependents on $\theta$ and assume the process of $\theta$ is:

$$\frac{d\theta}{\theta} = mdt + sdw$$

m and d only depends on $\theta$ and t.

Suppose the $f$ is the price of derivative which only dependt on $\theta$ and t. Suppose the process followed by $f$ is:

$$\frac{df}{f} = \mu dt + \sigma dw$$

The $\mu$ and $\sigma$ should always satisfy the following formula:
$$\frac{\mu - r}{\sigma} = \lambda$$

And the $\lambda$ is known as the market price of risk of $\theta$. If the $\lambda$ equals to 0, then it's tranditional Risk-Neutral world.

## Extend to Multi Factors

Suppose there are multiple $\theta_i$ follow the process:

$$\frac{d\Theta_i}{\Theta_i} = m_idt + s_idw_i$$

And the the derivative price $f$ can be written as:

$$\frac{df}{f} = \mu dt + \sum_{i=1}^n\sigma_i dw_i$$

Then they should satisfy the following equation:

$$\mu - r = \sum_{i=1}^n\sigma_i\lambda_i$$

Where $\lambda_i$ is the market price of risk for $\theta_i$.

# The Equavalent Martingale Measure Result

Suppose there are 2 traded securities $f$ and $g$ dependent on a single source of uncertainty. And define $\theta$ as follows:
$$\theta = \frac{f}{g}$$

By define the Market Price of Risk $\lambda$ as the volatility $\sigma_g$ of security g. $\theta$ can be proved as Martingale. Below is the prove
> $$df = (r+\sigma_g \sigma_f)fdt + \sigma_f f dw$$

> $$dg = (r+\sigma_g^2)gdt + \sigma_g g dw$$

> Then
> $$\ln f = (r + \sigma_g \sigma_f -\sigma_f^2/2)dt + \sigma_f dw$$

> $$\ln g = (r + \sigma_g^2/2)dt + \sigma_g dw$$

> Subtract $\ln g$ from $\ln f$

> $$d(\ln f - \ln g) = d\ln\frac{f}{g} = -\frac{(\sigma_f - \sigma_g)^2}{2}dt + (\sigma_f -  \sigma_g)dw$$

> Assume $X = \frac{f}{g}$  and use Ito's lemma

> $$d(\frac{f}{g}) = (\sigma_f - \sigma_g)\frac{f}{g}dz$$

By proving that $\theta$ is martingle. This could be used to calculate the price of security $f$.
$$\frac{f_0}{g_0} = E_g(\frac{f_T}{g_T})$$

where $E_g$ denotes the expected value in a world defined by numeraire $g$.

So the problem right now is how to choose $g$.

## Money Market Account as the Numeraire (It's also traditional Risk-Neutral world)
Suppose numeraire g follow the below process:
$$dg = rgdt$$
and 

$$g_0 = 1$$

So the security $g$ can be calculated as below:

$$f_0 = \hat{E}(e^{-\int_0^Trdt}f_T)$$

where $\hat{E}$ is risk-neutral expectation. Because $\lambda = 0$ as defined in $g$'s process.

If we assume the interest rate is constant, it can be rewritten as below:

$$f_0 = e^{-rT}\hat{E}(f_T)$$

## Zero-Coupon Bond Price as Numeraire

Define $g_t = P(t,T)$ as the price at time t of a risk-free zero-coupon bond that pays off $1 at time T. 

So, $g_0 = P(0,T)$ and $g_T = P(T,T) = 1$

Apply this to the formula, we can get the equation of $f_0$ as below:

$$f_0 = P(0,T)E_g(\frac{f_T}{g_T}) =P(0,T)E_g(f_T) $$

Also, if security $f$ is a forward contract and $\theta$ is the underlying Asset. The value at time T would be:

$$f_T = \theta_T - F$$

put this back to formula .

$$f_0 = P(0,T)E_g(\theta_T-F)=P(0,T)(E_g(\theta)-F) = 0$$

where $F$ is the forward price set at time 0 to make the value of forward contract to be 0.