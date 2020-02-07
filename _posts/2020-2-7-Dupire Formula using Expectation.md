---
layout:     post
title:      Local Volatility - 3
subtitle:   Dupire Formula as an Expe
date:       2020-2-7
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

# Derive Dupire Formula by Conditional Expectation
## Assumption
Assume the underlying stock follows the geometric brownian motion under Risk-neutral world:

$$dS_t = \mu_tS_tdt+ \sigma_tS_tdw$$

Since it's under RN world, the $\mu_t$ can be expressed as:

$$\mu_t = r_t - q_t$$

Where, $r_t$ is instantaneous risk free rate and $q_t$ is divident.

## Derivation
Now, define a function $f$

$$f(S_T,T) = P(0,T)(S_T-K)^+$$

Note, $S_T$ is a random number, so as $f$.

Apply Ito's Lemma to function $f$:

$$df = [\frac{\partial f}{\partial T} + \mu_T S_T\frac{\partial f}{\partial S_T} + \frac{1}{2}\sigma_T^2S_T^2\frac{\partial^2 f}{\partial S_T^2}]dT + [\sigma_TS_T\frac{\partial f}{\partial S_T}]dw$$

> $$\frac{\partial f}{\partial T} = -r_TP(0,T)(S_T-K)^+$$
> $$\frac{\partial f}{\partial S_T} = P(0,T)1_{(S_T>K)}$$
> $$\frac{\partial^2 f}{\partial S_T^2} = P(0,T)\delta_{(S_T-K)}$$

Plug in the above items

$$df = P(0,T) * [-r_T(S_T-K)^+ + \mu_T S_T1_{(S_T>K)} + \frac{1}{2}\sigma_T^2S_T^2\delta_{(S_T-K)}]dT + P(0,T)*[\sigma_TS_T\delta_{(S_T-K)}]dw$$

> $$(S_T-K)^+ = (S_T-K)1_{(S_T>K)}$$
> $$\mu = r_T - q_T$$

Take expectation on $df$, and $E[dw] = 0$. The second term can be ignored. The expected value of $f$ can be viewed as $dC$ (Because $f$ is discounted payoff).

$$dC = E(df) = P(0,T) * E[-r_T(S_T-K)^+ + \mu_T S_T1_{(S_T>K)} + \frac{1}{2}\sigma_T^2S_T^2\delta_{(S_T-K)}]dT \\
= P(0,T) * E[r_T K1_{(S_T>K)} - q_TS_T1_{(S_T>K)} + \frac{1}{2}\sigma_T^2S_T^2\delta_{(S_T-K)}]dT$$

Rearrange

$$\frac{dC}{dT} = P(0,T) * E[r_T K1_{(S_T>K)} - q_TS_T1_{(S_T>K)} + \frac{1}{2}\sigma_T^2S_T^2\delta_{(S_T-K)}]$$

> Recall the results from [last article](https://kaihuahuang.github.io/2019/12/03/Dupire-Formula/)
> $$C = P(0,T)E[(S_T-K)^+] = P(0,T)E[S_T1_{(S_T>K)}] - P(0,T)KE[1_{(S_T>K)}]$$
> $$\frac{dC}{dK} = -P(0,T)E[1_{(S_T>K)}]$$

$$\frac{dC}{dT} = -K(r_T-q_T)\frac{dC}{dK}-q_TC + \frac{1}{2}P(0,T)E[\sigma_T^2S_T^2\delta_{(S_T-K)}]$$

> Since $\delta_{(S_T-K)}$ is the derivative of $1_{(S_T>K)}$, it only has value at the point $S_T = K$, 0 otherwise . 
> $$E[\sigma_T^2S_T^2\delta_{(S_T-K)}] = E[\sigma_T^2S_T^2|S_T = K]E[\delta_{(S_T-K)}] \\
 = K^2E[\sigma_T|S_T = K]E[\delta_{(S_T-K)}]$$
# References
[Fokker Planck Equation Derivation: Local Volatility, Ornstein Uhlenbeck, and Geometric Brownian](https://www.youtube.com/watch?v=MmcgT6-lBoY)  
[The Dupire Formula](http://wwwf.imperial.ac.uk/~mdavis/FDM11/DUPIRE_FORMULA.PDF)  
[Derivation of Local Volatility](https://www.frouah.com/finance%20notes/Dupire%20Local%20Volatility.pdf)  
[DerivationofFokker–Planck EquationfortheLocal VolatilityModel](https://link.springer.com/content/pdf/bbm%3A978-1-137-46275-6%2F1.pdf)  


