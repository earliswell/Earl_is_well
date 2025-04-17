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
