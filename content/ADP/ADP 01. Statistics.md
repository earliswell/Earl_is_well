---
title: 01. Statistics
draft: false
tags:
  - "#statistics"
  - "#통계학"
  - "#probability"
  - "#확률"
  - "#Certification"
  - "#자격증"
---
## 1. 기초통계량

### 대표값 (평균, 중앙값)

**정의**: 데이터의 중심 경향을 나타내는 측정값으로, 데이터 집합의 전형적인 값을 요약

**수식**:

- 평균(Mean): $\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i$
- 중앙값(Median): 데이터를 정렬했을 때 중앙에 위치한 값 (짝수 개일 경우 중앙 두 값의 평균)

**특징**:

- 평균은 극단값에 민감하지만 모든 데이터를 활용
- 중앙값은 극단값에 강건하여 왜곡된 분포에 적합

**코드 예시**:

```python
import numpy as np
import pandas as pd

data = [1, 3, 5, 7, 100]  # 극단값 포함
mean_value = np.mean(data)  # 23.2
median_value = np.median(data)  # 5
```

**개념의 활용**:

- 데이터가 정규분포에 가까우면 평균 사용
- 극단값이 존재하거나 분포가 심하게 왜곡되었을 때는 중앙값 사용
- A/B 테스트에서 시험군과 대조군의 결과 차이 분석 시 활용

### 분산과 표준편차

**정의**: 데이터의 퍼진 정도(분산성)를 측정하는 통계량

**수식**:

- 분산(Variance): $s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i - \bar{x})^2$ (표본)
- 표준편차(Standard Deviation): $s = \sqrt{s^2}$
- 변동계수(CV): $CV = \frac{s}{\bar{x}}$

**특징**:

- 분산은 원 데이터와 단위가 제곱 형태로 달라 해석이 어려움
- 표준편차는 분산의 제곱근으로 원 데이터와 같은 단위를 가짐
- 변동계수는 서로 다른 단위나 평균이 다른 데이터 간 비교에 유용

**코드 예시**:

```python
variance = np.var(data, ddof=1)  # 표본분산(자유도 보정)
std_dev = np.std(data, ddof=1)   # 표본표준편차
cv = std_dev / np.mean(data)     # 변동계수
```

**개념의 활용**:

- 데이터의 안정성 평가(낮은 분산 = 안정적)
- 신뢰구간 및 가설검정의 기초
- 품질관리에서 제품 일관성 측정
- 서로 다른 측정 단위를 가진 변수의 분산을 비교할 때 변동계수 활용

### 분위수와 사분위수

**정의**: 데이터를 크기순으로 나열했을 때 특정 비율에 위치하는 값

**수식**:

- p분위수: 전체 데이터 중 p% 이하인 값
- 사분위수: Q1(25%), Q2(50%, 중앙값), Q3(75%)
- 사분위범위(IQR): Q3 - Q1

**특징**:

- 분포 형태에 상관없이 데이터의 분산 정도를 파악 가능
- 이상치 탐지에 유용 (보통 Q1-1.5×IQR 미만 또는 Q3+1.5×IQR 초과)

**코드 예시**:

```python
q1 = np.percentile(data, 25)
median = np.percentile(data, 50)
q3 = np.percentile(data, 75)
iqr = q3 - q1

# 이상치 탐지
lower_bound = q1 - 1.5 * iqr
upper_bound = q3 + 1.5 * iqr
outliers = [x for x in data if x < lower_bound or x > upper_bound]
```

**개념의 활용**:

- 비대칭 분포나 이상치가 있는 데이터의 분포 특성 파악
- 박스플롯을 통한 데이터 시각화 기초
- 데이터 전처리 과정에서 이상치 식별 및 처리
- 로버스트한 통계분석 수행 시 중앙값과 함께 활용

### 왜도와 첨도

**정의**:

- 왜도(Skewness): 분포의 비대칭성을 측정
- 첨도(Kurtosis): 분포의 뾰족한 정도와 꼬리 부분의 두께를 측정

**수식**:

- 왜도: $\text{Skewness} = \frac{1}{n}\sum_{i=1}^{n}(\frac{x_i-\bar{x}}{s})^3$
- 첨도: $\text{Kurtosis} = \frac{1}{n}\sum_{i=1}^{n}(\frac{x_i-\bar{x}}{s})^4 - 3$

**특징**:

- 왜도: 0(대칭), 양수(오른쪽으로 꼬리), 음수(왼쪽으로 꼬리)
- 첨도: 0(정규분포), 양수(뾰족하고 꼬리가 두꺼움), 음수(평평하고 꼬리가 얇음)

**코드 예시**:

```python
from scipy import stats

skewness = stats.skew(data)
kurtosis = stats.kurtosis(data)  # Fisher's 정의(정규분포=0)

# 정규성 판단에 활용
if abs(skewness) < 0.5 and abs(kurtosis) < 0.5:
    print("Data is approximately normal")
```

**개념의 활용**:

- 데이터의 정규성 판단에 활용
- 금융 데이터 분석에서 위험 평가(첨도가 높을수록 극단적 값 발생 가능성)
- 변수 변환 필요성 판단(정규성 가정이 필요한 분석 전)
- 왜도가 큰 경우 로그 변환 등을 통해 정규성 확보 가능

## 2. 확률

### 조건부 확률

**정의**: 사건 B가 발생했다는 조건 하에서 사건 A가 발생할 확률

**수식**: $P(A|B) = \frac{P(A \cap B)}{P(B)}$ (단, $P(B) > 0$)

**특징**:

- 사건 간 의존성을 나타내는 핵심 개념
- 독립사건의 경우: $P(A|B) = P(A)$
- 베이즈 정리의 기초가 됨

**코드 예시**:

```python
# 예시: 두 개의 주사위를 던졌을 때
# A: 합이 8 이상, B: 첫 번째 주사위가 4 이상
import numpy as np

# 모든 가능한 (주사위1, 주사위2) 조합
outcomes = [(i, j) for i in range(1, 7) for j in range(1, 7)]
total_outcomes = len(outcomes)

# 사건 A와 B 정의
A = [(i, j) for i, j in outcomes if i + j >= 8]
B = [(i, j) for i, j in outcomes if i >= 4]
AB = [(i, j) for i, j in outcomes if i + j >= 8 and i >= 4]

# 확률 계산
P_A = len(A) / total_outcomes
P_B = len(B) / total_outcomes
P_AB = len(AB) / total_outcomes
P_A_given_B = P_AB / P_B

print(f"P(A|B) = {P_A_given_B}")
```

**개념의 활용**:

- 의학 진단 테스트의 정확도 분석(양성예측도, 음성예측도)
- 베이지안 머신러닝 알고리즘의 기초
- 질병 위험 요인 분석 등 인과관계 추론
- 금융 리스크 평가 및 보험 계리 모델

### 베이즈 정리

**정의**: 조건부 확률을 역으로 계산하는 방법으로, 사전 지식과 관측 데이터를 결합해 사후 확률을 계산

**수식**: $P(A|B) = \frac{P(B|A) \times P(A)}{P(B)}$

**특징**:

- 사전확률 P(A)에 새로운 증거 B를 반영해 사후확률 P(A|B) 계산
- 불확실성을 정량화하고 새로운 정보로 업데이트하는 프레임워크 제공
- 머신러닝과 통계적 추론의 기반

**코드 예시**:

```python
# 예시: 질병 진단 테스트
# P(Disease): 질병 발생률 = 0.01
# P(Positive|Disease): 민감도 = 0.95
# P(Negative|No Disease): 특이도 = 0.90

P_disease = 0.01
sensitivity = 0.95  # P(Positive|Disease)
specificity = 0.90  # P(Negative|No Disease)

P_positive_given_disease = sensitivity
P_positive_given_no_disease = 1 - specificity

# 베이즈 정리 적용: P(Disease|Positive)
P_positive = (P_positive_given_disease * P_disease + 
              P_positive_given_no_disease * (1 - P_disease))
P_disease_given_positive = (P_positive_given_disease * P_disease) / P_positive

print(f"양성예측도 P(Disease|Positive) = {P_disease_given_positive:.4f}")
```

**개념의 활용**:

