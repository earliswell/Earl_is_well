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
- ![[Time_Series. Figure.03.png]]
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
- ![[Pasted image 20250415130759.png]]

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
- 즉, $t$ 시기에만 잠깐 Shock으로 인한 값의 변동이 생겼을 때를 통해서 effect의 크기를 볼 수 있다. 
	- 예를 들어, 코로나로 인하 딱 하루만 컨디션이 저하됐다고 가정했을 때와 같은 예시를 들 수 있다.
- To focus on the ceteris paribus effect of $z$ on $y$, we set the error term in each time period to zero. Then
$$
\begin{align}
 & y_{t-1} = \alpha_{0} + \delta_{0} c + \delta_{1}c + \delta_{2}c \\
 & y_{t} = \alpha_{0} + \delta_{0}(c+1) +\delta_{1}c + \delta_{2}c \\
 & y_{t+1} = \alpha_{0} + \delta_{0}c + \delta_{1}(c+1) + \delta_{2}c \\
 & y_{t+2} = \alpha_{0} + \delta_{0}c + \delta_{1}c + \delta_{2}(c+1) \\
 & y_{t+3} = \alpha_{0} + \delta_{0} c + \delta_{1}c + \delta_{2}c
\end{align}
$$
- From the first two equations, $y_{t} - y_{t-1} = \delta_{0}$, which shows that $\delta_{0}$ is the immediate change in $y$ due to the one-unit increase in $z$ at time $t$. Usually, $\delta_{0}$ is called the impact propensity or impact multiplier.