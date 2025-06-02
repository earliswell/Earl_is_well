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
- 하지만, 이는 두 분포를 알고 있다는 가정 하에서만 샘플링을 진행할 수 있음. 
- 나름 분포를 직관적으로 근사할 수 있는 기법이라고 이해를 하면 될듯.
## 3.2 Probability Integral Transformation
- $X$라는 확률변수의 누적확률밀도함수(또는 분포함수, $F(x)$)가 강증가(strictly increasing)함수이고, 역함수 $F^{-1}(x)$가 알려져 있다고 가정하자. 이러한 조건이 만족될 때, $X$를 샘플링하기 위해 적용가능한 샘플링 기법이 **Probability Integral Transformation**(PIT) 또는 Inversion Sampling이라고 한다. 
- PIT를 이용한 샘플링 알고리즘은 아래와 같이 두 단계로 이루어짐.
###### 알고리즘 3.3: PIT
1. $i=1, 2, \dots, n$에 대해서 $Unif(0, 1)$에서 $u_{i}$를 샘플링한 뒤 저장.
	- $u_{i} \sim Unif(0,1)$: 모든 확률 값(0~1)이 동일한 가능성으로 선택
	- $u_{i}$: $F(X)$의 누적 확률 값을 의미함.
	- Uniform distribution의 역할: 확률 공간을 "공정하게 분할"
2. 각 $u_{i}(i=1, 2, \dots ,n)$에 대해서 $F^{-1}(u_{i})$를 계산한 뒤 저장함.
	- $z_{i} = F^{-1}(u_{i})$: 확률값 $u_{i}$를 실제 값 $x_{i}$로 변환.
	- 이를 무한히 반복하면, $z_{1}, z_{2} \dots z_{n}$이 목표분포 $F$를 따름.
#### 이론적 배경
- 먼저 $U \sim Unif(0,1)$이고, $Z = F^{-1}(U)$는 확률변수 $U$의 함수이기 때문에 $Z$도 확률변수가 된다. 
- 이에 확률변수 $Z$의 CDF가 $X$의 CDF와 동일하다면 PIT가 올바른 샘플링 기법임.
- $Z$의 CDF는
$$
Pr(Z \leq z) = Pr(F^{-1}(U) \leq F(z))
$$
- $F(\cdot)$은 강증가함수이기 때문에 역함수가 존재함.
$$
Pr(F^{-1}(U) \leq z) = Pr(F(F^{-1}(U)) \leq F(z))
$$
- $F(F^{-1}(U)) = U$이므로,
$$
Pr(F(F^{-1}(U)) \leq F(z)) = Pr(U \leq F(z))
$$
- 마지막으로, $U$는 균일 분포를 따르므로, $Pr(U \leq F(z)) = F(z)$가 된다. 
	- 추가적으로 $F(z)$는 CDF로 해당 값의 범위는 \[0, 1]임.
	- 예를 들어, $F(z) = 0.6$ → $Pr(U \leq 0.6) = 0.6$!
- 따라서, $F(\cdot)$는 $X$의 누적밀도이므로 증명이 완료됨.
- 이 또한, 확률변수가 다변수인 경우에는 적용하기 힘들며, 일변수의 경우에도 분포의 역함수를 계산해야하기 때문에 한계를 지닌다. 
###### Example 2. 절단된 정규 분포 샘플링
- PIT 기법의 대표적인 응용 사례가 바로 절단된 정규 분포 샘플링디ㅏ. 우선 $\Phi(x)$는 표준 정규 분포의 CDF라고 하자. 이 때 서포트(support)가 \[-2, 3]으로 절단된 표준 정규 분포 $\text{TruncatedNormal}_{[-2, 3]}(0,1)$를 샘플링하고자 한다. 
	- 여기에서 서포트(support)는 구간으로 이해하면 된다.
