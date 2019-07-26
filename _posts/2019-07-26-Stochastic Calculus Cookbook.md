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
1. Brownian motion is martingale. $E(W_t|F_s) = W_s $
2. $E(W_t^2-t|F_s) = W_s^2-s$
3. $E[exp(\lambda W_t - \frac{1}{2}\lambda^2t)|F_s] = exp(\lambda W_s-\frac{1}{2}\lambda^2s)$
4. $cov(W_t,W_s) = s$

> **Note:** 
> The above proof can be done by replace $W_t = W_t-W_s+W_s$



# References
[Interest Rate Models]()


  [1]: http://static.zybuluo.com/williamhkh/sokkdt6ivb1ag7iw4e8yobib/image_1df1n21nu13ps9t2uo41vku1199.png
  [2]: http://static.zybuluo.com/williamhkh/0tplxy5ef7hu7w2j6km6n9d6/image_1df1n5iupjcd9qi1b7vo6g1rr5m.png




