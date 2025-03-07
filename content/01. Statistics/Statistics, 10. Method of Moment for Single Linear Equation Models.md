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
- LSE $b_{lse}$ is just a $sample\,analog$ of this expression of $\beta$, obtained by replacing $E(xx')$ and $E(xy)$ with their sample versions $N^{-1}\sum_{i}x_{i}x_{i}'$ and $N^{-1}\sum_{i}x_{i}y_{i}$. Instead of identifying $\beta$ by minimizing the prediction error, here $\beta$ is identified by the "information" (i.e., the assumption) that the observed $x$ is "orthogonal" to the unobserved $u$.
- For any $k \times 1$ constant vector $\gamma$,
$$
\gamma'E(xx')\gamma = E(\gamma'xx'\gamma) = E\{(x'\gamma)(x'\gamma)\} = E\{(x'\gamma)^2\} \geq 0.
$$
- Hence $E(xx')$ is positive semidefinite (p.s.d). Assume that
$$
E(xx') \, \text{is of full rank.}
$$
- As $E(xx')$ is p.s.d., this full rank condition is equivalent to $E(xx')$ being positive definite (p.d.) and thus being invertible. Note that $E(xx')$ being p.d. is equivalent to $E^{-1}(xx')$ being p.d. where $E^{-1}(xx')$ means $\{E(xx')\}^{-1}$.

#### Zero Moments and Independence
- The assumption $E(xu) = 0$ is the weakest for the LSE to be a valid estimator for $\beta$ as can be seen in the next subsection.
- In econometrics, the following two assumptions have been used as well as LSE:
	1. $E(u|x) = 0 \, \{ \iff E(y|x) =x'\beta \,\ \text{for the linear model} \}$
	2. $u$ is independent of $x$ and $E(u) = 0$
- Note that $E(u|x) = 0$ implies $E(u) = E\{E(u|x)\} =0$. for the three assumptions, the following implications hold:
	- independence of $u$ from $x$ and $E(u) = 0 \Longrightarrow E(u|x) = 0 \Longrightarrow E(xu)=0;$ 
	- the last implication holds because $E(xu) = E\{xE(u|x)\} = 0$
		- [[Statistics, 03. Multivariate Probability Distributions#Conditional Expectations]]
- The regressor vector $x$ is often said to be $exogenous$ if any one of three conditions holds. 
- The function $E(y|x) = x'\beta$ is called the $(mean) \, regression \, function$ , which is nothing but a location measure in the distribution of $y|x$.

## Asymptotic Properties of LSE
- As $N \to \infty$, ==the sample will be "close" to the populations==, and we would want $b_{lse}$ to converge to $\beta$ in some sense. This is necessary for $b_{lse}$ to be a "valid" estimator for $\beta$.
- Going further, to be a "good" estimator for $\beta$, $b_{lse}$ should converge fast to $\beta$.
	- For instance, both $N^{-1}$ and $N^{-2}$ converge to 0, and they are valid "estimators" for 0, but $N^{-2}$ is better than $N^{-1}$ because $N^{-2}$ converges to 0 faster.
- This subsection discusses these issues in the names **"consistency"** and **"asymptotic distribution"**.

#### LLN and LSE Consistency
- A $law \, of \, large \, numbers$ (LLN), for and iid random variable ($rv$) sequence $z_{1}, \dots, z_{N}$ with $E(z) < \infty$, hold that
$$
\frac{1}{N} \sum_{i} z_{i} \to^p E(z) \quad \text{as} \,N \to \infty
$$
- where "$\to^p$" denotes convergence in probability:
$$
P\left( |\frac{1}{N}\sum_{i}z_{i} - E(z) | < \epsilon\right) \to 1 \quad \text{as} \, N \to \infty \quad \text{for any constant} \, \epsilon >0;
$$
- (the estimator) $\bar{z}_{N} \equiv N^{-1}\sum_{i}z_{i}$ is said to be "consistent" for (the parameter) $E(z)$.
- If $\bar{z}_{N}$ is a matrix, the LLN applies to each component. This element-wise convergence in probability of $\bar{z}_{N}$ to $E(z)$ is equivalent to $|\bar{z}_{N}-E(z)| \to^p 0$ where $|A| \equiv \{tr(A'A)\}^{1/2}$ for a matrix A-the usual matrix norm-in the sense that the element-wise convergence implies the norm convergence and vice versa.
	- $|A|$ : 행렬 A의 표준 Norm
	- 요소별 수렴(element-wise convergence)과 노름 수렴(norm convergence)가 동등하다.
	- 따라서, $A'A$의 trace의 제곱근으로 정의된 행렬 norm을 사용했을 때, $\bar{z}_{n}$과 $E(z)$ 사이의 차이가 확률적으로 0으로 수렴한다.
- As "$\bar{z}_{N} -E(z) \to^p 0$" means that the difference between $\bar{z}_{N}$ and $E(z)$ converges to 0 in probability, for two rv matrix sequences $W_{N}$ and $M_{N}$, "$W_{N} - M_{N} \to^p 0$" (or $W_{N} \to^p M_{N}$) means that the difference between the two rv matrix sequences converges to zero in probability.
- Substitute $y_{i} = x_{i}'\beta + u_{i}$ into $b_{lse}$ to get
$$
b_{lse} = \beta + \left( \frac{1}{N} \sum_{i}x_{i}x_{i}' \right)^{-1}\frac{1}{N}\sum_{i}x_{i}u_{i}
$$
- $b_{lse} = \left( \frac{1}{N}\sum_{i}x_{i}x_{i}' \right)^{-1}\frac{1}{N}\sum_{i}x_{i}y_{i}$
- Cleary, $b_{lse} \neq \beta$ due to the second term on the right-hand side (rhs) which shows that each $x_{i}u_{i}$ contributes to the deviation $b_{lse} - \beta$. Using the LLN, we have
$$
\frac{1}{N}\sum_{i}x_{i}u_{i} \to^p E(xu) = 0 \quad \text{and} \quad \frac{1}{N}\sum_{i}x_{i}x_{i}' \to^p E(xx').
$$
- Substituting these into the preceding display, we can get $b_{lse} \to^p \beta$, but we need to deal with the inverse: for a square random matrix $W_{N}$, when $W_{N} \to^p W$, will $W^{-1}$ converge to $W^{-1}$ in probability?
- It is known that, for a rv matrix $W_{N}$ and a constant matrix $W_{o}$,
$$
f(W_{n}) \to^p f(W_{o}) \quad \text{if }  W_{N}\to^p W_{o}  \text{ and }  f(\cdot)  \text{ is continuous at }W_{o}.
$$
- The inverse $f(W) = W^{-1}$ of $W$, when it exists, is the adjoint of $W$ divided by the determinant $\det(W)$. Because $\det(W)$ is a sum of products of elements of $W$ and the adjoint consists of determinants, both $\det(W)$ and the adjoint are continuous in $W$, which implies that $W^{-1}$ is continuous in $W$
	- $\det(A) \neq 0$와 동치인 것들 
		- Full rank
		- 가역행렬(invertible matrix & nonsingular matrix) 
		- 역행렬 존재
- Thus $W^{-1}$ is continuous at $W_{o}$ so long as $W^{-1}_o$ exists, and using the last display, we get $A^{-1}_{N} \to^p A^{-1}$ if $A_N \to^p A$ so long as $A^{-1}$ exists; 
- note that $A^{-1}_{N}$ exists for a large enough $N$ bacuase $\det(A_{N}) \neq 0$ for a large enough $N$. Hence,
$$
\left( \frac{1}{N} \sum_{i}x_{i}x_{i}' \right)^{-1} \to^p E^{-1}(xx') < \infty \quad \text{as } N \to \infty
$$
- Therefore, $b_{lse}$ is $\beta$ plus a product of two terms, one consistent for a zero vector and the other consistent for a bounded matrix;
- thus the product is consistent for zero, and we have $b_{lse} \to^p \beta: b_{lse}\, is \, consistent \, for \, \beta$.

#### CLT and $\sqrt{ N }$-Consistency
- For the asymptotic distribution of the **LSE**, a $central \, limit \, theorem$ (**CLT**) is needed that, for an iid random vector sequence $z_{1}, \dots, z_{N}$ with finite second moments,
$$
\frac{1}{\sqrt{ N }} \sum_{i}\{z_{i} - E(z)\} \rightsquigarrow N(0, E[\{ z - E(z) \} \{ z - E(z) \}']) \quad \text{as } N \to \infty
$$
- where "$\rightsquigarrow$" denotes $convergence$ $in$ $distribution$; i.e., letting $\Psi(\cdot)$ denote the df of $N(0, E[\{ z - E(z)\}\{z - E(z)\}'])$,
$$
\lim_{ n \to \infty } P \left\{ \frac{1}{\sqrt{ N }\sum_{i}\{z_{i} - E(z)\}} \leq t \right\} = \Psi(t) \quad \forall t 
$$
