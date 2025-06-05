---
title: 04. Statistical Analysis
draft: false
tags:
  - example-tag
---
## 4.1 Fitting ARIMA models: The Box-jenkins approach
- 이 과정은 ARIMA(p, d, q)에서 p와 q를 찾는 것이다. → Test(white noise), PACF
- The Box-Jenkins approach to fitting ARIMA models can be divided into three parts:
	- Identification;
	- Estimation;
	- Verification.
#### 4.1.1 Identification
- This refers to initial preprocessing of the data to make it stationary, and choosing plausible values of $p$ and $q$ (which can of course be adjusted as model fitting progresses).
- To assess whether the data come from a stationary process we can
	- look at the data → plot 그리기!
	- consider transforming it (e.g. by taking logs) → pattern을 보기 조금 더 쉬워짐.
	- consider if we need to difference the series to make it stationary. → ==Unit root test==
- If this model is non stationary, then try differencing the series, and maybe a second time if necessary. (==In practice it is rare to go beyond d=2 stages of differencing.==)
#### 4.1.2 Estimation: AR processes
- For the AR($p$) process
$$
X_{t} = \sum_{i=1}^{p} \alpha_{i}X_{t-i} + \epsilon_{t}
$$
- 