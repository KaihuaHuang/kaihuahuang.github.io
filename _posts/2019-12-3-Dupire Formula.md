---
layout:     post
title:      Local Volatility - 2
subtitle:   Dupire Formula
date:       2019-12-3
author:     William
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - Volatility
    - Stochastic Calculus
    - Math
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

# Quick Review
From the derivation of [Last Article - Fokker Planck Equation](https://kaihuahuang.github.io/2019/12/03/Fokker-Planck-Equation/), below is the final conclusion.

$$\frac{\partial p(s,t)}{\partial t} - \frac{1}{2}*\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2} + \frac{\partial [\mu(s,t) p(s,t)]}{\partial s} = 0$$

Which describe the an euqation that the pdf of future underlying asset price should satisfy. And if the $\mu(s,t)$ and $\sigma(s,t)$ is the risk-neutral parameters for $dS$. Then the $p(s,t)$ is the risk-neutral probability for asset price.

In last article, we generally assume that the SDE for s is as below:

$$dS = \mu(S,t)dt + \sigma(S,t)dw$$

In this article, we will derive the Dupire Formula.


By using it, we can calculate $\sigma^2(s,t)$ from market quotes.

# Dupire Formula
To simpilify the derivation, we uses call option payoff as an example:

$$f(s) = (s-K)^+$$

Other payoff functions just follow the same derivation procedures. Suppose $C(K,t)$ is the observable call option price from market, $K$ is strike price and $t$ is time to maturity.

Under risk-neutral measure, the $C(K,t)$ can be calculated below:

$$C(K,t) = P(0,t)\tilde{E}[(s-K)^+] \\
= P(0,t)\int_K^{+\infty}(s-K)p(s,t)ds$$

Where $P(0,t)$ is the current price of zero coupon bond which pays $1 at time t. And it's observable from market.

$$P(0,t) = e^{-\int_0^t r(u)du}$$

Note that, $r(u)$ is deterministic here. 

## Three Derivatives
To obtain final result, we need to calculate 3 derivative: $\frac{\partial C}{\partial K}$, $\frac{\partial^2 C}{\partial K^2}$ and $\frac{\partial C}{\partial t}$:

- $\frac{\partial C}{\partial K}$


$$\frac{\partial C}{\partial K} = P(0,t)\frac{\partial (\int_K^{+\infty}(s-K)p(s,t)ds)}{\partial K}\\
=P(0,t) [-(K-K)f(K,t)*1 +  \int_K^{+\infty}-p(s,t)ds]\\
=-P(0,t)\int_K^{+\infty}p(s,t)ds$$


- $\frac{\partial^2 C}{\partial K^2}$

$$\frac{\partial^2 C}{\partial K^2} = -P(0,t)\frac{\partial (\int_K^{+\infty}p(s,t)ds)}{\partial K} \\ = -P(0,t)[-p(K,t)*1] = P(0,t)p(K,t)$$

- $\frac{\partial C}{\partial t}$

$$\frac{\partial C}{\partial t} = \frac{\partial P(0,t)}{\partial t}\int_K^{+\infty}(s-K)p(s,t)ds + P(0,t)\frac{\partial (\int_K^{+\infty}(s-K)p(s,t)ds)}{\partial t} \\
= -r(t)C + P(0,t)\int_K^{+\infty}(s-K)\frac{\partial p(s,t)}{\partial t}ds$$

> $$\frac{\partial P(0,t)}{\partial t} = \frac{\partial (e^{\int_0^t r(u)du})}{\partial t} = -r(t)P(0,t)$$


## Main Equation
$\frac{\partial C}{\partial t}$ would be our starting point, rearrange it:

$$\frac{\partial C}{\partial t} + r(t)C = P(0,t)\int_K^{+\infty}(s-K)\frac{\partial p(s,t)}{\partial t}ds$$

Plug in Fokker Planck Equation to replace $\frac{\partial p(s,t)}{\partial t}$:

$$\frac{\partial C}{\partial t} + r(t)C = P(0,t)\int_K^{+\infty}(s-K)[\frac{1}{2}*\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2} - \frac{\partial [\mu(s,t) p(s,t)]}{\partial s}]ds$$

## Two Intergrals
Next step would be solve the 2 intergrals in the right hand side:

- $I_1 = \int_K^{+\infty}(s-K)\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2}ds$

Applying intergral by part, which

$$u = s - K, v = \frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}$$

So the intergral would be:
$$I_1 = (s-K)\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}|^{+\infty}_{s=K} - \int_K^{+\infty}\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}df \\
= 0 - 0 - \int_K^{+\infty}\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}df$$

> Where 

> $$\lim_{s \to +\infty}\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s} = 0$$

