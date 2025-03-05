---
title: 08. Hypothesis Testing
draft: false
tags:
  - test
  - error
  - Hypothesis
---

## Introduction
- 통계의 목적은 관측된 데이터를 통해서 알려지지 않은 모집단을 잘 추론하는 것이다. 또한, 우리가 통계를 하는 이유는 가설의 유의성을 파악하기 위함에 있다. 귀무가설과 대립가설을 통해서 유의 n% 이내의 유의성을 증명하는 것이 이 파트의 중요 부분이라고 생각된다. 따라서, 여러 컨셉에 맞게 우리는 가설 검정을 할 것이고 그에 맞는 Test의 방법에 대해서 심도있게 공부를 한다고 생각하자.
	
**The Elements of a Statistical Test**
1. Null Hypothesis, $H_{0}$
2. Alternative Hypothesis, $H_{\alpha}$
3. Test statistic
4. Rejection region

- 또한 One-tail, Two-tails 기법이 있지만 Two-way를 집중적으로 알면 우리는 가설 검정에 대해서 더욱 이해가 잘 될 것이라 기대된다.
- 우리가 주장하고 싶은 바는 Alternative Hypothesis에 있다. 즉 우리의 Test Statistic의 값에 따라서 우리의 값이 유의미한지를 보일 수 있다. 다른 예시를 들면, 우리의 평균 추정치가 0인지 즉 Null Hypothesis를 $H_{0} : \theta = 0$으로 설정했을 때 우리의 추정 계수가 0이라면 Effect가 없다는 의미를 지니고 있다. 이러한 과정 속에서 가설 검정을 공부하면 좋은 방법이 될 듯 싶다.
## Common Large-Sample Tests
![[FIgure 10.3.png]]
1. Null Hypothesis, $H_{0} : \theta = \hat{\theta}$ 
2. Alternative Hypothesis, $H_{\alpha}: \theta \neq \hat{\theta}$
3. Test statistic $Z =\frac{\hat{\theta}-\theta}{\sigma_{\hat{\theta}}}$
4. Rejection region 
$$
Z = \frac{\text{estimator for the parameter} - \text{value for the parameter given by} \,H_{0}}{\text{standard error of the estimator}}
$$

**Large Sample $\alpha$-Level Hypothesis Tests**
$$ 
\begin{align}  \\
&H_{0} : \theta=\theta_{0} \\
&H_{\alpha} : \begin{cases} \theta > \theta_{0} \quad &(\text{upper-tail alternative.}) \\
\theta < \theta_{0} \quad &(\text{lower-tail alternative.}) \\
\theta \neq \theta_{0} \quad &(\text{two-tail alternative.})
\end{cases}  \\
&\text{Test Statistic} : Z=\frac{\hat{\theta}-\theta}{\sigma_{\hat{\theta}}} \\
&\text{Rejection region} : \begin{cases} {z>z_{\alpha}} \quad &(\text{upper-tail RR.}) \\
{z < -z_{\alpha}} \quad &(\text{lower-tail RR.}) \\
{|z| > z_{\alpha/2}} \quad &(\text{two-tailed RR.})
\end{cases} 
\end{align}
$$
## Relationships Between Hypothesis-Testing Procedures and Confidence Intervals
- 우리는 이제 어느 구간에서 우리의 Null hypothesis를 기각할 것인가에 대한구체적인 수가 필요할 것이다. 우선, 우리는 $\hat{\theta} \pm z_{\alpha/2}\sigma_{\hat{\theta}}$를 기억하자. 우리의 Estimator와 Test statistic의 범위를 통해 기각을 할 것이고, 이 범위에 벗어나면 우리는 우리의 Null Hypothesis를 기각할 수 있다.
$$
\hat{\theta}-z_{\alpha/2}\sigma_{\hat{\theta}} \leq \theta_{0} \leq \hat{\theta}+z_{\alpha/2}\sigma_{\hat{\theta}}
$$
- For this reason, we usually do not accept the null hypothesis that $\theta=\theta_{0}$, even if the value $\theta_{0}$ falls inside our confidence interval.

## Another Way to Report the Results of a Statistical Test: Attained Significance Levels, or $p$-values

- $\alpha$ of a type 1 error is often called the significance level, 즉 $\alpha$는 1종오류, 신뢰구간이라고 종종 불린다.

**DEFINITION 10.2**
- If $W$ is a test statistic, the p-value, or attained significance level, is the smallest level of significance $\alpha$ for which the observed data indicate that the null hypothesis should be rejected.

