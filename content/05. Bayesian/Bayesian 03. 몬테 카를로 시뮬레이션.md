---
title: 03. 몬테 카를로 시뮬레이션
draft: false
tags:
  - "#몬테카를로"
  - "#시뮬레이션"
  - "#샘플링"
  - "#베이지안"
---
## 들어가며
- 베이지안 분석에서는 사후 분포 $\pi(\theta|Y)$에 관심을 갖는다. 따라서, 계량모형의 파라미터를 추론한다는 것은 곧 파라미터의 사후 분포를 추정한다는 것이다.
- 우리는 앞선 [[Bayesian 02. 깁스샘플링]]은 쉽게 우리가 샘플링을 할 수 있었던 이유가, 사전 분포($\pi(\theta)$)와 사후 분포 $\pi(\theta|Y)$의 켤레 성을 통해서 둘의 분포적 성질을 이용할 수 있었다. 
- 하지만 켤레 사전 분포가 아닌 사전 분포를 설정하거나 켤레 사전 분포를 찾을 수 없을 경우에는 깁스 샘플링을 적용할 수 없다 !
- 이러한 시점에서 '사후 분포를 시뮬레이션(또는 샘플링)한다'라는 것의 의미를 되새겨 보자. 
	- 베이지안 추정과정에서 우리에게 주어진 것은 사후 분포의 밀도 함수 $\pi(\theta|Y)$와 그것의 커넬 $f(Y|\theta)\times \pi(\theta)$이다!
- 우리는 이러한 밀도 함수를 알고 있다고 해서 그 분포의 평균과 분산과 같은 통계적 대표값들을 알아낼 수 있을까? 
- 예를 들어, 확률변수 $X$의 밀도 함수가 $f(x)$라고 했을 때, $X$의 평균과 분산은 각각 아래와 같이 정의된다.
$$
E(X) \int xf(x)dx, \quad Var(X) = \int(x - E(X))^2f(x)dx
$$
- 하지만, 우리는 이러한 식들이 적분이 가능할 지라도 계산가능하지 않는 경우가 일반적이다. 또한, $X$가 일변수가 아니라 다변수인 경우에는 더더욱 적분을 하기 힘들어진다. 이러한 경우들을 대비하기 위하여 우리는 '사후 분포를 시뮬이션 한다'는 의미를 다시 되짚어볼 필요가 있다.
	- 이는 사후 분포의 밀도함수는 커넬을 이용해서 사후 분포의 샘플을 추출해내는 작업을 말한다. 그리고 사후 분포로부터 샘플을 추출할 수 있도록 해주는 기법을 시뮬레이션 기법 또는 **사후 샘플링**(posterior sampling) 기법이라고 한다.
	- 이를 통해 파라미터의 임의의 함수에 대한 대푯값을 제시하고 통계적인 추론을 쉽게 할 수 있도록 만든다.
- 사전 분포가 켤레인 경우에는 사후 분포 또는 완전 조건부 분포가 표준적 → 깁스 샘플링
- 하지만, 사전 분포가 켤레가 아닌 경우에는 깁스 샘플링을 적용할 수 없음 → Markov chain Monte Carlo(MCMC)
	- 이는 사전 분포가 켤레이든 아니든 상관없이 사후 분포를 샘플링할 수 있다.
- MCMC 기법을 이해하기 위해서는 이 방법의 토대가 되는 몬테 카를로(Monte Carlo) 시뮬레이션 기법에 대한 이해가 선행되어야 한다. 이 장에서는 여러 몬테 카를로 시뮬레이션에 대해 고찰하고, 다음 장에서 MCMC 기법에 대해서 자세히 다루고자 한다.
## 3.1 Method of Composition
- $f_{X, Z}(x, z)$는 확률변수 $X$와 $Z$의 결합 밀도함수(Joint density probability)이며
- $f_{X}(x)$와 $f_{Z}(z)$는 각각 $X$와 $Z$의 주변확률밀도함수(Marginal density probability)라고 하자.
- 그리고 $f_{X|Z}(x|z)$는 $X$의 조건부 확률밀도함수(Conditional density probability)이다.
- 이 때, $Z$가 연속확률변수(Continuous random variable)라면
$$
\begin{equation}
\begin{split}
f_{X}(x)  & = \int f_{X, Z}(x,z)dz \\
 & = \int f_{X|Z}(x|z)f_{Z}(z)dz
