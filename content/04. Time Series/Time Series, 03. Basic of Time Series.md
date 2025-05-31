---
title: 03. Basic of Time Series
draft: false
tags:
  - "#Lag"
  - "#ACF"
  - "#PACF"
  - "#MCMC"
---
## Lag Operator
- Time Series에서 자주 사용하는 기법이라 생각해보자. Lag Operator는 다음과 같이 정의된다.
$$
LY_{t} = Y_{t-1}
$$
- Defining $L^2 = LL$ such that $L^2Y_{t} = LY_{t-1} = Y_{t-2}$. In general, we have
$$
L^kY_{t} = Y_{t-k}
$$
- We can use the lag operator to express the AR(2) process. Because
$$
\begin{align}
 & Y_{t} = \phi_{1}Y_{t-1} + \phi_{2}Y_{t-2} + e_{t} \\
 & Y_{t} = \phi_{1}LY_{t}+\phi_{2}L^2Y_{t} + e_{t} \\
 & (1 - \phi_{1}L - \phi_{2}L^2)Y_{t} = e_{t}
\end{align}
$$
- Letting $\phi(L) \equiv 1 - \phi_{1}L - \phi_{2}L^2$, then the AR(2) can be written as
$$
\phi(L)Y_{t} = e_{t}
$$
- where $\phi(L)$ means polynomial equation in lag operator. This means AR process always can be expressed as $\phi(L)Y_{t} = e_{t}$

#### Wold Representation with Lag Operator
$$
\begin{align}
 & Y_{t} = e_{t} + \psi e_{t-1} + \psi_{2} e_{t-2} + \cdots + \phi_{j}e_{t-j} + \cdots \\
 & Y_{t} = e_{t} + \psi Le_{t} + \psi_{2}L^2e_{t} + \cdots + \phi_{j}L^je_{t} + \cdots \\
 & Y_{t} = (1 + \psi L + \psi_{2}L^2 + \cdots + \psi_{j}L^2 + \cdots)e_{t} \\
 & Y_{t} = \psi(L)e_{t}, \text{ where } \psi(L) = \sum_{j=0}^{\infty}\psi_{j}L^j, \psi_{0}=1 
\end{align}
$$
- Reminder. (AR(1))
$$
Y_{t} = \delta + \phi Y_{t-1} + e_{t}, e_{t} \sim iidN(0, \sigma^2)
$$
- By back-substitution, we have
$$
\begin{align}
 & Y_{t} = \phi Y_{t-1} + e_{t} \\
 & Y_{t} = \phi(\phi Y_{t-2} +e_{t-1})+ e_{t} \\
 & Y_{t} = \phi^2Y_{t-2} + e_{t} + \phi e_{t-1} \\
 & Y_{t} = \phi^2(\phi Y_{t-3} + e_{t-2}) + e_{t} + \phi e_{t-1} \\
 & Y_{t} = \phi^3Y_{t-3} + e_{t} + \phi e_{t-1} + \phi^2e_{t-2} \\
 & \,\,\vdots \\
 & Y_{t} = e_{t} + \phi e_{t-1} + \phi^2e_{t-2} + \cdots + \phi^j e_{t-j}+ \cdots
\end{align}
$$
- With the Lag operator
$$
\begin{align}
 & Y_{t} = \phi Y_{t-1} + e_{t} \\
 & (1-\phi L)Y_{t} = e_{t} \\
 & Y_{t} = \frac{1}{1-\phi L}e_{t}, \quad \text{If we consider }|\phi L|<1
\end{align}
$$
- $\frac{1}{1-\phi L}$이 마치 공비가 $\phi L$인 합처럼 생김 !!
$$
\frac{1}{1-\phi L} = 1 + \phi L + (\phi L)^2 + (\phi L)^3 + \cdots
$$
- Then,
$$
\begin{align}
 & Y_{t} = [1 + \phi L +(\phi L)^2 + (\phi L)^3 + \cdots]  e_{t} \\
 & Y_{t} = e_{t} + \phi e_{t-1} + \phi^2e_{t-2} + \phi^3e_{t-3} + \cdots