- 의학 검사 결과 해석(양성반응 시 실제 질병 가능성)
- 스팸 필터링 알고리즘(메시지 내용 기반 스팸 확률)
- 금융 사기 탐지(트랜잭션 패턴 기반 사기 가능성)
- 베이지안 A/B 테스트(사전 지식 활용한 효율적 테스트)

## 3. 확률분포

### 이산확률분포

#### 베르누이분포

**정의**: 성공/실패 두 가지 결과만 가능한 단일 시행을 모델링하는 확률분포

**수식**:

- 확률질량함수: $P(X=x) = p^x(1-p)^{1-x}$ (x=0,1)
- 평균: $E(X) = p$
- 분산: $Var(X) = p(1-p)$

**특징**:

- 가장 단순한 확률분포로 이항분포의 기초
- 파라미터 p는 성공확률

**코드 예시**:

```python
from scipy import stats
import numpy as np

# 베르누이 분포(성공확률 0.3)
p = 0.3
bern = stats.bernoulli(p)

# PMF 계산
print(f"P(X=0) = {bern.pmf(0)}")  # 0.7
print(f"P(X=1) = {bern.pmf(1)}")  # 0.3

# 랜덤 샘플 생성
samples = bern.rvs(size=1000)
print(f"샘플 평균: {np.mean(samples)}")
```

**개념의 활용**:

- 이진 분류 문제(성공/실패, 합격/불합격 등)
- 로지스틱 회귀의 결과값 모델링
- A/B 테스트에서 전환율 모델링
- 베이지안 모델의 사전분포로 활용

#### 이항분포

**정의**: n번의 독립적인 베르누이 시행에서 성공 횟수를 모델링하는 확률분포

**수식**:

- 확률질량함수: $P(X=k) = \binom{n}{k}p^k(1-p)^{n-k}$
- 평균: $E(X) = np$
- 분산: $Var(X) = np(1-p)$

**특징**:

- 시행 횟수(n)와 성공확률(p)로 특징지어짐
- 정규분포로 근사 가능(n이 크고 p가 0이나 1에 극단적으로 가깝지 않을 때)

**코드 예시**:

```python
# 이항분포(10회 시행, 성공확률 0.3)
n, p = 10, 0.3
binom = stats.binom(n, p)

# PMF 계산
for k in range(n+1):
    print(f"P(X={k}) = {binom.pmf(k):.4f}")

# 특정 확률 계산
print(f"P(X<=2) = {binom.cdf(2):.4f}")  # 누적분포함수
print(f"P(X>=8) = {1-binom.cdf(7):.4f}")  # 8 이상일 확률

# 평균과 분산
print(f"평균: {binom.mean()}, 분산: {binom.var()}")
```

**개념의 활용**:

- 불량품 수 예측(n개 제품 중 불량품 수)
- 시장 조사에서 특정 응답 수 예측
- 품질관리 프로세스의 통계적 기반
- 유전학에서 유전자 출현 빈도 모델링

#### 포아송분포

**정의**: 주어진 시간 또는 공간에서 사건 발생 횟수를 모델링하는 확률분포

**수식**:

- 확률질량함수: $P(X=k) = \frac{\lambda^k e^{-\lambda}}{k!}$
- 평균: $E(X) = \lambda$
- 분산: $Var(X) = \lambda$

**특징**:

- 단위 시간/공간당 평균 발생률 λ로 특징지어짐
- 평균과 분산이 동일(λ)
- 희소 사건(rare events) 모델링에 적합

**코드 예시**:

```python
# 포아송분포(평균 발생률 2.5)
lambda_val = 2.5
poisson = stats.poisson(lambda_val)

# PMF 계산
for k in range(10):
    print(f"P(X={k}) = {poisson.pmf(k):.4f}")

# 특정 확률 계산
print(f"P(X<=1) = {poisson.cdf(1):.4f}")
print(f"P(X>=5) = {1-poisson.cdf(4):.4f}")

# 평균과 분산 확인
print(f"평균: {poisson.mean()}, 분산: {poisson.var()}")

# 적합도 검정
observed_counts = [10, 25, 30, 20, 10, 5]  # 관측된 빈도
k_values = np.arange(len(observed_counts))
expected_counts = poisson.pmf(k_values) * sum(observed_counts)

# 카이제곱 검정
chi2, p_value = stats.chisquare(observed_counts, expected_counts)
print(f"Chi-squared: {chi2:.4f}, p-value: {p_value:.4f}")
```

**개념의 활용**:

- 콜센터 통화량 예측
- 웹사이트 트래픽 분석
- 사고 발생 빈도 분석
- 생물학에서 세포 변이 빈도 모델링
- 보험 청구 횟수 예측

#### 초기하분포

**정의**: N개 중 K개가 특정 특성을 가질 때, n개를 뽑아 특정 특성을 가진 항목의 수를 모델링하는 확률분포

**수식**:

- 확률질량함수: $P(X=k) = \frac{\binom{K}{k}\binom{N-K}{n-k}}{\binom{N}{n}}$
- 평균: $E(X) = n\frac{K}{N}$
- 분산: $Var(X) = n\frac{K}{N}(1-\frac{K}{N})(\frac{N-n}{N-1})$

**특징**:

- 비복원 추출에 사용(이항분포는 복원 추출 가정)
- 모집단 크기(N), 특성 가진 항목 수(K), 표본 크기(n)로 특징지어짐

**코드 예시**:

```python
# 초기하분포(전체 20개 중 7개가 특성 보유, 8개 추출)
N, K, n = 20, 7, 8
hypergeom = stats.hypergeom(M=N, n=K, N=n)

# PMF 계산
for k in range(min(n+1, K+1)):
    print(f"P(X={k}) = {hypergeom.pmf(k):.4f}")

# 특정 확률 계산
print(f"P(X<=2) = {hypergeom.cdf(2):.4f}")
print(f"P(X>=4) = {1-hypergeom.cdf(3):.4f}")

# 평균과 분산
print(f"평균: {hypergeom.mean()}, 분산: {hypergeom.var()}")
```

**개념의 활용**:

- 품질관리에서 로트 수용 샘플링
- 카드 게임에서 특정 조합 확률 계산
- 생태학에서 표본추출 기반 개체수 추정
- 감사에서 결함 있는 항목 추정

### 연속확률분포

#### 정규분포

**정의**: 대부분의 자연현상을 모델링하는 데 사용되는 종 모양의 연속확률분포

**수식**:

