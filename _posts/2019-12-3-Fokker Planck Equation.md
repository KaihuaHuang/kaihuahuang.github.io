---
layout:     post
title:      Local Volatility - 1
subtitle:   Fokker Planck Equation
date:       2019-12-3
author:     William
header-img: img/post-bg-regression.jpg
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

# Motivation for Local Volatility
In the market, implied volatility can be back out by using obeservable market quotes. Options with different strikes and expirations have different Black-Scholes implied volatility. So traders have to continuously change the volatility assumption of BS formula to match the market quotes. 

To avoid this tedious work, naturally, traders want loose the constant volatility assumption and build a volatility model so that the model can always generate no-arbitrage option price. 

# Fokker Planck Equation
## Assumption
Fokker Planck Equation is a PDE that the probability density function (pdf) of future underlying asset price should satisfy. 

Suppose the underlying asset follows below SDE in risk-neutral space:

$$dS_t = \mu(S,t)dt + \sigma(S,t)dw_t$$

Suppose there is an arbitrary twice differenciable function $f$. In finance, $f(S_t)$ can be considered as payoff function. 

## Expectation Euqation
Applying Ito's Lemma to the $f(S_t)$:

$$df = \frac{\partial f}{\partial s}dS + \frac{1}{2}*\frac{\partial^2 f}{\partial s^2}dSdS\\
= (\frac{\partial f}{\partial s}\mu + \frac{1}{2}*\frac{\partial^2 f}{\partial s^2}\sigma^2)dt + \frac{\partial f}{\partial s}\sigma dw$$

Take expectation on both side, then random term $dw$ will vanish:

$$E[df] = E[(\frac{\partial f}{\partial s}\mu + \frac{1}{2}*\frac{\partial^2 f}{\partial s^2}\sigma^2)dt]$$

Rearrange the formula:

$$\frac{dE(f)}{dt} = E[\frac{\partial f}{\partial s}\mu + \frac{1}{2}*\frac{\partial^2 f}{\partial s^2}\sigma^2]$$

Next step will be calculate the $E(f(S_t))$. And we defined a pdf function $p(S,t)$ which can be intuitively considered as the probability of underlying asset end up with S at time t. t can be viewed as maturity time. So it's fixed. The two expectations can be expressed as below:

$$E[f(S_t)] = \int_{-\infty}^{+\infty} f(s)p(s,t)ds$$

And

$$E[\frac{\partial f}{\partial s}\mu + \frac{1}{2}*\frac{\partial^2 f}{\partial s^2}\sigma^2] = \int_{-\infty}^{+\infty} (\frac{\partial f}{\partial s}\mu + \frac{1}{2}*\frac{\partial^2 f}{\partial s^2}\sigma^2)p(s,t)ds\\
=\int_{-\infty}^{+\infty} \frac{\partial f}{\partial s}\mu p(s,t)ds + \int_{-\infty}^{+\infty}\frac{1}{2}*\frac{\partial^2 f}{\partial s^2}\sigma^2p(s,t)ds$$

### Calculate the derivative on First Expectation - Left Hand Side

$$\frac{dE(f)}{dt} = \frac{d(\int_{-\infty}^{+\infty} f(s)p(s,t)ds)}{dt}$$

Just applying the [Leibniz's Rule](https://en.wikipedia.org/wiki/Leibniz_integral_rule)
>![image_1dr6mihr95rk1vvg1pg1o6n19d99.png-4.7kB][1]
>![image_1dr6mj9jgasrmo8mbf1o5slufm.png-16.1kB][2]

So the result of the Left Hand Side is:

$$\frac{dE(f)}{dt} = \int_{-\infty}^{+\infty} \frac{\partial}{\partial t}[f(s)p(s,t)]ds \\
= \int_{-\infty}^{+\infty} f(s)\frac{\partial p(s,t)}{\partial t}ds$$

### Calculate Second Expectation - Right Hand Side
Denote $\frac{\partial f}{\partial s}$ as $f'(s)$ and $\frac{\partial^2 f}{\partial s^2}$ as $f''(s)$. Let's calculate the first intergral.

$$\int_{-\infty}^{+\infty} f'(s)\mu(s,t) p(s,t)ds = f(s)\mu(s,t) p(s,t)|^{\infty}_{-\infty} - \int_{-\infty}^{+\infty} f(s)\frac{\partial [\mu(s,t) p(s,t)]}{\partial s}ds \\
= - \int_{-\infty}^{+\infty} f(s)\frac{\partial [\mu(s,t) p(s,t)]}{\partial s}ds$$

> The intergration by part is used here, where

> $$u = f(x), v = \mu(s,t) p(s,t)$$

> And, the probability of asset price goes into infinity is 0 almost surely 

> $$\lim_{s \to \infty} p(s,t) = 0$$

> so $uv$ term disappear in the last euqation

Then the second intergral is calculated below:

$$\frac{1}{2}*\int_{-\infty}^{+\infty}f''(s)\sigma^2(s,t)p(s,t)ds \\
=\frac{1}{2}*f'(s)\sigma^2(s,t) p(s,t)|^{\infty}_{-\infty} - \frac{1}{2}*\int_{-\infty}^{+\infty} f''(s)\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}ds \\
= - \frac{1}{2}*\int_{-\infty}^{+\infty} f'(s)\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}ds \\
= - \frac{1}{2}*[(f(s)\frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s})|^{\infty}_{-\infty} - \int_{-\infty}^{+\infty} f(s)\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2}ds] \\
= \frac{1}{2}*\int_{-\infty}^{+\infty} f(s)\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2}ds$$

