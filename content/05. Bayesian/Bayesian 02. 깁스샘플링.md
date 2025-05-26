---
title: 02. 깁스샘플링
draft: false
tags:
  - "#샘플링"
  - "#완전조건부분포"
---
## 들어가며
- 우리는 어떠한 알려진 분포를 활용한다면, 분포의 특성을 이용해서 평균과 분산 등의 통계적 대푯값을 제시할 수 있다.
- 그렇다면 분포의 정보를 안다는 것은 매우 중요한 부분 중에 하나인데, 우리는 만약 사후 분포의 모양을 알 수 없다면 어떻게 해야할까? 
- 이러한 의문을 해결하기 위하여 샘플링 기법들을 활용하고 있으며, 본 장에서는 깁스 샘플링에 대한 공부를 이어나가고자 한다.

## 다중선형회귀모형
- 아래와 같은 전형적인 다중선형회귀식을 고려해보자.
$$
Y = X_{1}\beta_{1}+X_{2}\beta_{2}+ \dots + X_{k}\beta_{k} + \epsilon, \, \epsilon|X_{1},X_{2},\dots, X_{k} \sim N(0, \sigma^2\mathbf{I}_{T})
$$
- $T$: 표본 크기
- $Y: T\times 1$, 종속변수
- $\mathbf{X}=(X_{1}, X_{2}, \dots, X_{k})$, 각각 선형 독립(independent) $T\times 1$
- $\epsilon: T \times 1$, 오차항 벡터(→ 등분산(homogenous), No-correlation)
- $\mathbf{I}_{T}$: $T \times T$ 항등행렬(Identify matrix)
- $\beta = (\beta_{1}, \beta_{2}, \dots, \beta_{k})'$, 회귀계수의 벡터
$$
Y|X, \beta, \sigma^2 \sim N(\mathbf{X}\beta, \sigma^2\mathbf{I}_{T})
$$
- 위 식과 같이 표현되며 frequentist에 따르면 [[Time Series, 01. Basic of Regression#Finite sample properties of OLS (Classical assumptions)|회귀 분석의 고전적 가정(Classical Assumption)]] 하에서 빈도주의에 입각한 [[Statistics, 10. Method of Moment for Single Linear Equation Models#Least Squares Estimator (LSE)|최소자승추정량(Ordinary Least Squares Estimator)]]은 아래 식과 같다.
$$
\hat{\beta}_{OLS} = \mathbf{(X'X)^{-1}X'Y}, \, k \times 1
$$
- 반면 베이지안 방법론으로 선형 회귀식을 추정하고자 할 때, 위에서 주어진 회귀 식 만으로는 계량 모형이 완성되지 않는다. → 왜? 베이지안에서는 $\beta$와 $\sigma^2$이 어떻게 주어지는지 (또는 생성되는지)에 대한 사전 분포가 추가적으로 제시되어야 함!! 
	- 베이지안은 항상 파라미터에 대한 불확실성을 정량적으로 제시하기 위함임.
- 일반적으로 각 파라미터의 분포는 다음과 같은 분포를 따름.
$$
\begin{align}
\sigma  & \sim IG\left( \frac{\alpha_{0}}{2}, \frac{\delta_{0}}{2} \right) \\
\beta|\sigma^2  & \sim N(\beta_{0}, \sigma^2B_{0})
\end{align}
$$
- 이렇게 연구자는 경제학 이론이나 선험적인 직관 등 ==설득력 있는 근거==를 기반으로 $\alpha_{0}, \delta_{0}, \beta_{0}, B_{0}$의 구체적인 값들을 설정해야 한다.
- 