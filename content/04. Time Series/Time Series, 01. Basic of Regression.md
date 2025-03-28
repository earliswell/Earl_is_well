---
title: 01. Basic of Regression
draft: false
tags:
  - "#OLS"
  - "#BASIC"
  - "#Linear"
---
## Modeling Review

$$
y = \alpha + \beta x+\epsilon
$$
- 위 기본 식에 대해서 우리는 생각해보아야함.
	- $\epsilon$ - Error(에러) 인 경우 우리는 관측하지 못한 어떤 값을 의미함.
	- $\epsilon$ - Reisdual(잔차)라고 부르는 경우 우리는 이를 예측값과 실제 값의 차이를 의미할 때 사용함.
		- $\epsilon = y - \hat{y}$
- 우리가 데이터를 통해 누군가에게 설명을 할 때 특정한 어떤 값을 통해서 우리의 분석 내용에 대해 전달하는 데, 이때 우리는 대푯값을 통해 누군가에게 설명하는 경우가 많음. 
	- 예를 들어, 1시간 당 평균 n개의 빵이 팔렸다. 
	- 이러한 대푯값을 나열하면, 평균, 최빈값, 최소값, 최대값 등등이 있는데, 대부분 평균을 사용함
	- 평균값은 가운데 있는 정도를 의미하고(First moment), 매우 직관적이고, 계산에 용이함(Decompose)[[Statistics, 01. Discrete Distributions#Moments Moment-Generating Functions]]
- 따라서 위 모형에서 $y$는 종속변수(dependence variable, target, response, outcome) 등으로 표현되고, $x$는 독립변수(independence variable, feature, input, predictor) 등으로 표현된다. $\epsilon$은 measurement errors(측정 오차)라고 한다.
- 우리가 어떤 데이터를 분석한다고 했을 때, 우리가 제시해야 할 목적에 대해서 분석 방법이 다양해진다.
	- 어떤 y를 예측하거나 (예측)
	- y에 미치는 영향을 추정하거나 (인과추론)
	- x 자체에 대한 현상을 분석하거나 (현상분석)
		- 예를 들면, 우리 반 키의 평균이 170 정도이다.
- 따라서, 어떤 방법으로 분석할지, 그렇다면 우리의 방법론에 적합한 input과 output에 대해서 깊이 고민해볼 필요가 있다.
## Regression function, 회귀 분석
$$
f(x) = E(Y|X=x)
$$
- 포멀하게 회귀 분석은 X가 특정 값으로 주어졌을 때의 평균값을 의미한다. 즉 conditional expectation이다. 따라서, X가 어떤 특정 값인 Value인 것을 의미한다. 예를 들어, 우리가 관측된 데이터를 사용한다면 X가 given인 것이다.

### Linear regression refresher
$$
\begin{align}
Y & = \beta_{0} + \beta_{1}X_{1} + \beta_{2}X_{2} + \cdots + \beta_{p}X_{p} + \epsilon \\
 & = \underbrace{\beta_{0}+\sum_{j=1}^{P} \beta_{j}X_{j}}_{f_{L}(X)} + \underbrace{\epsilon}_{error}
\end{align}
$$
- 우리가 선형이라고 말하는 것은 파라미터 $\beta$에 대한 선형을 의미한다. 즉 어떤 $X^2$의 값이 들어온다고 하여도 우리는 선형이라 말한다. 하지만 실제 회귀 함수는 선형이 아닐 가능성이 크다.
- ![[Pasted image 20250311210751.png]]
- 위 노란색의 그래프가 신이 만든 실제 회귀 함수라고 했을 때, 실제 선형 회귀 함수와는 다른 모습을 갖는다. 즉, 우리는 에러의 분포로 부터 만들어진 선형 함수를 통해 평균적으로 대표적인 값을 나열하는 것이 선형회귀 분석이라고 볼 수 있다.
	- → 하지만 선형 회귀 함수로도 Quadratic하게 fitting할 수 있다! (제곱텀을 추가하면 됨)

### The meaning of linear
- Here's the linear regression model again:
$$
Y = \beta_{0} + \sum_{j=1}^{p} \beta_{j}X_{j} + \epsilon
$$
- The $\beta_{j}, \, j=0,1,\dots, p$ are called model coefficients or parameters
- 위 식에서 $\beta$들을 우리는 계수(coefficient) 또는 파라미터(parameter)라고 하는데 우리는 이 때, estimator와 estimate에 대해서 고민을 해보아햐 한다.
	- estimator(추정량) : Random → 분포가 있음 → 평균을 구할 수 있음 $E(\hat{\beta})$
	- estimate(추정치) : Value → 하나의 점 → Fixed
- 즉 우리는 Random에 대한 개념에 대해서 깊이 고민할 필요가 있다. 어떤 변수로 주어진다면 이는 곧 Random하다는 의미이다. Random하다는 것은 분포를 갖을 수 있다. 즉 어떠한 값으로 정해지지 않았다는 것이다. 그렇다면 기존 coefficient는 어떠한 값일까? 우리가 어떤 분포로부터 추정한 값이기 때문에 estimator라고 한다. 또한, 통계에서 expectation of estimator가 estimate와 같다면 이는 unbiased한 성질을 갖는다. 
- 그렇다면 parameter를 고정한 상태에서 X를 대입한다면? → 이는 예측 모델이 되는 것이거 parameter는 fixed 상태이기 때문에 estimate가 된다.

### Estimation of the parameters by least squares
- Suppose that we have data $(x_{i}, y_{i}),\, i=1, \dots, n$
$$
y = 
\begin{pmatrix}
y_{1} \\
y_{2} \\
\vdots \\
y_{n}
\end{pmatrix} \qquad
\mathbf{X} = 
\begin{pmatrix}
x_{11}  & x_{12}  & \cdots  & x_{1p} \\
x_{21} & x_{22} & \cdots  & x_{2p} \\
\vdots & \vdots & \vdots & \vdots \\
x_{n1} & x_{n2} & \cdots  & x_{np}
\end{pmatrix} \qquad
\beta =
\begin{pmatrix}
\beta_{1} \\
\beta_{2} \\
\vdots \\
\beta_{p}
\end{pmatrix}
$$

- Linear regression estimates the parameters $\beta_{j}$ by finding the parameter values that minimize the residual sum of squares (RSS):
$$
\begin{align}
 RSS(\hat{\beta}_{j})  & = \sum_{i=1}^{n} (y_{i}-\hat{y_{i}})^2 \\
 & =\sum_{i=1}^{n} (y_{i}-\left[\mathbf{X}\hat{\beta}\right])^2, \quad \text{ where } \,\hat{y}= \mathbf{X}\hat{\beta} \\
\end{align}
$$
- 따라서 우리는 RSS를 항상 최소화 하는 방향으로 고민을 해야한다. 또한, $\mathbf{X}$는 항상 주어지는 것(given)이라고 생각하자. → 따라서 우리가 추정해야 할 것은 $\beta$이기 때문에, 우리는 $\frac{\partial RSS}{\partial \beta_{j}}$를 통해 기울기가 0이되는 부분을 찾아 $\beta_{j}$의 최솟값을 찾아 나가자.
- The quantity $e_{i} = y_{i} - \hat{y}_{i}$ is called a residual.
- 또한, 우리가 Quadratic → Absolute으로 바꾸게 되면 우리는 Median으로 추정하게 된다.
###### 왜 절대 오차는 중앙값을 추정할까?
1. **극단값에 대한 민감도** : 제곱 오차는 큰 오차에 더 많은 가중치를 부여함(→ 제곱이니까), 이로 인해서 잉상치(outliers)에 매우 민감함. 반면, 절대 오차는 오차의 크기에 선형적으로 반응해서 이상치에 덜 민감함.
2. **수학적 증명** : 단일 변수 $c$에 대해서 $\sum|y_{i}-c|$를 최소화하는 문제를 생각해보자. → 이 함수의 최소값은 $c$가 데이터의 중앙값일 때 달성된다.
3. **기하학적 해석** : 숫자들의 중앙값은 모든 숫자로부터 절대 거리의 합을 최소화하는 지점이다. 반면, 평균은 모든 숫자로부터 제곱 거리의 합을 최소화한다.
- 간단한 예시를 들어보자 ! 
	- 만약, 데이터가 \[1, 3, 4, 7, 100\]이 있다고 가정했을 때,
	- 평균 : 23 / 중앙값 : 4
	- 절대 오차의 합이 최소가 되는 지점을 찾으려면, 데이터를 정렬한 다음 중간 위치의 값을 선택한다. 이는 정확히 중앙값의 정의이다.
## Linear regression estimation (Ordinary Least Squares Estimation, OLS)
$$
y_{i} = \beta_{0} + \beta_{1}x_{1} + \cdots + \beta_{n}x_{n} + u_{i} \quad i=1,2, \dots, n
$$
- Let $u$ be the $n \times 1$ vector of unobservable errors or disturbances. → 관측되지 않은 데이터의 벡터
- Then, we can write the linear system for all $n$ observations in matrix notation:
$$
\begin{align}
 & \mathbf{y} = \mathbf{X}\boldsymbol{\beta} + \mathbf{u}  \\ 
 & \text{where } \boldsymbol{\beta} = (\beta_{0}, \beta_{1}, \dots, \beta_{k})'  \qquad  \text{ and }  \underset{n \times (k+1)} {\mathbf{X}}  =  
\begin{bmatrix}
\mathbf{x_{1}} \\
\mathbf{x_{2}} \\
\vdots  \\
\mathbf{x_{n}}
\end{bmatrix}  
= \begin{bmatrix}
1  & x_{11}  &  x_{12} & \cdots & c_{1k} \\
1 & x_{21} & x_{22} & \cdots  & c_{2k} \\
\vdots & \vdots & \vdots & \vdots & \vdots \\
1 & x_{n1} & x_{n2} & \cdots & x_{nk}
\end{bmatrix}
\end{align}
$$
- 하나의 열이 1로 채워져 있는 것을 볼 수 있는데, 이는 절편(intercept)을 의미한다. 즉 $x_{10}, x_{20} \cdots, x_{n0} = 1$ 이다.
- This is a problem in multivariable calculus. For $\boldsymbol{\hat{\beta}}$ to minimize the sum of squared residuals, it must solve the first order condition
- [[Statistics, 09. Linear Models and Estimation by Least Squares#The Method of Least Squares]]
$$
\begin{align}
SSR(\mathbf{b})  & = \sum_{i=1}^{n} (y_{i}-\mathbf{x}_{i}\mathbf{b})^2 \\
 & = \sum_{i=1}^{n} \hat{u_{i}}^2 = \mathbf{\hat{u}}'\mathbf{\hat{u}} = (\mathbf{y}- \mathbf{X}\boldsymbol{\hat{\beta}})'(\mathbf{y}-\mathbf{X}\boldsymbol{\hat{\beta}}) \\
\text{also}, \\
 & \frac{\partial SSR(\boldsymbol{\hat{\beta}})}{\partial \mathbf{b}} = 0. \\
 & \sum_{i=1}^{n} \mathbf{x}_{i}'(y_{i} - \mathbf{x_{i}\boldsymbol{\hat{\beta}}}) = 0
\end{align}
$$
- Which is identical to the first order conditions. We want to write these in matrix form to make them easier to manipulate.
$$
\begin{align}
\mathbf{X}'(\mathbf{y} - \mathbf{X}'\boldsymbol{\hat{\beta}}) = 0 \\
(\mathbf{X'X})\boldsymbol{\hat{\beta}} = \mathbf{X'y} 
\end{align}
$$
- Assuming that the $(k+1) \times (k+1)$ symmetric matrix $\mathbf{X'X}$ is nonsingular(= invertible, independent, full-rank), we can premultiply both sides by $(\mathbf{X'X})^{-1}$ to solve for the OLS estimator $\hat{\beta}$: 
$$
\boldsymbol{\hat{\beta}} = \mathbf{(X'X)}^{-1}\mathbf{X'y} 
$$

## Regression through the Origin
- 우리는 보통 절편(intercept)가 존재하는 상황의 regression을 마주한다. 하지만 만약 절편이 0인 경우에 대해서 고민을 해볼 필요가 있다. 과연 절편이 0인 어떤 상황이 존재할까? 
- 우리는 이러한 답을 더미 변수에서 고민해볼 필요가 있다. 즉, 예를 들어 남자와 여자인 경우 두 개의 변수를 모델에 할당하고 절편이 1인경우 우리는 이러한 매트릭스$\mathbf{X}$의 independent가 깨져, 역행렬을 구할 수 없다.
$$
\mathbf{X} = 
\begin{bmatrix}
1  & 0 & 1 \\
1 & 1 & 0  \\
1 & 1 & 0 
\end{bmatrix}
$$
- 즉, 이는 Full-rank condition이 깨져 매트릭스 $\mathbf{X}$의 역행렬을 구할 수 없다. 이에 따라, 우리는 절편 항을 삭제하거나 남자 또는 여자의 더미 변수를 제거해야 추정이 가능해진다. 그렇다면 절편 항을 삭제하는 경우는 어떤 경우일까? → 이는 우리의 종속 변수 y에 미치는 각 항목(남자, 여자)의 coefficient를 파악하고 싶을 때 절편 항을 0으로 두곤 한다. (일반적으로는 더미 변수 중 주요 변수 하나만 모델에 입력한다.)

### Finite sample properties of OLS (Classical assumptions)
###### Assumption E.1, Linear in Parameters
- The model can be written as in (5), where $\mathbf{y}$ is an observed $n \times 1$ vector, $\mathbf{X}$ is an $n \times (k=1)$ observed matrix, and $\mathbf{u}$  is an $n \times 1$ vector of unobserved errors or disturbances.
- $\beta$, 즉 파라미터에 대한 선형적 가정을 의미한다.
###### Assumption E.2, No Perfect Collinearity
- The matrix $\mathbf{X}$ has rank $(k \times 1)$.
- 매트릭스 $\mathbf{X}$의 independent(= invertible, full-rank, nonsingular)를 의미한다. (→ 역행렬을 구해야 추정할 수 있음.)

→ Assumption E.1 & E.2는 우리가 선형 회귀 분석을 위해서 무조건 만족되는 가정이다.

###### Assumption E.3, Zero Conditional Mean
- Conditional on the entire matrix $\mathbf{X}$, each error $u_{i}$ has zero mean: $E(u_{i}|\mathbf{X})= 0, i=1, 2, \dots, n.$
- 매트릭스 $\mathbf{X}$는 주어지는 것 (Given)임을 잊지 말자. $\mathbf{X}$가 주어졌을 때 에러의 평균은 0이다. 위 조건을 통해서, $E(u\mathbf{X}) = 0$이며, 이는 관측되지 않은 데이터 $u$와 관측된 데이터인 $\mathbf{X}$가 독립임을 의미한다.
- 하지만? 현실에서는 이런 조건이 만족되기 쉽지 않으며, 만약 위 조건이 위배된다면 내생성(bias) 문제가 생긴다. 내생성의 문제는 우리의 coefficient의 크기를 희석시킨다는 문제가 있다. 따라서, 이러한 문제를 해결하기 위해 여러 인과추론의 모델을 통해 위 가정이 위배되었을 때, 어떻게 극복해 나갈 것인지 고민할 필요가 있다.
	- [[Statistics, 10. Method of Moment for Single Linear Equation Models#Omitted Variable Bias]]

###### Assumption E.4, Homoskedasticity
- Conditional on $\mathbf{X}$, the variance are constant:
$$
Var(u_{i}|\mathbf{X}) = \sigma^2, i = 1, 2, \dots, n.
$$
- [[Statistics, 10. Method of Moment for Single Linear Equation Models#LSE Asymptotic Distribution]]
- 이는 에러항의 모든 분산이 동일하다는 의미를 갖는다(동분산성). 이 가정 또한 현실세계에서 많이 위배가 된다. 예를 들어, 소득에 따른 저축액을 분석하는 회귀식이 있다고 가정하자. 그렇다면 100만원을 버는 상태에 저축액의 분산과 1억을 버는 상태에서 저축액의 분산은 과연 동일할까?

###### Assumption E.5, No Serial Correlation
- Conditional on $\mathbf{X}$, the errors are uncorrelated for all $i \neq j$:
$$
Cov(u_{i}, u_{j}|\mathbf{X}) = 0, \, \text{ all } i \neq j.
$$
- Assumption E.5 is automatically satisfied under random sampling.
- Assumption E.5 can be unrealistic, particularly in models that do not include lags of $y_{t}$. (including, say $y_{t-1}$ in $\mathbf{x_{t}}$ is ruled out by Assumption E.3)
- We can combine Assumptions E.4 and E.5 into a simple expression using matrix notation:
$$
Var(\mathbf{u}|\mathbf{X}) = \sigma^2\mathbf{I}_{n}
$$
- Under this assumption, the $n \times n$ variance-covariance matrix $Var(\mathbf{u}|\mathbf{X})$ depends only on a single parameter, $\sigma^2$, and we often say that $\mathbf{u}$ has a **scalar variance-covariance matrix**. (The "scalar" is $\sigma^2$.)
- 가정 5번은 에러텀 간의 상관관계가 존재하지 않는다는 것이다. 이는 특히, 시계열에서 중요한 내용인데, 만약 $y_{t-1}$과 같은 시차 변수 $\mathbf{x}_{t}$에 포함되면 Assumption E.3의 가정이 깨지게 된다. 이는 결국, $y_{t}$와 $y_{t-1}$ 사이에도 상관관계가 생겨, 설명변수와 오차항 사이의 독립성이 깨지게 된다. 이러한 자기상관문제는 OLS 추정량이 여전히 Unbiased할 수 있지만, 표준오차의 추정이 부정호가해져 → 통계적 추론(가설검정 등)에 문제가 생긴다. 
- **자기상관이 표준오차에 미치는 영향**
	-  OLS 추정에서 회귀계수의 표준오차는 기본적으로 오차항의 분산-공분산 구조에 의존한다. 자기상관이 없다는 가정(Assumption E.5)이 위반되면 다음과 같은 일이 발생한다.
	1. **분산의 잘못된 추정**: OLS는 기본적으로 오차항이 독립적이라 가정 $Var(\mathbf{u}|\mathbf{X}) = \sigma^2$. 그러나 자기상관이 있으면 실제 분산-공분산 구조는 더욱 복잡해짐.($Var(\mathbf{u}|\mathbf{X}) \neq \sigma^2$). 오차항의 상관관계가 있으므로 대각행렬($\mathbf{I}$)이 아닌 다른 형태의 행렬이 된다.
	2. **공분산 요소의 무시**: 자기상관이 있으면 기존에 분산으로만 계산되던 값이 공분산 요소까지 포함해야한다. OLS가 공분산 요소를 무시하기 때문에 표준오차가 잘못 추정된다. 그렇다면, 공분산 요소를 고려하지 않고 가설 검정에서 유의하던 것이 유의미하지 않게 될 수 있음.
- **수학적 설명** : OLS 추정량은 분산은 일반적으로 다음과 같이 계산된다.
	- $Var(\boldsymbol{\beta}) = (\mathbf{X'X})^{-1}\mathbf{X'}Var(u^2)\mathbf{X}\mathbf{(X'X)}^{-1}$으로 자기상관이 없다면 $Var(\mathbf{u}) = \sigma^2$이므로, $Var(\boldsymbol{\beta}) = \sigma^2(\mathbf{X'X})^{-1}$와 같이 계산된다. 
	- 그러나, 자기상관이 있으면 $Var(\mathbf{u}) = \sigma^2\Omega$ ($\Omega$는 대각행렬이 아닌 다른 형태)이므로, $Var(\boldsymbol{\beta}) = (\mathbf{X'X})^{-1}\mathbf{X'}(\sigma^2\Omega)\mathbf{X}(\mathbf{X'X})^{-1}$이다.
	- 따라서, 기존 가정의 OLS는 $\Omega=\mathbf{I}$라고 가정하여 이 복잡한 구조를 반영하지 못한다.
- **실제 영향** : 자기상관의 유형에 따라 표준오차가 과소추정되거나(양의 자기상관) 과대추정될 수 있다.(음의 자기상관), 실제로는 양의 자기상관이 더 흔하며, 이 경우에는
	1. 표준오차가 실제보다 작게 추정된다.
	2. $t$-통계량이 실제보다 크게 나타난다.
	3. $p$-value가 실제보다 작게 계산된다.
	4. 결과적으로, 유의하지 않은 변수가 유의한 것으로 잘못 판단될 가능성이 높아짐.

- Assumption E.1 through E.5 comprise the **Gauss-Markov assumptions**. 
- **BLUE**, Best Linear Unbiased Estimator
	- What is the Best? → Small variance & Unbiased.
	- MVUE(Minimum Variance Unbiased Estimator); Case of not regression estimator.

###### THEOREM E.1, Unbiasedness of OLS
- Under Assumptions [[#Assumption E.1, Linear in Parameters]], [[#Assumption E.2, No Perfect Collinearity]] and [[#Assumption E.3, Zero Conditional Mean]], the OLS estimator $\boldsymbol{\hat{\beta}}$ is unbiased for $\boldsymbol{\beta}$.
- **Proof**: Use Assumptions E.1 and E.2 and single algebra to write
$$
\begin{align}
\boldsymbol{\hat{\beta}} &  = (\mathbf{X}'\mathbf{X})^{-1}\mathbf{X}'\mathbf{y} = (\mathbf{X}'\mathbf{X})^{-1}\mathbf{X}'(\mathbf{X}\boldsymbol{\beta} + \mathbf{u}) \\
 &  = (\mathbf{X}'\mathbf{X})^{-1}\mathbf{X'}\mathbf{X}\boldsymbol{\beta}  + (\mathbf{X}'\mathbf{X})^{-1}\mathbf{X'}\mathbf{u} \\
 & = \boldsymbol{\beta} + (\mathbf{X}'\mathbf{X})^{-1}\mathbf{X'}\mathbf{u},
\end{align}
$$
- Where we use the fact that $(\mathbf{X}'\mathbf{X})^{-1}(\mathbf{X'}\mathbf{X}) = \mathbf{I}_{{k+1}}$. Taking the expectation conditional on $\mathbf{X}$ gives
$$
\begin{align}
E(\boldsymbol{\hat{\beta}}|\mathbf{X})  & =  \boldsymbol{\beta} + (\mathbf{X}'\mathbf{X})^{-1}\mathbf{X'}E(\mathbf{u}|\mathbf{X}) \\
 & = \boldsymbol{\beta} + (\mathbf{X}'\mathbf{X})^{-1}\mathbf{X'}\mathbf{0} = \boldsymbol{\beta},
\end{align}
$$
- Because $E(\mathbf{u}|\mathbf{X} ) = 0$ under [[#Assumption E.3, Zero Conditional Mean]].

###### THEOREM E.2, Variance-Covariance Matrix of the OLS Estimator.
- Under Assumptions [[#Assumption E.1, Linear in Parameters]] through [[#Assumption E.5, No Serial Correlation]]
$$
Var(\boldsymbol{\hat{\beta}}|\mathbf{X}) = \sigma^2(\mathbf{X'}\mathbf{X})^{-1}
$$
- **Proof**: From the last formula in equation (E.12), we have
$$
Var(\boldsymbol{\hat{\beta}}|\mathbf{X}) = Var[(\mathbf{X'}\mathbf{X})^{-1}\mathbf{X'}\mathbf{u}|\mathbf{X}] = (\mathbf{X'}\mathbf{X})^{-1}\mathbf{X'}[Var(\mathbf{u}|\mathbf{X})]\mathbf{X}(\mathbf{X'}\mathbf{X})^{-1}
$$
- Now, we use equation (E.13) to get
$$
\begin{align}
Var(\boldsymbol{\hat{\beta}}|\mathbf{X}) &  = (\mathbf{X'}\mathbf{X})^{-1}\mathbf{X'}(\sigma^2\mathbf{I}_{n})\mathbf{X}(\mathbf{X'}\mathbf{X})^{-1} \\
 & =\sigma^2(\mathbf{X'}\mathbf{X})^{-1}\mathbf{X'}\mathbf{X}(\mathbf{X'}\mathbf{X})^{-1} = \sigma^2(\mathbf{X'}\mathbf{X})^{-1} 
\end{align}
$$

###### THEOREM E.4, Unbiasedness of $\hat{\sigma}^2$
- Under Assumptions [[#Assumption E.1, Linear in Parameters]] through [[#E.5]], $\hat{\sigma}^2: E(\hat{\sigma}^2|\mathbf{X}) = \sigma^2$  for all $\sigma^2 >0$.
- **Proof**: Write $\mathbf{\hat{u}} = \mathbf{y} - \mathbf{X}\boldsymbol{\hat{\beta}} = \mathbf{y} - \mathbf{X(X'X)^{-1}X'y} = \mathbf{My} = \mathbf{Mu}$, where $\mathbf{M} = \mathbf{I}_{n} -\mathbf{X(X'X)^{-1}X'}$, and the last equality follows because $\mathbf{MX} = 0$ because $\mathbf{M}$ is symmetric and idempotent,
$$
\mathbf{\hat{u}'u} = \mathbf{u'M'Mu} = \mathbf{u'Mu}.
$$
- Because $\mathbf{u'Mu}$ is scalar, it equals its trace. Therefore,
$$
\begin{align}
E(\mathbf{u'Mu|X})  & = E[tr(\mathbf{u'Mu|X})] = E[tr(\mathbf{Muu'|X})] \\
 & =tr[E(\mathbf{Muu'|X})] = tr[\mathbf{ME(uu'|X)}] \\
 & =tr(\mathbf{M\sigma^2I}_{n}) = \sigma^2tr(\mathbf{M}) = \sigma^2(n-k-1)
\end{align}
$$
- The last equality follows from 
$$
\begin{align}
tr(\mathbf{M}) &  = tr(\mathbf{I}_{n}) - tr[\mathbf{X(X'X)^{-1}X'}]  = n - tr[\mathbf{(X'X)^{-1}X'X}]  \\
 & = n - tr(\mathbf{I}_{k+1}) = n - (k+1) = n-k-1.
\end{align}
$$
- Therefore,
$$
E(\hat{\sigma}^2|\mathbf{X}) = E(\mathbf{u'Mu|X})/(n-k-1) = \sigma^2
$$
- 추가 내용 ! Trace에 대해서 !!
	1. The trace of Matrix is the sum of its diagonal elements, which makes it a linear operation. → Trace는 diagonal 항의 합이다.
	2. For any matrices A and B where the trace exists: $tr(A+B) =tr(A)+ tr(B)$ and for any scalar $c: tr(cA) = c \cdot tr(A)$ → Decomposition이 가능한 특징이 있음.
	3. The Expectation operator has similar linearity properties. → 기댓값은 선형의 성질이 있음. → $E(X+Y) = E(X)+ E(Y)$ and $E(cX) = cE(X)$.
	4. Because both operations are linear, we can exchange their order: $E[tr(A)] = tr[E(A)]$

###### Assumption E.6, Normality of Errors (추가 가정)
- Conditional on $\mathbf{X}$ the $u_{i}$ are independent and identically distributed as Normal(0, $\sigma^2$). Equivalently, $\mathbf{u}$ given $\mathbf{X}$ is distributed as multivariate normal with mean zero and variance-covariance matrix $\sigma^2\mathbf{I}_{n}: u\sim \text{Normal}(0, \sigma^2\mathbf{I}_{n})$.

###### THEOREM E.5, Normality of $\hat{\beta}$
- Under the classical linear model Assumptions [[#Assumption E.1, Linear in Parameters]] through [[#Assumption E.6, Normality of Errors]]. $\boldsymbol{\hat{\beta}}$ conditional on $\mathbf{X}$ is distributed as multivariate normal with mean $\boldsymbol{\beta}$ and variance-covariance matrix $\sigma^2(\mathbf{X'X})^{-1}$

###### THEOREM E.6, Distribution of $\mathbf{t}$ Statistic.
- Under Assumptions [[#Assumption E.1, Linear in Parameters]] through [[#Assumption E.6, Normality of Errors]].
$$
(\hat{\beta}_{j} - \beta_{j}) / se(\hat{\beta}_{j}) \sim t_{n-k+1}, \, j=0, 1, \dots, k.
$$
- **Proof**: The proof requires several steps; the following statements are initially conditional on $\mathbf{X}$. First, by [[#THEOREM E.5, Normality of $ hat{ beta}$]], $(\hat{\beta}_{j} - \beta_{j})/sd(\hat{\beta}_{j}) \sim \text{Normal}(0, 1)$, where $sd(\hat{\beta}_{j}) = \sigma \sqrt{ C_{jj} }$ and $c_{jj}$ is the $j^{th}$ diagonal element of $\mathbf{(X'X)^{-1}}$. Next, under Assumptions [[#Assumption E.1, Linear in Parameters]] through [[#Assumption E.6, Normality of Errors]], conditional on $\mathbf{X}$,
$$
(n -k -1)\hat{\sigma}^2 / \sigma^2 \sim \chi^2_{n-k-1}.
$$
- [[Statistics, 06. Estimation#Confidence Intervals for $ sigma 2$]], [[Statistics, 08. Hypothesis Testing#Testing Hypotheses Concerning Variances]] 참고.
- It follows from Property 1 for the chi-square distribution in Advanced Treatment D that $(\mathbf{u}/\sigma^2)'\mathbf{M(u/\sigma) \sim \chi}^2_{n-k-1}$ (because $\mathbf{M}$ has rank $n-k-1$).
- We also normal distribution in Advanced Treatment D, that $\boldsymbol{\hat{\beta}}$ and $\mathbf{Mu}$ are independent. Because $\boldsymbol{\hat{\sigma}^2}$ is a function of $\mathbf{Mu}, \boldsymbol{\hat{\beta}}$  and $\boldsymbol{\hat{\sigma}^2}$ are also independent.
$$
(\boldsymbol{\hat{\beta}_{j} - \beta_{j}} ) / se(\boldsymbol{\hat{\beta_{j}}}) = [(\boldsymbol{\hat{\beta}_{j} - \beta_{j}} )/sd(\boldsymbol{\hat{\beta_{j}}})]/(\boldsymbol{\hat{\sigma}^2/\sigma^2})^{1/2},
$$
- Which is the ratio of a standard normal random variable and the square root of a $\chi^2_{n-k-1} / (n-k-1)$ random variable. We just Showed that these are independent, so, by definition of a $t$ random variable, $(\hat{\beta}_{j}- \beta_{j}) / se(\hat{\beta}_{j})$ has the $t_{n-k-1}$ distribution. 
- Because this distribution does not depend on $\mathbf{X}$, it is the unconditional distribution of $(\hat{\beta}_{j} - \beta_{j})/se(\hat{\beta}_{j})$ as well.

## The Gauss-Markov Theorem
- Under [[#Assumption E.1, Linear in Parameters]] through [[#Assumption E.5, No Serial Correlation]], $\hat{\beta}_{0},\hat{\beta}_{1}, \hat{\beta}_{2}, \dots, \hat{\beta}_{k}$ are the best linear unbiased estimators (**BLUEs**) of $\beta_{0}, \beta_{1}, \dots, \beta_{k}$, 

###### Table 1.

|           | Coefficient | Std. Error | t-statistic | p-value  |
| --------- | ----------- | ---------- | ----------- | -------- |
| Intercept | 2.939       | 0.3199     | 9.42        | < 0.0001 |
| TV        | 0.046       | 0.0014     | 32.81       | < 0.0001 |
| Radio     | 0.189       | 0.0086     | 21.89       | < 0.0001 |
| Newspaper | -0.0001     | 0.0059     | -0.18       | 0.8599   |
- Coefficient : $\mathbf{(X'X)^{-1}X'y}$
- Std. Error : $\sigma^2(\mathbf{X'X})^{-1}$
- t-statistic: $\frac{\text{Coefficient}}{\text{Std. Error}}$
- p-value: Error term의 분포에 따라 계산되는 Coefficient가 0에 가까울 확률!
	- 보통 t-분포를 따름(→ 왜? 우리는 모집단의 분산을 모르는 경우가 많기 때문에 !)
- 우리는 해석할 때 주의해야 한다. (Holding the other budgets ==fixed==). 즉, 다른 변수들이 고정되어 있고, 하나의 변수의 단위가 변했을 때 우리의 Coefficient만큼의 종속변수에 영향을 준다! 
## Degression: Statistical test
- Let $\hat{\theta}$ be a statistic that is normally distributed with mean $\theta$ and standard error $\sigma_{\hat{\theta}}$. 
	- $\hat{\theta} \sim N(\theta, \sigma^2_{\hat{\theta}})$
	- statistic = estimator = random (분포를 갖는다는 의미임)
	- parameter = estimate = deterministic (어떤 정해진 값이 있음.)
$$
Z = \frac{\hat{\theta} - \theta}{ \sigma_{\hat{\theta}}} \sim N(0, 1)
$$
- $E\left( \frac{\hat{\theta} - \theta}{ \sigma_{\hat{\theta}}} \right) = E\left( \frac{\hat{\theta}}{\sigma_{\hat{\theta}}} \right) - E\left( \frac{\theta}{\sigma_{\hat{\theta}}} \right) = \frac{\theta - \theta}{\sigma_{\hat{\theta}}} = 0$
- $V\left( \frac{\hat{\theta}-\theta}{ \sigma_{\hat{\theta}}} \right) = E\left[\left( \frac{\hat{\theta}-\theta}{\sigma_{\hat{\theta}}} \right)^2 \right] - E\left[\left( \frac{\hat{\theta}- \theta}{ \sigma_{\hat{\theta}}} \right) \right] ^2 = \frac{1}{\sigma_{\hat{\theta}}^2}E[(\hat{\theta}-\theta)^2] = \frac{1}{\sigma_{\hat{\theta}}^2}V(\hat{\theta}) = 1$

$$
P(-z_{\alpha/2} \leq Z \leq z_{\alpha/2}) = 1-\alpha
$$
![[Time_Series, Figure.02.png]]
- Substituting for $Z$ in the probability statement, we have $(1-\alpha)$% confidence interval for $\theta$ 
$$
\begin{align}
P\left( -z_{\alpha/2} \leq\frac{\hat{\theta}- \theta}{\sigma_{\hat{\theta}}} \leq z_{\alpha/2} \right)  = 1- \alpha\\
P (\hat{\theta} - z_{\alpha/2} \sigma_{\hat{\theta}} \leq \theta \leq \hat{\theta} + z_{\alpha/2}\sigma_{\hat{\theta}}) = 1-\alpha
\end{align}
$$

## The Elements of a Statistical Test
1. Null hypothesis, $H_{0}$ → 귀무가설 (단정적인 표현 $H_{0}: \beta = 0$)
2. Alternative hypothesis, $H_{1}$ → 대립가설 (단정적이지 않은 표현,  $\neq, >, <$, 귀무가설이 아니다.)
3. Test statistic: $z, t, \chi^2, F$ → 검정통계량
	1. 모집단의 분포를 아는 경우 또는 샘플의 수가 엄청 많은 경우 →  $z$
	2. 모집단의 분산을 모르거나 샘플의 수가 작은 경우 → $t$
		- $T = \frac{Z}{\sqrt{ \chi^2_{v} /v}}, \,v=dof$
	3. 에러텀의 분산 검정 → $\chi^2$ (Homoscedasticity or Heteroscedasticity)
		- $\chi^2_{k} = \sum_{i}^kZ_{i}^2$
	4. 회귀 모형의 유의성 검정(적어도 하나의 독립변수가 종속변수를 설명하는데 유의한지)를 검정 → $F$ 
		- $H_{0}: \beta_{0}=\beta_{1}=\cdots=\beta_{k}=0$
		- $F_{v_{1}, v_{2}} = \frac{\chi^2_{v_{1}}/v_{1}}{\chi^2_{v_{2}}/v_{2}}$
4. Rejection region → 기각역 (분포와 유의수준)

###### What is $\alpha$ (Type 1 and Type 2 Errors)
- If $H_{0}: \textit{not gulity } \text{ vs } \,H_{\alpha}: \textit{guilty}$

| given/decision                    | deciding not guilty | deciding guilty        |
| --------------------------------- | ------------------- | ---------------------- |
| truly not guilty ($H_{0}$ True)   | right decision      | type-1-error event     |
| truly guilty ($H_{\alpha}$ False) | type-2-error event  | right decision (power) |
###### p-value
- If $W$ is a test statistic, the p-value, or attained significance level($=\alpha$), is the smallest level of significance $\alpha$ for which the observed data indicate that the null. → 귀무가설을 기각할 수 있는 정도에 가장 적은 $\alpha$값.

###### T-test (Point Estimation) → 각각의 $\beta$를 추정
- 점 추정과 구간 추정이 있음. 어떤 하나의 값을 추정하는 것이 Point Estimation. 신뢰구간과 같은 것을 추정하는 것을 interval estimation이라고 함.
- Independent small Sample: [[Statistics, 08. Hypothesis Testing#Small-Sample Hypothesis Testing for $ mu$ and $ mu_{1}- mu_{2}$]]
- Mean Differencing: [[Statistics, 08. Hypothesis Testing#Small-Sample Hypothesis Testing for $ mu$ and $ mu_{1}- mu_{2}$]]

#### Explanatory Variable Selection
- What if we add too many explanatory variables to the model? (Goodness-of-fit, $R^2$) or What it too few explanatory variables are used for estimation? (Omitted variable bias)

###### Goodness-of-fit
- As with simple regression, we can define the total sum of squares (SST), the explained sum of squares (SSE), and the residual sum of squares or sum of squared residuals (SSR) as
$$
\begin{align}
 & SST = \sum_{i=1}^{N} (y_{i}-\bar{y})^2 \\
 & SSE = \sum_{i=1}^{N} (\hat{y}_{i}-\bar{y})^2 \\
 & SSR = \sum_{i=1}^{N} (\hat{u}_{i})^2,  \textit{where } \hat{u} = y- \hat{y}
\end{align}
$$
- Using the same argument as in the simple regression case, we can show that
$$
SST = SSE + SSR
$$
- In other words, the total variation in $y_{i}$ is the sum of the total variations in $\hat{y}_{i}$ and in $\hat{u}_{i}$
$$
\frac{SSR}{SST} + \frac{SSE}{SST} = 1
$$
- Just as in the simple regression case, the R-squared is defined to be
$$
R^2 = \frac{SSE}{SST} = 1 - \frac{SSR}{SST} = \frac{\left( \sum_{i=1}^{N} (y_{i}- \bar{y})(\hat{y}_{i} - \bar{\hat{y}}) \right)^2}{\left( \sum_{i=1}^{N} (y_{i}-\bar{y})^2 \right) \left(  \sum_{i=1}^{N} (\hat{y}_{i}-\bar{\hat{y}})^2 \right)} = \frac{Cov(y, \hat{y})}{SD(y)SD(\hat{y})}
$$
- $\rho = \frac{Cov(x, y)}{SD(x) \cdot SD(y)}$ → 상관계수

