---
title: 01. 베이지안 계량경제학의 이해
draft: false
tags:
  - "#bayesian"
  - "#베이지안"
  - "#확률과정"
---
## 통계에 대해서
- 우리는 종종 통계가 왜 필요한 지에 대해서 고민을 해보아야 한다.
- 통계는 왜 필요할까?
$$
Y = X\beta+\epsilon
$$
- 우리는 통계를 배우면 이러한 선형 회귀 모형에 대해서 공부를 하게 된다. 여기에서 우리가 관심있는 값은 무엇일까? 바로 $\beta$이다. 즉, 통계에서는 설명변수 $X$가 $Y$에 미치는 영향의 크기인 $\beta$을 알아내고 싶은 것이다. 
- 여기에서 $\beta$는 모수(Parameter)이다. 이 때까지 배워왔던 **빈도주의(Frequentist)** 관점에서는 신의 준 값으로 즉 고정된(fixed) 값이다. 이것을 무엇을 의미하냐면, 우리의 샘플 데이터는 Random의 형식을 띄고, 이를 통해 추정한 값 $\hat{\beta}$의 통계적 성질들을 이용해 어떠한 "점(Point)"을 추정한다고 생각하면 된다. 
- 이와 반대로 **베이지안(Bayesian)** 관점에서 $\beta$는 Random이다. → 이것의 의미하는 것은 우리는 관측된 데이터를 통해서 모수의 불확실성을 나타내는 것이다. 그렇다면 우리는 불확실성을 어떻게 나타낼 수 있을까? 이에 대한 답은 분포를 추정하는 것이다. 따라서 모수에 대한 분포를 통해서 모수가 분포에 있을 구간에 따라 나타나는 확률을 제시하여 불확실성을 정량화 한다고 이해를 해보자. 
- 이러한, 설명문의 경우는 직관적으로 이해하기 힘들다. 따라서, 예시를 들어 개념에 대한 이해를 확장해보고자 한다.
$$
\frac{1}{\sqrt{ 2\pi \sigma^2 }} \exp\left\{ -\frac{(x-\mu)^2}{2\sigma^2} \right\}
$$
- 이러한 정규 분포가 있다고 생각을 해보자. 우리는 두 개의 관점에서 위 수식을 이해할 수 있다.
	- $x|\mu, \sigma^2$ : density → 파라미터의 가정을 한 pdf라 생각.
	- $\mu, \sigma^2|x$ : Likelihood  → 데이터가 주어졌을 때 파라미터를 추정.
- Likelihood의 사고를 확장하기 위하여 $x=1$이라 생각해보자. 그렇다면 $\mu_{1}=3, \sigma^2_{1}=1$도 가능할 수 있고, $\mu_{2}=1, \sigma^2_{2}=1$도 가능할 수도 있다. 그렇다면 우리는 어떤 것이 더 개연성에 맞는 시나리오일까? 를 고민해보자. 아마 $\mu_{2}, \sigma^2_{2}$의 조합이 더 가능성이 높아 보인다. 이런 일련의 과정을 MLE라고 하는데 장점 3가지만 나열하고 나머지는 넘기도록 하자.
	1. Asymptotically Normal
	2. Asymptotically Consistency
	3. Asymptotically Efficient
###### MLE(Maximum Likelihood Estimation) 예시
- 동전이 있는데, 앞, 뒷면이 나오는 것이 unfair하다고 생각해보자. 
	- $\text{Event} = {H, T}$
	- $\theta = Pr(H)$ : 앞면이 나올 확률을 $\theta$
	- $Y=1,0,0,0,1,0,0,0,0,0$
- 우리가 구해야 할 것은 $f(Y|\theta)$=$L(\theta|Y)$ 임.
	- 위 수식은 베이지안 관점에서 작성된 수식임. 이유는 빈도주의에서는 보통 $f(x; \theta)$로 표기함. 모수는 fixed되는 값이라서, Conditional로 표기되지 않음.
	- 로그를 사용해서 Likelihood를 maximize하는 것이 일반적임.
$$
\begin{align}
 f(Y|\theta)  & = \theta^2(1-\theta)^8 \\
 & = \prod \theta^{y_{i}}(1-\theta)^{1-y_{i}} \\
 & = L(\theta|y)
\end{align}
$$
### 베이지안
- 베이지안을 먼저 수식으로 바라봐보자.
$$
\begin{align}
P(A|B)  & = \frac{P(A,B)}{P(B)} = \frac{P(B|A)P(A)}{P(B)} \\
 & = \frac{P(B|A)}{P(B)}P(A)
\end{align}
$$
- 위 식을 살펴보면 좌항과 우항의 $P(A)$ 둘 다 모두 A에 대한 확률을 의미하고 있다. 따라서 둘의 차이는 정보가 주어졌음에 따라 달라지는 어떠한 값에 대한 것을 확률적으로 파악한다고 생각을 해보자.
	- 달라지는 정보는 $\frac{P(B|A)}{P(B)}$이며, 만약 사건 A와 B가 독립이라면 이 값은 1이다.
- 우리는 확률적 과정에 대해서 생각할 때 파라미터와 데이터의 관계로 바라본다.
$$
\pi(\theta|y) = \frac{f(y|\theta)}{f(y)} \pi(\theta)
$$
- $\pi(\theta)$ : prior → 사전 정보(Information)로 경험, 이론 등과 같은 파라미터에 대한 믿음의 수치를 뜻한다. 보통 베타 분포를 많이 활용함.
	- $Beta(\alpha, \beta)$: $\pi(\theta) = \frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}\theta^{\alpha-1}(1-\theta)^{\beta-1}$
- $f(y|\theta)$ : likelihood, $= L(\theta|y)$
- $\pi(\theta|y)$ : posterior $\propto f(y|\theta)\pi(\theta)$
- $f(y)$ : Marginal → Constant