---
title: 02. Regression Analysis with Time Series Data
draft: false
tags:
  - "#OLS"
  - "#TimeSeries"
---

## Cross-Sectional Data
- A cross-sectional data set consists of a sample of individuals, households, firms, cities, states, countries, or a variety of other units, taken at a given point in time. → 한 시점의 $i$
- An important feature of cross-sectional data is that we can often assume that they have been obtained by <mark style="background: #FF5582A6;">random sampling from the underlying population</mark>
- ![[Time_Series, Figure.03.png]]
## Time Series Data
- A time series data set consists of observations on a variable or several variables over time. → 여러 시점의 관측된 집합 !
- Because past events can influence future events and lags(과거값) in behavior are prevalent in the social sciences, time is an important dimension in a time series data set.
- A key feature of time series data that makes them more difficult to analyze than cross-sectional data is that observations can rarely, if ever, be assumed to be independent across time. Time series are related, often strongly related, to their recent histories.
- Another feature of time series data that can require special attention is the data frequency at which the data are collected → 주기성을 요구할 수 있다? 
	- $lag$ → Frequency (A와 B의 관측 시점이 매우 다른데!) 주기를 보일 수 있다?
- The most common frequencies are daily, weekly, monthly, quarterly, and annually.
	- seasonal pattern: 계절성 패턴. → Time Series Analysis에서 중요한 요소임.
	- 추가적으로 추세(Trend), 계절성(Seasonality), 주기성(Cyclical component), 불규칙요소(Irregular component)
	- 경향성과 계절성을 관측 시기에 영향을 받는다. 
- ![[Time_Series, Figure.04.png]]

## Stochastic Process (확률적 과정)
- Formally, a sequence of random variables indexed by time is called a stochastic process(=random) or a time series process. ("Stochastic" is a synonym for ==random==)
	- "Random이기는 하나 구조화된 무작위성", 즉 모든 Random → Stochastic과정으로 모델링 될 수 있는 것은 아님.
	- 순서가 있어야 함!(Sequence, 수열): $\{a_{1}, a_{2}, \dots, a_{t}\}$
- When we collect a time series data set, we obtain **one possible outcome**, or realization, of the stochastic process.
	- 즉, stochastic process가 실현된 값이 one possible outcome !!
- We can only see a single realization because we cannot go back in time and start the process over again. → 우리는 한 시간대에 살아가고 있기 때문에 오직 하나의 실존에 대해서만 값을 알고 있음. 예를 들어서, 우리는 오늘의 나는 여러 개의 시뮬레이션 중에서 오로지 1개의 실현된 시뮬레이션을 살아가고 있다!!

## Static Models
- Suppose that we have time series data available on two variables, say $y$ and $z$, where $y_{t}$ and $z_{t}$ are dated contemporaneously. → 하나의 시점에 두 변수가 관측됨 !
- A static model relating $y$ to $z$ is → 한 명의 t개의 point 값.
$$
y_{t} = \beta_{0} + \beta_{1}z_{t} + u_{t}, \, t=1, 2, \dots, n
$$
- The name "static model" comes from the fact that we are modeling a contemporaneous relationship between $y$ and $z$ → 동일 $t$ 에 대해서 다른 변수에 미치는 !
	- **시간 독립성**: 모델의 구조와 매개변수(파라미터)가 시간에 따라 변하지 않음.
	- **지연 효과 없음**: 과거 값들이 현재 값에 영향을 미치지 않음.
	- **순간적 관계**: 변수들 간의 관계가 동일 시점에서만 고려됨.
- 반대되는 용어는 Dynamic Models임. 이는 이전 시점의 값에 영향을 미치는 것을 의미함.
	- 예를 들어서, $y_{t} = \beta_{0} + \beta_{1}x_{t} + \beta_{2}x_{t-1} + e_{t}$ 와 같은 !!
- Usually, a static model is postulated when a change in $z$ at time $t$ is believed to have an immediate effect on $y$:
$$
\triangle y_{t} = \beta_{1} \triangle z_{t}
$$
- when $\triangle u_{t} = 0$
- Static regression models are also used when we are interested in knowing the tradeoff between $y$ and $z$

## Finite Distributed Lag Models
- 시간에 따른 변수의 영향이 즉각적으로 나타나지 않고 여러 시점에 걸쳐 분산되어 나타내는 현상을 모델링 !
- In a finite distributed lag (FDL) model, we allow one or more variables to affect $y$ with a lag:
$$
y_{t} = \alpha_{0} + \delta_{0}z_{t} + \delta z_{t-1} + \delta_{2}z_{t-2} + u_{t}
$$
- which is an FDL of order two($==$ Lag의 order!!!)
- To interpret the coefficients, suppose that $z$ is a constant, equal to $c$, in all time periods before time $t$. At time $t, z$ increases by one unit to $c+1$ and then reverts to its previous level at time $t+1$. (That is, the increase in $z$ is temporary.) More precisely,
$$
\dots, z_{t-2} = c, z_{t-1} = c, z_{t} = c+1, z_{t+1} = c, z_{t+2} = c, \dots
$$
- 즉, $t$ 시기에만 Temporary Shock으로 인한 값의 변동이 생겼을 때를 통해서 effect의 크기를 볼 수 있다. 
	- 예를 들어, 코로나로 인하 딱 하루만 컨디션이 저하됐다고 가정했을 때와 같은 예시를 들 수 있다.
