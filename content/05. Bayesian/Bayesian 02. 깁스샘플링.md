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
- 위 식에서 $\beta$에 대한 사전 분산을 $B_{0}$가 아니라 $\sigma^2B_{0}$이라고 한 이유는 두 가지이다.
	1. $\beta$의 사후 분포의 도출이 보다 용이해진다.
	2. 오차항의 크기인 $\sigma^2$이 클수록 연구자가 갖는 $\beta$에 대한 믿음의 강도가 약해질 수 있기 때문이다.
- $\alpha_{0}, \delta_{0}, \beta_{0}, B_{0}$와 같이 사전 분포의 평균이나 분산을 결정하는 파라미터들을 하이퍼 파라미터(hyper-parameter)라고 한다. → 이는 주어진 값이기 때문에 확률 변수(random variable)이 안됨. 
#### Case 1. $\sigma^2$이 알려져 있는 경우
- 알려져 있다는 것은 무엇을 의미할까? → 주어졌다는 의미이고 Random이 아닌 Fixed되었다는 의미이다. 
- 따라서, 더 이상 추정 대상이 아니라는 것이다. 이 경우, 모형은 아래과 같이 수정된다.
$$
\begin{align}
\beta  & \sim N(\beta_{0}, \sigma^2B_{0}), \\
Y|\beta  & \sim N(\mathbf{X}\beta, \sigma^2\mathbf{I}_{T})
\end{align}
$$
- 결과적으로 우리는 $\beta$에 대한 사후 분포만을 도출하면 된다. 
$$
\pi(\beta|Y) = \frac{f(Y|\beta)}{f(Y)}\pi(\beta)
$$
- $f(Y)$는 상수이므로 사후 밀도는 우도와 사전 밀도에 곱에 비례함.
$$
\begin{align}
\pi(\beta|Y)  &  \propto f(Y|\beta)\pi(\beta) \\
 & = N(Y|\mathbf{X}\beta, \sigma^2\mathbf{I}_{T}) \times N(\beta|\beta_{0}, \sigma^2B_{0})
