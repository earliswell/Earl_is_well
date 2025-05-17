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