- To focus on the ceteris paribus effect of $z$ on $y$, we set the error term in each time period to zero $u_{t} = 0$. Then,
$$
\begin{align}
 & y_{t-1} = \alpha_{0} + \delta_{0} c + \delta_{1}c + \delta_{2}c \\
 & y_{t} = \alpha_{0} + \delta_{0}(c+1) +\delta_{1}c + \delta_{2}c \\
 & y_{t+1} = \alpha_{0} + \delta_{0}c + \delta_{1}(c+1) + \delta_{2}c \\
 & y_{t+2} = \alpha_{0} + \delta_{0}c + \delta_{1}c + \delta_{2}(c+1) \\
 & y_{t+3} = \alpha_{0} + \delta_{0} c + \delta_{1}c + \delta_{2}c
\end{align}
$$
- From the first two equations, $y_{t} - y_{t-1} = \delta_{0}$, which shows that $\delta_{0}$ is the immediate change in $y$ due to the one-unit increase in $z$ at time $t$. Usually, $\delta_{0}$ is called the **impact propensity** or **impact multiplier**.
	- 시간에 따른 지연 효과(Lag Effects) 분석
		- t+1 시점의 변화: $y_{t+1} - y_{t} = \delta_{1} -\delta_{0}$
		- t+2 시점의 변화: $y_{t+2} - y_{t+1} = \delta_{2} - \delta_{1}$
		- t+3 시점의 변화: $y_{t+3} - y_{t+2} = -\delta_{2}$
	- Total multiplier: $\delta_{0} + \delta_{1} + \delta_{2}$
- Similarly, $y_{t+1} - y_{t-1} = \delta_{1}$ is the change in $y$ one period after the temporary(→ $t$기의 temporary shock이 1기 이후에 변화가 생기) change and $y_{t+2} - y_{t-1} = \delta_{2}$ is the change in $y$ two periods after the change.
- At time $t+3$, $y$ has reverted back to its initial level: $y_{t+3} = y_{t-1}$. This is because we have assumed that only two lags of $z$.
- The lag distribution, which summarizes the dynamic effect that a temporary increase in $z$ has on $y$.![[Time_Series, Figure.05.png]]
	- The figure above implies that the largest effect is at the first lag. The lag distribution has a useful interpretation. If we standardize the initial value of $y$ at $z_{t-1} = 0$, the lag distribution traces out all subsequent values of $y$ due to a one-unit, temporary increase in $z$. → 만약 우리의 초기 값을 0으로 했을때, Shock이 들어왔을 때의 효과를 추적(trace out)할 수 있다는 의미?!
- The change in $y$ due to a permanent increase in $z$: Before time $t$, $z$ equals the constant $c$. At time $t$, $z$ increases permanently to $c+1$:
$$
\begin{align}
 & y_{t-1} = \alpha_{0} + \delta_{0}c + \delta_{1}c + \delta_{2}c \\
 & y_{t} = \alpha_{0} + \delta_{0}(c+1) + \delta_{1}c + \delta_{2}c \\
 & y_{t+1} = \alpha_{0} + \delta_{0}(c+1) + \delta_{1}(c+1) + \delta_{2}c \\
 & y_{t+2} = \alpha_{0} + \delta_{0}(c+1) + \delta_{1}(c+1) + \delta_{2}(c+1) \\
 & y_{t+3} = \alpha_{0} + \delta_{0}(c+1) + \delta_{1}(c+1) + \delta_{2}(c+1)
\end{align}
$$
- With the permanent increase in $z$, after one period, $y$ has increased by $\delta_{0} + \delta_{1}$, and after two periods, $y$ has increased by $\delta_{0}+\delta_{1}+\delta_{2}$. There are no further changes in $y$ after two periods. → 2개의 시기 이후에 추정되는 값들은 전부 같음 !!
- This shows that the sum of the coefficients on current and lagged $z$, $\delta_{0} + \delta_{1} + \delta_{2}$, is the long-runchange in $y$ given a permanent increase in $z$ and is called the long-run propensity(LPR) or long-run-multiplier. 