- 확률밀도함수: $f(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
- 평균: $E(X) = \mu$
- 분산: $Var(X) = \sigma^2$

**특징**:

- 평균(μ)과 표준편차(σ)로 특징지어짐
- 표준정규분포: 평균 0, 표준편차 1인 정규분포
- 중심극한정리에 의해 많은 확률변수의 합은 정규분포에 근사
- 68-95-99.7 법칙: μ±1σ(68%), μ±2σ(95%), μ±3σ(99.7%)

**코드 예시**:

```python
# 정규분포(평균 10, 표준편차 2)
mu, sigma = 10, 2
norm = stats.norm(mu, sigma)

# 특정 확률 계산
print(f"P(X<=8) = {norm.cdf(8):.4f}")
print(f"P(9<=X<=12) = {norm.cdf(12) - norm.cdf(9):.4f}")

# 특정 분위수 계산
print(f"25% 분위수: {norm.ppf(0.25):.4f}")
print(f"50% 분위수(중앙값): {norm.ppf(0.5):.4f}")
print(f"95% 분위수: {norm.ppf(0.95):.4f}")

# 정규성 검정
data = np.random.normal(mu, sigma, 100)
shapiro_test = stats.shapiro(data)
print(f"Shapiro-Wilk 검정: 통계량={shapiro_test.statistic:.4f}, p값={shapiro_test.pvalue:.4f}")

# QQ 플롯 (정규성 시각적 확인)
from statsmodels.graphics.gofplots import qqplot
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))
qqplot(data, line='s')
plt.title('Normal Q-Q Plot')
plt.grid(True)
```

**개념의 활용**:

- 많은 자연현상 모델링(키, 몸무게, IQ 등)
- 통계적 가설검정의 기초
- 시그마 품질관리 방법론
- 금융에서 자산 수익률 모델링(단기)
- 머신러닝 알고리즘의 오차 분포 가정

#### 지수분포

**정의**: 사건 발생 간격의 시간을 모델링하는 연속확률분포

**수식**:

- 확률밀도함수: $f(x) = \lambda e^{-\lambda x}$ (x≥0)
- 평균: $E(X) = \frac{1}{\lambda}$
- 분산: $Var(X) = \frac{1}{\lambda^2}$

**특징**:

- 포아송 과정에서 사건 간 시간 간격 분포
- 무기억성(memoryless) 특성: $P(X>s+t|X>s) = P(X>t)$
- 파라미터 λ는 단위 시간당 평균 발생률

**코드 예시**:

```python
# 지수분포(λ=0.5, 평균 대기시간 2)
lambda_val = 0.5
exp = stats.expon(scale=1/lambda_val)  # scale=1/λ

# 특정 확률 계산
print(f"P(X<=1) = {exp.cdf(1):.4f}")
print(f"P(X>3) = {1-exp.cdf(3):.4f}")

# 특정 분위수 계산
print(f"중앙값(50% 분위수): {exp.ppf(0.5):.4f}")
print(f"95% 분위수: {exp.ppf(0.95):.4f}")

# 무기억성 특성 확인
s, t = 2, 1
p1 = (1 - exp.cdf(s + t)) / (1 - exp.cdf(s))  # P(X>s+t|X>s)
p2 = 1 - exp.cdf(t)  # P(X>t)
print(f"P(X>{s+t}|X>{s}) = {p1:.4f}, P(X>{t}) = {p2:.4f}")
```

**개념의 활용**:

- 장비 수명 모델링(고장 시간)
- 고객 대기시간 분석
- 약물 반감기 모델링
- 생존분석에서 기초 분포
- 신뢰성 공학에서 고장률 모델링

#### 균일분포

**정의**: 주어진 범위 내 모든 값이 동일한 확률을 가지는 연속확률분포

**수식**:

- 확률밀도함수: $f(x) = \frac{1}{b-a}$ (a≤x≤b)
- 평균: $E(X) = \frac{a+b}{2}$
- 분산: $Var(X) = \frac{(b-a)^2}{12}$

**특징**:

- 최소값(a)과 최대값(b)으로 특징지어짐
- 난수 생성의 기초

**코드 예시**:

```python
# 균일분포(a=2, b=6)
a, b = 2, 6
unif = stats.uniform(loc=a, scale=b-a)  # loc=a, scale=b-a

# 특정 확률 계산
print(f"P(X<=3) = {unif.cdf(3):.4f}")
print(f"P(4<=X<=5) = {unif.cdf(5) - unif.cdf(4):.4f}")

# 평균과 분산
print(f"평균: {unif.mean():.4f}, 분산: {unif.var():.4f}")

# 랜덤 샘플 생성 및 시각화
samples = unif.rvs(size=1000)
plt.figure(figsize=(10, 6))
plt.hist(samples, bins=20, density=True, alpha=0.7)
x = np.linspace(a, b, 100)
plt.plot(x, unif.pdf(x), 'r-', lw=2)
plt.title('Uniform Distribution')
plt.grid(True)
```

**개념의 활용**:

- 시뮬레이션 및 난수 생성
- 베이지안 분석에서 무정보 사전분포
- 양자화 오차 모델링
- 추첨 과정 모델링(모든 결과가 동일한 확률)

### 공분산과 상관계수

**정의**:

- 공분산: 두 변수 간 선형 관계의 방향과 강도를 측정
- 상관계수: 공분산을 표준화하여 -1에서 1 사이 값으로 표현

**수식**:

- 공분산: $Cov(X,Y) = E[(X-\mu_X)(Y-\mu_Y)] = \frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})$
- 피어슨 상관계수: $\rho_{X,Y} = \frac{Cov(X,Y)}{\sigma_X \sigma_Y} = \frac{\sum(x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum(x_i-\bar{x})^2\sum(y_i-\bar{y})^2}}$

**특징**:

- 상관계수 해석: -1(완전 음의 상관), 0(무상관), 1(완전 양의 상관)
- 공분산/상관계수는 선형관계만 측정(비선형 관계는 감지 못함)
- 피어슨 상관계수는 이상치에 민감

**코드 예시**:

```python
# 공분산과 상관계수 계산
x = np.array([1, 2, 3, 4, 5])
y = np.array([2, 3, 5, 4, 6])

# 공분산
cov_xy = np.cov(x, y)[0, 1]  # 공분산 행렬의 비대각 요소
print(f"공분산: {cov_xy:.4f}")

# 상관계수
corr_xy = np.corrcoef(x, y)[0, 1]  # 상관행렬의 비대각 요소
print(f"피어슨 상관계수: {corr_xy:.4f}")

# pandas를 이용한 계산
df = pd.DataFrame({'x': x, 'y': y})
print("공분산 행렬:\n", df.cov())
print("상관 행렬:\n", df.corr())

# 스피어만 순위상관계수 (비선형 관계 탐지에 유용)
spearman_corr = stats.spearmanr(x, y).correlation
print(f"스피어만 순위상관계수: {spearman_corr:.4f}")
```

**개념의 활용**:

- 변수 간 관계 파악 및 예측모델 구축 기초
- 포트폴리오 이론에서 자산 간 의존성 측정
- 다중공선성 진단
- 차원 축소 기법(PCA 등)의 기반
- 상관이 높은 변수 중 일부만 선택해 모델 단순화

---

## 1. 통계적 추정

### 점추정

**정의**: 미지의 모수(모집단 특성값)를 단일 값으로 추정하는 방법

**수식**:

- 표본평균(모평균 추정): $\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i$
- 표본분산(모분산 추정): $s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i - \bar{x})^2$
- 표본비율(모비율 추정): $\hat{p} = \frac{x}{n}$ (x: 성공 횟수)

**특징**:

- 불편성(unbiasedness): 추정량의 기댓값이 모수와 일치
- 일치성(consistency): 표본 크기가 증가할수록 모수에 수렴
- 효율성(efficiency): 분산이 작은 추정량이 더 효율적

**코드 예시**:

```python
import numpy as np
from scipy import stats

# 표본 데이터
data = np.random.normal(loc=100, scale=15, size=50)  # 평균 100, 표준편차 15인 모집단에서 50개 표본 추출

# 점추정
sample_mean = np.mean(data)  # 표본평균(모평균 추정)
sample_var = np.var(data, ddof=1)  # 표본분산(모분산 추정), ddof=1은 n-1로 나누는 옵션
sample_std = np.std(data, ddof=1)  # 표본표준편차(모표준편차 추정)

print(f"모평균 추정값: {sample_mean:.2f}")
print(f"모분산 추정값: {sample_var:.2f}")
print(f"모표준편차 추정값: {sample_std:.2f}")

# 이항분포 모비율 추정
successes = 42
trials = 100
p_hat = successes / trials
print(f"모비율 추정값: {p_hat:.2f}")
```

**개념의 활용**:

- 표본을 통한 모집단 특성 추정(평균 키, 평균 소득 등)
- 정확한 값보다는 점추정과 함께 불확실성 측정(구간추정)이 필요
- 추정량 선택 기준: 불편성, 일치성, 효율성, 충분성 등

### 구간추정

**정의**: 모수가 특정 신뢰수준으로 포함될 것으로 기대되는 구간을 추정하는 방법

**수식**:

- 일반적인 형태: 점추정값 ± (임계값 × 표준오차)
- 신뢰구간 해석: 같은 방식으로 구간을 100번 구성하면, 그 중 약 95개(95% 신뢰수준의 경우)는 참값을 포함

#### 모평균에 대한 구간추정

**수식**:

- 모표준편차를 알 때: $\bar{x} \pm z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}}$
- 모표준편차를 모를 때(n≥30): $\bar{x} \pm z_{\alpha/2} \cdot \frac{s}{\sqrt{n}}$
- 모표준편차를 모르고 n<30: $\bar{x} \pm t_{\alpha/2, n-1} \cdot \frac{s}{\sqrt{n}}$

**코드 예시**:

