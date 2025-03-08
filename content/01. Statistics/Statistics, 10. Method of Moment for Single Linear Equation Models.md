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
\lim_{ n \to \infty } P \left\{ \frac{1}{\sqrt{ N }}\sum_{i}\{z_{i} - E(z)\} \leq t \right\} = \Psi(t) \quad \forall t 
$$
- when $w_{N} \to^p 0$, it is also denote as $w_{N} = o_{p}(1)$; "$o_{p}(1)$" is the probabilistic analog for $o(1)$ where $o(1)$ is a sequence converging to 0. For $\bar{z}_{N}$, we thus have $\bar{z}_{N} - E(z) = o_{p}(1)$.
	- In comparison to $w_{N} = o_{p}(1)$, "$w_{N}= O_{p}(1)$" means that {$w_{N}$} is $bounded$ $in$ $probability$ (or $stochastically$ $bounded$)-ie., "not explosive as $N \to \infty$" (even if it does not converge to anything) in the probabilistic sense.
	- Note that $o_{p}(1)$ is also $O_{p}(1)$. 
- Formally, $w_{N}=O_{p}(1)$ is that, for any constant $\epsilon > 0$, there exists a constant $\delta_{\epsilon}$ such that
	- "sup"는 "supremum"의 약자로, 최소 상계(least upper bound)를 의미한다. 수학적으로 집합의 모든 원소보다 크거나 같은 값 중에서 가장 작은 값을 가리킨다.
	- $\sup_{N}$은 모든 $N$에 대해서 취할 수 있는 확률 값 $P\{|w_{N} > \delta_{\epsilon} \}$의 최댓값을 의미한다.
	- 이 식은 어떤 $N$을 선택하더라도, 확률 $P\{|w_{N} > \delta_{\epsilon}\}$가 항상 $\epsilon$보다 작다는 것을 의미한다.
$$
\sup_{N}P\{|w_{N} > \delta_{\epsilon}|\} < \epsilon.
$$
- To understand $O_{p}$ better, consider $N^{-1}$ and $N^{-2}$, both of which converge to 0.
- Observe $N^{-1}/N^{-1} = 1$, but $N^{-1}/N^{-1+\epsilon} = 1/N^{\epsilon} \to 0$ whereas $N^{-1} / N^{-1-\epsilon} = N^{\epsilon} \to \infty$  for any constant $\epsilon >0$. Thus the "(fastest) convergence rate" is $N^{-1}$ which, when divided into $N^{-1}$, makes the resulting ratio bounded. Analogously, the convergence rate for $N^{-2}$ is $N^{-2}$. 
- Now consider $z_{N} \equiv z/\sqrt{ N }$ where $z$ is a $rv$. Then $\sqrt{ N }z_{N} = z = O_{p}(1)$ (or $z_{N} = O_{p}(1/\sqrt{ N })$) because we can choose $\delta_{\epsilon}$ for any constant $\epsilon > 0$ such that
	- $\sqrt{ N }z_{N} = z$가 $O_{p}(1)$(확률적으로 유계)임을 보여줍니다.
	- 따라서 $z_{N}$은 $O_{p}(1/\sqrt{ N })$이다. $\to$ 즉, 0으로 수렴하는 속도가 $1/\sqrt{ N }$이다.
$$
\sup_{N}P(|\sqrt{ N }z_{N}> \delta_{\epsilon}) = \sup_{N}P(|z| > \delta_{\epsilon}) = P(|z| > \delta_{\epsilon}) < \epsilon.
$$
- For an estimator $a_{N}$ for a parameter $\alpha$, in most cases, we have $\sqrt{ N } (a_{N} - \alpha)=O_{p}(1)$: $a_{N}$ is $\sqrt{ N }-consistent.$ This means that $a_{N}\to^p \alpha$, and that the convergence rate is $N^{-1/2}$ which, when divided into $a_{N} - \alpha$, ==makes the resulting product bounded in probability.== For most cases in our discussion, it would be harmless to think of the $\sqrt{ N }$-consistency of $a_{N}$ as $\sqrt{ N } (a_{N}-\alpha)$ converging to a normal distribution as $N\to \infty$.
- Analogously to $o(1)O(1) = o(1)$-"a sequence converging to zero" times "a bounded sequence" converges to zero-it holds that $o_{p}(1)O_{p}(1) = o_{p}(1)$. Likewise, $o_{p}(1) + O_{p}(1) = O_{p}(1)$.
###### Slutsky Lemma
- shows more: if $w_{N} \rightsquigarrow w$ (thus $w_N = O_{p}(1)$) and $m_{N} \to^p m_{o}$, then
	1. $m_{N}w_{N} \rightsquigarrow m_{o}w$ : not just the product $m_{N}w_{N}$ is $O_{p}(1)$, its asymptotic distribution is that of $w$ times the constant $m_{o}$.
	2. $m_{N}w_{N} \rightsquigarrow m_{o}+w$ : can be understood analogously.

