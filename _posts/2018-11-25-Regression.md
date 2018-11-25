---
layout:     post
title:      Regression Review
subtitle:   From basic to lasso regression
date:       2018-11-25
author:     William
header-img: img/post-bg-regression.jpg
catalog: true
tags:
    - regression
    - math
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
= \left[\begin{matrix}
y_1\\
y_2\\
...\\
y_n
\end{matrix} 
\right]
$$

In short hand, $X*w = y$. If $X$ is invertible then $w = X^{-1}y $

## References
[标准与局部加权线性回归](https://zhuanlan.zhihu.com/p/30422174)  
[岭回归和LASSO](https://zhuanlan.zhihu.com/p/30535220)  
[脊回归（Ridge Regression）](https://blog.csdn.net/daunxx/article/details/51578787)  
[线性回归（Linear Regression）](https://blog.csdn.net/daunxx/article/details/51556677)  
[Lasso回归和岭（Ridge）回归](https://blog.csdn.net/qq_30981697/article/details/71438636)  
[简单易学的机器学习算法——岭回归(Ridge Regression)](https://blog.csdn.net/google19890102/article/details/27228279)  



