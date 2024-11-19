### DEFINITION
Let $Y$ denote any random variable. The distribution function of Y, denoted by $F(y)$, is such that $F(y) = P(Y \leq y)$ for $-\infty < y < \infty$  

- 우리는 Continuous라는 연속적인 값에서 갖는 분포에 대해서 알아보려고 한다. 이전 [[Discrete Distributions]]는 특정 횟수 혹은 셀 수 있는 무언가를 의미했지만, Countinuous에서는 어떠한 범위 내에 확률을 추출한다고 생각하겠다.
- 우리가 가장 주의해야할 점은 Continuous Distributions에서는 **어떤 상수 값에서의 확률**은 항상 0이다.$P(X=x) = 0$
- 확률은 항상 **구간**의 넓이로 계산된다. $P(a \leq X \leq b) = \int_a^b f(x)dx$

### The Probability Distribution for a Continuous Random Variable

#### THEOREM4.1, Properties of a Distribution Function
If $F(y)$ is a distribution function, then
1. $F(-\infty) \equiv \lim_{y\to-\infty} F(y) = 0$ 
2. $F(\infty) \equiv \lim_{y\to\infty} F(y)= 1$
3. $F(y)$ is a nondecreasing function of y.
여기에서 $F(y)$는 누적분포함수 (CDF: Cumulative Distribution Function)을 의미한다.

우리는 CDF와 PDF의 관계를 알아야 하는데
연속함수에서 확률 분포 함수 (PDF: Probability Distribution Function)은 CDF의 미분 값, 즉 한 값에서의 기울기를 의미, 변동량, First Difference라고 이해를 하면 좋겠다. 

$\to$ 따라서 PDF와 CDF의 관계는
1. CDF를 미분하면 PDF
2. PDF를 적분하면 CDF 

#### THEOREM 4.3
If the random variable $Y$ has density function $f(y)$ and $a < b$, then the probability that $Y$ falls in the interval $[a, b]$  is $$P(a \leq Y \leq b) = \int_a^b f(y)dy$$![[Pasted image 20241119111535.png]]


### Expected Values for Continuous Random Variables

#### DEFINITION 4.5
The expected value of continuous random variable $Y$ is$$E(Y) = \int_{-\infty}^\infty yf(y)dy$$

-  $E(g(Y) = \int_{-\infty}^\infty g(y)f(y)dy$
1. $E(c) = c$
2. $E[cg(Y)] = cE[g(Y)]$
3. $E[g_1(Y)+g_2(Y)+\dots+g_k(Y)] = E[g_1(Y)] + E[g_2(Y)] + \dots + E[g_k(Y)]$

If $g(Y) = (Y - \mu)^2$라면 분산을 구하는 것과 같음.
- $V(Y) = E[(Y-\mu)^2] = E(Y^2) - \mu^2$


#### DEFINITION 4.7
The constants that determine the specific form of a density function are called **Parameters** of the density function
- 위 정의는 parameters(매개변수)라는 것은 확률밀도함수 (CDF)의 모양을 결정하는 상수값이라고 생각하면 됨.
- 일례로 평균이 $\mu$를 따르고 분산이 $\sigma^2$를 따르는 분포를 정규분포([[Normal Distribution]])을 따름.
- 여기에서 우리는 $\mu$와 $\sigma^2$를 Parameters라고 부른다.

## [[Uniform Distribution]]
- Parameters : $\theta_1, \theta_2$ 
- Probability Function$$f(y) = \frac{1}{\theta_2 - \theta_1};  \\\ \theta_1 \leq y \leq \theta_2$$
- Mean : $$\frac{\theta_1 + \theta_2}{2}$$
- Variance$$\frac{(\theta_2 - \theta_1)^2}{12}$$
## [[Normal Distribution]]
- parameters : $\mu, \sigma$ 
- Probability Function$$f(y) = \frac{1}{\sigma \sqrt{2\pi}}\exp\left[-\frac{1}{2\sigma^2}(y-\mu)^2\right], \\\ -\infty\leq y\leq \infty$$
- Mean : $\mu$
- Variance : $\sigma^2$
- 우리는 $\frac{x - \mu}{\sigma}$ 를 통해서 평균 0, 분산 1로 만들 수 있음. 우리는 이를 표준화(Standardization)이라고 부르기도 함.
## [[Exponential Distribution]]
- Probability Function$$f(y) = \frac{1}{\beta}e^{-y/\beta}; \\\ \beta >0, \\\ 0\leq y \leq \infty$$
- Mean : $\beta$
- Variance : $\beta^2$

## [[Gamma Distribution]]
- Probability Function$$f(y) = \left[\frac{1}{\Gamma(\alpha)\beta^{\alpha}}\right]y^{\alpha-1}e^{-y/\beta}; \\\ 0 \leq y \leq \infty$$
- Mean : $\alpha\beta$
- Variance : $\alpha\beta^2$

## [[Chi-square Distribution]]
- Probability Function$$f(y) = \frac{y^{(v/2)-1}e^{-y/2}}{2^{v/2}\Gamma(v/2)}; \ y^2 > 0$$
- Mean : $v$
- Variance : $2v$

## [[Beta Distribution]]
- Probability Function$$f(y) = \left[\frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)}\right]y^{\alpha-1}(1-y)^{\beta-1}; \ 0<y<1$$
- Mean $$\frac{\alpha}{\alpha+\beta}$$
- Variance $$\frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$$