```python
# 모평균에 대한 95% 신뢰구간
# 케이스 1: 모표준편차를 알 때
known_sigma = 15
z_critical = stats.norm.ppf(0.975)  # 95% 신뢰구간의 z값(양측)
margin_of_error = z_critical * (known_sigma / np.sqrt(len(data)))
confidence_interval = (sample_mean - margin_of_error, sample_mean + margin_of_error)
print(f"모평균 95% 신뢰구간(σ 알 때): {confidence_interval}")

# 케이스 2: 모표준편차를 모를 때(n≥30)
if len(data) >= 30:
    margin_of_error = z_critical * (sample_std / np.sqrt(len(data)))
    confidence_interval = (sample_mean - margin_of_error, sample_mean + margin_of_error)
    print(f"모평균 95% 신뢰구간(σ 모를 때, n≥30): {confidence_interval}")

# 케이스 3: 모표준편차를 모르고 n<30
else:
    t_critical = stats.t.ppf(0.975, df=len(data)-1)  # t분포 임계값
    margin_of_error = t_critical * (sample_std / np.sqrt(len(data)))
    confidence_interval = (sample_mean - margin_of_error, sample_mean + margin_of_error)
    print(f"모평균 95% 신뢰구간(σ 모를 때, n<30): {confidence_interval}")

# 내장 함수 사용
mean_ci = stats.norm.interval(0.95, loc=sample_mean, scale=sample_std/np.sqrt(len(data)))
print(f"모평균 95% 신뢰구간(내장함수): {mean_ci}")
```

#### 모비율에 대한 구간추정

**수식**: $\hat{p} \pm z_{\alpha/2} \cdot \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$ (단, $n\hat{p} \geq 5$ 및 $n(1-\hat{p}) \geq 5$)

**코드 예시**:

```python
# 모비율에 대한 95% 신뢰구간
p_hat = successes / trials
std_error = np.sqrt((p_hat * (1 - p_hat)) / trials)
margin_of_error = z_critical * std_error
proportion_ci = (p_hat - margin_of_error, p_hat + margin_of_error)
print(f"모비율 95% 신뢰구간: {proportion_ci}")

# 내장 함수 사용
from statsmodels.stats.proportion import proportion_confint
prop_ci = proportion_confint(successes, trials, alpha=0.05, method='normal')
print(f"모비율 95% 신뢰구간(내장함수): {prop_ci}")
```

#### 모분산에 대한 구간추정

**수식**: $\left(\frac{(n-1)s^2}{\chi^2_{\alpha/2, n-1}}, \frac{(n-1)s^2}{\chi^2_{1-\alpha/2, n-1}}\right)$

**코드 예시**:

```python
# 모분산에 대한 95% 신뢰구간
chi2_lower = stats.chi2.ppf(0.025, df=len(data)-1)
chi2_upper = stats.chi2.ppf(0.975, df=len(data)-1)
var_ci_lower = (len(data) - 1) * sample_var / chi2_upper
var_ci_upper = (len(data) - 1) * sample_var / chi2_lower
var_ci = (var_ci_lower, var_ci_upper)
print(f"모분산 95% 신뢰구간: {var_ci}")

# 모표준편차 신뢰구간
std_ci = (np.sqrt(var_ci_lower), np.sqrt(var_ci_upper))
print(f"모표준편차 95% 신뢰구간: {std_ci}")
```

**특징**:

- 신뢰수준(confidence level): 보통 90%, 95%, 99% 사용
- 신뢰구간 폭은 신뢰수준과 표본 크기에 의존
- 신뢰수준 ↑ 또는 표본 크기 ↓ → 신뢰구간 폭 ↑
- 신뢰수준 ↓ 또는 표본 크기 ↑ → 신뢰구간 폭 ↓

**개념의 활용**:

- 모수의 불확실성 정량화
- 다양한 통계적 검정(가설검정의 대안)
- 표본 크기 결정(목표 정밀도 달성에 필요한 표본 수)
- 실험 결과의 신뢰성 평가

### 표본크기 결정

**정의**: 원하는 정밀도(오차한계)와 신뢰수준으로 모수를 추정하기 위해 필요한 표본 크기를 결정하는 방법

**수식**:

- 모평균 추정 시: $n = \left(\frac{z_{\alpha/2} \cdot \sigma}{E}\right)^2$ (E: 허용오차)
- 모비율 추정 시: $n = \frac{z_{\alpha/2}^2 \cdot p(1-p)}{E^2}$ (사전정보 없을 때 p=0.5 사용)

**코드 예시**:

```python
# 모평균 추정을 위한 표본 크기 계산
sigma = 15  # 모표준편차(사전 추정값 또는 파일럿 연구 결과)
E = 3  # 허용오차(모평균을 ±3 단위로 추정하고 싶을 때)
confidence_level = 0.95
z_value = stats.norm.ppf(1 - (1 - confidence_level) / 2)
n_mean = int(np.ceil((z_value * sigma / E) ** 2))
print(f"모평균 추정에 필요한 표본 크기(95% 신뢰수준, 오차±{E}): {n_mean}")

# 모비율 추정을 위한 표본 크기 계산
p = 0.5  # 보수적 접근(최대 표본 크기 산출)
E = 0.05  # 허용오차(모비율을 ±5%p로 추정하고 싶을 때)
n_prop = int(np.ceil((z_value**2 * p * (1-p)) / (E**2)))
print(f"모비율 추정에 필요한 표본 크기(95% 신뢰수준, 오차±{E}): {n_prop}")

# 사전 정보가 있는 경우(예: 대략적인 비율이 0.3로 예상될 때)
p_prior = 0.3
n_prop_prior = int(np.ceil((z_value**2 * p_prior * (1-p_prior)) / (E**2)))
print(f"모비율 추정에 필요한 표본 크기(사전정보 p≈{p_prior}): {n_prop_prior}")
```

**특징**:

- 신뢰수준 ↑ 또는 허용오차 ↓ → 필요한 표본 크기 ↑
- 모비율의 경우 p=0.5가 가장 보수적(최대 표본 크기 산출)
- 사전 연구나 파일럿 테스트로 모표준편차 추정 가능

**개념의 활용**:

- 설문조사, 여론조사 설계
- 임상시험 및 실험 설계
- 품질관리 샘플링 계획
- 시장조사 규모 결정

## 2. 가설검정

### 가설검정의 기본 개념

#### 귀무가설과 대립가설

**정의**:

- 귀무가설(H₀): 검정하고자 하는 주장의 반대(보통 "차이가 없다" 형태)
- 대립가설(H₁): 연구자가 입증하고자 하는 주장(보통 "차이가 있다" 형태)

**특징**:

- 귀무가설은 기본적으로 채택되는 가설로, 증거가 충분할 때만 기각
- 대립가설은 양측검정(≠), 우측검정(>), 좌측검정(<) 형태로 설정 가능
- 가설은 모수에 대한 명제로 설정

**코드 예시**:

```python
# 가설 설정 예시(코드가 아닌 설명)
"""
예시 1: 평균 검정
H₀: μ = 100 (귀무가설)
H₁: μ ≠ 100 (양측 대립가설)

예시 2: 비율 검정
H₀: p ≤ 0.5 (귀무가설)
H₁: p > 0.5 (우측 대립가설)

예시 3: 두 집단 평균 비교
H₀: μ₁ = μ₂ 또는 μ₁ - μ₂ = 0 (귀무가설)
H₁: μ₁ ≠ μ₂ 또는 μ₁ - μ₂ ≠ 0 (양측 대립가설)
"""
```

**개념의 활용**:

- 새로운 치료법, 전략, 방법의 효과 검증
- 제품 품질이 기준을 충족하는지 확인
- 두 집단 간 차이 유무 판단
- 데이터 기반 의사결정의 기초

#### 유의수준과 p-값

**정의**:

- 유의수준(α): 귀무가설이 참일 때 이를 기각할 확률(1종 오류 확률)
- p-값: 귀무가설 하에서 관측된 결과보다 극단적인 결과가 나올 확률

**특징**:

- 일반적으로 α = 0.05(5%) 또는 0.01(1%) 사용
- p-값 < α이면 귀무가설 기각
- p-값은 증거의 강도를 나타냄
- p-값이 작을수록 귀무가설에 반하는 증거가 강함

#### 1종 오류와 2종 오류

**정의**:

