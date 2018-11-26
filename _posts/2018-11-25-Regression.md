---
layout:     post
title:      Regression Summary
subtitle:   From basic to lasso regression
date:       2018-11-25
author:     William
header-img: img\post-bg-regression.jpg
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
# Regression
## Basic Linear Regression
### Definition
Suppose $y$ is terget value vector, $X$ is input matrix. X has n samples and each sample has m features with 1 intercept.

$$
y = \left[\begin{matrix}
y_1\\
y_2\\
...\\
y_n
\end{matrix} 
\right]
$$


$$
X = \left[\begin{matrix}
1 & x_{11} & x_{12} &... & x_{1m}\\
1 & x_{21} & x_{22} &... & x_{2m}\\
: & : & : & : & :\\
1 & x_{n1} & x_{n2} &... & x_{nm}
\end{matrix} 
\right]
$$

The first column of X is for intercept. The basic linear regression is to find a coefficients vector $w$ so that 

$$
\left[\begin{matrix}
1 & x_{11} & x_{12} &... & x_{1m}\\
1 & x_{21} & x_{22} &... & x_{2m}\\
: & : & : & : & :\\
1 & x_{n1} & x_{n2} &... & x_{nm}
\end{matrix} 
\right] * 
\left[\begin{matrix}
w_0\\
w_1\\
...\\
w_m
\end{matrix} 
\right]
=\left[\begin{matrix}
y_1\\
y_2\\
...\\
y_n
\end{matrix} 
\right]
$$

In short hand, $X*w = y$. In most cases, there is no such $w$ satisfy the equation. Since $X$ is not square matrix then it's not invetible. We cannot simply calculate $w = X^{-1}y$.

### Ordinary Least Squares
Since there is no exactly matched $w$, so we can only try our best to find optimal $w$. Intuitively,  the optimal $w$ should minimize the distance between the dots and line.

![](http://www.sthda.com/english/sthda-upload/images/machine-learning-essentials/linear-regression.png)

The length of red line is:

$$ |y_i - x_iw|$$

Then we can define our loss function as:

$$J(w) = \sum_{i = 1}^n(y_i - x_iw)^2 = (y-Xw)^T(y-Xw)$$

Take derivative on $w$:

$$\frac{\partial J}{\partial w} = -2X^T(y-Xw) = 0$$

As a result:

$$X^T(y-Xw)= 0\\X^Ty - X^TXw = 0\\
X^TXw = X^Ty\\ w = (X^TX)^{-1}X^Ty$$

### Digging into OLS
**Why we use sum of square but not absolute value or third/ fourth power?** 

Everything comes from maximum likelihood estimation!

There is implied assumption - **the residuals are normally distributed!**

Denote $y_i$ is true value, $\hat{y}_i$ is estimated value.

$$\epsilon_i = y_i - \hat{y}_i\\ \epsilon \sim N(\mu,\sigma^2)$$

So the probability for $\epsilon_i$ is:

$$p(\epsilon_i) = \frac{1}{\sqrt{2\pi}\sigma}*exp[-\frac{(\epsilon_i-u)^2}{2\sigma^2}]$$

Then the likelihood function is:

$$ L(\mu,\sigma) = \prod_{i=1}^n \frac{1}{\sqrt{2\pi}\sigma}*exp[-\frac{(\epsilon_i-u)^2}{2\sigma^2}]$$

The log likelihood function would be:

$$ L(\mu,\sigma) = - n\log\sigma - \frac{n}{2}log2\pi - \sum_{i=1}^n\frac{(\epsilon_i-u)^2}{2\sigma^2}\\=- \frac{n}{2}\log\sigma^2 - \frac{n}{2}log2\pi - \sum_{i=1}^n\frac{(\epsilon_i-u)^2}{2\sigma^2}$$

Take derivative on $\mu$ and $\sigma^2$:

$$\frac{\partial L}{\partial \mu} = \frac{\sum(\epsilon_i-\mu)}{\sigma^2}=0$$

$$\frac{\partial L}{\partial \sigma^2}=-\frac{n}{2\sigma^2} + \frac{\sum(\epsilon_i-\mu)^2}{2\sigma^4} \\\\= \frac{\sum(\epsilon_i-\mu)^2-n\sigma^2}{2\sigma^4} = 0$$

Intermediate result:

$$\mu = \frac{\sum \epsilon_i}{n} $$

$$\sigma^2 = \frac{\sum(\epsilon_i-\mu)^2}{n}$$

A good estimation should make $\mu$ and $\sigma^2$ close to zero.




## References
[标准与局部加权线性回归](https://zhuanlan.zhihu.com/p/30422174)  
[岭回归和LASSO](https://zhuanlan.zhihu.com/p/30535220)  
[脊回归（Ridge Regression）](https://blog.csdn.net/daunxx/article/details/51578787)  
[线性回归（Linear Regression）](https://blog.csdn.net/daunxx/article/details/51556677)  
[Lasso回归和岭（Ridge）回归](https://blog.csdn.net/qq_30981697/article/details/71438636)  
[简单易学的机器学习算法——岭回归(Ridge Regression)](https://blog.csdn.net/google19890102/article/details/27228279)  
[线性回归损失函数为什么要用平方形式](https://blog.csdn.net/saltriver/article/details/57544704)  