## Small-Sample Hypothesis Testing for $\mu$ and $\mu_{1}-\mu_{2}$
- Small Sample에서는 T-test를 통해서 우리의 가설검정을 진행한다. 
	- 참고 : [[Statistics, 06. Estimation#Small-Sample Confidence Intervals for $ mu$ and $ mu_1 - mu_2$]]

$$
T = \frac{\bar{Y}-\mu_{0}}{S/\sqrt{ n }}
$$
- Because the $t$ distribution is symmetric and mound-shaped, the rejection region for a small-sample test of the hypothesis $H_{0}:\mu=\mu_{0}$ must be located in the tails of the $t$ distribution and be determined in a manner similar to that used with the large-sample $Z$ statistic.

**A Small-Sample Test for $\mu$**
- Assumptions: $Y_{1}, Y_{2}, \dots, Y_{n}$ constitute a random sample from a normal distribution with $E(Y_{i})=\mu$
$$ 
\begin{align}  \\
&H_{0} : \mu=\mu_{0} \\
&H_{\alpha} : \begin{cases} \mu > \mu_{0} \quad &(\text{upper-tail alternative.}) \\
\mu < \mu_{0} \quad &(\text{lower-tail alternative.}) \\
\mu \neq \mu_{0} \quad &(\text{two-tail alternative.})
\end{cases}  \\
&\text{Test Statistic} : T=\frac{\bar{Y}-\mu_{0}}{S/\sqrt{ n }} \\
&\text{Rejection region} : \begin{cases} {t>t_{\alpha}} \quad &(\text{upper-tail RR.}) \\
{t < -t_{\alpha}} \quad &(\text{lower-tail RR.}) \\
{|t| > t_{\alpha/2}} \quad &(\text{two-tailed RR.})
\end{cases} 
\end{align}
$$
- with $v = n-1$ df. 통상 2보다 큰 값을 가지면 귀무가설을 기각한다. 
- 또한 우리가 Two-Sample Test를 할 때는 우리의 표준편차를 weighting을 통해서 조정해줘야한다.
$$
S_{p}^2 = \frac{(n_{1}-1)S_{1}^2+(n_{2}-1)S_{2}^2}{n_{1}+n_{2}-2}
$$
- is the pooled estimator for $\sigma^2$, then
$$
T = \frac{(\bar{Y}_{1}-\bar{Y}_{2}) - (\mu_{1} - \mu_{2})}{S_{p}\sqrt{ \frac{1}{n_{1}} + \frac{1}{n_{2}} }}
$$

**Small-Sample Tests for Comparing Two Population Means**
- Assumptions: Independent samples from normal distributions with $\sigma_{1}^2 = \sigma_{2}^2$
$$ 
\begin{align}  \\
&H_{0} : \mu_{1}-\mu_{2} =D_{0}\\
&H_{\alpha} : \begin{cases} \mu_{1}-\mu_{2} >D_{0} \quad &(\text{upper-tail alternative.}) \\
\mu_{1}-\mu_{2} < D_{0}\quad &(\text{lower-tail alternative.}) \\
\mu_{1}-\mu_{2} \neq D_{0} \quad &(\text{two-tail alternative.})
\end{cases}  \\
&\text{Test Statistic} : T = \frac{(\bar{Y}_{1}-\bar{Y}_{2}) - (\mu_{1} - \mu_{2})}{S_{p}\sqrt{ \frac{1}{n_{1}} + \frac{1}{n_{2}} }}, \, \text{where} \, S_{p}^2 = \frac{(n_{1}-1)S_{1}^2+(n_{2}-1)S_{2}^2}{n_{1}+n_{2}-2} \\
&\text{Rejection region} : \begin{cases} {t>t_{\alpha}} \quad &(\text{upper-tail RR.}) \\
{t < -t_{\alpha}} \quad &(\text{lower-tail RR.}) \\
{|t| > t_{\alpha/2}} \quad &(\text{two-tailed RR.})
\end{cases} 
\end{align}
$$
- degrees of freedom $v=n_{1}+n_{2}-2$.
- 위 테스트로 우리가 실험하고자 하는 것은 두 집단의 평균이 같은지 다른지를 테스트 할 때 유용한 가설 검정 기법으로 활용될 것이다.

## Testing Hypotheses Concerning Variances
- 분산을 검정할 때는 $\chi^2$를 이용한 검정을 한다. 카이제곱 검정이 분산 검정에 적합한 이유에 대해서 고민을 해 본 결과
1. 표본 분산의 수학적 특성
	- 정규분포에서 표본을 추출할 때, $(n-1)s^2/\sigma^2$의 통계량이 자연스럽게 카이제곱 분포를 따르게 되고
	- 이는 표준정규분포를 따르는 독립적인 확률변수들의 제곱한이 카이제곱 분포를 따르는 것과 연결 !
2. 분산의 본질적 특성과 카이제곱
	- 분산은 편차의 제곱합을 기반으로 계산되고
	- 카이제곱 분포 역시 정규분포를 따르는 확률변수들의 제곱합으로 정의되어
	- 제곱이라는 특성이 일치하게 된다.
- 또한, 분산은 본질적으로 항상 양의 값을 갖게되며, 카이제곱 분포도 양의 값을 가지므로, 분산의 특성과 연결된다는 생각을 갖게된다.

- 우리의 가설 $H_0 : \sigma^2 = \sigma_{0}^2$로 설정한다. 따라서 우리가 주장하고 싶은 귀무가설 $H_{1} : \sigma^2 \neq \sigma_{0}^2$이며 따라서 두 분산이 다르다는 것을 보이고 싶어한다. 이러한 분산 검정은 이분산성 검정 (Heteroscedasticity Test)에서 사용되며, 이는 등분산성 이분산성 검정에서 유용한 Test라고 이해를 하고 넘어가도록 하겠다. 또는 잔차가 정규분포를 따르는지에 대한 여부를 검정할 수 있다.
$$
\chi^2 = \frac{(n-1)S^2}{\sigma_{0}^2}
$$
- ![[Figure 10.10.png]]

**Test of Hypotheses Concerning a Population Variance**
- Assumptions: $Y_{1}, Y_{2}, \dots, Y_{n}$ constitute a random sample from a normal distribution with
$$
E(Y_{i}) = \mu \quad \text{and} \quad  V(Y_{i}) = \sigma^2
$$

$$
\begin{align}  \\
&H_{0} : \sigma^2 = \sigma_{0}^2\\
&H_{\alpha} : \begin{cases}\sigma^2 > \sigma_{0}^2 \quad &(\text{upper-tail alternative.}) \\
\sigma^2 < \sigma_{0}^2\quad &(\text{lower-tail alternative.}) \\
\sigma^2 \neq \sigma_{0}^2\ \quad &(\text{two-tail alternative.})
\end{cases}  \\
&\text{Test Statistic} : \chi^2 = \frac{(n-1)S^2}{\sigma_{0}^2} \\
&\text{Rejection region} : \begin{cases} {\chi^2>\chi^2_{\alpha}} \quad &(\text{upper-tail RR.}) \\
{\chi^2 < \chi^2_{1-\alpha}} \quad &(\text{lower-tail RR.}) \\
{\chi^2 > \chi^2_{\alpha/2}}  \, \text{{or}} \, \chi^2<\chi^2_{1-\alpha/2}\quad &(\text{two-tailed RR.})
\end{cases} 
\end{align}
$$
- Notice that $\chi^2_{\alpha}$ is chosen so that, for $v=n-1$ df, $P(\chi^2 > \chi^2_{\alpha})$. 
- Sometimes we wish to compare the variances of two normal distributions, particularly by testing to determine whether they are equal. These problems are encountered in comparing the precision of two measuring instruments, the variation in quality characteristics of a manufactured product, or the variation in scores for two testing procedures.
- 즉 우리는 두 분산이 동일한지에 대한 여부를 검정하고 싶을 때의 기법을 소개해보려고 한다. 이때는 표본분산의 비율을 통해 검정을 진행하고 어떤 특정 값보다 클 때, 두 분산이 다르다는 것을 보인다. 우리는 이럴 때 사용하는 검정은 F 분포 검정이라고 한다. 
- $\chi^2$와 $F$의 검정 차이를 생각해보자
	- $\chi^2$ 검정의 경우 단일 분포의 분산을 알려진 특정 값과 비교하며
		- 예시) : 이 저울의 측정 오차가 $0.1kg^2$ 이라는 규격을 만족하는가?
	- $F$ 검정은 두 분포의 분산 비율을 검정한다.
		- 예시) : 새로운 저울이 기존 저울보다 더 정밀한가?