- 1종 오류(Type I): 귀무가설이 참인데 기각하는 오류(거짓 양성)
- 2종 오류(Type II): 귀무가설이 거짓인데 기각하지 못하는 오류(거짓 음성)

**특징**:

- 1종 오류 확률 = α(유의수준)
- 2종 오류 확률 = β
- 검정력(power) = 1 - β(대립가설이 참일 때 이를 올바르게 검출할 확률)
- 표본 크기 증가 → 2종 오류 감소(검정력 증가)

**개념의 활용**:

- 1종 오류: 무고한 사람을 유죄 판결(심각한 결과)
- 2종 오류: 유죄인 사람을 무죄 판결(검정력 부족)
- 의학검사: 질병 진단의 민감도와 특이도
- 적절한 표본 크기 결정에 활용

### 주요 가설검정 방법

#### 정규성 검정

**정의**: 데이터가 정규분포를 따르는지 검정하는 방법(많은 통계적 검정법의 가정)

**주요 검정법**:

- Shapiro-Wilk 검정: 작은 표본에 효과적(일반적으로 n≤50)
- Kolmogorov-Smirnov 검정: 큰 표본에 적합(often with Lilliefors correction)
- Anderson-Darling 검정: 분포의 꼬리 부분에 더 민감

**코드 예시**:

```python
# 정규성 검정
from scipy import stats
import matplotlib.pyplot as plt

# 데이터 생성
data_normal = np.random.normal(loc=0, scale=1, size=50)  # 정규분포
data_nonnormal = np.random.exponential(scale=1, size=50)  # 지수분포(비정규)

# Shapiro-Wilk 검정
sw_normal = stats.shapiro(data_normal)
sw_nonnormal = stats.shapiro(data_nonnormal)

print(f"정규 데이터 Shapiro-Wilk 검정: 통계량={sw_normal.statistic:.4f}, p값={sw_normal.pvalue:.4f}")
print(f"비정규 데이터 Shapiro-Wilk 검정: 통계량={sw_nonnormal.statistic:.4f}, p값={sw_nonnormal.pvalue:.4f}")

# Kolmogorov-Smirnov 검정
ks_normal = stats.kstest(data_normal, 'norm')
ks_nonnormal = stats.kstest(data_nonnormal, 'norm')

print(f"정규 데이터 K-S 검정: 통계량={ks_normal.statistic:.4f}, p값={ks_normal.pvalue:.4f}")
print(f"비정규 데이터 K-S 검정: 통계량={ks_nonnormal.statistic:.4f}, p값={ks_nonnormal.pvalue:.4f}")

# 시각적 검정(QQ plot)
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# 정규 데이터 QQ plot
stats.probplot(data_normal, dist="norm", plot=axes[0])
axes[0].set_title('Normal Data Q-Q Plot')

# 비정규 데이터 QQ plot
stats.probplot(data_nonnormal, dist="norm", plot=axes[1])
axes[1].set_title('Non-normal Data Q-Q Plot')

plt.tight_layout()
```

**특징**:

- 귀무가설: 데이터가 정규분포를 따른다
- p-값 < α → 정규성 가정 기각
- 작은 표본에서는 정규성 검정의 검정력이 낮을 수 있음
- QQ plot으로 시각적 확인 병행 권장

**개념의 활용**:

- t-검정, 분산분석 등 모수적 검정의 가정 확인
- 비정규 데이터에 비모수 검정 적용 필요성 판단
- 데이터 변환(로그, 제곱근 등) 필요성 판단
- 통계 모델링 전 분포 특성 파악

#### 등분산 검정

**정의**: 두 집단 이상의 분산이 동일한지 검정하는 방법(t-검정, 분산분석의 가정)

**주요 검정법**:

- Levene 검정: 정규성 가정에 덜 민감(robust)
- Bartlett 검정: 정규성 가정 하에서 더 강력
- F 검정: 두 집단만 비교할 때 사용

**코드 예시**:

```python
# 등분산 검정
# 두 집단 데이터 생성
group1 = np.random.normal(loc=10, scale=2, size=30)
group2 = np.random.normal(loc=12, scale=2, size=30)  # 동일 분산
group3 = np.random.normal(loc=14, scale=4, size=30)  # 다른 분산

# Levene 검정
levene_test_equal = stats.levene(group1, group2)
levene_test_unequal = stats.levene(group1, group3)

print(f"동일분산 Levene 검정: 통계량={levene_test_equal.statistic:.4f}, p값={levene_test_equal.pvalue:.4f}")
print(f"이분산 Levene 검정: 통계량={levene_test_unequal.statistic:.4f}, p값={levene_test_unequal.pvalue:.4f}")

# Bartlett 검정
bartlett_test_equal = stats.bartlett(group1, group2)
bartlett_test_unequal = stats.bartlett(group1, group3)

print(f"동일분산 Bartlett 검정: 통계량={bartlett_test_equal.statistic:.4f}, p값={bartlett_test_equal.pvalue:.4f}")
print(f"이분산 Bartlett 검정: 통계량={bartlett_test_unequal.statistic:.4f}, p값={bartlett_test_unequal.pvalue:.4f}")

# F 검정(두 집단)
f_statistic = np.var(group1, ddof=1) / np.var(group3, ddof=1)
if f_statistic < 1:
    f_statistic = 1 / f_statistic
    df1, df2 = len(group3) - 1, len(group1) - 1
else:
    df1, df2 = len(group1) - 1, len(group3) - 1
    
p_value = 2 * (1 - stats.f.cdf(f_statistic, df1, df2))  # 양측검정
print(f"F 검정: 통계량={f_statistic:.4f}, p값={p_value:.4f}")
```

**특징**:

- 귀무가설: 모든 집단의 분산이 동일하다
- p-값 < α → 등분산 가정 기각
- 데이터가 정규분포를 따를 때 Bartlett 검정이 더 강력
- 비정규 데이터에는 Levene 검정 권장

**개념의 활용**:

- t-검정에서 등분산/이분산 방법 선택
- 분산분석(ANOVA) 적용 가능성 평가
- 품질관리에서 공정 변동성 비교
- 측정 시스템의 일관성 평가

#### t-검정

**정의**: 두 집단의 평균 차이를 검정하거나 한 집단의 평균을 특정 값과 비교하는 검정

**주요 유형**:

- 일표본 t-검정: 한 집단의 평균을 특정 값과 비교
- 독립표본 t-검정: 서로 다른 두 집단의 평균 비교
- 대응표본 t-검정: 동일 대상의 전후 비교(paired)

**수식**:

- 일표본 t-검정: $t = \frac{\bar{x} - \mu_0}{s/\sqrt{n}}$
- 독립표본 t-검정(등분산): $t = \frac{\bar{x}_1 - \bar{x}_2}{s_p\sqrt{\frac{1}{n_1}+\frac{1}{n_2}}}$, 여기서 $s_p^2 = \frac{(n_1-1)s_1^2 + (n_2-1)s_2^2}{n_1+n_2-2}$
- 대응표본 t-검정: $t = \frac{\bar{d}}{s_d/\sqrt{n}}$, 여기서 $\bar{d}$는 차이의 평균

**코드 예시**:

```python
# t-검정 예시
# 일표본 t-검정
sample = np.random.normal(loc=102, scale=15, size=40)  # 평균이 102인 표본
popmean = 100  # 비교할 참값

one_sample_t = stats.ttest_1samp(sample, popmean)
print(f"일표본 t-검정: 통계량={one_sample_t.statistic:.4f}, p값={one_sample_t.pvalue:.4f}")

# 독립표본 t-검정
# 가정: group1, group2는 위에서 정의됨
indep_t_equal = stats.ttest_ind(group1, group2, equal_var=True)  # 등분산 가정
indep_t_unequal = stats.ttest_ind(group1, group3, equal_var=False)  # Welch's t-test(이분산)

print(f"독립표본 t-검정(등분산): 통계량={indep_t_equal.statistic:.4f}, p값={indep_t_equal.pvalue:.4f}")
print(f"독립표본 t-검정(이분산): 통계량={indep_t_unequal.statistic:.4f}, p값={indep_t_unequal.pvalue:.4f}")

# 대응표본 t-검정
# 치료 전후 데이터
before = np.random.normal(loc=100, scale=15, size=30)
after = before + np.random.normal(loc=5, scale=5, size=30)  # 평균적으로 5 증가

paired_t = stats.ttest_rel(before, after)
print(f"대응표본 t-검정: 통계량={paired_t.statistic:.4f}, p값={paired_t.pvalue:.4f}")
```

