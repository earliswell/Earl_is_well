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
- parameters : $\mu, \sigma$ ![[Pasted image 20241121103413.png]]
- Probability Function$$f(y) = \frac{1}{\sigma \sqrt{2\pi}}\exp\left[-\frac{1}{2\sigma^2}(y-\mu)^2\right], \\\ -\infty\leq y\leq \infty$$
- Mean : $\mu$
- Variance : $\sigma^2$
- 우리는 $\frac{x - \mu}{\sigma}$ 를 통해서 평균 0, 분산 1로 만들 수 있음. 우리는 이를 표준화(Standardization)이라고 부르기도 함.
## [[Gamma Distribution]]
- 우선 분포는 Symmetric(대칭)이 아니다. 감마 분포를 이해하기 위해서는 우리는 Event에 대한 두 가지 경우를 이해할 필요가 있는데, 
	 1. 대기 시간 (Waiting Time) : 한 이벤트에서 다음 이벤트까지 걸리는 시간
		 1. 엔진 고장 간격
		 2. 계산대 도착 간격
	 2. 서비스 시간 (Service Time) : 특정 작업을 완료하는 데 걸리는 시간
		 1. 정비 점검 완료 시간
- 이러한 분포적 특성으로 인해 음수가 될 수 없다 ($\to$ 시간은 항상 양수이기 때문에) 또한 대부분의 경우 평균 근처에 몰려있고, 극단적으로 긴 시간은 상대적으로 드물게 발생한다.
- 따라서 우리는 "어떤 이벤트가 발생할 때까지의 시간" 또는 "이벤트들 사이의 시간 간격"의 분포에 대해 이야기를 한다.![[Pasted image 20241121104720.png]]
- Probability Function$$f(y) = \left[\frac{1}{\Gamma(\alpha)\beta^{\alpha}}\right]y^{\alpha-1}e^{-y/\beta}; \\\ 0 \leq y \leq \infty$$
- Gamma Function $\Gamma(\alpha) = \int_{0}^{\infty} y^{\alpha-1}e^{-y}dy$
	- $\Gamma(1) = 1$
	- $\Gamma(\alpha) = (\alpha-1)\Gamma(\alpha-1) \quad \text{where} \, \alpha > 1$
- Mean : $\alpha\beta$
- Variance : $\alpha\beta^2$
#### DEFINITION 4.10
==Let v be a positive integer==. A random variable Y is said to have a [[Continuous Distributions#Chi-square Distribution]] with v degrees of freedom if and only if Y is gamma-distributed random variable with parameters $\alpha = v/2$ and $\beta =2$

## [[Chi-square Distribution]]
- Chi-square Distribution이라는 것은 표준정규분포를 따르는 확률변수들의 제곱합 분포이다.$$Q = \sum_iZ_i^2 \sim \chi^2(v)  $$
- 이는 자유도(v)가 핵심 파라미터이며, 항상 양의 값을 갖는 분포이다.
- 일반적인 대기시간, 생존시간 모델링에 사용되며 
- Probability Function$$f(y) = \frac{y^{(v/2)-1}e^{-y/2}}{2^{v/2}\Gamma(v/2)}; \ y^2 > 0$$
- Mean : $v$
- Variance : $2v$

## [[Exponential Distribution]]
- Gamma 분포에서 $\alpha=1$인 경우이다.
	- $\Gamma(1)=1$
- Probability Function$$f(y) = \frac{1}{\beta}e^{-y/\beta}; \\\ \beta >0, \\\ 0\leq y \leq \infty$$
- Mean : $\beta$
- Variance : $\beta^2$
- 주요한 특성으로는 "Memory less property"가 있다.
	- $P(X>s +t | X>s) = P(x>t)$ 를 의미하며, 즉 s라는 시간이 이미 흘렀을 지라도 분포 내에 같은 확률로 존재한다.
	- 예를 들어, 한 고객이 들어온 후 다음 고객이 들어올 때까지의 대기 시간을 예시로 둔다면 시간 내에 손님이 올 확률은 어디에서나 동일하다는 의미이다.
- 또한, [[Discrete Distributions#Poisson]]과 헷갈릴 수 있다.
	- 위 예를 다시 인용하자면
	- Poisson : 1시간 동안 상점에 들어오는 고객 수  (즉 이산적인 값, Discrete)
	- Exponential : 한 고객이 들어온 후 다음 고객이 들어올 때까지의 대기 시간 (연속적인 값, Continuous)


## [[Beta Distribution]]![[Pasted image 20241121141442.png]]
- y의 값이 0과 1사이의 값을 갖는다.
- 비율/비중의 분포를 모델링할 때 사용하며, 확률에 대한 확률분포를 나타낼 때? 
	- 마치 $\alpha$와 $\beta$를 "성공"과 "실패"의 관측 횟수로 해석할 수 있음.
	- 
- Probability Function$$f(y) = \left[\frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)}\right]y^{\alpha-1}(1-y)^{\beta-1}; \ 0<y<1$$
- Beta Function : $B(\alpha, \beta) = \int_0^1 y^{\alpha-1}(1-y)^{\beta-1} dy = \frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha + \beta)}$ 
- Mean $$\frac{\alpha}{\alpha+\beta}$$
- Variance $$\frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$$

#### Binomial과 Beta분포의 관계
- Binomial 분포 : n번의 시행에서 성공 횟수의 분포
	- $X \sim \text{Bin}(n, p)$
	- p ; 성공 확률
- Beta 분포 : 성공 확률 p 자체의 불확실성을 표현
	- $p \sim \text{Beta}(\alpha, \beta)$
	- p ; 확률 변수
- Bayesian에서 Beta분포에 대한 해석이 있는데 이는 추후 공부를 하도록 하자 !
	- 미리 약간의 관계성
	- Prior (사전확률): $p \sim \text{Beta}(\alpha, \beta)$
	- Likelihood (가능도): $X|p \sim \text{Bin}(n, p)$
	- Posterior (사후확률): $p|X \sim \text{Beta}(\alpha+x, \beta+n-x)$
- 예를 들어, 동전 던지기의 경우 
	- 동전의 앞면이 나올 확률 p를 모를 때
	- $\text{Beta}(1, 1)$로 시작 (정보가 없을 때의 사전 분포)
	- 10번 던져서 6번의 앞면이 나왔다면 사후분포는 $\text{Beta}(7, 5)$ 

## Some General Comments
- The purpose of probabilistic model is to provide the mechanism for making inferences about a population based on information contained in sample.
- 즉, 확률적 모델의 목적은 샘플로부터 모집단을 합리적으로 추론하기 위함이다.
- A good model is one that yields good inferences about the population of interest.