> One can intuitively think pdf $p(s,t)$ remains 0 when s is arbitrarily large.

Apply intergral by part again, which

$$u = s, v = \frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}$$

then

$$I_1 = -[s\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}|^{+\infty}_{s=K} - \int_K^{+\infty}\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}ds]\\
= -\sigma^2(s,t) p(s,t)|^{+\infty}_{s=K} \\ = \sigma^2(K,t) p(K,t)$$

Also from $\frac{\partial^2 C}{\partial K^2}$, we know that:

$$\frac{\partial^2 C}{\partial K^2} = P(0,t)p(K,t)$$

Plug it into $I_1$

$$I_1 = \frac{\sigma^2(K,t)}{P(0,t)}\frac{\partial^2 C}{\partial K^2}$$

- $I_2 = \int_K^{+\infty}(s-K)\frac{\partial [\mu(s,t) p(s,t)]}{\partial s}]ds$

Intergral by part, which

$$u = s-K, v = \mu(s,t) p(s,t)$$

Then,

$$I_2 = (s-K)\mu(s,t) p(s,t)|^{+\infty}_{s=K} - \int_K^{+\infty}\mu(s,t) p(s,t)ds \\
= - \int_K^{+\infty}\mu(s,t) p(s,t)ds$$

To make this solvable, we need to further asseme that:

$$\mu(s,t) = [r(t)-q(t)]s$$

Then

$$I_2 = -[r(t)-q(t)]\int_K^{+\infty}sp(s,t)ds$$

From previous derivation, we already know:

$$\frac{-1}{P(0,t)}\frac{\partial C}{\partial K} = \int_K^{+\infty}p(s,t)ds$$

and 

$$\frac{C(K,t)}{P(0,t)} = \int_K^{+\infty}(s-K)p(s,t)ds$$

So

$$I_2 = -[r(t)-q(t)][\frac{C(K,t)}{P(0,t)}-\frac{-K}{P(0,t)}\frac{\partial C}{\partial K}]$$

## Back to Main Equation

$$\frac{\partial C}{\partial t} + r(t)C = P(0,t)[\frac{1}{2}*I_1 - I_2]$$

Pulg in $I_1$ and $I_2$:

$$\frac{\partial C}{\partial t} + r(t)C = P(0,t)[\frac{1}{2}*\frac{\sigma^2(K,t)}{P(0,t)}\frac{\partial^2 C}{\partial K^2} + [r(t)-q(t)][\frac{C(K,t)}{P(0,t)}-\frac{-K}{P(0,t)}\frac{\partial C}{\partial K}]]\\
= \frac{\sigma^2(K,t)}{2}\frac{\partial^2 C}{\partial K^2} + [r(t)-q(t)]C+[r(t)-q(t)]K\frac{\partial C}{\partial K}$$

rearrange it:

$$\sigma^2(K,t) = \frac{\frac{\partial C}{\partial t}+q(t)C+[r(t)-q(t)]K\frac{\partial C}{\partial K}}{\frac{1}{2}\frac{\partial^2 C}{\partial K^2}}$$

## Note

This solution is different from some answers in the references. It's becuase my assumtion for S is:

$$dS = [r(t)-q(t)]Sdt + \sigma(S,t)dw$$

But their assumtion is lognormal distribution:

$$dS = [r(t)-q(t)]Sdt + \sigma(S,t)Sdw$$

Below is their final answer:

$$\sigma^2(K,t) = \frac{\frac{\partial C}{\partial t}+q(t)C+[r(t)-q(t)]K\frac{\partial C}{\partial K}}{\frac{1}{2}K^2\frac{\partial^2 C}{\partial K^2}}$$

# References
[Fokker Planck Equation Derivation: Local Volatility, Ornstein Uhlenbeck, and Geometric Brownian](https://www.youtube.com/watch?v=MmcgT6-lBoY)  
[The Dupire Formula](http://wwwf.imperial.ac.uk/~mdavis/FDM11/DUPIRE_FORMULA.PDF)  
[Derivation of Local Volatility](https://www.frouah.com/finance%20notes/Dupire%20Local%20Volatility.pdf)  
[DerivationofFokker–Planck EquationfortheLocal VolatilityModel](https://link.springer.com/content/pdf/bbm%3A978-1-137-46275-6%2F1.pdf)  



  [1]: http://static.zybuluo.com/williamhkh/ba0h7aql0urafy86a8xft7cv/image_1dr6mihr95rk1vvg1pg1o6n19d99.png
  [2]: http://static.zybuluo.com/williamhkh/inbqdqn01tznyisx6o0a8tgw/image_1dr6mj9jgasrmo8mbf1o5slufm.png