\end{split}
\end{equation}
$$
- 이 성립한다. 그리고 $Z$가 이산확률변수 (discrete random variable)일 경우에는 아래와 같이 표현된다.
$$
f_{X}(x) = \sum_{z}f_{X|Z}(x|z)f_{Z}(z)
$$
- 위의 식들은 두 가지 의미가 있다. 첫째, 확률변수 $X$의 확률밀도함수 $f_{X}(x)$를 모르더라도, 만약 어떤 확률변수 $Z$를 샘플링할 수 있고(어떤 분포로 부터 draw를 할 수 있고) 주어진 $Z$에 대해서 $X$의 조건부 확률밀도함수 $f_{X|Z}(x|z)$를 알고 있다면 $X$의 확률밀도를 아래와 같이 대수의 법칙을 이용해서 수치적으로 근사할 수 있음.
$$
f_{X}(x) \approx \frac{1}{n}\sum_{i=1}^{n} f_{X|Z}(x|z_{i})
$$
- 두 번째 의미는 $Z$와 $X|Z$를 샘플링할 수 있다면, 아래 제시하는 알고리즘을 이용하여 $X$의 분포를 모르더라도 $X$를 샘플링할 수 있다는 것이다. → 이는 즉, $X$를 직접적으로 샘플링할 수 없더라도, 어떤 샘플링 가능한 확률변수($Z$)가 주어졌을 때의 조건부 분포 $(X|Z)$를 샘플링할 수 있다면, $X$의 분포로부터 샘플링이 가능하다는 의미이다.
- 이렇게 Marginal density와 Conditional density 간의 관계를 이용해서 $X$의 분포를 샘플링하거나 $X$의 밀도 함수를 계산하는 기법을 **Method of Composition**(이후 MoC)이라고 한다.
###### 알고리즘 3.1: Method of Composition
1. $i = 1, 2, \dots, n$에 대해서 $Z$의 분포로부터 $z_{i}$를 샘플링한 뒤 저장 → Draw
2. 각 $z_{i}(i=1, 2, \dots, n)$에 대해서 조건부 분포 $X|z_{i}$로 부터 $x_{i}$를 샘플링한 뒤 저장 → 조건부 분포로부터 Draw!
###### Example: Method of Composition을 이용한 스튜던트-$t$ 분포 샘플링
- 자유도: $v$, 평균: $0$, 스케일 파라미터: $\sigma^2$인 스튜던트-$t$ 분포 $St(0, \sigma^2, v)$을 따르는 확률변수 $W$를 샘플링한다고 했을 때, 우리는 이를 직접적으로 샘플링하는 법을 모른다고 **가정**해보자.
$$
\begin{align}
\Lambda  & \sim G\left( \frac{v}{2}, \frac{v}{2} \right) \text{ and } f_{\Lambda}(\lambda)\text{ 의 확률밀도함수} \\
W  & \sim St(0, \sigma^2, v) \text{ and } f_{W}(w)\text{의 확률밀도 함수} \\ 
W|\lambda  & \sim N(0, \lambda^{-1}\sigma^2) \text{ and } f_{W|\Lambda}(w|\lambda) = \Lambda \text{가  } \lambda\text{일 때, } W\text{의 조건부 확률밀도함수}
\end{align}
$$
- 여기서 $W|\Lambda = \lambda \sim N(0, \lambda^{-1}\sigma^2)$이고, $\Lambda \sim G(v/2, v/2)$일 때, $W$의 주변 분포(Marginal Distribution)가 
$$
St(0, \sigma^2, v)
$$
- 임을 보이고자 한다 ! 따라서,
$$
\begin{equation}
\begin{split}
& f_{W}(w)  = \int f_{W|\Lambda}(w | \lambda)f_{\Lambda}(\lambda)d\lambda \\
& \quad \propto \int \lambda^{1/2} \exp\left( - \frac{\lambda}{2\sigma^2} w^2\right) \times \lambda^{v/2 - 1} \exp\left( - \frac{v}{2}\lambda \right)d\lambda \\
& \quad = \int \lambda^{(v+1)/2 - 1} \exp\left( - \frac{(w^2 + v\sigma^2)\lambda}{2\sigma^2} \right)d\lambda
\end{split}
\end{equation}
$$
- 여기서 $a = (v+1)/2, b=(w^2 + v\sigma^2)/2\sigma^2$라고 두면, 위 적분기호 안의 함수는 감마 분포 Gamma($a, b$)의 밀도함수, Gamma($\lambda| a,b$)으로 표현된다.
$$
\begin{equation}
\begin{split}
\int f_{W|\Lambda}(w|\lambda)f_{\Lambda}(\lambda)d\lambda & \propto \int Gamma(\lambda| a, b)d\lambda \\
& = \frac{\Gamma(a)}{b^a} \int\frac{b^a}{\Gamma(a)}\lambda^{a-1}e^{-b\lambda}d \lambda
\end{split}
\end{equation}
$$
- 확률밀도함수 정의상,
$$
\int\frac{b^a}{\Gamma(a)}\lambda^{a-1}e^{-b\lambda}d\lambda = 1
$$
- 이므로, 위 수식은 아래와 같이 다시 작성할 수 있음.
$$
\begin{align}
\begin{split}
\int f_{W|\Lambda}(w|\lambda)f_{\Lambda}(\lambda)d\lambda  & \propto \frac{\Gamma(a)}{b^a} = \Gamma\left( \frac{v+1}{2} \right)/ \left( \frac{w^2+v\sigma^2}{2\sigma^2} \right)^{\frac{v+1}{2}} \\
 & \propto (w^2+v\sigma^2)^{-\frac{v+1}{2}} \\
 & \propto \left( 1 + \frac{w^2}{v\sigma^2} \right)^{- \frac{v+1}{2}}