**특징**:

- 가정: 표본의 정규성(n≥30일 경우 중심극한정리로 완화 가능)
- 독립표본 t-검정은 등분산 여부에 따라 다른 통계량 사용
- 대응표본 t-검정은 독립표본보다 통계적 검정력이 높음
- 신뢰구간도 함께 제시하는 것이 좋음

**개념의 활용**:

- 신약 효과 검증(대조군과 비교)
- 교육방법 효과 비교
- 제품 개선 전후 성능 비교
- 두 지역, 두 집단 간 특성 차이 검증

#### 분산분석(ANOVA)

**정의**: 세 개 이상 집단의 평균 차이를 검정하는 방법

**주요 유형**:

- 일원배치 분산분석: 하나의 요인에 따른 집단 간 차이
- 이원배치 분산분석: 두 개 요인의 주효과와 상호작용 효과 분석
- 반복측정 분산분석: 동일 대상의 반복 측정 데이터 분석

**수식**:

- F 통계량: $F = \frac{MS_{Between}}{MS_{Within}} = \frac{SS_{Between}/(k-1)}{SS_{Within}/(n-k)}$
- SS(제곱합): $SS_{Total} = SS_{Between} + SS_{Within}$

**코드 예시**:

```python
# 분산분석(ANOVA) 예시
import statsmodels.api as sm
from statsmodels.formula.api import ols

# 일원배치 분산분석
# 3개 집단 데이터
group_a = np.random.normal(loc=10, scale=2, size=30)
group_b = np.random.normal(loc=12, scale=2, size=30)
group_c = np.random.normal(loc=14, scale=2, size=30)

# 데이터프레임 구성
import pandas as pd
df = pd.DataFrame({
    'values': np.concatenate([group_a, group_b, group_c]),
    'group': np.repeat(['A', 'B', 'C'], [30, 30, 30])
})

# scipy를 이용한 일원배치 분산분석
f_stat, p_val = stats.f_oneway(group_a, group_b, group_c)
print(f"일원배치 분산분석(scipy): F={f_stat:.4f}, p값={p_val:.4f}")

# statsmodels를 이용한 일원배치 분산분석
model = ols('values ~ C(group)', data=df).fit()
anova_table = sm.stats.anova_lm(model, typ=2)
print("일원배치 분산분석(statsmodels):")
print(anova_table)

# 사후검정(Tukey's HSD)
from statsmodels.stats.multicomp import pairwise_tukeyhsd
tukey = pairwise_tukeyhsd(df['values'], df['group'], alpha=0.05)
print("\nTukey's HSD 사후검정:")
print(tukey)

# 이원배치 분산분석 예시
# 두 번째 요인 추가
df['gender'] = np.repeat(['M', 'F'], 45)
model2 = ols('values ~ C(group) + C(gender) + C(group):C(gender)', data=df).fit()
anova_table2 = sm.stats.anova_lm(model2, typ=2)
print("\n이원배치 분산분석:")
print(anova_table2)
```

**특징**:

- 가정: 정규성, 등분산성, 독립성
- F 분포를 따르는 검정통계량 사용
- 유의한 차이가 있을 경우 사후검정(Tukey, Scheffe, Bonferroni 등) 필요
- 이원배치에서는 상호작용효과(교호작용)도 분석 가능

**개념의 활용**:

- 여러 처리 방법, 약품, 전략 비교
- 다양한 요인의 영향력 분석
- 품질에 영향을 미치는 요인 식별
- 실험 설계와 분석의 기초 방법

#### 비모수 검정

**정의**: 정규성 가정이 필요 없는 검정 방법(순위나 부호에 기반)

**주요 유형**:

- Mann-Whitney U 검정(윌콕슨 순위합 검정): 독립표본 t-검정의 비모수적 대안
- 윌콕슨 부호순위 검정: 대응표본 t-검정의 비모수적 대안
- Kruskal-Wallis 검정: 일원배치 분산분석의 비모수적 대안
- 카이제곱 검정: 범주형 변수의 독립성, 동질성, 적합도 검정

**코드 예시**:

```python
# 비모수 검정 예시
# Mann-Whitney U 검정(독립 2표본)
# 정규성을 만족하지 않는 데이터
group1_nonormal = np.random.exponential(scale=2, size=30)
group2_nonormal = np.random.exponential(scale=3, size=30)

u_stat, p_val = stats.mannwhitneyu(group1_nonormal, group2_nonormal, alternative='two-sided')
print(f"Mann-Whitney U 검정: 통계량={u_stat}, p값={p_val:.4f}")

# 윌콕슨 부호순위 검정(대응표본)
before_nonormal = np.random.exponential(scale=2, size=30)
after_nonormal = before_nonormal + np.random.exponential(scale=1, size=30)

w_stat, p_val = stats.wilcoxon(before_nonormal, after_nonormal)
print(f"윌콕슨 부호순위 검정: 통계량={w_stat}, p값={p_val:.4f}")

# Kruskal-Wallis 검정(3개 이상 집단)
group_a_nonormal = np.random.exponential(scale=2, size=30)
group_b_nonormal = np.random.exponential(scale=2.5, size=30)
group_c_nonormal = np.random.exponential(scale=3, size=30)

kw_stat, p_val = stats.kruskal(group_a_nonormal, group_b_nonormal, group_c_nonormal)
print(f"Kruskal-Wallis 검정: 통계량={kw_stat:.4f}, p값={p_val:.4f}")

# 카이제곱 독립성 검정
# 범주형 데이터 예시
contingency_table = np.array([[30, 10, 10], [15, 15, 20]])
chi2_stat, p_val, dof, expected = stats.chi2_contingency(contingency_table)
print(f"카이제곱 독립성 검정: 통계량={chi2_stat:.4f}, p값={p_val:.4f}, 자유도={dof}")
print("기대빈도:")
print(expected)
```

**특징**:

- 정규성 가정이 필요 없어 다양한 분포에 적용 가능
- 일반적으로 모수적 검정보다 검정력이 낮음
- 극단값에 덜 민감(robust)
- 표본 크기가 작을 때 유용
- 측정값이 아닌 순위를 사용하는 경우가 많음

**개념의 활용**:

- 서열척도 데이터 분석(만족도, 선호도 등)
- 비정규 분포를 따르는 자료 분석
- 표본 크기가 작은 연구
- 범주형 데이터의 관계 분석(독립성, 동질성)

### 카이제곱 검정

**정의**: 범주형 데이터를 분석하는 비모수적 방법

**주요 유형**:

- 적합도 검정: 관측빈도가 기대빈도와 일치하는지 검정
- 독립성 검정: 두 범주형 변수 간 관련성 유무 검정
- 동질성 검정: 여러 집단의 분포가 동일한지 검정

**수식**: $\chi^2 = \sum \frac{(O - E)^2}{E}$ (O: 관측빈도, E: 기대빈도)

**코드 예시**:

```python
# 카이제곱 검정 예시
# 적합도 검정
# 예: 주사위가 공정한지(모든 면이 동일한 확률로 나오는지)
observed = np.array([15, 10, 12, 8, 9, 16])  # 각 면이 나온 횟수
expected = np.ones(6) * np.sum(observed) / 6  # 기대빈도(동일확률)

chi2_stat, p_val = stats.chisquare(observed, expected)
print(f"카이제곱 적합도 검정: 통계량={chi2_stat:.4f}, p값={p_val:.4f}")

# 독립성 검정
# 예: 성별과 선호 색상의 관계
# 관측 데이터(행: 성별, 열: 색상)
observed = np.array([
    [30, 10, 25],  # 남성
    [15, 20, 30]   # 여성
])

chi2_stat, p_val, dof, expected = stats.chi2_contingency(observed)
print(f"카이제곱 독립성 검정: 통계량={chi2_stat:.4f}, p값={p_val:.4f}, 자유도={dof}")

# 동질성 검정(독립성 검정과 동일한 방법으로 수행)
# 다른 해석: 두 집단(행)의 분포가 동일한지 검정
```

