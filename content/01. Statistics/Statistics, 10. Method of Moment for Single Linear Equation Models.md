---
title: 10. Method of Moment for Single Linear Equation Models.
draft: false
tags:
  - "#OLS"
  - "#LSE"
  - "#consistency"
---

## Least Squares Estimator (LSE)
- This Section introduces standard linear models with exogenous regressors, and then reviews least squares estimator (LSE) for regression functions, which is a "bread-and-butter" estimators in econometrics. Differently from the conventional approach, however, LSE will be viewed as a MOM. Also differently from the conventional approach, we will adopt a large sample framework and invoke only a few assumptions.
## LSE as a Method of Moment (MOM)
- [[Statistics, 07. Properties of Point Estimators and Methods of Estimation#The Method of Moments]]를 참고해서 연결짓기
- Consider Linear Model
$$
y_{i} = x'_{i}\beta + u_{i}, i=1, \dots, N
$$
- where $x_{i}$ is a $k \times 1$ "regressor" vector with its first component being 1
	- $x_{i} = (1, x_{i_{2}} \dots, x_{ik})'$
- $\beta=(\beta_{1}, \dots, \beta_{k})'$ is a $k \times 1$ parameter vector reflecting effect of $x_{i}$ on $y_{i}$ and $u_{i}$ is an error term. In $\beta, \beta_{1}$ is called the "intercept" whereas $\beta_{2}, \dots, \beta_{k}$ are called the "slopes".
- The left-hand side variable $y_{i}$ is the "dependent" or "response" variable, whereas components of $x_{i}$ are "regressors", "explanatory variables", or "independent variables".
	- $x_{i}$ as a collection of the observed variable affecting $y_{i}$
	- $u_{i}$ as a collection of the unobserved variable affecting $y_{i}$
- Main Goal : Finding $\beta$ with data $(x'_{i}, y_{i}), i=1, \dots, N$.
- Assume : $(x'_{i}, y_{i}), i=1, \dots, N$, are $independent \,\, and \,\, identical \,\, distributed \,\,(iid)$ unless otherwise noted, which means that each $(x'_{i}, y_{i})$ is an independent draw from a common probability distribution.
- The linear model is linear in $\beta$, but not necessarily linear in $x_{i}$, and it is more general than it looks. For instance, $x_{3}$ may be $x_{2}^2$, in which case $\beta_{2}x_{2}+\beta_{3}x_{2}^2$ depicts a quadratic relationship between $x_{2}$ and $y:$ the "effect" of $x_{2}$ on $y$ is then $\beta_{2} + 2\beta_{3}x_{2}$- the first derivative of $\beta_{2}x_{2} + \beta_{3}x_{2}^2$ with respect to (wrt) $x_2$. 
	- Example 1), with $y$ monthly salary and $x_{2}$ age, the effect of age on monthly salary may be quadratic: going up to a certain age and then declining after. Also $x_4$ may be $x_{2}x_{3},$ in which case the effect of $x_{2}$ on $y$ is  $\beta_{2}+ \beta_{4}x_{3}$
		- $\beta_{2}x_{2}+\beta_{3}x_{3}+\beta_{4}x_{2}x_{3}=(\beta_{2}+\beta_{4}x_{3})x_{2}+\beta_{3}x_{3}$
	- For instance, $x_{3}$ can be education level: the effect of age on monthly salary is not the constant slope $\beta_{2}$, but $\beta_{2} + \beta_{4}x_{3}$ which varies depending on education level. The display can be written also as $\beta_{2}x_{2} + (\beta_{3} + \beta_{4}x_{2})x_{3}$ to be interpreted analogously.
		- The term $x_{2}x_{3}$ is called the $interaction \,term$ between $x_{2}$ and $x_{3}$, and its coefficient is the interaction effect.
		- 회귀계수 해석에 주의해야함. (Be careful with the interpretation of the interaction term coefficients.) → 

#### LSE and Moment Conditions
- The least squares estimator (LSE) for $\beta$ is obtained by minimizing
$$
\frac{1}{N}\sum_{i}(y_{i}- x'_{i}b)^2
$$
- wrt $b$, where $y_{i} - x'_{i}b$ can be viewed as a "prediction error" in predicting $y_{i}$ with the linear function $x'_{i}b$. LSE is also often called $ordinary$ _LSE (OLS)_, relative to "generalized LSE" to appear later.
$$
\frac{1}{N}\sum_{i}(y_{i} - x'_{i}b_{lse}) = 0 \iff \frac{1}{N}\sum_{i=1}x_{i}y_{i } = \frac{1}{N}\sum_{i}x_{i}x'_{i}\cdot b_{lse}.
$$
- Assuming that $N^{-1}\sum_{i}x_{i}x'_{i}$ is invertible, solve this for $b_{lse}$ to get 
$$
b_{lse} = \left( \frac{1}{N}\sum_{i}x_{i}x_{i}' \right)^{-1} \cdot \frac{1}{N}\sum_{i}x_{i}y_{i} = \left( \sum_{i}x_{i}x_{i}' \right)^{-1}\cdot \sum_{i}x_{i}y_{i}
$$
- The residual $\hat{u}_{i} \equiv y_{i} - x_{i}'b_{lse}$, which is an estimator for $u_{i}$, has zero sample mean and zero sample covariance with the regressors due to the first-order condition:
$$
\frac{1}{N}\sum_{i}x_{i}(y_{i}-x_{i}'b_{lse}) = \left( \frac{1}{N}\sum_{i}\hat{u}_{i} , \frac{1}{N}\sum_{i}x_{i_{2}}\hat{u}_{i}, \dots, \frac{1}{N}\sum_{i}x_{ik}\hat{u}_{i} \right)
$$
- Instead of minimizing $N^{-1}\sum_{i}(y_{i} - x_{i}'b)^2$, LSE  can be motivated directly from a moment condition. Observe that the LSE first-order condition at $b=\beta$ is $N^{-1}\sum_{i}x_{i}u_{i} = 0$, and its population version is 
$$
\begin{align}
& E(xu) = 0 \iff
\begin{bmatrix}
E(u) \\
E(x_{2}u) \\
\vdots \\
E(x_{k}u)
\end{bmatrix} = 
\begin{bmatrix}
0 \\
0 \\
\vdots \\
0
\end{bmatrix} \\
\iff
& E(u) = 0, \,COV(x_{j},u) = 0 \, (\text{or } COR(x_{j}, u) = 0), j = 2, \dots, k
\end{align}
$$

- as $COV(x_{j},u) = E(x_{j,} u) - E(x_{j})E(u)$, where $COV$ and $COR$ stand for covariance and correlation, respectively. Replacing $u$ with $y - x'\beta$
$$
E\{x(y - x'\beta)\} = 0 \iff E(xy) = E(xx')\beta
$$
- which is a restriction on the joint distribution of $(x' , y)$. Assuming that $E(xx')$ is invertible, we get
$$
\beta = \{E(xx')\}^{-1} \cdot E(xy)
$$
