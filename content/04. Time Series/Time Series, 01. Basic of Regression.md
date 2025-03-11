---
title: 01. Basic of Regression
draft: false
tags:
  - example-tag
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
$$
Y = \beta_{0} + \sum_{j=1}^{p} \beta_{j}X_{j}+\epsilon
$$
- 위 식에서 $\beta$들을 우리는 계수(coefficient) 또는 파라미터(parameter)라고 하는데 우리는 이 때, estimator와 estimate에 대해서 고민을 해보아햐 한다.
	- estimator(추정량) : Random → 분포가 있음 → 평균을 구할 수 있음 $E(\hat{\beta})$
	- estimate(추정치) : Value → 하나의 점 → Fixed
- 즉 우리는 Random에 대한 개념에 대해서 깊이 고민할 필요가 있다. 어떤 변수로 주어진다면 이는 곧 Random하다는 의미이다. Random하다는 것은 분포를 갖을 수 있다. 즉 어떠한 값으로 정해지지 않았다는 것이다. 그렇다면 기존 coefficient는 어떠한 값일까? 우리가 어떤 분포로부터 추정한 값이기 때문에 estimator라고 한다. 또한, 통계에서 expectation of estimator가 estimate와 같다면 이는 unbiased한 성질을 갖는다. 
- 그렇다면 parameter를 고정한 상태에서 X를 대입한다면? → 이는 예측 모델이 되는 것이거 parameter는 fixed 상태이기 때문에 estimate가 된다.