**특징**:

- 귀무가설: 적합도(분포 일치), 독립성(변수 간 관계 없음), 동질성(분포 동일)
- 각 셀의 기대빈도가 5 미만인 경우가 전체의 20% 이상이면 피셔의 정확검정 권장
- 자유도: 적합도(k-1), 독립성((r-1)(c-1))
- 효과 크기: 크래머의 V, 파이계수 등으로 측정

**개념의 활용**:

- 설문조사 결과 분석(범주형 응답)
- 마케팅 전략과 구매 행동 관계 분석
- 질병과 위험요인의 연관성 평가
- 여러 집단 간 선호도/반응 패턴 비교

## 3. 가설검정의 실무 적용

### 검정 유형 선택 가이드

**변수 유형에 따른 선택**:

- 연속형 vs 연속형: 상관분석(Pearson, Spearman)
- 범주형 vs 범주형: 카이제곱 검정, 피셔의 정확검정
- 연속형 vs 범주형(2집단): t-검정, Mann-Whitney U 검정
- 연속형 vs 범주형(3집단+): 분산분석, Kruskal-Wallis 검정

**데이터 특성에 따른 선택**:

- 정규성 만족: 모수적 검정(t-검정, ANOVA 등)
- 정규성 불만족: 비모수 검정(Mann-Whitney, Kruskal-Wallis 등)
- 대응 표본: 대응표본 t-검정, 윌콕슨 부호순위 검정
- 독립 표본: 독립표본 t-검정, Mann-Whitney U 검정

**표본 크기에 따른 고려사항**:

- 대표본(n≥30): 중심극한정리로 정규성 가정 완화 가능
- 소표본(n<30): 정규성 검정 중요, 비모수 검정 고려
- 매우 작은 표본: 정확검정(피셔의 정확검정 등) 고려

### 실무 적용 순서

1. **문제 정의 및 가설 설정**
    
    - 분명한 연구 질문 정의
    - 귀무가설과 대립가설 명확히 설정
    - 적절한 유의수준(α) 결정
2. **적절한 검정 방법 선택**
    
    - 데이터 유형 확인(연속형/범주형)
    - 비교 집단 수 및 관계(독립/대응) 확인
    - 필요한 가정 검토(정규성, 등분산성 등)
3. **검정 수행 및 결과 해석**
    
    - 검정통계량 및 p-값 계산
    - 유의수준과 비교하여 가설 채택/기각 결정
    - 효과 크기 계산(단순 유의성 외 영향 크기 판단)
4. **결과 보고 및 시각화**
    
    - 검정 결과의 통계적/실질적 의미 해석
    - 검정력 분석 및 표본 크기 적절성 평가
    - 결과 시각화(상자그림, 막대그래프 등)

**코드 예시(종합)**:

```python
# 종합 예시: 두 처리법의 효과 비교
# 1. 데이터 준비
treatment_a = np.random.normal(loc=75, scale=12, size=40)
treatment_b = np.random.normal(loc=82, scale=15, size=45)

# 2. 가설 설정
# H0: μA = μB (두 처리법의 평균 효과는 동일하다)
# H1: μA ≠ μB (두 처리법의 평균 효과는 다르다)

# 3. 필요한 가정 검토
# 3.1. 정규성 검정
shapiro_a = stats.shapiro(treatment_a)
shapiro_b = stats.shapiro(treatment_b)
print(f"처리법 A 정규성 검정: W={shapiro_a.statistic:.4f}, p={shapiro_a.pvalue:.4f}")
print(f"처리법 B 정규성 검정: W={shapiro_b.statistic:.4f}, p={shapiro_b.pvalue:.4f}")

# 3.2. 등분산성 검정
levene_test = stats.levene(treatment_a, treatment_b)
print(f"등분산성 검정: W={levene_test.statistic:.4f}, p={levene_test.pvalue:.4f}")

# 4. 적절한 검정 선택 및 수행
# 정규성 만족 시 t-검정, 불만족 시 Mann-Whitney U 검정
if shapiro_a.pvalue > 0.05 and shapiro_b.pvalue > 0.05:
    # 정규성 만족
    if levene_test.pvalue > 0.05:
        # 등분산 가정
        t_stat, p_val = stats.ttest_ind(treatment_a, treatment_b, equal_var=True)
        test_name = "독립표본 t-검정(등분산)"
    else:
        # 이분산
        t_stat, p_val = stats.ttest_ind(treatment_a, treatment_b, equal_var=False)
        test_name = "Welch's t-검정(이분산)"
    
    print(f"{test_name}: t={t_stat:.4f}, p={p_val:.4f}")
    
    # 효과 크기(Cohen's d)
    mean_diff = np.mean(treatment_b) - np.mean(treatment_a)
    pooled_std = np.sqrt(((len(treatment_a) - 1) * np.var(treatment_a, ddof=1) + 
                          (len(treatment_b) - 1) * np.var(treatment_b, ddof=1)) / 
                         (len(treatment_a) + len(treatment_b) - 2))
    cohen_d = mean_diff / pooled_std
    print(f"효과 크기(Cohen's d): {cohen_d:.4f}")
else:
    # 정규성 불만족 → 비모수 검정
    u_stat, p_val = stats.mannwhitneyu(treatment_a, treatment_b, alternative='two-sided')
    print(f"Mann-Whitney U 검정: U={u_stat}, p={p_val:.4f}")
    
    # 효과 크기(r)
    r = u_stat / (len(treatment_a) * len(treatment_b))
    print(f"효과 크기(r): {r:.4f}")

# 5. 결과 시각화
plt.figure(figsize=(10, 6))
plt.boxplot([treatment_a, treatment_b], labels=['Treatment A', 'Treatment B'])
plt.title('Treatment Effects Comparison')
plt.ylabel('Effect Score')
plt.grid(True, linestyle='--', alpha=0.7)

# 유의성 표시
if p_val < 0.05:
    plt.annotate('*', xy=(1.5, max(np.max(treatment_a), np.max(treatment_b))), 
                 fontsize=20, ha='center')
```

## 4. ADP 기출 문제 유형 대응

### 검정력 및 표본 크기

**정의**: 검정력은 대립가설이 참일 때 이를 올바르게 검출할 확률(1-β)

**수식**:

- 표본 크기 계산: $n = \frac{(z_{\alpha/2} + z_{\beta})^2 \cdot 2\sigma^2}{d^2}$ (d: 검출하려는 평균 차이, α: 유의수준, β: 2종 오류)

**코드 예시**:

```python
# 검정력 분석
from statsmodels.stats.power import TTestIndPower, TTestPower

# 독립표본 t-검정의 검정력 계산
effect_size = 0.5  # 중간 효과 크기(Cohen's d)
alpha = 0.05
sample_size = 30  # 각 집단의 표본 크기

# 독립표본 t-검정 검정력
power_analysis = TTestIndPower()
power = power_analysis.power(effect_size, nobs1=sample_size, alpha=alpha)
print(f"독립표본 t-검정 검정력: {power:.4f}")

# 필요한 표본 크기 계산
target_power = 0.8
sample_size_needed = power_analysis.solve_power(effect_size, power=target_power, alpha=alpha)
print(f"목표 검정력({target_power})을 위한 필요 표본 크기: {np.ceil(sample_size_needed)}")

# 검정력 곡선
sample_sizes = np.arange(10, 100, 5)
powers = [power_analysis.power(effect_size, nobs1=n, alpha=alpha) for n in sample_sizes]

plt.figure(figsize=(10, 6))
plt.plot(sample_sizes, powers, 'b-', linewidth=2)
plt.axhline(y=0.8, color='r', linestyle='--', label='Target Power (0.8)')
plt.xlabel('Sample Size (per group)')
plt.ylabel('Statistical Power')
plt.title('Power Analysis for Independent t-test')
plt.grid(True)
plt.legend()
```

**특징**:

- 일반적으로 목표 검정력은 0.8(80%) 이상 권장
- 효과 크기, 유의수준, 표본 크기가 검정력에 영향
- 표본 크기 ↑, 효과 크기 ↑, 유의수준 ↑ → 검정력 ↑
- 효과 크기 해석: 작음(0.2), 중간(0.5), 큼(0.8)

