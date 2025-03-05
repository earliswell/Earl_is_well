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
	-  