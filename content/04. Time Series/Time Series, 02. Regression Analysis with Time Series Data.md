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
## Time Series Dta
- A time series data set consists of observations on a variable or several variables over time. → 여러 시점의 관측된 집합 !
- Because past events can influence future events and lags(과거값) in behavior are prevalent in the social sciences, time is an important dimension in a time series data set.
- A key feature of time series data that makes them more difficult to analyze than cross-sectional data is that observations can rarely, if ever, be assumed to be independent across time. Time series are related, often strongly related, to their recent histories.
- Another feature of time series data that can require special attention is the data frequency at which the data are collected → 주기성을 요구할 수 있다? 
	- $lag$ → Frequency (A와 B의 관측 시점이 매우 다른데!) 주기를 보일 수 있다?
- The most common frequencies are daily, weekly, monthly, quarterly, and annually.
	- seasonal pattern: 계절성 패턴. → Time Series Analysis에서 중요한 요소임.
	- 
	- 