**개념의 활용**:

- 연구 설계 단계에서 필요 표본 크기 산정
- 연구 결과의 신뢰성 평가
- 귀무가설 기각 실패 시 검정력 부족 여부 확인
- 실무적으로 의미 있는 효과 크기 설정

### 모델 비교 및 선택

**정의**: 여러 통계 모델 중 데이터를 가장 잘 설명하는 모델을 선택하는 방법

**주요 지표**:

- AIC(Akaike Information Criterion): 모델 적합도와 복잡성을 고려
- BIC(Bayesian Information Criterion): AIC보다 복잡성에 더 큰 페널티
- Adjusted R²: 설명력과 변수 수를 고려한 결정계수
- 우도비 검정(Likelihood Ratio Test): 중첩 모델 간 비교

**코드 예시**:

```python
# 모델 비교 예시
import statsmodels.api as sm
import numpy as np
import pandas as pd

# 가상 데이터 생성
np.random.seed(123)
X1 = np.random.normal(0, 1, 100)
X2 = np.random.normal(0, 1, 100)
X3 = np.random.normal(0, 1, 100)
y = 2 + 3*X1 + 1.5*X2 + 0.5*X3 + np.random.normal(0, 2, 100)

df = pd.DataFrame({'X1': X1, 'X2': X2, 'X3': X3, 'y': y})

# 여러 모델 구성
X_model1 = sm.add_constant(df[['X1']])
X_model2 = sm.add_constant(df[['X1', 'X2']])
X_model3 = sm.add_constant(df[['X1', 'X2', 'X3']])

model1 = sm.OLS(df['y'], X_model1).fit()
model2 = sm.OLS(df['y'], X_model2).fit()
model3 = sm.OLS(df['y'], X_model3).fit()

# 모델 비교
print("Model 1 (X1):")
print(f"AIC: {model1.aic:.4f}, BIC: {model1.bic:.4f}, Adj. R²: {model1.rsquared_adj:.4f}")

print("\nModel 2 (X1, X2):")
print(f"AIC: {model2.aic:.4f}, BIC: {model2.bic:.4f}, Adj. R²: {model2.rsquared_adj:.4f}")

print("\nModel 3 (X1, X2, X3):")
print(f"AIC: {model3.aic:.4f}, BIC: {model3.bic:.4f}, Adj. R²: {model3.rsquared_adj:.4f}")

# 우도비 검정(중첩 모델 비교)
from scipy import stats
def lrtest(llmin, llmax, df):
    lr = 2 * (llmax - llmin)
    p = stats.chi2.sf(lr, df)
    return lr, p

# Model 1 vs Model 2 (df = 1)
lr12, p12 = lrtest(model1.llf, model2.llf, 1)
print(f"\nLR Test (Model 1 vs 2): LR={lr12:.4f}, p={p12:.4f}")

# Model 2 vs Model 3 (df = 1)
lr23, p23 = lrtest(model2.llf, model3.llf, 1)
print(f"LR Test (Model 2 vs 3): LR={lr23:.4f}, p={p23:.4f}")
```

**특징**:

- AIC, BIC: 작을수록 좋음
- Adjusted R²: 클수록 좋음
- 우도비 검정: 중첩 모델에만 적용 가능
- 검정 결과가 유의하면 복잡한 모델 선택, 그렇지 않으면 단순 모델 선택

**개념의 활용**:

- 최적의 예측 모델 선택
- 변수 선택(feature selection)
- 모델 복잡성과 설명력 간 균형 찾기
- 여러 통계적 접근법 중 최선의 방법 결정

### 비모수적 상관분석

**정의**: 변수 간 관계의 강도와 방향을 비모수적으로 측정하는 방법

**주요 유형**:

- 스피어만 순위상관계수: 변수의 순위를 사용한 상관분석
- 켄달의 타우: 일치쌍과 불일치쌍의 비교에 기반한 상관계수

**수식**:

- 스피어만 상관계수: $r_s = 1 - \frac{6\sum d_i^2}{n(n^2-1)}$ (d: 순위 차이)
- 켄달의 타우: $\tau = \frac{n_c - n_d}{\frac{1}{2}n(n-1)}$ (nc: 일치쌍 수, nd: 불일치쌍 수)

**코드 예시**:

```python
# 비모수적 상관분석
# 정규분포를 따르지 않는 데이터
x = np.random.exponential(scale=2, size=50)
y = 3 * x + np.random.normal(0, 2, 50)  # 비선형 관계

# 피어슨 상관계수(모수적)
pearson_r, p_value = stats.pearsonr(x, y)
print(f"피어슨 상관계수: r={pearson_r:.4f}, p={p_value:.4f}")

# 스피어만 순위상관계수
spearman_r, p_value = stats.spearmanr(x, y)
print(f"스피어만 순위상관계수: r={spearman_r:.4f}, p={p_value:.4f}")

# 켄달의 타우
kendall_tau, p_value = stats.kendalltau(x, y)
print(f"켄달의 타우: τ={kendall_tau:.4f}, p={p_value:.4f}")

# 시각화
plt.figure(figsize=(10, 6))
plt.scatter(x, y, alpha=0.7)
plt.title(f'Correlation: Pearson={pearson_r:.2f}, Spearman={spearman_r:.2f}, Kendall={kendall_tau:.2f}')
plt.xlabel('Variable X')
plt.ylabel('Variable Y')
plt.grid(True, linestyle='--', alpha=0.7)
```

**특징**:

- 비정규 데이터나 순서형 데이터에 적합
- 이상치에 덜 민감(robust)
- 비선형 관계도 탐지 가능
- 피어슨은 선형 관계만 측정하지만, 비모수적 방법은 더 일반적인 단조 관계 측정

**개념의 활용**:

- 서열척도 변수 간 관계 분석
- 비선형 관계 탐지
- 이상치가 있는 데이터 분석
- 표본 크기가 작은 경우

### 기출 문제 유형별 대응 전략

#### 1. 정규성 및 등분산성 검정

**유형**: "데이터가 정규분포를 따르는지 확인하라" / "등분산 가정을 만족하는지 확인하라"

**대응 전략**:

1. 정규성 검정: Shapiro-Wilk, Kolmogorov-Smirnov 검정
2. 시각적 확인: Q-Q plot, 히스토그램 활용
3. 등분산성 검정: Levene, Bartlett 검정
4. 결과에 따른 후속 검정 선택 명시(모수/비모수)

#### 2. 집단 간 차이 검정

**유형**: "A, B 집단 간 차이가 있는지 검정하라" / "차이가 존재하는지 귀무가설과 대립가설을 설정하고 검정하라"

**대응 전략**:

1. 명확한 가설 설정(귀무가설, 대립가설)
2. 사전 가정 검정(정규성, 등분산성)
3. 적절한 검정 선택 및 수행
4. 검정 결과의 통계적/실무적 해석
5. 효과 크기 계산 및 해석

#### 3. 범주형 변수 관계 분석

**유형**: "두 범주형 변수 간 관계가 있는지 확인하라" / "교차표를 작성하고 독립성 검정을 실시하라"

**대응 전략**:

1. 교차표 작성
2. 기대빈도 계산 및 확인(5 미만 셀 확인)
3. 카이제곱 검정 또는 피셔의 정확검정 수행
4. 관련성 측도 계산(크래머의 V, 파이계수)
5. 잔차 분석으로 관계 패턴 해석

#### 4. 분포 적합도 검정

**유형**: "데이터가 포아송분포를 따르는지 확인하라" / "정규분포를 따르는지 두 가지 방법으로 검정하라"

**대응 전략**:

1. 이론적 분포의 매개변수 추정
2. 관측빈도와 기대빈도 계산
3. 카이제곱 적합도 검정 수행
4. Q-Q plot 등 시각적 검정 병행
5. Kolmogorov-Smirnov 검정 같은 대안적 방법 제시

이러한 통계적 추정과 가설검정의 개념들은 ADP에서 핵심적인 부분이며, 다양한 실무 상황에서 데이터 기반 의사결정의 토대가 됩니다. 특히 가설 설정, 검정 수행, 결과 해석의 체계적인 접근이 중요하며, 데이터 특성에 맞는 적절한 검정법 선택이 분석의 타당성을 결정합니다.