## Unbiasedness of OLS
- [[Time Series, 01. Basic of Regression#Assumption E.3, Zero Conditional Mean]]: for each $t$, the expected value of the error $u_t$, given the explanatory variables for all time periods, is zero. Mathematically,
$$
E(u_{t}|X) = 0, t = 1, 2, \dots, n
$$
- We require $u_{t}$ to be uncorrelated with the explanatory variables also dated at time $t$: in conditional mean terms, → 즉, $t$에 따라서 변하면 안된다는 의미이다. 
$$
E(u_{t}|x_{t1}, \dots, x_{tk}) = E(u_{t}|x_{t}) = 0
$$
- We say that the $x_{tj}$ are contemporaneously exogenous: $u_{t}$ and the explanatory variables are contemporaneously uncorrelated: → 기존 Zero mean condition에서 $i \to t$, 시간에 따라 변하는 영향을 받으면 안됨 !!
$$
Cor(x_{tj}, u_{t}) = 0, \forall j
$$
- This requires more than contemporaneous exogeneity: $u_{t}$ must be uncorrelated with $x_{sj}$, even when $s \neq t$. This is a strong sense in which the explanatory variables must be exogenous (strictly exogenous)
- The average value of $x_{t}$ is unrelated to the independent variables in all time periods.
- [[Time Series, 01. Basic of Regression#Assumption E.5, No Serial Correlation]]: Conditional on $X$, the errors in two different time periods are uncorrelated: $Cor(u_{t}, u_{s}|X) = 0, \forall t\neq s$.
- When the assumption is false, we say that the errors suffer from serial correlation, or auto-correlation, because they are correlated across time. → OLS 추정으로 한계가 생김.
	- 실제로 우리가 현실에서 이 가정이 지켜지기 쉽지 않음. 어떠한 가격의 Time Series가 있다고 했을 때, 과연 어제의 값과 오늘의 값이 독립이라고 주장하기 어려움.

## Stochastic Process
- Marginal Effects → Impulse response(충격 반응)
	- 실행변수 → 종속변수에 미치는 크기 !!!!
	- OLS에서는 $\beta: y_{i} = \alpha + \beta x_{i} + e_{i} \to \beta =\frac{\triangle y}{\triangle x}$
- Transitory Shock(일시적) vs. Permanent Shock(지속적)
- Univariate vs. Multivariate
- Stochastic Process: → 구조화된 Random
$$
\{Y_{1}, Y_{2}, Y_{3}, \dots, Y_{T}\}
$$
- Time Series data:
$$
\{y^1_{1}, y^1_{2}, y^1_{3}, \dots, y^1_{T}\}
$$
- Where $y^1_{1}$ means data among ensemble of $Y_{1}$
	- 위 두 차이는 Time Series라는 것이 Stochastic Process의 시뮬레이션 하나가 실현된 것이라고 이해를 해보자.
- Expected value of stochastic process → 여러 ensemble의 평균 !! $t$기 마다 $\mu$가 다르다는 의미임.
$$
E(Y_{t}) = \operatorname{plim}_{n \to \infty} \frac{1}{n}\sum_{i=1}^{n} y^i_{t} = \mu_{t}
$$
- Variance of stochastic process
$$
V(Y_{t}) = \operatorname{plim}_{n\to \infty} \frac{1}{n}\sum_{i=1}^{n} (y^i_{t} - \mu_{t})^2 = \sigma^2_{t}
$$

- With no restrictions to the time series,
$$
\begin{align}
E(Y_{1}) = \mu_{1} & , V(Y_{1}) = \sigma^2_{1} \\
E(Y_{2}) = \mu_{2} & , V(Y_{2}) = \sigma^2_{2} \\
E(Y_{3}) = \mu_{3} & , V(Y_{3}) = \sigma^2_{3} \\ 
&\vdots 
\end{align}
$$
- One time data → No estimation (zero df)!! 
	- 사실 t기의 값은 하나밖에 존재하지 않음 !!! → 추정이 불가능하다.

## Stationarity of a Stochastic Process
- If the $\{Y_{1}, Y_{2}, Y_{3}, \dots, Y_{T}\}$ has same expected value and variance $(\mu, \sigma^2)$, we can use time series data <mark style="background: #FFB86CA6;">instead of using ensemble</mark>
- We need ASSUMPTION !! (Same expected value and variance)
$$
\begin{align}
 & E(Y_{t}) = \mu, \forall t \\
 & E(Y_{t}) = \sigma^2, \forall t
\end{align}
$$
- First and Second Moments are time-invariant → 시간에 불변.
- Weak Stationarity / Covariance Stationarity → 오롯이 First, Second에서만 !
$$
E(Y_{t}) = \text{plim}_{n \to \infty} \frac{1}{n} \sum_{i=1}^{n} y^i_{t} = \text{plim}_{T \to \infty} \frac{1}{T} \sum_{t=1}^{T} Y_{t} = \mu
$$
- $Cov(Y_{t}, Y_{t-1})$: 시간의 차이에 영향을 바지 않음.
- 첫 번째 $\text{plim}_{n \to \infty}\frac{1}{N}\sum_{i=1}^{n}y^i_{t}$ → Ensemble
- 두 번째 $\text{plim}_{T \to \infty}\frac{1}{T}\sum_{t=1}^{T}Y_{t}$ → Time Series

## Stationarity and Ergodicity
###### Stationarity and Ergodicity, Definition 1
- {$Y_{t}$} is covariance (weakly) stationary if $E(Y_{t}) = \mu$ (independent of $t$) and $Cov(Y_{t} , Y_{t-k})$ is independent of $t$ for all $k$. 
- $\gamma(k) = Cov(Y_{t}, Y_{t-k})$  is called the **auto-covariance function** !
- $\rho(k) = \gamma(k)/\gamma(0)$ is the **auto-correlation function** !
	- $\gamma(0) = Cov(Y_{t}, Y_{t}) = Var(Y_{t})$
###### Stationarity and Ergodicity, Definition 2
- {$Y_{t}$} is strictly stationary if the joint distribution of $Y_{t}, Y_{t-1}, \dots, Y_{t-k}$ is independent of $t$ for all $k$ 
###### Stationarity and Ergodicity, Definition 3
- A stationary time series is ==ergodic== if $\gamma(k) \to 0$ as $k \to +\infty$
###### Stationarity and Ergodicity, Theorem 1
- If {$Y_{t}$} is [[#Stationarity and Ergodicity, Definition 2|strictly stationary]] and [[#Stationarity and Ergodicity, Definition 3|ergodic]] and $x_{t} = f(Y_{t}, Y_{t-1}, \dots)$ is a random variable, then $x_{t}$ is strictly stationary and ergodic.
###### Stationarity and Ergodicity, Theorem 2
- If {$Y_{t}$} is [[#Stationarity and Ergodicity, Definition 2|strictly stationary]] and [[#Stationarity and Ergodicity, Definition 3|ergodic]]and $E(|Y_{t}|) < \infty$ → 유한하면, then as $T \to \infty$, 
$$
\text{plim}\frac{1}{T}\sum_{t=1}^{T} Y_{t} \to E(Y_{t})
$$
- Thus we can [[Statistics, 07. Properties of Point Estimators and Methods of Estimation#Consistency|Consistently]] estimate parameters using time-series sample moments
###### Stationarity and Ergodicity, Theorem 3
- If {$Y_{t}$} is [[#Stationarity and Ergodicity, Definition 2|strictly stationary]] and [[#Stationarity and Ergodicity, Definition 3|ergodic]] and $E(Y_{t})^2 < \infty$ → 유한하면, then as $T \to \infty$, 
$$
\begin{gather}
\text{plim}\frac{1}{T}\sum_{t=1}^{T} Y_{t}(\equiv \hat{\mu}) \to E(Y_{t}) \\
\text{plim} \frac{1}{T}\sum_{t=1}^{T} (Y_{t} - \hat{\mu})(Y_{t-k}- \hat{\mu})(\equiv \hat{\gamma(k)}) \to \gamma (k)  \\
\text{plim} \frac{\hat{\gamma}(k)}{\hat{\gamma}(0)} (\equiv \hat{\rho(k)}) \to \rho(k)
\end{gather}
$$
## Wold Decomposition (Wold form, Wold Representation)
- **Stationary process = Deterministic component + Stochastic component**
- Marginal → Impulse response ($\psi_{t}$ 를 찾기 !)
$$
Y_{t} = \mu + e_{t} + \psi_{1}e_{t-1} + \psi_{2}e_{t-2} + \psi_{3}e_{t-3} + \cdots
$$
- **Deterministic component** (Expected value): $\mu$
- **Stochastic component** (Shock or prediction error): $e_{t} + \psi_{1}e_{t-1} + \psi_{2}e_{t-2} + \psi_{3}e_{t-3} + \cdots$
- We assume
$$
e_{t} \sim iid(0, \sigma^2)
$$
- then
$$
E(Y_{t} = \mu)
$$
- Also, we assume (finite variance)
$$
V(Y_{t}) = \sigma^2 + \psi_{1}^2\sigma^2 + \cdots = \sigma^2\sum_{i=0}^{\infty}  \psi^2_{i} < \infty
$$
- Unconditional Expectation
$$
E(Y_{t}) = \mu
$$
- Conditional Expectation 
$$
E(Y_{t}|I_{t-1}) = \mu + \psi_{1}e_{t-1} + \psi_{2}e_{t-2} + \psi_{3}e_{t-3} + \cdots
$$
- $I_{t-1}$ : Information ($t-1$기 까지) → $t-1$기 까지 random이 아님 (→ 실행됨 | 고정됨)
- **Stationary process = Unconditional Expectation + Conditional Expectation**
$$
Y_{t} - E(Y_{t}|I_{t-1}) = e_{t}, e_{t} \sim iid(0, \sigma^2)
$$

## Impulse-Response Analysis
- If we have Stationary process
$$
\frac{\partial Y_{t}}{\partial e_{t-j}} = \frac{\partial Y_{t+1}}{ \partial e_{t}} = \psi_{j}
$$
- 과거 → 미래 Shock / Why? → Covariance Stationary.

#### Approximation of Wold Representation
$$
Y_{t} = \mu + e_{t} + \psi_{1}e_{t-1} + \psi_{2}e_{t-2} + \psi_{3}e_{t-3} + \cdots + \psi_{j}e_{t-j} + \cdots
$$
- If $\psi_{1} = \phi, \psi_{2} = \phi^2, \psi_{3} = \phi^3, \dots , \psi_{j}=\phi^j, \dots$ then we only need just one parameter, $\phi$, to do impulse-response analysis 
- 즉, $\psi$를 $\phi$의 n제곱 형태로 정의를 한다면 우리는 $\phi$만 안다면 [[#Impulse-Response Analysis|Impluse-response]]의 값을 구할 수 있음 !!

## Consider one parameter model such as (if Stationary)
$$
Y_{t} = \delta + \phi Y_{t-1} + e_{t}, e_{t} \sim iid\, N(0, \sigma^2)
$$
- 위 식에서 $\hat{\phi}$를 찾기 위해서는 그냥 OLS를 해서 찾을 수는 있음 ..!
- By Back-Substitution(반복 대입), we have
$$
\begin{align}
 & Y_{t} = \delta + \phi Y_{t-1} + e_{t} \\
 & Y_{t} = \delta + \phi(\delta + \phi Y_{t-2} + e_{t-1}) \\
 & Y_{t} = \delta + \phi \delta + \phi^2Y_{t-2} + e_{t} + \phi e_{t-1} \\
 & Y_{t} = \delta + \phi \delta + \phi^2(\delta + \phi Y_{t-3} + e_{t-2}) + e_{t} + \phi e_{t-1} \\
 & Y_{t} = \delta + \phi \delta + \phi^2 \delta + \phi^3Y_{t-3} + e_{t} + \phi e_{t-1} + \phi^2 e_{t-2} \\
 & \vdots \\
 & Y_{t} = \delta(1 + \phi + \phi^2 + \cdots + \phi^{j-1}) + \phi^jY_{t-j} + e_{t} + \phi e_{t-1} + \phi^2e_{t-2} + \cdots + \phi^je_{t-j}
\end{align}
$$
- If $|\phi|<1$ and $j \to \infty$
$$
Y_{t} = \frac{\delta}{1- \phi} + e_{t} + \phi e_{t-1} + \phi^2e_{t-2} + \cdots + \phi^je_{t-j}
$$
- We call the process AR(1)
## Autoregression: AR(p)
- In a time series, $Y_{1}, Y_{2}, \dots, Y_{T}$ are jointly distributed. We are interested in the conditional mean $E(Y_{t}|\Gamma_{t-1})$ where $\Gamma_{t-1} \equiv(Y_{t-1}, Y_{t-2}, \dots, Y_{1})$ denote past history of the time series. 
	- [[#Wold Decomposition (Wold form, Wold Representation)|Wold Form]]의 Information인 $I_{t-1}$과 같은 역할을 한다고 볼 수 있음.
	- 과거의 관측값을 조건부로 제시함으로써 이에 대한 데이터는 **상수** 취급!!!
- An autoregressive (AR) model specifies that only a finite number of past lags matter 
$$
E(Y_{t}|\Gamma_{t-1}) = E(Y_{t}| Y_{t-1}, Y_{t-2}, \dots, Y_{t-k})
$$
- The most common AR model is the linear model
$$
E(Y_{t}| Y_{t-1}, Y_{t-2}, \dots, Y_{t-k}) = \delta + \sum_{j=1}^{k}\phi_{j}Y_{t-j} 
$$
- Let $e_{t} = Y_{t} - E(Y_{t}|Y_{t-1}, Y_{t-2}, \dots, Y_{t-k})$, we have the AR model
$$
Y_{t} = \delta + \sum_{j=1}^{k} \phi_{j}Y_{t-j} +e_{t}
$$
- A sequence $Y_t$ is a martingale sequence if $E(Y_{t}|\Gamma_{t-1}) = Y_{t-1}$ → AR 모형의 특수 케이스임.
	- $t$기의 예측값이 $t-1$기의 값과 같은 경우 !!
- Example of martingale sequence: A random walk sequence
$$
Y_{t} = Y_{t-1} + e_{t}
$$
- $e_{t}$ is a martingale difference sequence (MDS) if $E(e_{t}|\Gamma_{t-1}) = 0$ 
	- 오차항 $e_{t}$가 마팅게일 차이 수열이라는 것 → 과거 정보 $\Gamma_{t-1}$를 모두 고려했을 때, 오차의 평균이 0이 된다는 것을 의미함. → 모델이 과거 정보에서 추출할 수 있는 모든 예측 가능한 패턴을 이미 포착했다는 것 !!!!!
- ==The MDS property== for the regression error plays the same role in a time-series regression as does [[Time Series, 01. Basic of Regression#Assumption E.3, Zero Conditional Mean|the conditional mean-zero property]] for the regression error in a cross-section regression. In fact, it is even more important in the time-series context, as it is difficult to derive distribution theories without this property.
- The MDS property implies $E(Y_{t-k}e_{t}) = 0$ for any $k>0$.

#### AR(1) process
$$
Y_{t} = \delta + \phi Y_{t-1} + e_{t}, e_{t} \sim iid(0, \sigma^2)
$$
- Mean:  $E(Y_{t}) = \frac{\delta}{1-\phi}$
$$
\begin{gather}
 & E(Y_{t} )= E(\delta) + E(\phi Y_{t-1}) + E(e_{t}) \\
 & E(Y_{t}) = \delta + \phi E(Y_{t-1})+ 0, \quad \delta\, \&\, \phi \text{ are scalar},E(e_{t}) = 0 \\
 & E(Y_{t} ) - \phi E(Y_{t-1}) = \delta, \quad E(Y_{t}) = E(Y_{t-1}) \, \text{where Stationary} \\
 & E(Y_{t}) (1-\phi) = \delta \\
\end{gather}
$$
- Variance: $V(Y_{t}) = \frac{\sigma^2}{1-\phi^2}$
$$
\begin{gather}
Var(Y_{t}) = Var(\delta) + Var(\phi Y_{t-1}) + Var(e_{t}) \\
Var(Y_{t}) = \phi^2Var(Y_{t-1}) + \sigma^2, \quad \delta \text{ is scalar} \\
Var(Y_{t}) - \phi^2Var(Y_{t-1}) = \sigma^2, \quad Var(Y_{t}) = Var(Y_{t-1}) \, \text{where Stationary} \\
Var(Y_{t})(1-\phi^2) = \sigma^2
\end{gather}
$$
- The shock depends on $\sigma^2$ and the persistence depends on $\phi$. If $\phi = 0$ it means white noise
	- 충격(Shock)의 크기: $\sigma^2 = V(e_{t})$  → 시계열이 각 시점에서 받는 무작위 충격의 크기 → "변동성" 또는 "불확실성"을 나타낸다. 즉, 이 $\sigma^2$ 자체가 커지는 것은 $e_{t}$의 갖을 수 있는 값의 범위가 다양해지니까 무작위 충격이 심해짐 ! → 시계열 그래프가 불규칙해진다. 
	- 지속성(Persistence): $\phi$ → 자기회귀 계수 → 과거 값이 현재 값에 미치는 크기, 충격의 영향이 얼마나 오래 지속? → "기억"이나 "관성"을 나타냄. 
		- $|\phi|$가 1에 가까울수록 → 과거의 충격이 상대적으로 오랫동안 영향을 미침
		- $|\phi|$가 0에 가까울수록 → 과거의 충격이 빠르게 소멸
		- $|\phi| =0$인 경우 → 이전 관측값이 현재 값에 영향을 주지 않음 
	- $|\phi| =0 \to Y_{t} = \delta + e_{t}$: 과거 값이 현재 값을 예측하는 데 아무런 도움이 안되고, 시계열은 단순 평균$(\delta)$ 주위를 무작위로 변동함 → 백색 잡음!이라는 의미임.
- The permanent effects of the shock $(\phi= 1)$ implies the stochastic process is not stationary → $E(Y_{t}) = \delta + E(Y_{t-1})$ ... 계속 누적됨.
- [[#Stationarity and Ergodicity, Definition 1|Auto-Covariance]] 유도하기
	- ![[Time_Series, Figure.06.png]]
- [[#Stationarity and Ergodicity, Definition 1|Auto-Correlation]] 유도하기
$$
\rho(k) = \frac{\gamma(k)}{\gamma(0)} = \phi^k \frac{\gamma(0)}{\gamma(0)} = \phi^k
$$
- Then the stationary condition for AR(1) is
$$
|\phi| < 1
$$
- If $|\phi| < 1$, then $Y_{t}$ is [[Time Series, 02. Regression Analysis with Time Series Data#Stationarity and Ergodicity| Strictly Stationary and Ergodic]]
- The Equation for $Y_t$ can have an intercept, the above results are unchanged except that the mean of $Y_{t}$ can be computed from the relationship $E(Y_{t}) = \frac{\delta}{1-\phi}$
	- AR(1) Process를 Wold Form으로 변환시켰을 때를 의미함.

###### Impulse-Response Analysis In AR(1)
- We already know that the AR(1) process can be expressed by the [[#Wold Decomposition (Wold form, Wold Representation)|Wold Representation]]
	- **Deterministic component** (Expected value): $\frac{\delta}{1-\phi}$
	- **Stochastic component** (Shock or prediction error): $e_{t} + \phi e_{t-1} + \phi^2e_{t-2} + \phi^3e_{t-3} + \cdots$
$$
Y_{t} = \frac{\delta}{1-\phi} + e_{t} + \phi e_{t-1} + \phi^2e_{t-2} + \dots + \phi^je_{t-j} + \dots
$$
$$
\frac{\partial Y_{t+k}}{\partial e_{t}} = \frac{\partial Y_{t}}{\partial e_{t-k}} = \phi^k
$$
#### AR(2) Process
$$
Y_{t} = \delta + \phi_{1}Y_{t-1} + \phi_{2}Y_{t-2} + e_{t}, e_{t} \sim iid(0, \sigma^2)
$$
- Mean in Stationary: $\frac{\delta}{1 - \phi_{1} - \phi_{2}}$ 
$$
\begin{gather}
E(Y_{t}) = \delta + \phi_{1}E(Y_{t-1}) + \phi_{2}E(Y_{t-2}) + E(e_{t}) \\
E(Y_{t}) - \phi_{1}E(Y_{t-1}) - \phi_{2}E(Y_{t-2}) = \delta \\
E(Y_{t}) (1-\phi_{1} - \phi_{2}) = \delta
\end{gather}
$$
- Auto-Covariance (assuming $\delta=0$):
$$
\begin{gather}
\gamma(k) = Cov(Y_{t}, Y_{t-k}) = E(Y_{t}Y_{t-k}) \\
\gamma(k) = \phi_{1}E(Y_{t-1}Y_{t-k}) + \phi_{2}E(Y_{t-2}Y_{t-k}) = \phi_{1}\gamma(k-1) + \phi_{2}\gamma(k-2) \\
\rho(k) = \phi_{1}\rho(k-1) + \phi_{2}\rho(k-2)
\end{gather}
$$

###### Impulse-response Analysis in AR(2)
- We want to make AR(2) like AR(1) to find the stationary condition easily
- **Define**
$$
\tilde{Y}_{t} \equiv 
\begin{pmatrix}
Y_{t} \\
Y_{t-1}
\end{pmatrix}, \tilde{Y}_{t-1} \equiv
\begin{pmatrix}
Y_{t-1} \\
Y_{t-2}
\end{pmatrix}
$$
- Consider the AR(2) process such as 
$$
\begin{align}
 & Y_{t} = \delta + \phi_{1}Y_{t-1} + \phi_{2}Y_{t-2} + e_{t}, \text{if }\delta =0 \\
 & Y_{t-1} = Y_{t-1}  \\
& \tilde{Y}_{t} = \begin{pmatrix}
Y_{t} \\
Y_{t-1}
\end{pmatrix} =  \begin{pmatrix}
 \phi_{1},  & \phi_{2} \\
 1, & 0
\end{pmatrix} \begin{pmatrix}
Y_{t-1} \\
Y_{t-2}
\end{pmatrix} + \begin{pmatrix}
e_{t} \\
0
\end{pmatrix}  \\
 & \Rightarrow \tilde{Y}_{t} = F \cdot \tilde{Y}_{t-1} + e_{t}
\end{align}
$$
- **State-space Form**
- With the State-space form of AR(2) the Wold Representation is
$$
\tilde{Y}_{t} = \tilde{e}_{t} + F \cdot \tilde{e}_{t-1} + F^2 \cdot \tilde{e_{t-1}} + \cdots + F^k \cdot \tilde{e}_{t-k} + \cdots
$$
- The Impulse-response means
$$
\frac{\partial Y_{t+k}}{\partial e_{t}} = \frac{\partial Y_{t}}{\partial e_{t-k}} = (1, 1) \text{ element of } F^k
$$
- Because
$$
\begin{pmatrix}
Y_{t} \\
Y_{t-1}
\end{pmatrix} = \begin{pmatrix}
e_{t} \\
0
\end{pmatrix} +\begin{pmatrix}
\phi_{1} & \phi_{2} \\
1 & 0 
\end{pmatrix}\cdot \begin{pmatrix}
e_{t-1} \\
0
\end{pmatrix} + \begin{pmatrix}
\phi_{1} & \phi_{2} \\
1 & 0
\end{pmatrix}^2 \cdot \begin{pmatrix}
e_{t-1} \\
0
\end{pmatrix} + \cdots + \begin{pmatrix}
\phi_{1} & \phi_{2}  \\
1 & 0
\end{pmatrix}^k \cdot \begin{pmatrix}
e_{t-k} \\
0
\end{pmatrix} + \cdots
$$

#### Characteristic root: Eigen Value 
- Let $x(x\neq 0)$, $F$ be a 2 by 1 non-zero, 2 by 2 vector and $\lambda$ be a scalar,
$$
F \cdot x = \lambda \cdot x \Longleftrightarrow \begin{pmatrix}
f(11) & f(12) \\
f(21) & f(22)
\end{pmatrix} \begin{pmatrix}
x_{1} \\
x_{2}
\end{pmatrix} = \lambda \cdot \begin{pmatrix}
x_{1} \\
x_{2}
\end{pmatrix}
$$
$$
(F - \lambda I_{2})x = 0
$$
- Since $x\neq 0$, the equations are singular 
$$
|F - \lambda I_{2}| = 0
$$
$$
\left| \begin{pmatrix}
f(11) & f(12) \\
f(21) & f(22)
\end{pmatrix} - \begin{pmatrix}
\lambda & 0 \\
0 & \lambda
\end{pmatrix}
\right| = 0 \Longleftrightarrow \left| 
\begin{pmatrix}
f_{11} - \lambda & f_{12} \\
f_{21}  &  f_{22}-\lambda
\end{pmatrix}
\right| = 0
$$
$$
\begin{gather}
(f_{11} - \lambda)(f_{22}- \lambda) - f_{12}f_{21} = 0 \\
\lambda^2 + a\lambda + b =0
\end{gather}
$$

- where $a\equiv-(f_{11}+f_{22})$ and $b \equiv f_{11}f_{22} - f_{12}f_{21}$. The last equation is the characteristic equation and corresponding roots are characteristic roots: $\lambda_{1}, \lambda_{2}$
- The characteristic vectors are
$$
\begin{align}
 & \lambda = \lambda_{1} \Rightarrow F\cdot x_{1} = \lambda_{1} \cdot x \\
 & \lambda = \lambda_{2} \Rightarrow F\cdot x_{2} = \lambda_{2} \cdot x \\
\end{align}
$$
- To find finite roots we need normalization
$$
x'_{1}x_{1} = 1, x_{2}'x_{2}=1
$$
- Then we have two roots of $x_{1}$ and two roots of $x_{2}$ we call the vectors with the roots the characteristic vectors
- Now consider the matrix of $F$, with the characteristic vectors
$$
F \begin{pmatrix}
x_{1} & x_{2}
\end{pmatrix} = \begin{pmatrix}
x_{1} & x_{2}
\end{pmatrix} \begin{pmatrix}
\lambda_{1} & 0 \\
0 & \lambda_{2}
\end{pmatrix} \Longleftrightarrow F \cdot C = C \cdot \Lambda
$$
$$
F^k = C \cdot \Lambda^k \cdot C^{-1} = \begin{pmatrix}
c_{11} & c_{12} \\
c_{21} & c_{22}
\end{pmatrix} \cdot \begin{pmatrix}
\lambda_{1}^k &  0 \\
0 & \lambda_{2}^k
\end{pmatrix} \cdot \begin{pmatrix}
c_{11}^*  & c_{12}^* \\
c_{21}^* & c_{22}^*
\end{pmatrix}
$$
- Finally in the impulse-response we have
$$
\frac{\partial Y_{t+k}}{\partial e_{t}} = (1, 1) \text{ element of }F^k = c_{11}c_{11}^* \lambda_{1}^k + c_{12}c_{12}^*\lambda_{2}^j \equiv W_{1}\lambda_{1}^k + W_{2}\lambda_{2}^k
$$
- As $k$ goes to $\infty$ the stationary condition means the auto-correlation closes to zero. Since $W_{1}+W_{2} =1$, the stationary condition for AR(2) is 
$$
|\lambda_{1}| < 1, |\lambda_{2}| < 1
$$
- This is the condition of $\lim_{ k \to \infty }\rho(k)=0$
###### Expansion to the AR(p)
- If we have AR(3) process then the stationary conditions are
$$
|\lambda_{1}| <1, |\lambda_{2}| <1, |\lambda_{3}|<1
$$


## Moving-Average: MA(q)
$$
Y_{t} = \mu + e_{t} + \theta_{1}e_{t-1} + \theta_{2}e_{t-2} + \cdots + \theta_{q}e_{t-q}
$$
- MA(1): $Y_{t} = \mu + e_{t}+\theta e_{t-1}$.
- $q \to \infty$, MA(q) = [[#Wold Decomposition (Wold form, Wold Representation)|Wold form]]
- This process explains the current $Y$ only using the shocks before time $q$
	- 즉, MA($q$) 프로세스는 현재의 $Y_{t}$ 값이 과거 $q$시기 동안의 충격(shock)에 의해 어떻게 영향을 받는지를 모델링함. 
###### Example, MA(2) in Stock market!!
- 주식 시장의 일일 수익률 MA(2) 모델링 가정
$$
R_{t} = \mu + e_{t} + 0.7e_{t-1} - 0.3e_{t-2}
$$
- 이것은 다음을 의미함:
	- 오늘의 수익률 $R_{t}$는 상수 평균 $\mu$를 중심으로 변동
	- 오늘 발생한 뉴스나 사건(충격$e_t$)이 즉시 완전히 반영.
	- 어제 발생한 뉴스 (충격 $e_{t-1}$)는 여전히 70% 영향력
	- 그제 발생한 뉴스 (충격 $e_{t-2}$)는 30%의 영향력을 가지지지만 역방향 (reversal effect!)
	- 3일 이상 지난 뉴스는 → 직접적인 충격이 없음.
#### MA(1) process
$$
Y_{t} = \mu + e_{t} + \theta e_{t-1}, e_{t} \sim iid(0, \sigma^2)
$$
- $q\to \infty$ : [[#Wold Decomposition (Wold form, Wold Representation)|Wold form]]
- Mean (no need stationarity):
$$
E(Y_{t}) = \mu
$$
- Variance (no need stationarity):
$$
Var(Y_{t}) = (1 + \theta^2)\sigma^2
$$
- Auto-covariance (assuming $\mu=0$)
$$
\begin{align}
 & \gamma(1) = \theta \sigma^2  \\
 & \gamma(2) = 0
\end{align}
$$
- <img src='Pasted image 20250416194153.png', width='400', height='400'>
- 
## ARMA(p, q)
$$
Y_{t} = \delta + \phi_{1}Y_{t-1}+\phi_{2}Y_{t-2} + \cdots + \phi_{p}Y_{t-p} + e_{t} + \theta_{1}e_{t-1} + \theta_{2} e_{t-2} + \cdots + \theta_{q}e_{t-q}
$$
- Back substitution can make the Wold representation with ARMA(p, q)
- 이는 AR 모델과 MA 모델의 장점을 결합하여 더 효율적이고 유연한 시계열 모델링을 가능하게 함.
	- 모델링 효율성: 복잡한 시계열 데이터를 적절히 모델링 하기 위해? 
	- 다양한 시계열 패턴 포착: AR(→ 상관관계), MA(→ 충격의 일시적 효과와 감쇠 패턴)
	- 통계적 효율성: AR과 MA 보다 더 작은 오차를 가짐 → AIC(Akaike Information Criterion)나 BIC(Bayesian Information Criterion)와 같은 정보 기준으로 평가
	- 이론적 완전성: ARMA 모델은 정상성 조건을 만족하는 시계열에 대한 통합적 표현 제공 → Wold 정리에 따라서, 어떤 약정상 시계열도 결국 무한한 MA 표현으로 나타낼 수 있으며, ARMA 모델은 이를 유한한 매개변수로 근사화.
