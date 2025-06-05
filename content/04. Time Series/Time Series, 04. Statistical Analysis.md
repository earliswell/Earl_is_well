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