- 이렇게 절단된 표준 정규 분포의 CDF $F(x)$는
$$
F(x) = \frac{\Phi(x) - \Phi(-2)}{\Phi(3) - \Phi(-2)}, \quad -2 \leq x \leq 3
$$
- PIT 기법 적용을 위한 첫 번째 단계는 $F(x)$의 역함수를 계산하는 것이다. $F(x)$의 역함수는 아래 식을 풀어야 한다.
$$
u = F(x) = \frac{\Phi(x) - \Phi(-2)}{\Phi(3) - \Phi(-2)}
$$
- 따라서 아래와 같이 도출된다.
$$
x = \Phi^{-1}(\Phi(-2) + u \times [\Phi(3) - \Phi(-2)])
$$
- 균일분포 $Unif(0,1)$에서 $n$개의 샘플 $\{u_{i}\}_{i=1}^n$을 추출한 뒤에, 각각의 $u_{i}$에 대해서 아래 식을 계산하여 저장한다.
$$
x_{i} = \Phi^{-1}(\Phi(-2) + u_{i}\times[\Phi(3) - \Phi(-2)])
$$
- 이렇게 \[-2, 3]의 범위에서 절단된 표준 정규 분포로부터 생성된 샘플 $\{x_{i}\}_{i=1}^n$을 획득한다!
###### 알고리즘 3.4: 절단된 정규 분포 샘플링
1. $i = 1, 2, \dots, n$에 대해서 $Unif(0,1)$에서 $u_{i}$를 샘플한 뒤 저장.
2. 각 $u_{i}(i=1, 2, \dots, n)$에 대해서 $F^{-1}(u)$를 계산한 뒤 저장.
$$
F^{-1}(u_{i}) = \Phi^{-1}(\Phi(a) + u_{i }\times[\Phi(b) - \Phi(a)])
$$
- PIT 샘플링 또는 Inversion Sampling은 매우 직관적이고 간단한 샘플링 기법이다.
- $u_{i} \sim Unif(0,1)$에서 10,000개 추출 → $F^{-1}(u_{i})$대입으로 샘플링을 끝낸다.
- 하지만, 이렇게 간단한 만큼 범용적 적용에는 제약이 있다.
	- 우리는 그렇다면 어떤 확률 분포의 CDF와 CDF의 역함수를 해석적으로 구할 수 있어야함를 알아야 하기 때문이다.
	- 또한, 다변수로 갈 수록 CDF의 역함수를 구하는 것은 복잡도가 급증함.
- 따라서, 이러한 한계를 극복하기 위해서 Acceptance-Rejection Method와 Importance Sampling에 대해서 알아보려고 한다.
## 3.3 Acceptance-Rejection Method
- PIT는 CDF의 역함수를 알아야 한다는 한계가 존재했다. 
- 하지만 Acceptance-Rejection 샘플링 기법(이후 A-R)기법은 샘플링하고자 하는 확률변수 $X$의 밀도 함수 $f(x)$의 커넬을 알고 있으면 적용가능한 샘플링 기법이다. 
- 이 때, 샘플링 대상인 확률변수 $X$의 분포를 타깃 분포(Target distribution)라고 한다. → 베이지안 추정을 위한 샘플링에서 타깃 분포는 곧 사후 분포를 의미한다.
#### 3.3.1 시뮬레이션 방법
- 우리의 목표는 타깃 분포(=사후 분포)의 커넬을 알고 있으면, 다음의 조건을 만족하는 상수 $c$와 확률밀도함수 $g(x)$가 존재한다고 하자.
$$
f(x) \leq cg(x)
$$
- $g(x)$는 샘플링이 가능한 임이의 분포의 밀도함수 또는 커넬이며, 이를 후보 생성 분포(candidate-generating distribution 또는 proposal distribution)라고 한다. 
- 즉, 제안 분포로부터 샘플링을 할 수 있고, 모든 $x$에 대해서 $cg(x)$가 $f(x)$보다 크거나 같도록 만들어주는 상수 $c$를 찾을 수 있다는 조건이 만족되어야 한다. 
- 위와 같은 조건을 만족하는 상수 $c$와 $g(x)$가 주어져 있거나 혹은 우리가 찾을 수 있다면 다음과 같이 A-R 알고리즘을 통해서 타깃 분포를 샘플링할 수 있다.
###### 알고리즘 3.5: Acceptance-Rejection Method
1. $j=1, 2, \dots, n$에 대해 제안 분포로 부터 $x_{j}$를 샘플링한 뒤 저장한다.
2. $j=1,2,\dots,n$에 대해 $u_{j} \sim Unif(0,1)$를 샘플링한 뒤 저장한다. 