> The intergration by part is used twice here, the first time

> $$u = f'(x), v = \sigma^2(s,t) p(s,t)$$

> the second time

> $$u = f(x), v = \frac{\partial [\sigma^2(s,t) p(s,t)]}{\partial s}$$

> And, the probability of asset price goes into infinity is 0 almost surely 

> $$\lim_{s \to \infty} p(s,t) = 0$$

> so $uv$ term disappear

Don't get lost here! Just substitude the two intergral results back to original formula:

$$E[\frac{\partial f}{\partial s}\mu + \frac{1}{2}*\frac{\partial^2 f}{\partial s^2}\sigma^2] = \frac{1}{2}*\int_{-\infty}^{+\infty} f(s)\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2}ds - \int_{-\infty}^{+\infty} f(s)\frac{\partial [\mu(s,t) p(s,t)]}{\partial s}ds \\
= \int_{-\infty}^{+\infty}f(s)[\frac{1}{2}*\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2} - \frac{\partial [\mu(s,t) p(s,t)]}{\partial s}]ds$$

Above is the final result for second expectation.

### Left Hand Side = Right Hand Side

$$\int_{-\infty}^{+\infty} f(s)\frac{\partial p(s,t)}{\partial t}ds = \int_{-\infty}^{+\infty}f(s)[\frac{1}{2}*\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2} - \frac{\partial [\mu(s,t) p(s,t)]}{\partial s}]ds$$

Rearrange the euqation:

$$\int_{-\infty}^{+\infty} f(s)[\frac{\partial p(s,t)}{\partial t} - \frac{1}{2}*\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2} + \frac{\partial [\mu(s,t) p(s,t)]}{\partial s}]ds = 0$$

This intergral is satisfied with any payoff function $f(s)$. As a result:

$$\frac{\partial p(s,t)}{\partial t} - \frac{1}{2}*\frac{\partial^2 [\sigma^2(s,t) p(s,t)]}{\partial s^2} + \frac{\partial [\mu(s,t) p(s,t)]}{\partial s} = 0$$

Finally! This the the Fokker Planck Equation which should be satisfied by pdf of underlying asset price.

## Fokker Planck Equation and Local Volatility
Why we spend so much time on deriving Fokker Planck Equation? Because in next article, this equation will be the basis to derive the formula of $\sigma^2(s,t)$ which is the what we want at the beginning.












## References
[Fokker Planck Equation Derivation: Local Volatility, Ornstein Uhlenbeck, and Geometric Brownian](https://www.youtube.com/watch?v=MmcgT6-lBoY)  
[The Dupire Formula](http://wwwf.imperial.ac.uk/~mdavis/FDM11/DUPIRE_FORMULA.PDF)  
[Derivation of Local Volatility](https://www.frouah.com/finance%20notes/Dupire%20Local%20Volatility.pdf)  
[DerivationofFokker–Planck EquationfortheLocal VolatilityModel](https://link.springer.com/content/pdf/bbm%3A978-1-137-46275-6%2F1.pdf)  



  [1]: http://static.zybuluo.com/williamhkh/ba0h7aql0urafy86a8xft7cv/image_1dr6mihr95rk1vvg1pg1o6n19d99.png
  [2]: http://static.zybuluo.com/williamhkh/inbqdqn01tznyisx6o0a8tgw/image_1dr6mj9jgasrmo8mbf1o5slufm.png