\end{align}
$$
#### AR(p) case with lag operator
$$
\begin{align}
 & Y_{t} = \phi_{1} Y_{t-1} + \phi_{2} Y_{t-2} + \phi_{3}Y_{t-3} + \cdots + \phi_{p}Y_{t-p} + e_{t} \\
 & (1 - \phi_{1}L - \phi_{2}L^2 - \phi_{3}L^3 + \cdots + \phi_{p}L^p)Y_{t} = e_{t} \\
 & \phi(L)Y_{t} = e_{t}, \text{ where } \phi(L) \equiv 1 - \phi_{1}L - \phi_{2}L^2 - \phi_{3}L^3 + \cdots + \phi_{p}L^p \\
 & Y_{t} = \phi(L)^{-1} e_{t}
\end{align}
$$
- [[#Wold Representation with Lag Operator]] shows
$$
\psi(L) = \phi(L)^{-1}
$$
#### Stationary condition: AR(2) case

$$
Y_{t} = \delta + \phi_{1}Y_{t-1} + \phi_{2}Y_{t-2} + e_{t}, e_{t} \sim iid(0, \sigma^2)
$$
- Auto-covariance (assuming $\delta = 0$):
$$
\begin{align}
 & \gamma(k) = Cov(Y_{t}, Y_{t-k}) = E(Y_{t}Y_{t_{k}}) \\
 & \gamma(k) = E(Y_{t}Y_{t-k}) = \phi_{1}E(Y_{t-{1}}Y_{t-k}) + \phi_{2}E(Y_{t-2}Y_{t-k}) + E(e_{t}Y_{t-k}) = \phi \gamma(k-1) \\
 & \rho(k) = \phi_{1}\rho(k-1) + \phi_{2}\rho(k-2)
\end{align}
$$
- From this autocorrelation function we know the characteristic equation such that
$$
\begin{align}
 & \rho(k) - \phi_{1}\rho(k-1) - \phi_{2}\rho(k-2) =0 \\
 & \to \lambda^2 - \phi_{1}\lambda - \phi_{2} =0
\end{align}
$$
- 추가적인 AR(2) Process는 [[Time Series, 02. Regression Analysis with Time Series Data#AR(2) Process]]를 참고하자.
- Now consider the lag operator, first, AR(1) case
$$
\begin{align}
\rho(k) = \phi \rho(k-1) \\
\to \lambda - \phi =0
\end{align}
$$
- The characteristic root of AR(1) is $\lambda = \phi$. The AR(1) with the lag operator $\phi(L) = 1- \phi L$, the root of  $\phi(L) =0$  is
$$
L = \frac{1}{\phi}
$$
- From the stationary condition $|\phi|<1$ we can rewrite the condition
$$
|L| > 1
$$
- Back to AR(2) case. Denote $L_{1},L_{2}$ be the characteristic roots of $1- \phi_{1} L-\phi_{2}L^2 = 0$.
- By using $L = \frac{1}{\lambda}$
$$
\begin{align}
 & 1 - \phi_{1} L - \phi_{2}L^2 =0 \\
 & \to 1 - \phi_{1}\frac{1}{\lambda} - \phi_{2}\frac{1}{\lambda^2} = 0 \\
 & \to \lambda^2 - \phi_{1}\lambda - \phi_{2} = 0 \\
 & |L_{1}|>1, |L_{2}|>2
\end{align}
$$
- AR(2)에서 특성근 방정식(eigen value equation)처럼 Characteristic Equation을 하는 것과 같음 !!!
- 처음에는 Lag Operator를 약속처럼 사용했으나, 점차 변수처럼(?) 활용할 수 있게됨. 이는 Lag operator가 갖는 특징이자 장점이라고 볼 수 있음.
- 따라서, $|L_{1}| >1, |L_{2}| >1$이 Stationary Condition이라고 볼 수 있음.
#### ARMA(p, q)
- First, consider the MA(q) case,
$$
\begin{align}
 & Y_{t} = \mu + e_{t} + \theta_{1}e_{t-2} + \theta_{2}e_{t-2} + \cdots + \theta_{q} e_{t-q} \\
 & Y_{t} = \mu +e_{t} + \theta_{1}Le_{t} + \theta_{2}L^2e_{t} + \cdots + \theta_{q}L^qe_{t} \\
 & Y_{t} = \mu + (1 + \theta_{1}L + \theta_{2}L^2 + \cdots + \theta_{q}L^q)e_{t} \\
 & Y_{t} = \mu + \theta(L) e_{t}, \text{ where } \theta(L) \equiv 1 + \theta_{1}L + \theta_{2} L^2 + \cdots + \theta_{q}L^q
\end{align}
$$
- Next, the ARMA(p, q) case,
$$
\begin{align}
 & Y_{t} = \mu + \phi_{1}Y_{t-1} + \phi_{2}Y_{t-2} + \cdots + \phi_{p}Y_{t-p} + e_{t} + \theta_{1}e_{t-1} + \theta_{2}e_{t-2} + \cdots + \theta_{q} e_{t-q} \\
 & (1 - \phi_{1}L - \phi_{2}L^2 - \phi_{3}L^3 + \cdots + \phi_{p}L^p)Y_{t} = \mu + (1 + \theta_{1}L + \theta_{2}L^2+ \cdots + \theta_{q}L^q)e_{t} \\
 & \phi(L)Y_{t} = \mu+ \theta(L)e_{t} \\
 & \text{where } \phi(L) \equiv 1 - \phi_{1}L -\phi_{2}L^2 - \phi_{3}L^3 + \cdots + \phi_{p}L^p \\
 & \text{and } \theta(L) \equiv 1 + \theta_{1}L + \theta_{2}L^2 + \cdots + \theta_{q}L^q
\end{align}
$$
- 따라서, Lag Operator를 활용해서 우리는 Stationary Condition을 파악할 수 있다. MA에서 Stationary Condition은 q가 유한한 것이다. 또한, AR에서는 위에서 확인할 수 있듯이 특성근을 활용하여 우리의 Stationary Condition을 쉽게 파악할 수 있다. 
## Autocorrelation Function (ACF) and Partial ACF (PACF)
- The coefficient of corrleation between two values in a teim series is called the autocorrelation function (ACF) for example the ACF for a time series $Y_{t}$ is given by:
$$
\rho(k), k=1, 2, 3, \dots
$$
- This value of $k$ is the time gap being considered and is called the lag. A lag 1 autocorrelation (i.e., $k=1$ in the above) is the correlation between values that are one time period apart. 
- More generally, a lag $k$ autocorrelation is the correlation between values that are $k$ time period apart.
- 즉, Autocorrelation Function은 $\rho(k) = \frac{\gamma(k)}{\gamma(0)}$이고, $\gamma(k) = E(Y_{t}Y_{t-k})$이기에 현재 $t$기와 $t-k$기 와의 상관관계를 의미한다. $\gamma(0) = V(Y_{t})$
- The ACF is a way to measure the linear relationship between an observation at time $t$ and the observations at previous times. → t와 t-k기의 선형 상관관계를 측정하는 방식임.
- If we assume an AR($k$) model, then we may wish to only measure the association between $Y_{t}$ and $Y_{t-k}$ and filter out the linear influence of the random variables that lie in between (i.e., $Y_{t-1}, Y_{t-2}, \dots, Y_{t-(k-1)}$), which requires a transformation on the times series. → 오직 $Y_{t}$와 $Y_{t-k}$의 상관관계에 관심이 있음. 
- Then by calculating the corrleation of the transformed time series we obtain the partial autocorrelation function (PACF) → 어떤 특정 시기의 Direct한 효과를 파악하고 싶은데 이는 시간이 점차 그 효과를 희석시키게 됨. 따라서 partial 관계를 통해 이를 극복해보고자 함.
- The PACF is most useful for **identifying the order of an autoregressive model.** 
- Specifically, sample partial autocorrelations that are significantly different from 0 indicate lagged terms of $y$ that are useful predictors of $Y_{t}$. It is important that the choice of the order makes sense.
- For example, suppose you have blood pressure readings for every day over the past two years. You may find that an AR(1) or AR(2) model is appropriate for modeling ==blood pressure==. 
- However, the PACF may indicate a large partial autocorrelation value at a lag of 17, but such a large order for an autoregressive model likely does not make much sense.
$$
\begin{align}
 & AR(1): Y_{t} = \phi_{11}Y_{t-1} + e_{t} \\
 & AR(2): Y_{t} = \phi_{21}Y_{t-1} + \phi_{22}Y_{t-2} + e_{t} \\
 & AR(3): Y_{t} = \phi_{31}Y_{t-1} + \phi_{32}Y_{t-2} + \phi_{33} Y_{t-3} + e_{t} \\
 & \,\,\,\,\,\,\,\, \vdots \\
 & AR(j): Y_{t} = \phi_{j1}Y_{t-1} + \phi_{j2}Y_{t-2} + \phi_{j3}Y_{t-3} + \cdots + \phi_{jj}Y_{t-j} + e_{t} \\
 & \,\,\,\,\,\,\,\, \vdots \\
\end{align}
$$
- The PACF uses the statistical test for $\phi$'s with the regression of AR(p) models to identify statistically significant coefficients of lags:
$$
H_{0}: \phi_{jj} = 0 \text{ v.s. } H_{1}: \phi_{jj} \neq 0
$$
- 즉 귀무가설을 기각하지 못하면, 어떤 시차의 $\phi$는 $Y_{t}$에 영향을 미치지 못한다는 것!
#### Google Stock Example
- The closing stock price of a share of Google Stock during 2005-02-07 to 2005-07-07
- ![[Time_Series, Figure.09.png|400x250]]
- Here we notice that there is a significant spike at a lag of 1 and much lower spikes for the subsequent lags. Thus, AR(1) model would likely be feasible for this data set.
- ![[Time_Series, Figure.10.png|400x250]]
- We next create a lag-1 price variable and consider a scatterplot of price versus this lag-1 variable: → AR(1)을 따를 것이라 가정
- ![[Time_Series, Figure.11.png|400x250]]

#### CLT Basics, again 
- Consider a simple model:
$$
Y_{i} = \mu + e_{i}, e_{i} \sim iid (0, \sigma^2)
$$
- $e_{i}$ : White-Noise
- The OLS estimator for $\mu$ is
$$
\hat{\mu} = \bar{Y} = \frac{1}{N}\sum_{i=1}^{N} Y_{i}
$$
- The mean and variance of the estimator is
$$
\begin{alignat}{2}
 & E(\hat{\mu}) = E(\bar{Y}) = \mu \\
 & Var(\hat{\mu}) = Var\left( \frac{1}{N}\sum_{i=1}^{N} Y_{i} \right) = \frac{\sigma^1}{N}
\end{alignat}
$$
- As $N \to \infty$
$$
\hat{\mu} \to^{p} \mu
$$
- If we want to see what is the distribution of $\hat{\mu}$ as $N \to \infty$, then we can use $\sqrt{ N }$ such that -> 수렴 속도를 덜 빠르게 하면 분포로 수렴하는 것을 관측할 수 있음.
$$
Var(\sqrt{ N }\hat{\mu}) = N Var(\hat{\mu}) = \sigma^2
$$
- As $N \to \infty$
$$
\begin{align}
 & \sqrt{ N } \hat{\mu} \sim (\sqrt{ N }\mu, \sigma^2) \\
 & \sqrt{ N }(\hat{\mu} - \mu) \sim (0, \sigma^2)
\end{align}
$$
- 즉, $\sqrt{ N }$이 수렴 속도를 잡아줌.
$$
\sqrt{ N }(\hat{\mu} - \mu) = \sqrt{ N }(\bar{Y} - \mu) = \frac{1}{\sqrt{ N }}\sum_{i=1}^{N} (Y_{i} - \mu)
$$
- By CLT we have
$$
\frac{1}{\sqrt{ N }}\sum_{i=1}^{N} (Y_{i}- \mu) \sim N(0, \sigma^2)
$$
## CLT Application with AR(1)
$$
Y_{t} = \phi Y_{t-1} + e_{t}, e_{t} \sim iid(0, \sigma^2)
$$
- The OLS estimator for $\phi$ is
$$
\hat{\phi} = \frac{\sum Y_{t-1} Y_{t}}{\sum Y_{t-1}^2} = \phi +\frac{\sum Y_{t-1}e_{t}}{\sum Y_{t-1}^2}
$$
- **Proof**
$$
\begin{align}
 \hat{\phi}  & = (Y_{t-1}'Y_{t-1})^{-1}Y_{t-1}'Y_{t} = \frac{\sum Y_{t-1}Y_{t}}{\sum Y_{t-1}^2}, \\
 & Y_{t} = \phi Y_{t-1}+e_{t}, \\
\hat{\phi}  & =(Y_{t-1}'Y_{t-1})^{-1}Y_{t-1}'Y_{t} = (Y_{t-1}'Y_{t-1})^{-1}Y_{t-1}'(\phi Y_{t-1}+e_{t}) \\
 & = \phi + (Y_{t-1}'Y_{t-1})^{-1}Y_{t-1}'e_{t} \\
 & = \phi + \frac{\sum Y_{t-1}e_{t}}{\sum Y_{t-1}^2}
\end{align}
$$
- Letting 
$$
\begin{align}
  & Z_{t} \equiv Y_{t-1}e_{t} \\
 E(Z_{t})  & = E(Y_{t-1}e_{t}) = E[E(Y_{t-1}e_{t}|I_{t-1})]   \\
& = E[Y_{t-1}E(e|I_{t-1})] = 0
\end{align}
$$
- 우리는 Expectation of Conditional Expectation의 성질을 이용해서 $I_{t-1}$이 주어졌을 때, 즉 Information이 주어졌을 때의 조건부 기댓값으로 성질을 변환하여 문제를 해결할 수 있다.
- $t-1$기의 정보가 주어졌기 때문에 $Y_{t-1}$이 상수취급 되며 이는 결국 $E(e_{t}|I_{t-1})$는 각 시기별 iid의 성질로 인하여 기댓값이 0이 된다. 
- and Variance
$$
\begin{align}
Var(Z_{t}) &  = E[(Y_{t-1}e_{t})]^2 = E[Y_{t-1}^2E(e_{t}^2|I_{t-1})] = \sigma^2E(Y_{t-1}^2) \\
 & =\sigma^2\gamma(0) = \sigma^2Var(Y_{t})
\end{align}
$$
- 위 식 또한, 조건부 기댓값의 기댓값 성질을 이용하는데, 기본 $Var(Z_{t})$는 $E[(Y_{t-1}e_{t})^2] - [E(Y_{t-1}e_{t})]^2$의 꼴 이다. 하지만 $t-1$기와 $t$기는 iid 성질로 인하여 독립이기 때문에 $E(Y_{t-1}e_{t} )=0$이다.
- 또한, $\gamma(0) = Cov(Y_{t}, Y_{t}) = Var(Y_{t})$와 같다.
- By applying CLT to the AR(1)
$$
\bar{Z} \sim \left( 0, \frac{\sigma^2\gamma(0)}{T} \right)
$$
- which means $\bar{z} \to^p 0$ as $T \to \infty$
- To find the asymptotic distribution we will use $\sqrt{ T }$
$$
\begin{align}
 & \sqrt{ T }\bar{Z} \sim N(0, \sigma^2\gamma(0)) \\
 & \to \frac{1}{\sqrt{ T }}\sum_{i=1}^{T} Z_{t} \sim N(0, \sigma^2\gamma(0)) \\
 & \to \frac{1}{\sqrt{ T }} \sum_{i=1}^{T} Y_{t-1}e_{t} \sim N(0, \sigma^2 \gamma(0))
\end{align}
$$
- $\gamma(0)$ can be estimated by
$$
\hat{\gamma(0)} = \frac{1}{T}\sum_{i=1}^{T} (Y_{t-1}- E(Y_{t-1}))^2 = \frac{1}{T}\sum_{i=1}^{T} (Y_{t-1})^2
$$
- $E(Y_{t-1})$: t-1기의 Prediction값의 평균.
- and we know
$$
\hat{\gamma}(0) →^p \gamma(0)
$$
- Now from the AR(1)
$$
\begin{align} 
\hat{\phi}  & = \phi + \frac{Y_{t-1}e_{t}}{\sum Y_{t-1}^2} = \phi + \frac{\frac{1}{T}\sum Y_{t-1}e_{t}}{\frac{1}{T}\sum Y_{t-1}^2} \\
 & = \phi+\frac{\frac{1}{\sqrt{ T }}\sum Y_{t-1}e_{t}}{\frac{1}{T}\sum Y_{t-1}^2}\frac{1}{\sqrt{ T }}
\end{align}
$$
- By the asymptotic theory,
$$
\begin{align}
 \frac{1}{\sqrt{ T }}\sum_{i=1}^{T} Y_{t-1}e_{t}  & \to^d N(0, \sigma^2\gamma(0)) \\
 \frac{1}{T}\sum Y_{t-1}^2  & \to^p \gamma(0)
\end{align}
$$
- Due to $\frac{1}{\sqrt{ T }}$
$$
\frac{\frac{1}{\sqrt{ T }}\sum Y_{t-1}e_{t}}{\frac{1}{T}\sum Y_{t-1}^2}\frac{1}{\sqrt{ T }} \to^p 0
$$
- 분자는 분포로 수렴하고, 분모는 점으로 수렴함. 그리고 $\frac{1}{\sqrt{ T }}$는 0으로 수렴하게 됨.
- In order to find the distribution
$$
\begin{align}
 & \hat{\phi} = \phi + \frac{\frac{1}{\sqrt{ T }}\sum Y_{t-1}e_{t}}{\frac{1}{T}\sum Y_{t-1}^2}\frac{1}{\sqrt{ T }} \\
 & \hat{\phi} - \phi = \frac{\frac{1}{\sqrt{ T }}\sum Y_{t-1}e_{t}}{\frac{1}{T}\sum Y_{t-1}^2}\frac{1}{\sqrt{ T }} \\
 \sqrt{ T }(\hat{\phi}-\phi)  & = \frac{\frac{1}{\sqrt{ T }}\sum Y_{t-1}e_{t}}{\frac{1}{T}\sum Y_{t-1}^2}\frac{1}{\sqrt{ T }} \to^d \gamma(0)N(0, \sigma^2\gamma(0))
\end{align}
$$
- 우리가 이렇게 하는 이유는 우리는 이러한 추정값들의 통계적 Test를 하고 싶은데, 그러려면 분포를 알아야 함. 따라서, 이러한 수식 전개를 통해 어떤 분포의 평균, 분산을 구하여 통계적으로 유의한지를 선보이고자함.
$$
\sqrt{ T }(\hat{\phi} - \phi) \to^d \gamma(0)N(0, \sigma^2\gamma(0)) = N(0, 1- \phi^2)\left( \because \gamma(0)=\frac{\sigma^2}{1-\phi^2} \right)
$$
- 이는 $\gamma(0)$을 없애기 위해서 분산 안으로 들어가게 되면서 제곱 수 $\frac{1}{\gamma(0)^2}$이 대입됨. 따라서, $\sigma^2\times\frac{\sigma^2}{1-\phi^2}\times\frac{(1-\phi^2)^2}{\sigma^4} = 1-\phi^2$
## Statistical Test for $\hat{\phi}$
- With the AR(1) process:
$$
Y_{t} = \phi Y_{t-1} + e_{t}, e_{t} \sim iid(0, \sigma^2)
$$
- To test $\hat{\phi}$ we need the statistical hypothesis:
$$
H_{0}: \phi=0 \text{ v.s. } H_{1}:\phi<0
$$
- T-test 가능 !
- Under the condition that the null hypothesis is true, we know
$$
\begin{align}
 & \sqrt{ T }(\hat{\phi} - \phi) \to^d N(0, 1 - \phi^2) \\
 & \to \sqrt{ T }\hat{\phi } \to^d N(0, 1)(\because \phi=0)
\end{align}
$$
## Unit Root (단위근)
- Consider the statistical test problem (Unit root test)
- Unit Root가 있다는 것은 → Non-stationary하다는 것이다. 또한, 이 테스트 기법이 주류이긴 하나 법칙은 아니니 "무조건" 옳다는 식의 접근은 올바르지 못하다.
- 우리는 따라서, 다음과 같은 가설을 세울 수 있다.
$$
H_{0}: \phi=1 \text{ v.s. } H_{1}:\phi<1 \text{ (Staionary Condition for AR(1))}
$$
- $\phi=1 \to \triangle Y_{t} = Y_{t}-Y_{t-1}$.
- Under the null hypothesis
$$
\begin{align}
 & \sqrt{ T }(\hat{\phi} - \phi) \to^d N(0, 1-\phi^2) \\
 & \to \sqrt{ T }(\hat{\phi} -1) \to^d N(0, 0)(\because \phi=1)
\end{align}
$$
- 즉, 이는 분포로 수렴하지 않는다는 문제가 생긴다. 
- 이럴 때 우리가 해결하는 방법은 두 가지 정도가 대표적이다.
#### Test Statistic using Monte Carlo Experiment
- Find the distribution of the test statistic
$$
\frac{\hat{\phi}-1}{SE(\hat{\phi})}
$$
- If we do not have theoretical distribution information we can generate the distribution information by using computer simulation.
- The goal is to make the distribution of $\sqrt{ T} \hat{\phi}$
###### Process
1. Generate 10,000 sets of data under $H_{0}$
$$
Y_{t} = e_{t}, \, e_{t} \sim iid(0, \sigma^2)
$$
- 위 수식은 분포에 대한 정보가 없다 !
- We can use numpy.random.normal or numpy.random.uniform(any iid random draw)
1. data set = 500 samples (we have 10,000 sets)
2. Run a regression for each set of data generated we have 10,000 sets of $\hat{\phi}$
$$
Y_{t} = \hat{\phi}Y_{t-1} + u_{t}
$$
3. Calculate $\sqrt{ T }\hat{\phi }$ for each from 2
4. Draw a histogram of $\sqrt{ T }\hat{\phi}$의 분포를 추정 가능 !
- 정리하자면, 우리의 시계열 모형의 Stationary를 파악하기 위해서, Unit Root Test를 통해서 파악함. 하지만 Unit Root Test를 해보려고 하니까. 애초에 귀무 가설 $H_{0}: \phi=1$이라는 발산되는 분포?를 우리는 알아낼 수 없음 → 이러한 테스트를 위위해서 몬테카를로 시뮬레이션을 진행할 수 있다! → 몬테카를로 시뮬레이션을 통해서 분포를 최대한 근사 시켜, 거기서 얻어낸 평균과 분산 등의 값으로 Test를 진행할 수 있기 때문에!!