\end{split}
\end{align}
$$
- 이는 결국 좌변이 스튜던트-$t$ 분포의 커넬과 비례하므로 $f_{W}(w)$은 스튜던트-$t$ 분포의 밀도함수가 된다.
###### 알고리즘 3.2: 스튜던트-$t$ 분포 샘플링
1. $i=1, 2, \dots, n$에 대해서 $Gamma(v/2, v/2)$로부터 $\lambda_{i}$를 샘플링한 뒤 저장 → $\lambda_{i}$를 Draw!
2. 각 $\lambda_{i}(i= 1, 2, \dots, n)$에 대해서 $N(0, \lambda^{-1}_{i}\sigma^2)$로부터 $w_{i}$를 샘플링한 뒤 저장 ! → $w_{i}$를 Draw!
- 이를 무한히 반복하면 스튜던트-$t$ 분포를 따르는 샘플들이 쫙 나열됨. 
- 이렇게 Draw한 샘플들을 통해서 히스토그램을 그리거나, 평균, 분산 등을 계산할 수 있음 !!!!
###### 나의 이해
- 어떤 $X$에 대한 분포를 알고 싶은데, $Z$에 대한 Marginal distribution과 $X|Z$에 대한 Conditional distribution을 알고 있을 때, MoC 기법을 통하여 우리는 $X$에 대한 분포를 근사할 수 있다는 것임.
- 이 기법은 각 샘플이 독립적으로 생성되므로 병렬 처리가 가능함. → 
- 
## 3.2 Probability Integral Transformation


