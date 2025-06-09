---
title: 04. Metropolis-Hastings 알고리즘
draft: false
tags:
  - "#샘플링"
  - "#MCMC"
  - "#베이지안"
---
## 들어가며
- 우리의 관심 추론 대상은 파라미터의 사후 분포이다. 
	- 여기서 사전 분포가 켤레라면 사후 분포가 표준적이거나 완전 조건부 분포가 표준적이다. 
	- 만약 사후 분포가 표준적이라면 굳이 시뮬레이션에 의존하지 않아도 됨. 
	- 완전 조건부 분포가 표준적이라면 [[Bayesian 02. 깁스샘플링#2.2 완전 조건부 분포와 깁스 샘플링|깁스 샘플링]]을 적용하여 사후 분포를 샘플링할 수 있음. 
- 하지만 사전 분포가 켤레가 아니라면 우리는 깁스 샘플링을 활용하지 못함. 이런 경우에는 MCMC 시뮬레이션 기법, 특히 Metropolis-Hastings (M-H) 알고리즘을 이용해서 사후 분포를 샘플링할 수 있음!
- 이전 장에서는 몬테 카를로 시뮬레이션에 대해서 공부를 진행했는데, 여기서는 Markov Chain이라는 성질이 결합된 시뮬레이션 기법임. 이러한 점에 대해서 알아가면 좋음.
## 4.1 Metropolis-Hastings 알고리즘 소개
- 우리가 시뮬레이션의 대상으로 삼는 타깃 분포는 파라미터$(\theta)$의 사후 분포이다. 사후 분포의 밀도함수는 $\pi(\theta|Y)$이지만 실제로 우리에게 주어지는 것은 커넬(우도함수 $\times$ 사전분포)의 형식이다. 즉,
$$
\begin{align}
\pi(\theta|Y)  & \propto f(Y|\theta)\pi(\theta) \\
 & =L(\theta|Y)\pi(\theta) \\
 & =p(\theta|Y)
\end{align}
$$
- 편의상 사후 분포의 커넬을 $p(\theta|Y)$로 표기한다. 그리고 $\theta^{(j)}$는 $j$ 번째 반복시행에서 추출된 사후 샘플을 나타낸다. 
- 한편, 앞서 배웠던 몬테 카를로 시뮬레이션은 $j-1$ 번째 추출됬던 샘플이 $j$에 영향을 미치지 않았다. 즉, 두 값은 독립적인 후보 생성 분포로 부터 샘플링되었다. 반면, MCMC 기반의 시뮬레이션 기법은 Markov Chain 성질에 의하여 현재 $j$기의 값은 오직 바로 전 과거 $j-1$기에 영향을 받는다.
- [[Bayesian 03. 몬테 카를로 시뮬레이션#3.3 Acceptance-Rejection Method|A-R 기법]]과 비슷하게 M-H 알고리즘은 후보 생성 분포로부터 $\theta^{(j)}$에 대한 프로포절을 생성하는 것이며, 생성된 프로포절을 $\theta^*$로 표기한다. 이 과정에서 다른 점은 후보 생성 분포가 $\theta^{(j-1)}$에 의존하는 것이다.
- 따라서, $\theta^{(j)}$의 후보 생성 분포는 $\theta^{(j-1)}$이 주어졌을 때의 조건부 분포이다. 
- 이러한 $\theta^*$의 후보 생성밀도(Proposal Density)는 아래와 같이 표기된다.
$$
q(\theta^*|\theta^{(j-1)}, Y)
$$
- M-H 비(M-H rate)를 계산해야하는데, 이는 다음과 같이 표시된다.
$$
\alpha(\theta^{(j-1)}, \theta^*) = \min\left\{ \frac{p(\theta^*|Y) q(\theta^{(j-1)}|\theta^*, Y)}{p(\theta^{(j-1)}) q(\theta^*|\theta^{(j-1)}, Y)}, 1 \right\}
$$
- 우리는 M-H rate을 결정하는 아래의 식을 어떻게 해석할 지 고민을 해보자.
$$
\frac{p(\theta^*|Y)q(\theta^{(j-1)}|\theta^*, Y)}{p(\theta^{(j-1)}|Y)q(\theta^*|\theta^{(j-1)}, Y)}
$$
- 우선, M-H rate이 크다는 것은 $p(\theta^*|Y)/p(\theta^{(j-1)}|Y)$가 크거니 혹은 $q(\theta^{(j-1)}|\theta^*, Y)/q(\theta^*|\theta^{(j-1)}, Y)$가 크다는 것을 의미한다. 
	- $p(\theta^*|Y)/p(\theta^{(j-1)}|Y)$가 크다는 것은 사후 분포 $\theta|Y$로부터 $\theta^*$가 $\theta^{(j-1)}$보다 생성될 확률이 높다는 것을 의미한다. → 이는 결국 $\theta^{(j)}$에 $\theta^*$가 저장될 확률이 더 크다.
	- $q(\theta^{(j-1)}|\theta^*, Y)/q(\theta^*|\theta^{(j-1)}, Y)$는 

## 알고리즘
#### 알고리즘 4.1: Metropolis-Hastings 알고리즘
0. 초기값 $\theta^{(0)}$를 사전 평균으로 설정하고, $j=1$로 둔다.
1. Proposal $\theta^*$을 후보 생성 분포 $\theta|\theta^{(j-1), Y}$로부터 샘플링한다.
2. M-H 비(M-H rate)를 계산한다.
$$
\alpha(\theta^{(j-1)}, \theta^*) = \min\left\{ \frac{p(\theta^*|Y)q(\theta^{(j-1)}|\theta^*, Y)}{p(\theta^{(j-1)|Y})q(\theta^*|\theta^{(j-1)}, Y)}, 1 \right\}
$$
3. $Unif(0,1)$에서 $u$를 샘플링한다. 
4. 따라서 $\theta^{(j)}$는 
$$
\theta^{(j)} = \begin{cases}
\theta^* \quad  & (u < \alpha(\theta^*, \theta^{(j-1)})) \\
\theta^{(j-1)} \quad  & (u \geq \alpha(\theta^*, \theta^{(j-1)}))
\end{cases}
$$
5. $j = j+1$로 설정하고, $j \leq n$이면 1 단계로 돌아간다. 