\end{align}
$$
- $N(x|\mu, \Sigma)$는 평균과 분산-공분산이 각각 $\mu$와 $\Sigma$인 정규 분포의 밀도함수다.
- $f(Y|\beta)$: likelihood
- $\pi(\beta)$: $\beta$에 대한 Prior density function이다. 
- $Y|\beta$와 $\beta$ 모두 다변량 정규분포를 따르기 때문에 $f(Y|\beta)$와 $\pi(\beta)$는 각각 아래와 같은 다변량 정규 분포(multivariate normal)의 밀도함수이다.
$$
\begin{align}
f(Y|\beta)  & = N(Y|\mathbf{X}\beta, \sigma^2\mathbf{I}_{T}) \\
 & = \left( \frac{1}{\sqrt{ 2\pi }} \right)^T \left( \frac{1}{\sigma^2} \right)^{T/2}\exp\left( -\frac{1}{2\sigma^2}(Y-\mathbf{X}\beta)'(Y-\mathbf{X}\beta) \right),  \\
 \\
\pi(\beta)  & =N(\beta|\beta_{0}, \sigma^2B_{0}) \\
 & = \left( \frac{1}{2\pi} \right)^K \frac{1}{|B_{0}|^{\frac{1}{2}}} \left( \frac{1}{\sigma^2} \right)^{k/2}\exp\left( -\frac{1}{2\sigma^2}(\beta - \beta_{0})'B_{0}^{-1}(\beta - \beta_{0}) \right)
\end{align}
$$
- 따라서, $f(Y|\beta) \times \pi(\beta)$은 다음과 같이 계산된다.
$$
\begin{align}
f(Y|\beta)\pi(\beta)  & = 
  \left( \frac{1}{\sqrt{ 2\pi }} \right)^T \left( \frac{1}{\sigma^2} \right)^{T/2}\exp\left( -\frac{1}{2\sigma^2}(Y-\mathbf{X}\beta)'(Y-\mathbf{X}\beta) \right)  \\
 & \times\left( \frac{1}{2\pi} \right)^K \frac{1}{|B_{0}|^{\frac{1}{2}}} \left( \frac{1}{\sigma^2} \right)^{k/2}\exp\left( -\frac{1}{2\sigma^2}(\beta - \beta_{0})'B_{0}^{-1}(\beta - \beta_{0}) \right)
\end{align}
$$
- 여기에서 우리가 구하고자 하는 것은 $\beta$에 대한 사후 분포인 $\pi(\beta|Y)$이다. 따라서, $\beta$와 관련없는 항들은 정규화 상수에 불과하기에 제거한다.
$$
\pi(\beta|Y) \propto \exp\left(  - \frac{1}{2\sigma^2} [(Y - \mathbf{X}\beta)'(Y- \mathbf{X}\beta) + (\beta - \beta_{0})'B_{0}^{-1}(\beta-\beta_{0})] \right)
$$
- $\beta'X'Y = Y'X\beta$이고, $\beta'B_{0}^{-1}\beta_{0} = \beta_{0}'B_{0}^{-1}\beta$이기 때문에, 식을 다음과 같이 정리할 수 있음.
$$
\pi(\beta|Y) \propto \exp\left( - \frac{1}{2\sigma^2}(Y'Y - 2\beta'\mathbf{X}'Y + \beta'\mathbf{X'X}\beta + \beta'B_{0}^{-1}\beta_{0} - 2\beta'B_{0}^{-1}\beta_{0} + \beta_{0}'B_{0}^{-1}\beta_{0}) \right)
$$
- 다시 $\beta$와 무관한 $Y'Y$와 $\beta_{0}'B_{0}^{-1}\beta_{0}$를 제외하여 아래와 같은 식을 얻는다.
$$
\begin{align}
\pi(\beta|Y)  & \propto \exp\left( - \frac{1}{2\sigma^2}(-2\beta'\mathbf{X'}Y + \beta'\mathbf{X'X}\beta + \beta'B_{0}^{-1}\beta - 2\beta'B_{0}^{-1}\beta_{0}) \right) \\
 & = \exp\left( - \frac{1}{2\sigma^2}(\beta'(\mathbf{X'X} + B_{0}^{-1})\beta - 2\beta'(\mathbf{X'}Y + B_{0}^{-1}\beta_{0})) \right)
\end{align}
$$
- 위 수식은 $\beta$의 사후 밀도 함수에서 정규화 상수가 누락된 것임.
- 이러한 정규화 상수를 제외한 부분을 **커넬(kernel)** 이라고 칭함.
- 

## Note
###### Note 2.1
- 평균이 $\beta_{1}$이고 분산-공분산이 $\sigma^2B_{1}$이 정규 분포를 따르는 $\beta$의 밀도함수로부터 커넬을 유도해보고자 한다.
$$
\begin{align}
N(\beta|\beta_{1}, \sigma^2B_{1})  & = \left( \frac{1}{\sqrt{ 2\pi \sigma^2 }} \right)^k \frac{1}{\sqrt{ |B_{1}| }} \exp\left( - \frac{1}{2\sigma^2}(\beta-\beta_{1})'B_{1}^{-1}(\beta- \beta_{1}) \right) \\
 & \propto \exp\left( -\frac{1}{2\sigma^2}(\beta - \beta_{1})'B_{1}^{-1}(\beta - \beta_{1}) \right) \\
 & = \exp\left( - \frac{1}{2\sigma^2}(\beta'B_{1}^{-1}\beta - 2\beta'B_{1}^{-1}\beta_{1} + \beta_{1}B_{1}^{-1}\beta_{1}) \right)
\end{align}
$$
- 여기서 $\beta_{1}B_{1}^{-1}\beta_{1}$ 항은 확률변수 $\beta$와 관련없는 상수항이기 때문에 제거 가능 !
$$
\begin{align}
N(\beta|\beta_{1}, \sigma^2B_{1})  & \propto \exp\left( -\frac{1}{\sigma^2}(\beta'B_{1}^{-1}\beta - 2\beta'B_{1}^{-1}\beta_{1}) \right)
\end{align}
$$