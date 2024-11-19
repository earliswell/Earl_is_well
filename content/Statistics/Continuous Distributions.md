### DEFINITION
Let $Y$ denote any random variable. The distribution function of Y, denoted by $F(y)$, is such that $F(y) = P(Y \leq y)$ for $-\infty < y < \infty$  

- 우리는 Continuous라는 연속적인 값에서 갖는 분포에 대해서 알아보려고 한다. 이전 [[Discrete Distributions]]는 특정 횟수 혹은 셀 수 있는 무언가를 의미했지만, Countinuous에서는 어떠한 범위 내에 확률을 추출한다고 생각하겠다.
- 우리가 가장 주의해야할 점은 Continuous Distributions에서는 **어떤 상수 값에서의 확률**은 항상 0이다.$P(X=x) = 0$
- 확률은 항상 **구간**의 넓이로 계산된다. $P(a \leq X \leq b) = \int_a^b f(x)dx$

#### DEFINITION 4.5
The expected value of continuous random variable $Y$ is$$E(Y) = \int_{-\infty}^\infty yf(y)dy$$

#### 

## [[Uniform]]
- Probability Function$$f(y) = \frac{1}{\theta_2 - \theta_1};  \\\ \theta_1 \leq y \leq \theta_2$$
- Mean : $$\frac{\theta_1 + \theta_2}{2}$$
- Variance$$\frac{(\theta_2 - \theta_1)^2}{12}$$
## [[Normal]]
- Probability Function$$f(y) = \frac{1}{\sigma \sqrt{2\pi}}\exp\left[-\frac{1}{2\sigma^2}(y-\mu)^2\right], \\\ -\infty\leq y\leq \infty$$
- Mean : $\mu$
- Variance : $\sigma^2$

## [[Exponential]]
- Probability Function$$f(y) = \frac{1}{\beta}e^{-y/\beta}; \\\ \beta >0, \\\ 0\leq y \leq \infty$$
- Mean : $\beta$
- Variance : $\beta^2$

## [[Gamma]]
- Probability Function$$f(y) = \left[\frac{1}{\Gamma(\alpha)\beta^{\alpha}}\right]y^{\alpha-1}e^{-y/\beta}; \\\ 0 \leq y \leq \infty$$
- Mean : $\alpha\beta$
- Variance : $\alpha\beta^2$

## [[Chi-square]]
- Probability Function$$f(y) = \frac{y^{(v/2)-1}e^{-y/2}}{2^{v/2}\Gamma(v/2)}; \ y^2 > 0$$
- Mean : $v$
- Variance : $2v$

## [[Beta]]
- Probability Function$$f(y) = \left[\frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)}\right]y^{\alpha-1}(1-y)^{\beta-1}; \ 0<y<1$$
- Mean $$\frac{\alpha}{\alpha+\beta}$$
- Variance $$\frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$$