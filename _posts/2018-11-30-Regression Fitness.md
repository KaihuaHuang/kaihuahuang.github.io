---
layout:     post
title:      Basic Linear Regression - 2
subtitle:   Evaluate the fittness of regression
date:       2018-11-30
author:     William
header-img: img/post-bg-regression.jpg
catalog: true
tags:
    - Regression
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

# Basic Linear Regression
## $R^2$ - Evaluation from econometrics perspective
SST - total sum of squares. It measures the total variation in the dependent variable.

$$SST = \sum_{i=1}^n(y_i-\bar y)^2$$

SSE - explained sum of squares. It measures the part explained by the estimated regression.

$$SSE = \sum_{i=1}^n(\hat y_i - \bar y)^2$$

SSR - residual sum of squares. It measures the part remains unexplained.

$$SSR = \sum_{i=1}^n(y_i - \hat y_i)^2$$

There is an important equation between these variables.

$$SST = SSE + SSR$$

> $$ SST =  \sum_{i=1}^n(y_i-\bar y)^2 = \sum_{i=1}^n(y_i - \hat y_i + \hat y_i -\bar y)^2 \\ 
= \sum[(y_i-\hat y_i)^2 + 2*(y_i-\hat y_i)(\hat y_i - \bar y) + (\hat y_i - \bar y)^2] \\ 
= \sum(y_i-\hat y_i)^2 + \sum(\hat y_i - \bar y)^2 + 2\sum \epsilon_i(\hat y_i - \bar y) \\
= SSE + SSR + 2\sum \epsilon_i \hat y_i - 2\bar y_i\sum \epsilon_i\\
= SSE + SSR 
$$

> - $\sum \epsilon_i = 0$
> - $cov(\epsilon_i,y_i) =0, E(\epsilon y) = 0$

The properties of $\epsilon$ comes from the derivative of OLS.

As shown above, the higher SSE the bettwe the regression. So we define $R^2$ to measure the fitness.

$$R^2 = \frac{SSE}{SST} = 1 - \frac{SSR}{SST}$$
## Adjusted $R^2$ - Improvement on $R^2$
Actually, when we increase the explanatory variables in the regression, we can always get bettwer $R^2$. But is that good?

Adjusted $R^2$ adds a punishment for increasing explanatory variables.

The adjusted $R^2$ formula (Suppose n is sample size, k is number of explanatory variables):

$$R_{adj}^2 = 1- \frac{\frac{1}{n-k-1}SSR}{\frac{1}{n-1}SST}$$

$R_{adj}^2$ is always smaller than $R^2$


## MSE - Bias Variance tradeoff

<center><img src = 'http://ww1.sinaimg.cn/large/83d6b255ly1fxqgzqsgk7j20bk0b4jvc.jpg'/></center>

MSE define as:

$$MSE = \frac{1}{n}\sum(y_i - \hat f(x_i))^2 = E[(y - \hat f(x))^2]$$

It can be decomposed into three components:

$$ MSE = Var(\hat f(x)) + Bias(\hat f(x))^2 + Var(\epsilon)$$


> $$ MSE = E[(y - \hat f(x))^2] = E[y^2] + E[\hat f(x)^2] - 2E[y \hat f(x)] \\
  = Var(y) + E(y)^2 + Var(\hat f(x)) + E(\hat f(x))^2 - 2E[y \hat f(x)] \\
  = Var(y) + Var(\hat f(x)) + [f(x)^2 + E(\hat f(x))^2 - 2fE[ \hat f(x)]] \\
  = Var(y) + Var(\hat f(x)) + [f(x) - E(\hat f(x))]^2 \\
  = Var(\epsilon) + Var(\hat f(x)) + Bias(\hat f(x))^2
  $$
  
> - $f$ is population regression function, $y = f(x) + \epsilon$
> - $\hat f$ is estimated regression function
> - $E(y) = E[f(x) + \epsilon] = f(x)$
> - $Var(y) = E[(y-E(y))^2] = E[(y-f(x))^2] = E[\epsilon^2] = Var(\epsilon)$
> - $Var(a) = E(a^2) - E(a)^2$
> - $Bias(\hat f(x)) = f(x) - \hat f(x)$

In the above equation, $Var(\epsilon)$ is irreducible, but $Var(\hat f(x))$ and $Bias(\hat f(x))^2$ are reducible.

So a good regression should minimize the reducible items. But bias and variance are negatively correlated and we need to balance them.

Sometimes, when variance is high, we will use some technics like [lasso regreesion or ridge regression]() to lower our variance. (The cost is increasing bias)







