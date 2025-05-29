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

## 2.1 다중선형회귀모형
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
- 우리는 결과적으로 선형회귀모형에서 $\sigma^2$이 알려져 있을 때 $\beta$에 대한 사후 분포가 다변량 정규 분포를 따른다는 것을 알았다 !!
	- $B_{1} = (\mathbf{X'X}+ B_{0}^{-1})^{-1}$
	- $\beta_{1} = B_{1}(\mathbf{X'}Y + B_{0}^{-1}\beta_{0})$
###### 사후 분포의 특성
- 위에서 주어진 $\beta$의 사후 분포 도출 결과를 직관적으로 해석해보자.
- $k=1$인 상황으로 설정하여, $\beta$의 사후 평균, $B_{1}(\mathbf{X'}Y + B_{0}^{-1}\beta_{0})$은 다음과 같이 표현됨.
$$
E(\beta|Y) = B_{1}(\mathbf{X'}Y+B_{0}^{-1}\beta_{0}) = \frac{\mathbf{X'}Y + B_{0}^{-1}\beta_{0}}{\mathbf{X'X} + B_{0}^{-1}}
$$
- 이 수식에서 우리는 사전정보가 전무하거나 있다 하더라도 믿음의 강도가 거의 없는 경우를 상정해보자. 이 경우에는 $\beta_{0}$은 임의의 값인 상수(scalar) 이겠지만, $B_{0}$는 무한대에 가까운 값을 가진다.
	- 왜냐하면 $\beta_{0}$이 설정될 수 있는 값이 너무 많아서 ? → 즉, $\beta_{0}$에 대한 사전 정보가 매우 불확실하거나 아예 없기 때문이다.
- 그렇게 되면 $B_{0}^{-1}$의 값은 거의 0이 된다. 다시 말해서, $B_{0}$이 커질수록 사후 평균은 최소자승추정량 $\hat{\beta}_{OLS} = (\mathbf{X'X)X'}Y$으로 수렴하게 됨. 
- 반대로 연구자의 사전적인 믿음이 대단히 강해서 $B_{0}$의 값이 거의 0에 가까운 경우를 생각해보자. 
- 이 경우에는 반대로 사후 평균이 $B_{1}(\mathbf{X'}Y + B_{0}^{-1}\beta_{0})$가 사전 평균, $\beta_{0}$에 가까워 진다. 
	- 이는 결국, $B_{0}^{-1}$은 무한대에 가까워 지는 수가 될 것이고, $\mathbf{X}$와 $Y$의 영향이 작아지게 됨. $\frac{\infty}{\infty}$ 
- 사후 평균은 결국 사전 평균 혹은 정보에만 의존한 $\beta$의 추정치의 가중 평균으로 결정됨.
$$
\begin{align}
B_{1}A  & = \frac{\mathbf{(X'X)^{-1}X'}Y + \mathbf{(X'X)}B_{0}^{-1}\beta_{0}}{1 + \mathbf{(X'X)}^{-1}B_{0}^{-1}} \\
 & =\frac{\hat{\beta}_{OLS} + \frac{Var(\hat{\beta}_{OLS})}{Var(\beta)}\beta_{0}}{1 + \frac{Var(\hat{\beta}_{OLS})}{Var(\beta)}} \\
 & = \frac{1}{1 + \frac{Var(\hat{\beta}_{OLS})}{Var(\beta)}}\hat{\beta}_{OLS} + \left( 1 - \frac{1}{1+ \frac{Var(\hat{\beta})}{Var(\beta)}} \right)\beta_{0} \\
 & = \frac{Var(\beta)}{Var(\beta) + Var(\hat{\beta}_{OLS})} + \frac{Var(\hat{\beta})}{Var(\beta) + Var(\hat{\beta}_{OLS})} \beta_{0}
\end{align}
$$
- 이는 표본의 크기가 커질수록 $Var(\hat{\beta})$의 값이 작아지게 된다. → 이는 표본의 증가가 $\hat{\beta}$의 값이 하나의 값으로 수렴할 Consistency와 이어지는 개념이라고 생각하면 됨. 즉, 표본의 무한히 뽑히면 OLS의 [[Statistics, 07. Properties of Point Estimators and Methods of Estimation#Consistency|Consistency]]의 성질을 다시 한 번 생각해보자.
- 반대로, $Var(\hat{\beta}_{OLS})$값이 커지게 되면 $\hat{\beta}_{OLS}$의 가중치는 작아지고 $\beta_{0}$의 가중치는 커짐. 이러한 경우에서는 사후 평균은 $\hat{\beta}_{OLS}$보다는 사전 평균에 더 가까운 값을 갖게 됨.
#### Case 2. $\beta$가 알려져 있는 경우
- 이 경우 $\beta$는 주어진 상수(constant)이기 때문에 선형회귀 모형을 아래와 같이 다시 표현할 수 있음.
$$
\begin{align}
\sigma^2  & \sim IG\left( \frac{\alpha_{0}}{2}, \frac{\delta_{0}}{2} \right), \\
Y|\sigma^2  & \sim N(\mathbf{X}\beta, \sigma^2\mathbf{I}_{T})
\end{align}
$$
- 우리의 목표는 $\sigma^2$의 사후 분포를 도출하는 것, 즉 $\pi(\sigma^2|Y)$를 알아내는 것이 목표이다.
$$
\begin{align}
\pi(\sigma^2|Y)  & = \frac{f(Y|\sigma^2)}{f(Y)}\pi(\sigma^2) \\
 & \propto f(Y|\sigma^2)\pi(\sigma^2)
\end{align}
$$
- $f(Y|\sigma^2)$는 수식(8)과 동일함. 
- $\sigma^2$의 사전 분포는 $(\alpha_{0}/2, \delta_{0}/2)$를 하이퍼 파라미터로 가지는 역감마 분포를 따르며, 커넬은 다음과 같음.
$$
\begin{align}
\pi(\sigma^2)  & =IG\left( \sigma^2|\frac{\alpha_{0}}{2}, \frac{\delta_{0}}{2} \right) \\
 & \propto \left( \frac{1}{\sigma^2} \right)^{\frac{\alpha_{0}}{2}+1} \exp\left(- \frac{\delta_{0}}{2\sigma^2} \right)
\end{align}
$$
- $\sigma^2$의 사후 분포는 다음과 같이 계산될 수 있음.
$$
\begin{align}
\pi(\sigma^2|Y)  & \sim f(Y|\sigma^2)\pi(\sigma^2) \\
 & =N(Y|\mathbf{X}\beta, \sigma^2\mathbf{I}_{T}) \times IG\left( \sigma^2|\frac{\alpha_{0}}{2}, \frac{\delta_{0}}{2} \right) \\
 & \propto \left( \frac{1}{\sigma^2} \right)^{T/2}\exp\left( -\frac{1}{2\sigma^2}(Y- \mathbf{X}\beta)'(Y - \mathbf{X}\beta) \right)  \times \left( \frac{1}{\sigma^2} \right)^{\alpha_{0}/2 + 1} \exp\left( -\frac{\delta_{0}}{2\sigma^2} \right)
\end{align}
$$
- 위 식을 $1/\sigma^2$와 $\exp(\cdot)$항으로 정리하면,
$$
\begin{align}
\pi(\sigma^2|Y)  & \propto \left( \frac{1}{\sigma^2} \right)^{(T+\alpha_{0})/2+1}\exp\left( - \frac{1}{2\sigma^2}[(Y-\mathbf{X}\beta)'(Y -\mathbf{X}\beta) + \delta_{0}] \right) \\
 & = \left( \frac{1}{\sigma^2} \right)^{\alpha_{1}/2+1}\exp\left( - \frac{1}{2\sigma^2}\delta_{1} \right) \\
\text{with } \alpha_{1}  & = \alpha_{0} + T \text{ and } \delta_{1} = \delta_{0} + (Y - \mathbf{X}\beta)'(Y - \mathbf{X}\beta)
\end{align}
$$
- 이는 결국 역감마 분포의 커넬임을 쉽게 알 수 있다. 즉,
$$
\sigma^2|Y \sim IG\left( \frac{\alpha_{1}}{2} , \frac{\delta_{1}}{2} \right)
$$
###### 켤레 사전 분포
- [[#Case 1. $ sigma 2$이 알려져 있는 경우|Case 1]]과 [[#Case 2. $ beta$가 알려져 있는 경우|Case 2]]로부터 우리는 $\beta$의 사전 분포가 정규 분포로 설정되면 사후 분포도 정규분포로 도출되고, $\sigma^2$의 사전 분포를 역감마 분포로 설정되면, 사후 분포도 역감마 분포로 도출된다는 사실을 확인했다. 
- 이처럼 파라미터의 사전 분포를 잘 알려진 분포로 설정하고, 사후 분포가 표준적인 분포로 도출되면, 그러한 사전 분포를 켤레 사전 분포(Conjugate Prior)라고 한다. 
	- 추가적인, 분포 관련 내용은 [위키피디아, Conjugate Prior](https://en.wikipedia.org/wiki/Conjugate_prior)를 참고해보자.

#### Case 3. $\beta$와 $\sigma^2$이 모두 알려져 있지 않은 경우.
- 앞서 우리가 설정했던 선형회귀 모형을 다시 한 번 써보면 다음과 같다.
$$
\begin{align}
\sigma^2 & \sim IG\left( \frac{\alpha_{0}}{2}, \frac{\delta_{0}}{2} \right), \\
\beta|\sigma^2  & \sim N(\beta_{0}, \sigma^2B_{0}), \\
\text{and } Y|\beta, \sigma^2  & \sim N(\mathbf{X}\beta, \sigma^1\mathbf{I}_{T})
\end{align}
$$
- 항상 말했든 우리의 목적은 ($\beta, \sigma^2$)의 결합 사후 분포(joint posterior distribution),
$$
\beta, \sigma^2 |Y
$$
- 또는 주변 사후 분포(Marginal posterior distribution),
$$
\beta|Y \text{ or } \sigma^2|Y
$$
- 를 도출하는 것이다.
- 따라서, $\beta, \sigma^2$의 결합 사후 밀도 $\pi(\beta, \sigma^2|Y)$는 우도 함수와 사전 밀도의 곱에 비례한다.
$$
\begin{align}
\pi(\beta, \sigma^2|Y)  & \propto f(Y|\beta, \sigma^2)\pi(\beta|\sigma^2)\pi(\sigma^2) \\
 & = N(Y|\mathbf{X}\beta, \sigma^2) \times N(\beta|\beta_{0}, \sigma^2B_{0}) \times IG\left( \frac{\alpha_{0}}{2}, \frac{\delta_{0}}{2} \right) \\
 & \propto \left( \frac{1}{\sigma^2} \right)^{(T+\alpha_{0})/2 + 1} \times \left( \frac{1}{\sigma^2} \right)^{k/2}  \\
 & \times \exp\left(  - \frac{1}{2\sigma^2}[(Y- \mathbf{X}\beta)'(Y-\mathbf{X}\beta) + (\beta - \beta_{0})'B_{0}^{-1}(\beta - \beta_{0}) + \delta_{0}] \right)
\end{align}
$$
- 이는 더 이상 표준분포로 정리되지 않음. 다시 말해서 커넬을 알 수 없다 !! → 이는 결국 결합 사후 분포의 종류는 물롱 평균이나 분산도 알 수 없다.

###### $\sigma^2$의 주변 사후 분포
- 우선 위 식을 $\beta$와 관련된 항과 그렇지 않은 항으로 정리하여 아래와 같이 표현한다.
$$
\begin{align}
\pi(\beta, \sigma^2|Y)  & \propto \left( \frac{1}{\sigma^2} \right)^{k/2} \times \exp\left( - \frac{1}{2\sigma^2}(\beta-\beta_{1})'B_{1}^{-1}(\beta-\beta_{1}) \right)  \times \left( \frac{1}{\sigma^2} \right)^{\alpha_{1}/2+1} \times \exp\left( -\frac{\delta_{1}}{2\sigma^2} \right)
\end{align}
$$
- 단,
$$
\begin{align}
 & B_{1} = (\mathbf{X'X} +B_{0}^{-1}), \, \beta_{1} = B_{1}(\mathbf{X'}Y + B_{0}^{-1}\beta_{0}), \\
 & \alpha_{1} = T + \alpha_{0}, \, \delta_{1} = \delta_{0} + Y'Y + \beta_{0}'B_{0}^{-1}\beta_{0}
 - \beta_{1}'B_{1}^{-1}\beta_{1} 
\end{align}
$$
- 다음으로 $\beta$와 $\sigma^2$의 결합 사후 밀도를 $\beta$에 대해서 적분하면 $\sigma^2$의 Marginal posterior distribution을 계산할 수 있음.
$$
\pi(\sigma^2|Y) = \int \pi(\beta,\sigma^2|Y)d\beta.
$$
- 위 식(39)에서 $\beta$와 무관한 식을 적분 밖으로 옮기면,
$$
\begin{equation}
\begin{split}
\pi(\sigma^2|Y) &\propto \left( \frac{1}{\sigma^2} \right)^{\alpha_{1}/2+1} \times \exp\left( -\frac{\delta_{1}}{2\sigma^2} \right) \\
&\quad \times \int\left( \frac{1}{\sigma^2} \right)^{k/2} \times  \exp\left( - \frac{1}{2\sigma^2}(\beta-\beta_{1})'B_{1}^{-1}(\beta-\beta_{1}) \right)d\beta
\end{split}
\end{equation}
$$
- 위 식이 얻어짐!
- 여기에서, 밀도함수의 적분 값은 1이라는 성질에 의해서 위 식의 적분 항은
$$
\begin{equation}
\begin{split}
& \int \left( \frac{1}{\sigma^2} \right)^{k/2} \times \exp\left( - \frac{1}{2\sigma^2}(\beta - \beta_{1})'B_{1}^{-1}(\beta - \beta_{1}) \right)d \beta \\
& = (\sqrt{ 2\pi })^k|B_{1}|^{1/2}
\end{split}
\end{equation}
$$
- 위와 같이 추출되지만, 결국 $B_{1}$과 $\sigma^2$는 무관하므로 $\pi(\sigma^2|Y)$는
$$
\pi(\sigma^2|Y) \propto\left( \frac{1}{\sigma^2} \right)^{\alpha_{1}/2+1} \times \exp\left(  - \frac{\delta_{1}}{2\sigma^2} \right)
$$
- 결국 $\sigma^2$의 주변 사후 밀도가 역감마 분포의 밀도함수로 도출되므로 $\sigma^2$의 주변 사후 분포(Marginal Posterior Distribution)는 아래와 같음.
$$
\sigma^2|Y \sim IG\left( \frac{\alpha_{1}}{2}, \frac{\delta_{1}}{2} \right)
$$
###### $\beta$의 주변 사후 분포
- 이제 $\beta$의 주변 사후 분포(Marginal Posterior Distribution)을 도출하기 위해서 $\pi(\beta, \sigma^2|Y)$를 $\sigma^2$에 대해서 적분을 해야함.
$$
\begin{equation}
\begin{split}
\pi(\beta, \sigma^2|Y) \propto \left( \frac{1}{\sigma^2} \right)^{(\alpha_{1}+k)/2+1} \times \exp\left( - \frac{1}{2\sigma^2}[(\beta - \beta_{1})'B_{1}^{-1}(\beta - \beta_{1}) + \delta_{1}] \right)
\end{split}
\end{equation}
$$
- 이후 $\pi(\beta, \sigma^2|Y)$를 $\sigma^2$에 대해서 적분하면
$$
\begin{align}
 \pi(\beta|Y)  & = \int \pi(\beta, \sigma^2|Y)d\sigma^2 \\
 & \propto [\delta_{1} + (\beta - \beta_{1})'B_{1}^{-1}(\beta - \beta_{1})]^{-(\alpha_{1} + k)/2}
\end{align}
$$
- 위 식이 유도되는데, 이는 다음에 차차 공부하면서 풀어나가보려고 한다. 
- 따라서, $\alpha_{1}, \delta_{1}$은 $\beta$와 관련이 없기 때문에 주변 사후 분포는 다음과 같이 정리된다.
$$
\begin{align}
\pi(\beta|Y)  & \propto [1 + (\beta - \beta_{1})'(\delta_{1}B_{1})^{-1}(\beta - \beta_{1})]^{-(\alpha_{1} +k)/2} \\
 & \propto \left[ 1 + \frac{1}{\alpha_{1}}(\beta - \beta_{1})'\left( \frac{\delta_{1}}{\alpha_{1}}B_{1} \right)^{-1}(\beta - \beta_{1}) \right]^{- (\alpha_{1}+k)/2}
\end{align}
$$
- 이는 평균이 $\mu$이고, 자유도는 $v$이며 스케일 파라미터가 $\Sigma$인 $k$ 차원 다변수 스튜던트-t 분포의 결합 밀도 $f(X=x)$의 커넬은 다음과 같다.
$$
\left( 1 + \frac{1}{v}(x - \mu)' \Sigma^{-1}(x - \mu) \right)^{- (v +k)/2}
$$
- 이로부터 $\beta$는 평균이 $\beta_{1}$, 자유도는 $\alpha_{1}$, 스케일 파라미터가 $(\delta_{1}/\alpha_{1})B_{1}$인 스튜던트-t 분포를 따른다.
$$
\beta|Y \sim St\left( \beta_{1}, \frac{\delta_{1}}{\alpha_{1}}B_{1}, \alpha_{1} \right)
$$
- 지금까지는 사후 분포를 수학적으로 도출하였다. 하지만 위와 같이 사후 분포가 수학적으로 도출가능한 경우는 예외적이다. → 웬만하면 거의 불가능함.
## 2.2 완전 조건부 분포와 깁스 샘플링
- 앞서 $\beta$의 사전 분포, $\beta|\sigma^2 \sim N(\beta_{0}, \sigma^2B_{0})$는 $\sigma^2$에 의존하였다. 
- 여기서 사전 분산이 작을수록 사전 분포가 자료의 정보에 비해 상대적으로 $\beta$의 사후 분포에 강하게 반영된다. 
- 실무적으로 사전 분산을 통해 연구자가 사전 정보의 양을 수치화하고자 할 때, 사전 분산을 $\sigma^2$과 독립적으로 설정하는 것이 보다 정밀하거나 설득력있는 경우가 많다.
- **$\sigma^2$에 의존하지 않는 $\beta$의 사전 분포**
$$
\beta \sim N(\beta_{0}, B_{0})
$$
- $\sigma^2$의 사전 분포는 역감마 분포를 가정하면 선형 회귀모형은 아래와 같이 표현할 수 있다.
$$
\begin{equation}
\begin{split}
\sigma^2 & \sim IG\left( \frac{\alpha_{0}}{2}, \frac{\delta_{0}}{2} \right),\\
\beta & \sim N(\beta_{0}, B_{0}),\\
Y|\beta, \sigma^2 & \sim N(\mathbf{X}\beta, \sigma^2\mathbf{I}_{T})
\end{split}
\end{equation}
$$
#### 2.2.1 Case A. $\sigma^2$이 알려져 있는 경우
- 이 경우, 사후 분포는 $\beta|Y \sim N(B_{1}(\sigma^{-2}\mathbf{X'}Y+B_{0}^{-1}\beta_{0}), B_{1}), \text{where }B_1=(\sigma^{-2}\mathbf{X'X}+B_{0}^{-1})$이다.
$$
\begin{equation}
\begin{split}
B_{1} & = (\sigma^{-2}\mathbf{X'X}+B_{0}^{-1})^{-1}, \\
A & = \sigma^{-2}\mathbf{X'}Y + B_{0}^{-1}\beta_{0}, \\
\beta|Y & \sim N(B_{1}A_{1}, B_{1})
\end{split}
\end{equation}
$$

#### 2.2.2 Case B. $\beta$가 알려져 있는 경우
- $\sigma^2$의 사후 분포는 $\beta$의 사전 분포가 $\sigma^2$에 의존한 경우와 동일함.
$$
\begin{equation}
\begin{split}
\sigma^2|Y & \sim IG\left( \frac{\alpha_{1}}{2}, \frac{\delta_{1}}{2} \right), \\
\text{with } \alpha_{1} & =\alpha_{0} \text{ and } \delta_{1} = \delta_{0} + (Y - \mathbf{X}\beta)'(Y - \mathbf{X}\beta)
\end{split}
\end{equation}
$$
#### 2.2.3 Case C. $\beta$와 $\sigma^2$이 모두 알려져 있지 않은 경우
- $\beta$와 $\sigma^2$의 결합 사후 밀도 $\pi(\beta, \sigma^2|Y)$는 우도함수, $\beta$의 사전 밀도, $\sigma^2$의 사전 밀도의 곱에 비례한다:
$$
\begin{align}
\pi(\beta, \sigma^2|Y)  & \propto f(Y|\beta, \sigma^2)\pi(\beta)\pi(\sigma^2) \\
 & =N(Y|\mathbf{X}\beta, \sigma^2)\times N(\beta|\beta_{0}, B_{0}) \times IG\left( \sigma^2|\frac{\alpha_{0}}{2}, \frac{\delta_{0}}{2} \right)
\end{align}
$$
- 이 때는 $(\beta, \sigma^2)$의 결합 사후 분포뿐만 아니라 주변 사후 분포 또한 표준적인 분포로 도출되지 않음.
#### 2.2.4 완전 조건부 분포(Full Conditional Distribution)
- 우리는 파라미터의 개수가 많아질수록 [[#2.2.3 Case C. $ beta$와 $ sigma 2$이 모두 알려져 있지 않은 경우|Case C]]와 같이 우리는 사후 분포를 예쁘게 정의된 분포로 추정하기 어려우며, 우리는 이럴 때 시뮬레이션 방법(Simulation Method)에 의존한다. 
- 이 때, 가장 대표적이고 대중적인 방법론이 바로 깁스 샘플링이다. 
- 깁스 샘플링은 $(\beta, \sigma^2)|Y$의 분포가 표준적이지 않더라도 $\beta|Y,\sigma^2$과 $\sigma^2|Y, \beta$의 분포는 표준적일 때 적용가능한 시뮬레이션 방법이다. 
- 이럴 때 특정 파라미터를 제외한 다른 모든 파라미터와 자료가 주어졌을 때 분포를 완전 조건부 분포(Full Conditional Distribution)라 부른다.
- 이는 결합 사후 분포 $(\beta, \sigma^2)|Y$ 및 주변 사후 분포 $(\beta|Y)$, $(\sigma^2|Y)$와 다른 것임을 유의해야한다.
#### 2.2.5 깁스 샘플링 알고리즘
- 깁스 샘플링의 구체적인 실험방법을 이해하기 위해서 [[#2.2.1 Case A. $ sigma 2$이 알려져 있는 경우|Case A]]와 [[#2.2.2 Case B. $ beta$가 알려져 있는 경우|Case B]]를 돌이켜보자.
- 우리는 어떠한 값이 주어져 있다면 나머지 하나의 사후 분포 도출이 그렇게 어렵지 않았다는 것을 파악할 수 있다.
- Case A 에서는 $\sigma^2$이 주어졌을 때, $\beta$의 사후 분포가 다변량 정규 분포임을 알았다.
- Case B 에서는 $\beta$가 알려져 있을 때, $\sigma^2$의 사후 분포가 역감마 분포임을 알았다.
- 따라서, Case A으로 부터는 $\beta$를 $\beta|Y,\sigma^2$로부터 추출하는 방법을 알 수 있으며, Case B는 $\sigma^2$를 $\sigma^2|Y, \beta$로부터 추출하는 방법을 제공한다.
- **깁스 샘플링**의 핵심적인 아이디어는 $\beta|Y, \sigma^2$과 $\sigma^2|Y, \beta$을 번갈아가며 추출한 샘플들이 결과적으로 $(\beta, \sigma^2)|Y$로부터 추출된 샘플들이라는 것이다. 
###### 깁스 샘플링 알고리즘
1. **초기화**: 모든 파라미터 $\theta_{1}, \theta_{2}, \dots, \theta_{k}$에 초기값을 할당함.
2. **반복**: ($t=1,2, \dots$)
	- $\theta^t_{1} \sim P(\theta_{1}|\theta^{t-1}_{2}, \theta^{t-1}_{3}, \dots, \theta^{t-1}_{k})$에서 샘플링
	- $\theta^t_{2} \sim P(\theta_{2}|\theta^t_{1}, \theta^{t-1}_{3}, \dots, \theta^{t-1}_{k})$에서 샘플링
	- ...
	- $\theta^t_{k} \sim P(\theta_{k}|\theta^t_{1},\theta^t_{2}, \dots, \theta^t_{k-1})$에서 샘플링
3. **수렴 및 수집**:
	- 결국 최종 업데이트된 $(\theta^t_{1}, \theta^t_{2}, \dots, \theta^t_{k})$가 만들어짐.
	- 이 샘플들이 결합 사후 분포 $P(\theta_{1}, \theta_{2}, \dots, \theta_{k}|data)$를 따름
4. **통계량 계산**:
	- 수집된 샘플을 사용하여 평균, 분산, 분위수 등등을 계산할 수 있음.
	- 이를 통해 다양한 가설 검정 및 예측을 수행 !
###### 알고리즘 2.1: 선형회귀모형 - 깁스 샘플링
- **0 단계** : 초기값 $\sigma^2(=\sigma^{2(0)})$를 설정하고, $j=1$로 둔다.
- **1 단계** : 주어진 $\sigma^{2(j-1)}$로 부터 아래 식을 계산하고, 
$$
B_{1}= \left( \frac{1}{\sigma^{2(j-1)}}\mathbf{X'X}+B_{0}^{-1} \right)^{-1},  \, A = \frac{1}{\sigma^{2(j-1)}}\mathbf{X'}Y + B_{0}^{-1}\beta_{0}
$$
- $\beta^{j}$를 $N(B_{1}A, B_{1})$에서 샘플링하고 저장한다.
- **2 단계** : 주어진 $\beta^{j}$로부터 $\sigma^{2(j)}$를 아래 역감마 분포로부터 샘플링한 뒤 저장한다.
$$
IG\left( \frac{\alpha_{0}+T}{2}, \frac{(Y - \mathbf{X}\beta^{j})'(Y- \mathbf{X}\beta) + \delta_{0}}{2} \right)
$$
- **3 단계** : $j = j+1$로 설정하고, $j \leq n$(= 시뮬레이션 크기)이면 1단계로 돌아간다. 
- 이렇게 매 반복시행마다 수정되는 완전 조건부 분포로부터 $\beta$와 $\sigma^2$을 번갈아가며 생성함으로 써 $(\beta, \sigma^2)$의 결합 사후 분포로부터의 샘플을 추출할 수 있다는 것이 깁스 샘플링의 핵심이다.
###### 사후 분포 수렴과 번인(burn-in)
- 샘플링을 시작하기 전에 $\sigma^2$의 사전 평균을 $\sigma^2$의 초기값으로 사용한다.
- 사전 평균과 사후 평균의 차이가 큰 경우에는 초기에 추출된 값들이 결합 사후 분포로부터 추출되지 않았을 가능성이 높다. → 이러한 불확실성을 없애주기 위하여 처음 $n_{0}$ 번의 반복응로부터 추출된 샘플을 제거한 다음, $(n-n_{0})$번의 시뮬레이션 결과만을 이용해서 ==사후 평균, 표준오차, 신용구간== 등을 추론하게 된다. 
- 이렇게 임의의 초기값에서 사후 분포로 수렴하는 데 소요되는 초기 $n_{0}$번의 시뮬레이션을 **번인(burn-in)** 이라고 부른다.
###### 사후 분포 추론
==<파라미터의 사후 분포>==
- 번인(burn-in) 이후의 시뮬레이션 크기를 $n_{1}(=n-n_{0})$라고 표기하면, 깁스 샘플링의 결과로부터 우리는 아래와 같이 $n_{1} \times (k+1)$ 행렬 형태로 저장된 $(\beta, \sigma^2)$의 사후 샘플들(posterior draws)를 얻게 된다. 편의상 이 행렬을 $MHm$이라고 표기한다.
$$
MHm = \begin{pmatrix}
\beta^{(n_{0}+1)'} & \sigma^{2(n_{0}+1)} \\
\beta^{(n_{0}+2)'}  & \sigma^{2(n_{0}+2)} \\
\vdots & \vdots \\
\beta^{(n)'}  & \sigma^{2(n)}
\end{pmatrix} = 
\begin{pmatrix}
\beta_{1}^{(n_{0}+1)}  &  \beta_{2}^{(n_{0}+1)} & \cdots  & \beta_{k}^{(n_{0}+1)}  &  \sigma^{2(n_{0}+1)} \\
\beta_{1}^{(n_{0}+2)}  &  \beta_{2}^{(n_{0}+2)}  &  \cdots  &  \beta_{k^{(n_{0}+2)}}  &  \sigma^{2(n_{0}+2)} \\
\vdots  &  \vdots  &    &  \vdots  &  \vdots \\
\beta_{1}^{(n)}  &  \beta_{2}^{(n)}  &  \cdots  &  \beta_{k}^{(n)}  &  \sigma^{2(n)}
\end{pmatrix}
$$
- $MHm$ 행렬 자체는 $(\beta, \sigma^2)$의 결합 사후샘플들이다. 
- 그리고 첫 번째 열은 $\beta_{1}$의 주변 사후 분포(marginal posterior distribution, $\beta_{1}|Y$)로부터의 샘플이다. 
- 따라서 첫 번째 열의 평균과 분산을 계산하면 $\beta_{1}$의 사후 평균과 사후 분산이 추정된다.
$$
E[\beta_{1}|Y] \approx \frac{1}{n_{1}}\sum_{j=1}^{n_{1}} \beta_{1}^{(j)} \text{ and } Var[\beta_{1}|Y] \approx \frac{1}{n_{1}}\sum_{j=1}^{n_{1}} (\beta_{1}^{(j)} - E[\beta_{1}|Y])^2
$$
- 크기 순으로 정렬하여 히스토그램을 그리면 ! 신용구간을 얻을 수 있음.

==<사후 예측 분포>==
- 다음으로 ($T+1$) 시점의 종속변수, $y_{T+1}$을 예측해보도록 하자!
- 미래의 종속변수는 현재 시점$(T)$에서 확률 변수로 취급됨. → $y_{T+1}$을 예측한다는 것은 $T$ 시점까지의 정보를 이용해서 $y_{T+1}|Y$로 정의되는 $y_{T+1}$의 사후 분포 예측(Posterior predictive distribution)을 도출하는 것이다!!!
- 이 때, 사후 예측 밀도 함수는 다음과 같이 정의된다.
$$
\begin{equation}
\begin{split}
f(y_{T+1}|Y) & = \int f(y_{T+1}, \beta, \sigma^2|Y)d(\beta, \sigma^2) \\
& = \int f(y_{t+1}|\beta, \sigma^2, Y)\pi(\beta, \sigma^2|Y)d(\beta, \sigma^2)
\end{split}
\end{equation}
$$
- 사후 예측 분포 또한 $(\beta, \sigma^2)$과 마찬가지로 해석적으로 도출되지 않으므로 시뮬레이션 기법에 의존해서 수치적으로 계산된다. 
- 이는 이미 추출된 파라미터의 사후 샘플들을 이용해서 아래와 같이 $y_{T+1}$의 사후 샘플을 추출하면 된다.
$$
y^{(j)}_{T+1}|\beta^{(j)}, \sigma^{2(j)} \sim N(x'_{T+1}\beta^{(j)}, \sigma^{2(j)}) \text{ for }j = n_{0}+1, n_{0}+2, \dots, n
$$
- 더 구체적으로 설명하자면, $j$번째 반복시행에서 추출된 사후 샘플, $(\beta^{(j)}, \sigma^{2(j)})$을 이용해서 위 식의 정규분포로부터 하나의 $y^{(j)}_{T+1}$를 임의 추출하여 저장한다. 
- 각각의 파라미터 사후 샘플로부터 그에 대응하는 $y_{T+1}$의 사후 샘플도 $n_{1}$개 저장된다. 
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