- $F$ 분포의 검정의 귀무 가설 $H_{0} : \sigma_{1}^2 = \sigma_{2}^2$이며, 대립가설은 $H_{\alpha}: \sigma_{1}^2>\sigma_{2}^2$ 이다. 
$$
RR = \left\{\frac{S_{1}^2}{S_{2}^2} < k\right\},
$$
- where $k$ is chosen so that the probability of a type 1 error is $\alpha$.
$$
F = \frac{(n_{1}-1)S_{1}^2}{\sigma_{1}^2(n_{1}-1)} / \frac{(n_{2}-1)S_{2}^2}{\sigma_{2}^2(n_{2}-1)} = \frac{S_{1}^2\sigma_{2}^2}{S_{2}^2\sigma_{1}^2}
$$
- has an $F$ distribution with $n_{1}-1$ numerator degrees of freedom and $n_{2}-1$ denominator degrees of freedom. $\to$ 표본의 분산의 자유도를 따른다고 생각하자.
	- numerator : 분자
	- denominator : 분모

**Test of the Hypothesis $\sigma_{1}^2 = \sigma_{2}^2$**
- Assumptions: Independent samples from normal populations.
$$
\begin{align}
&H_{0} : \sigma_{1}^2 = \sigma_{2}^2 \\ 
&H_{\alpha} : \sigma_{1}^2 > \sigma_{2}^2  \\
& \text{Test statistic} : F = \frac{S_{1}^2}{S_{2}^2} \\
\end{align}
$$
- Rejection region: $F > F_{\alpha}$, where $F_\alpha$ is chosen so that $P(F>F_{\alpha}) = \alpha$ when $F$ has $v_{1}=n_{1}-1$ numerator degrees of freedom and $v_{2} = n_{2}-1$ denominator degrees of freedom.