#### LSE Asymptotic Distribution
- Observe
$$
\sqrt{ N }(b_{lse} - \beta) = \left( \frac{1}{N} \sum_{i}x_{i}x_{i}' \right)^{-1}\cdot \frac{1}{\sqrt{ N }} \sum_{i}x_{i}u_{i}
$$
- From the CLT, we have
$$
\frac{1}{\sqrt{ N }} \sum_{i}x_{i}u_{i} \rightsquigarrow N \{0, E(xx'u^2)\}.
$$
- Using Slutsky Lemma (1),
	- if $B_{N} \rightsquigarrow N(0, C )$ and $A_{N} \to^p A$, then $A_{N}B_{N} \rightsquigarrow N(0, ACA')$
- Apply this to
$$
B_{N} = \frac{1}{\sqrt{ N }}\sum_{i}x_{i}u_{i} \text{ and } A_{N} = \left( \frac{1}{N}\sum_{i}x_{i}x_{i}' \right)^{-1} \to^p E^{-1}(xx')
$$
- to get
$$
\sqrt{ N }(b_{lse}-\beta) \rightsquigarrow N(0, \Omega) \quad \text{where } \Omega \equiv E^{-1}(xx')E(xx'u^2)E^{-1}(xx'): \quad (^*)
$$
- $\sqrt{ N }(b_{lse}- \beta)$ is $asymptotically \,\, normally \,\, with \,\, mean \,\, 0 \,\, variacne \,\, \Omega$.
$$
b_{lse} \sim N \left\{  \beta , \frac{1}{N}E^{-1}(xx')E(xx'u^2)E^{-1}(xx')  \right\} \quad (^{**})
$$
- The asymptotic variance $\Omega$ of $\sqrt{ N }(b_{lse}- \beta)$ can be estimated consistently with (this point will be further discussed later)
$$
\Omega_{N} \equiv \left( \frac{1}{N} \sum_{i}x_{i}x_{i}' \right)^{-1}\left( \frac{1}{N}\sum_{i}x_{i}x_{i}'\hat{u}_{i}^2 \right) \left( \frac{1}{N} \sum_{i}x_{i}x_{i}' \right)^{-1}
$$
- Alternatively (and informally), the asymptotic variance of $b_{lse}$ is estimated consistently with 

$$
\frac{\Omega_{N}}{N} = \left( \sum_{i}x_{i}x_{i}' \right)^{-1}\left( \sum_{i}x_{i}x_{i}'\hat{u}_{i}^2 \right)\left( \sum_{i}x_{i}x_{i}' \right)^{-1}
$$
## $R^2$ 
- Recall the LSE asymptotic variance estimator $\Omega_{N}/N \equiv [\omega_{N, hj}], h, j= 1, \dots,k;$.i.e, the element of $\Omega_{N}/N$ in row $h$ and column $j$ is denoted as $\omega_{N, hj}$. The t-$values$ (t-$ratios$, or z-$values$) and defined as 
$$
\frac{b_{lse, j}}{\sqrt{ \omega_{N, jj} }}, \quad j=1,\dots, k, \quad \text{where } b_{lse} = (b_{lse, 1}, \dots, b_{lse, k})'.
$$
- Since the diagonal of $\Omega_{N}/N$ is the asymptotic variances of $b_{lse, j}, j=1, \dots, k$, the $j$th t-value asymptotically follows $N(0, 1)$ under the $H_{0} : \beta_{j}=0$, and hence it is a test statistic for $H_{0} : \beta_{j}=0$. The off-diagonal terms of $\Omega_{N}/N$ are the asymptotic covariances for $b_{lse, j}, j=1, \dots, k$, and ==they are used for hypotheses involving multiple parameters.==
- The "$standard \,\, error$ (of model)" $s_{N}$ and "R-$squared$" $R^2$ are defined as 
$$
\begin{align}
 & s_{N} \equiv \left( \frac{\sum_{i}\hat{u}_{i}^2}{N-k} \right)^{1/2} \to^p SD(u), \\
 & R^2 \equiv 1 - \frac{N^{-1}\sum_{i}\hat{u}_{i}^2}{N^{-1}\sum_{i}(y_{i}- \bar{y})^2} \to^p 1 - \frac{V(u)}{V(y)} = \frac{V(x'\beta)}{V(y)}, \quad \text{as} \\
 & V(y) = V(x'\beta + u) = V(x'\beta) + V(u) \quad \text{bacause } COV(x'\beta, u)  = 0.
\end{align}
$$
- $R^2$ shows the proportion of $V(y)$ that is explained by $x'\beta$, and $R^2$ measures the "$model \,\, fitness$." In general, the higher the $R^2$ is the better, because the less is buried in the unobserved $u$. 
- But this statement should be qualified, because $R^2$ keeps increasing by adding more regressors into the model.
$$
R^2 = \frac{\left\{  \sum_{i}(\hat{y}_{i}-\bar{\hat{y}})(y_{i}- \bar{y})  \right\}^2}{\sum_{i}(\hat{y}_{i} - \bar{\hat{y}})^2 \cdot \sum_{i}(y_{i}-\bar{y}^2)} = (\text{sample correlation of Y and } \hat{Y})^2
$$
## Omitted Variable Bias
- In the model $y = x'_{f}\beta_{f} + x_{g}' + \beta_{g} +u$, what happens if $x_{g}$ is not used in estimation? 
	- This is an important issue, as we may not have (or use) all relevant regressors in the data. With $x_{g}$ not used, $x_{g}'\beta_{g} + u \equiv v$ becomes the new error term in the model, and the consequence of not using $x_{g}$ depends on $COR(x_{f}, x_{g})$.
	- To simplify the discussion, assume that the model is written in mean-deviation form, i.e., $E(y) = E(x'_{f})\beta_{f} + E(x_{g}')\beta_{g} + E(u)$ is subtracted from the model to yield
$$
y - E(y) = \{ x_{f}' - E(x_{f}') \}\beta_{f} + \{x_{g}' - E(x_{g}') \}\beta_{g} + \{ u - E(u) \}
$$
- and we redefine $y$ as $y - E(y)$, $x_{f}$ as $x_{f} - E(x_{f})$ and so on. So long as we are interested in slopes in $\beta_{f}$, the mean deviation model is adequate.
- If $COR(x_{f}, x_{g}) = 0$ (i.e., if $E(x_{f}x_{g}) = 0$), then $\beta_{f}$ can still be estimated consistently by the LSE of $y$ on $x_f$. The only downside is that, in general, $SD(v) > SD(u)$ as $v$ has more terms than $u$, and thus $R^2$ will drop.
- If $COR(x_{f}, x_{g})  \neq 0$, however, then $COR(x_{f}, v) \neq 0$ makes $x_f$ an $endogenous$ $regressor$ and ==the LSE becomes inconsistent==. Specifically, the LSE of $y$ on $x_{f}$ is 
$$
\begin{align}
b_{f}  & = \left( \frac{1}{N} \sum_{i}x_{if}x_{if}' \right)^{-1}\frac{1}{N}\sum_{i}x_{i}y_{i} \\
 & = \left( \frac{1}{N} \sum_{i}x_{if}x_{if}' \right)^{-1}\frac{1}{N}\sum_{i}x_{if}(x_{if}'\beta_{f} + v_{i}) \\
 & = \beta_{f} + \left( \frac{1}{N} \sum_{i}x_{if}x_{if}' \right)^{-1}\frac{1}{N}\sum_{i}x_{if}v_{i} \\
& = \beta_{f} + \left( \frac{1}{N} \sum_{i}x_{if}x_{if}' \right)^{-1}\frac{1}{N}\sum_{i}x_{if}(x_{ig}'\beta_{g}+u_{i}) \\
& = \beta_{f} + \left( \frac{1}{N} \sum_{i}x_{if}x_{if}' \right)^{-1}\frac{1}{N}\sum_{i}x_{if}x_{ig}' \cdot \beta_{g}  + \left( \frac{1}{N} \sum_{i}x_{if}x_{if}' \right)^{-1} \frac{1}{N}\sum_{i}x_{if}u_{i}  \\
 & \text{ which is constant for } \beta_{f} + E^{-1}(x_{f}x_{f}')E(x_{f}x_{g}') \cdot \beta_{g}
\end{align}
$$
- The term other than $\beta_{f}$ is called the $omitted$ $variable$ $bias$, which is 0 if $either \,\, \beta_{g}= 0$ (i.e., $x_g$ is not omitted at all) or if $E^{-1}(x_{f}x_{f}')E(x_{f}x_{g}') = 0$ which is the population linear projection coefficient of regressing $x_{g}$ on $x_{f}$.
- In simple words, if $COR(x_{f}, x_{g} = 0)$, then there is no omitted variable bias. When LSE is run on some data and if resulting estimates do not make sense intuitively, in most cases, the omitted variable bias formula will provide a good guide on what might have gone wrong.
- One question that might arise when $COR(x_{f}, x_{g}) \neq 0$ is what happens if a subvector $x_{f_{2}}$ of $x_{f}$ is correlated to $x_{g}$ while the other subvector $x_{f1}$ of $x_{f}$ is not where $x_{f} = (x_{f1}', x_{f2}')'$. In this case, will $x_{f1}$ still be subject to the omitted variable bias? The answer depends on $COR(x_{f1}, x_{f2})$ as can be seen in
$$
\begin{align}
E^{-1}(x_{f}x_{f}')E(x_{f}x_{g}') & =  
\begin{bmatrix}
E(x_{f1}x_{f1}')  & E(x_{f1}x_{f2}') \\
E(x_{f2}x_{f1}')  & E(x_{f2}x_{f2}')
\end{bmatrix}^{-1} \begin{bmatrix}
0 \\
E(x_{f2}x_{g}') 
\end{bmatrix} \text{ as } E(x_{f1}x_{g}') = 0 \\
 & = 
\begin{bmatrix}
0 \\
E^{-1}(x_{f2}x_{f2}')E(x_{f2}x_{g}')
\end{bmatrix} \text{ if } E(x_{f1}x_{f2}') = 0.
\end{align}
$$
- Hence if $E(x_{f1}x_{f2}') = 0$, then there is no omitted variable bias for $x_{f1}$. Otherwise, the bias due to $E(x_{f2}x_{g}') \neq 0$ gets channeled to $x_{f1}$ through $COR(x_{f1} x_{f2})$.
- In this case $COR(x_{f1}, x_{f2}) = 0, \,COR(x_{f1}, x_{g})=0$ but $COR(x_{f2}, x_{g}) \neq 0$, we can in fact use only $x_{f1}$ as regressors-no omitted variable bias in case. Nevertheless, using $x_{f2}$ as regressors makes the model error term variance smaller, which leads to a higher $R^2$ and higher t-values for $x_{f1}$. 
	- 위 문장은 $x_{f}$의 subvector 중에서 우리가 생략한 변수와 내생성 문제를 발생시키는 변수에 대한 내용을 말하고 있다. 따라서, $x_{f2}$의 경우가 위 Case에 해당하는 내용인데, 우리는 만약 $x_{f1}$만 회귀분석에 사용한다면 이는 내생성의 문제가 발생되지 않는다. → 상관관계가 0이기 때문에
	- 하지만 그럼에도 불구하고 우리는 $x_{f2}$를 회귀분석 식에 할당하는 것이 우리의 모델 오차항의 분산을 줄기오, 이는 더 높은 $R^2$(설명력)을 얻으며 $x_{f1}$에 대한 높은 t-통계량을 얻는다.
		- 