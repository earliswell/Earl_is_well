---
title: 01. Statistics
draft: false
tags:
  - "#statistics"
  - "#통계학"
  - "#probability"
  - "#확률"
---
# 기초통계량: 정의, 예시 및 코드 구현

기초통계량은 데이터셋의 특성을 요약하는 수치들로, 데이터 분석의 기본이 됩니다. 각 통계량의 정의와 파이썬 코드로 구현하는 방법을 살펴보겠습니다.

## 1. 평균 (Mean)

**정의**: 모든 값의 합을 값의 개수로 나눈 것입니다.

**수식**: $\bar{x} = \frac{\sum_{i=1}^{n}x_i}{n}$

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import scipy.stats as stats

# 데이터 생성
data = [4, 8, 6, 5, 3, 2, 8, 9, 2, 5]

# 평균 계산
mean_value = np.mean(data)  # 또는 sum(data)/len(data)
print(f"평균: {mean_value}")  # 평균: 5.2

# pandas 사용
df = pd.DataFrame({'값': data})
print(f"pandas 평균: {df['값'].mean()}")  # pandas 평균: 5.2
```

## 2. 중앙값 (Median)

**정의**: 데이터를 정렬했을 때 중앙에 위치하는 값입니다. 데이터의 개수가 짝수일 경우, 중앙에 위치한 두 값의 평균을 취합니다.

**특징**: 이상치(outlier)에 덜 민감하여 왜곡된 분포에서 중심 경향을 더 잘 나타냅니다.

**코드 예시**:

```python
# 중앙값 계산
median_value = np.median(data)
print(f"중앙값: {median_value}")  # 중앙값: 5.0

# pandas 사용
print(f"pandas 중앙값: {df['값'].median()}")  # pandas 중앙값: 5.0

# 이상치가 있는 경우 비교
data_with_outlier = data + [100]  # 이상치 추가
print(f"이상치 있는 데이터의 평균: {np.mean(data_with_outlier)}")  # 이상치 있는 데이터의 평균: 13.82
print(f"이상치 있는 데이터의 중앙값: {np.median(data_with_outlier)}")  # 이상치 있는 데이터의 중앙값: 5.0
```

## 3. 왜도 (Skewness)

**정의**: 분포의 비대칭성을 측정하는 지표입니다.

**해석**:

- 왜도 = 0: 대칭 분포 (정규분포)
- 왜도 > 0: 오른쪽으로 꼬리가 긴 분포 (양의 왜도, 우측 편향)
- 왜도 < 0: 왼쪽으로 꼬리가 긴 분포 (음의 왜도, 좌측 편향)

**수식**: $\text{Skewness} = \frac{1}{n}\sum_{i=1}^{n}\left(\frac{x_i-\bar{x}}{\sigma}\right)^3$

**코드 예시**:

```python
# 왜도 계산
skewness = stats.skew(data)
print(f"왜도: {skewness}")  # 왜도: 0.11262749661320474

# pandas 사용
print(f"pandas 왜도: {df['값'].skew()}")  # pandas 왜도: 0.11262749661320474

# 다양한 왜도 시각화
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

# 양의 왜도 (오른쪽으로 긴 꼬리)
positive_skew = np.random.exponential(size=1000)
axes[0].hist(positive_skew, bins=30)
axes[0].set_title(f'양의 왜도: {stats.skew(positive_skew):.2f}')

# 대칭 분포 (왜도 ≈ 0)
normal_dist = np.random.normal(size=1000)
axes[1].hist(normal_dist, bins=30)
axes[1].set_title(f'대칭 분포 (왜도 ≈ 0): {stats.skew(normal_dist):.2f}')

# 음의 왜도 (왼쪽으로 긴 꼬리)
negative_skew = -np.random.exponential(size=1000)
axes[2].hist(negative_skew, bins=30)
axes[2].set_title(f'음의 왜도: {stats.skew(negative_skew):.2f}')

plt.tight_layout()
plt.show()
```

## 4. 첨도 (Kurtosis)

**정의**: 분포의 뾰족한 정도를 측정하는 지표입니다. 정규분포 대비 꼬리 부분의 두꺼움을 나타냅니다.

**해석**:

- 첨도 = 3: 정규분포 (메소커틱, mesokurtic)
- 첨도 > 3: 정규분포보다 뾰족한 분포 (렙토커틱, leptokurtic)
- 첨도 < 3: 정규분포보다 완만한 분포 (플래티커틱, platykurtic)

**참고**: `scipy.stats`에서는 정규분포의 첨도를 0으로 조정한 "초과 첨도(excess kurtosis)"를 사용합니다. 즉, 정규분포의 첨도인 3을 뺀 값을 반환합니다.

**수식**: $\text{Kurtosis} = \frac{1}{n}\sum_{i=1}^{n}\left(\frac{x_i-\bar{x}}{\sigma}\right)^4$

**코드 예시**:

```python
# 첨도 계산 (scipy.stats는 초과 첨도를 반환)
kurtosis = stats.kurtosis(data)
print(f"초과 첨도: {kurtosis}")  # 초과 첨도: -0.9804899212734136
print(f"첨도: {kurtosis + 3}")  # 첨도: 2.0195100787265864

# pandas 사용
print(f"pandas 초과 첨도: {df['값'].kurtosis()}")  # pandas 초과 첨도: -0.9804899212734136

# 다양한 첨도 시각화
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

# 높은 첨도 (뾰족한 분포)
high_kurtosis = np.random.normal(size=1000) * 0.5 + np.random.laplace(size=1000) * 0.5
axes[0].hist(high_kurtosis, bins=30)
axes[0].set_title(f'높은 첨도: {stats.kurtosis(high_kurtosis) + 3:.2f}')

# 정규분포 (첨도 = 3)
normal_dist = np.random.normal(size=1000)
axes[1].hist(normal_dist, bins=30)
axes[1].set_title(f'정규분포 (첨도 = 3): {stats.kurtosis(normal_dist) + 3:.2f}')

# 낮은 첨도 (완만한 분포)
low_kurtosis = np.random.uniform(size=1000)
axes[2].hist(low_kurtosis, bins=30)
axes[2].set_title(f'낮은 첨도: {stats.kurtosis(low_kurtosis) + 3:.2f}')

plt.tight_layout()
plt.show()
```

## 5. 분위수 (Quantiles)

**정의**: 데이터를 크기 순서대로 나열했을 때 특정 비율에 해당하는 값입니다.

**주요 분위수**:

- 중앙값 (Median): 50% 분위수
- 사분위수 (Quartiles): 25%(Q1), 50%(Q2), 75%(Q3) 분위수
- 십분위수 (Deciles): 10%, 20%, ..., 90% 분위수
- 백분위수 (Percentiles): 1%, 2%, ..., 99% 분위수

**코드 예시**:

```python
# 분위수 계산
q1 = np.percentile(data, 25)  # 1사분위수 (25%)
q2 = np.percentile(data, 50)  # 2사분위수 (50%, 중앙값)
q3 = np.percentile(data, 75)  # 3사분위수 (75%)

print(f"1사분위수(Q1): {q1}")  # 1사분위수(Q1): 3.0
print(f"2사분위수(Q2): {q2}")  # 2사분위수(Q2): 5.0
print(f"3사분위수(Q3): {q3}")  # 3사분위수(Q3): 8.0

# pandas 사용
quantiles = df['값'].quantile([0.25, 0.5, 0.75])
print("pandas 분위수:")
print(quantiles)

# 여러 분위수 한번에 계산
deciles = np.percentile(data, np.arange(0, 101, 10))
print(f"십분위수(Deciles): {deciles}")

# 분위수 시각화 (박스플롯)
plt.figure(figsize=(8, 6))
plt.boxplot(data)
plt.title('데이터의 분위수 표현 (박스플롯)')
plt.grid(True)
plt.show()
```

## 6. 분산 (Variance)

**정의**: 데이터가 평균으로부터 퍼져 있는 정도를 측정합니다. 각 값과 평균의 차이를 제곱한 값들의 평균입니다.

**수식**: $\sigma^2 = \frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n}$ (모분산) $s^2 = \frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}$ (표본분산)

**특징**: 제곱을 취하기 때문에 원래 데이터와 단위가 다릅니다.

**코드 예시**:

```python
# 분산 계산 (표본 분산 계산 - 자유도 n-1 사용)
variance = np.var(data, ddof=1)  # ddof=1: 자유도 1 (n-1로 나눔)
print(f"표본 분산: {variance}")  # 표본 분산: 6.622222222222222

# 모분산 계산 (n으로 나눔)
pop_variance = np.var(data, ddof=0)  # ddof=0: 자유도 0 (n으로 나눔)
print(f"모 분산: {pop_variance}")  # 모 분산: 5.96

# pandas 사용 (기본적으로 표본 분산 계산)
print(f"pandas 분산: {df['값'].var()}")  # pandas 분산: 6.622222222222222
```

## 7. 표준편차 (Standard Deviation)

**정의**: 분산의 제곱근으로, 데이터가 평균으로부터 얼마나 퍼져 있는지를 원래 데이터와 같은 단위로 나타냅니다.

**수식**: $\sigma = \sqrt{\sigma^2}$ (모표준편차) $s = \sqrt{s^2}$ (표본표준편차)

**코드 예시**:

```python
# 표준편차 계산 (표본 표준편차)
std_dev = np.std(data, ddof=1)
print(f"표본 표준편차: {std_dev}")  # 표본 표준편차: 2.5732970374516482

# 모표준편차 계산
pop_std_dev = np.std(data, ddof=0)
print(f"모 표준편차: {pop_std_dev}")  # 모 표준편차: 2.4413111231467406

# pandas 사용 (기본적으로 표본 표준편차 계산)
print(f"pandas 표준편차: {df['값'].std()}")  # pandas 표준편차: 2.5732970374516482

# 표준편차 시각화
plt.figure(figsize=(10, 6))
plt.bar(range(len(data)), data, alpha=0.7)
plt.axhline(mean_value, color='r', linestyle='-', label=f'평균: {mean_value}')
plt.axhline(mean_value + std_dev, color='g', linestyle='--', label=f'평균 + 표준편차: {mean_value + std_dev:.2f}')
plt.axhline(mean_value - std_dev, color='g', linestyle='--', label=f'평균 - 표준편차: {mean_value - std_dev:.2f}')
plt.legend()
plt.title('데이터, 평균, 표준편차 시각화')
plt.grid(True)
plt.show()
```

## 8. 변동계수 (Coefficient of Variation)

**정의**: 표준편차를 평균으로 나눈 값으로, 상대적인 분산 정도를 측정합니다. 단위가 다른 데이터 간의 산포도 비교에 유용합니다.

**수식**: $CV = \frac{s}{\bar{x}} \times 100\%$

**특징**: 평균이 큰 데이터셋과 작은 데이터셋의 변동성을 비교할 때 유용합니다.

**코드 예시**:

```python
# 변동계수 계산
cv = (std_dev / mean_value) * 100
print(f"변동계수: {cv:.2f}%")  # 변동계수: 49.49%

# 두 데이터셋 비교 예시
data1 = [100, 105, 95, 102, 98]  # 평균 100, 범위 10
data2 = [10, 10.5, 9.5, 10.2, 9.8]  # 평균 10, 범위 1

std1 = np.std(data1, ddof=1)
mean1 = np.mean(data1)
cv1 = (std1 / mean1) * 100

std2 = np.std(data2, ddof=1)
mean2 = np.mean(data2)
cv2 = (std2 / mean2) * 100

print(f"데이터셋1 - 평균: {mean1}, 표준편차: {std1:.2f}, 변동계수: {cv1:.2f}%")
print(f"데이터셋2 - 평균: {mean2}, 표준편차: {std2:.2f}, 변동계수: {cv2:.2f}%")
```

## 기초통계량의 활용

1. **데이터 요약**: 대용량 데이터셋의 특성을 압축적으로 파악할 수 있습니다.
2. **이상치 감지**: 평균, 중앙값, 표준편차를 통해 잠재적 이상치를 식별할 수 있습니다.
3. **데이터 분포 이해**: 왜도와 첨도를 통해 데이터의 분포 형태를 파악할 수 있습니다.
4. **데이터셋 비교**: 변동계수를 통해 서로 다른 규모의 데이터셋의 산포도를 비교할 수 있습니다.
5. **통계적 추론**: 표본에서 계산된 통계량을 활용해 모집단에 대한 추론을 할 수 있습니다.
---
## 확률
#### 조건부 확률

**정의**: 조건부 확률은 특정 사건 B가 발생했다는 전제 하에서 다른 사건 A가 발생할 확률을 의미합니다. 즉, "B가 주어졌을 때 A의 확률"입니다.

**수식**:

$$P(A|B) = \frac{P(A \cap B)}{P(B)}$$
- P(A|B): B가 주어졌을 때 A의 확률
- P(A ∩ B): A와 B가 동시에 일어날 확률(교집합)
- P(B): 사건 B가 일어날 확률 (단, P(B) > 0)

**특징**

1. 범위: 조건부 확률도 일반 확률과 마찬가지로 0과 1 사이의 값을 가집니다.
2. 독립 사건: 두 사건 A와 B가 독립이면 P(A|B) = P(A)입니다.
3. 곱셈 법칙: P(A ∩ B) = P(B) × P(A|B)
4. 전체 확률 법칙: 표본 공간의 분할 B₁, B₂, ..., Bₙ에 대해 P(A) = Σᵢ P(A|Bᵢ)P(Bᵢ)
5. 베이즈 정리: P(A|B) = \[P(B|A)P(A)] / P(B)

**코드 예시**:
```python
import numpy as np
import pandas as pd

# 예시 1: 카드 뽑기 조건부 확률
def card_conditional_prob():
    # 카드 정의
    suits = ['Heart', 'Diamond', 'Club', 'Spade']
    ranks = ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']
    
    # 모든 카드 조합
    cards = [(suit, rank) for suit in suits for rank in ranks]
    
    # 사건 정의
    event_A = [(suit, rank) for suit, rank in cards if rank == 'A']  # 에이스
    event_B = [(suit, rank) for suit, rank in cards if suit == 'Heart']  # 하트 
    event_A_and_B = [(suit, rank) for suit, rank in cards if suit == 'Heart' and rank == 'A']  # 하트 에이스
    
    # 확률 계산
    P_A = len(event_A) / len(cards)  # P(에이스)
    P_B = len(event_B) / len(cards)  # P(하트)
    P_A_and_B = len(event_A_and_B) / len(cards)  # P(하트 에이스)
    
    # 조건부 확률 P(A|B): 하트 중에서 에이스가 나올 확률
    P_A_given_B = P_A_and_B / P_B
    
    return {
        'P(A)': P_A,
        'P(B)': P_B,
        'P(A∩B)': P_A_and_B,
        'P(A|B)': P_A_given_B
    }

# 함수 실행
results = card_conditional_prob()
print(f"P(에이스) = {results['P(A)']}")
print(f"P(하트) = {results['P(B)']}")
print(f"P(하트 에이스) = {results['P(A∩B)']}")
print(f"P(에이스|하트) = {results['P(A|B)']}")
```

# 확률분포 개념

## 1. 공분산과 상관계수

**정의**:

- 공분산: 두 확률변수의 선형 관계 정도를 나타내는 값으로, 두 변수가 함께 변화하는 경향을 측정합니다.
- 상관계수: 공분산을 각 변수의 표준편차의 곱으로 나눈 값으로, -1에서 1 사이의 값을 가지며 선형 관계의 강도와 방향을 나타냅니다.

**수식**:

- 공분산: $Cov(X,Y) = E[(X-\mu_X)(Y-\mu_Y)] = E[XY] - E[X]E[Y]$
- 상관계수: $\rho_{X,Y} = \frac{Cov(X,Y)}{\sigma_X \sigma_Y}$

**특징**:

- 공분산은 단위가 있고, 상관계수는 단위가 없는 표준화된 측정치입니다.
- 상관계수가 1이면 완벽한 양의 선형관계, -1이면 완벽한 음의 선형관계, 0이면 선형관계가 없음을 의미합니다.
- 두 변수가 독립이면 공분산과 상관계수는 0이지만, 역은 성립하지 않습니다(상관계수가 0이어도 독립이 아닐 수 있음).

**코드 예시**:

```python
import numpy as np

# 두 변수 생성
x = np.array([1, 2, 3, 4, 5])
y = np.array([2, 4, 5, 4, 5])

# 공분산 계산
cov_xy = np.cov(x, y)[0, 1]
print(f"공분산: {cov_xy}")

# 상관계수 계산
corr_xy = np.corrcoef(x, y)[0, 1]
print(f"상관계수: {corr_xy}")
```

**개념의 활용**:

- 두 변수 간의 관계를 탐색할 때(EDA 과정에서)
- 예측 모델에서 다중공선성을 진단할 때
- 포트폴리오 구성 및 리스크 관리 시 자산 간 상관관계 분석 시
- 차원 축소 기법(PCA)의 기초 개념으로 활용될 때
- 회귀분석에서 변수 선택 시 다중공선성 문제를 확인할 때

## 2. 베르누이 분포

**정의**: 성공 확률이 p인 단 한 번의 시행에서 성공(1) 또는 실패(0)와 같이 두 가지 결과만 가능한 확률분포입니다.

**수식**:

- 확률질량함수: $P(X=x) = p^x(1-p)^{1-x}, x \in {0,1}$
- 평균: $E[X] = p$
- 분산: $Var(X) = p(1-p)$

**특징**:

- 가장 단순한 이산형 확률분포입니다.
- 결과는 반드시 0 또는 1입니다.
- 이항분포의 특수한 경우(n=1)입니다.
- 모든 시행이 독립적입니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt

# 베르누이 시행 시뮬레이션 (p=0.3)
p = 0.3
bernoulli_trials = np.random.binomial(n=1, p=p, size=1000)

# 결과 빈도 계산 및 시각화
values, counts = np.unique(bernoulli_trials, return_counts=True)
plt.bar(values, counts/1000)
plt.xlabel('결과 (0=실패, 1=성공)')
plt.ylabel('상대 빈도')
plt.title(f'베르누이 분포 (p={p})')
plt.show()
```

**개념의 활용**:

- 동전 던지기, 합격/불합격 등 이진 결과를 모델링할 때
- 로지스틱 회귀의 출력 결과를 해석할 때
- A/B 테스트 분석 시
- 기계학습에서 이진 분류 문제의 확률적 기반으로 사용할 때
- 베이지안 네트워크에서 이진 노드를 표현할 때

## 3. 이항분포

**정의**: 성공 확률이 p인 독립적인 n번의 베르누이 시행에서 성공 횟수 X의 확률분포입니다.

**수식**:

- 확률질량함수: $P(X=k) = \binom{n}{k}p^k(1-p)^{n-k}, k=0,1,...,n$
- 평균: $E[X] = np$
- 분산: $Var(X) = np(1-p)$

**특징**:

- 각 시행은 독립적입니다.
- 시행 횟수 n이 고정되어 있습니다.
- 모든 시행에서 성공 확률 p는 동일합니다.
- n이 큰 경우 정규분포로 근사할 수 있습니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import binom

# 이항분포 확률질량함수
n, p = 10, 0.3
k = np.arange(0, n+1)
pmf = binom.pmf(k, n, p)

# 시각화
plt.bar(k, pmf)
plt.xlabel('성공 횟수 (k)')
plt.ylabel('확률')
plt.title(f'이항분포 B({n}, {p})')
plt.show()
```

**개념의 활용**:

- 고정된 수의 독립적인 시행에서 성공 횟수를 모델링할 때
- 품질 관리에서 불량품 개수 예측 시
- 여론조사나 설문조사 결과의 신뢰구간 계산 시
- 임상 시험에서 치료 효과 분석 시
- A/B 테스트에서 전환율(conversion rate) 비교 시

## 4. 음이항분포

**정의**: 성공 확률이 p인 베르누이 시행에서, r번째 성공을 달성하기 위해 필요한 총 시행 횟수 X의 확률분포입니다.

**수식**:

- 확률질량함수: $P(X=k) = \binom{k-1}{r-1}p^r(1-p)^{k-r}, k=r,r+1,...$
- 평균: $E[X] = \frac{r}{p}$
- 분산: $Var(X) = \frac{r(1-p)}{p^2}$

**특징**:

- 성공 횟수 r이 고정되어 있습니다.
- 시행 횟수는 변수로, 이론적으로는 무한대가 될 수 있습니다.
- 각 시행은 독립적입니다.
- 기하분포는 r=1인 특수한 경우입니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import nbinom

# 음이항분포 확률질량함수
r, p = 3, 0.3  # 3번째 성공을 위한 분포
k = np.arange(r, 20)  # r부터 시작
pmf = nbinom.pmf(k-r, r, p)  # scipy에서는 k-r로 표현

# 시각화
plt.bar(k, pmf)
plt.xlabel('시행 횟수 (k)')
plt.ylabel('확률')
plt.title(f'음이항분포 NB({r}, {p})')
plt.show()
```

**개념의 활용**:

- 목표 성공 횟수를 달성하기 위한 시도 횟수를 모델링할 때
- 특정 수의 고객을 획득하기까지 필요한 마케팅 접촉 횟수 예측 시
- 품질 관리에서 r번째 불량품 발견까지의 생산 개수 예측 시
- 스포츠 경기에서 특정 수의 득점을 달성하기까지의 시도 횟수 예측 시
- 의약품 임상시험에서 목표 환자 수 모집에 필요한 시간 예측 시

## 5. 초기하분포

**정의**: 크기가 N인 유한 모집단에서, M개의 성공과 N-M개의 실패가 있을 때, 크기 n인 비복원 추출 표본에서 얻은 성공 횟수 X의 확률분포입니다.

**수식**:

- 확률질량함수: $P(X=k) = \frac{\binom{M}{k}\binom{N-M}{n-k}}{\binom{N}{n}}, max(0, n+M-N) \leq k \leq min(n, M)$
- 평균: $E[X] = n\frac{M}{N}$
- 분산: $Var(X) = n\frac{M}{N}(1-\frac{M}{N})(\frac{N-n}{N-1})$

**특징**:

- 비복원 추출이므로 각 시행은 독립적이지 않습니다.
- 모집단 크기 N, 성공 항목 수 M, 표본 크기 n이 모두 고정되어 있습니다.
- N이 매우 크고 n이 상대적으로 작을 때는 이항분포로 근사됩니다.
- 유한 모집단 수정 계수 $(N-n)/(N-1)$가 있습니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import hypergeom

# 초기하분포 확률질량함수
N, M, n = 50, 20, 10  # 전체 50개 중 20개가 성공, 10개 추출
k = np.arange(0, min(n, M) + 1)
pmf = hypergeom.pmf(k, N, M, n)

# 시각화
plt.bar(k, pmf)
plt.xlabel('성공 횟수 (k)')
plt.ylabel('확률')
plt.title(f'초기하분포 H({N}, {M}, {n})')
plt.show()
```

**개념의 활용**:

- 비복원 추출 상황에서 성공 횟수를 모델링할 때
- 카드 게임에서 특정 카드를 뽑을 확률 계산 시
- 품질 관리에서 배치 샘플링 검사 시
- 선거 투표용지 재검표 시 표본 추출 설계 시
- 생태학에서 표본을 통한 개체 수 추정 시

## 6. 포아송분포

**정의**: 주어진 시간 또는 공간에서 사건이 발생하는 평균 횟수가 λ일 때, 실제 발생 횟수 X의 확률분포입니다.

**수식**:

- 확률질량함수: $P(X=k) = \frac{\lambda^k e^{-\lambda}}{k!}, k=0,1,2,...$
- 평균: $E[X] = \lambda$
- 분산: $Var(X) = \lambda$

**특징**:

- 평균과 분산이 같습니다(λ).
- 사건 발생은 서로 독립적입니다.
- 작은 시간/공간 단위로 쪼개면 사건 발생 확률은 그 단위에 비례합니다.
- 고정된 시간/공간 안에서 사건이 발생할 횟수의 분포입니다.
- n이 크고 p가 작은 이항분포(n,p)는 λ=np인 포아송분포로 근사됩니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import poisson

# 포아송분포 확률질량함수
lambda_val = 3.5  # 평균 발생 횟수
k = np.arange(0, 15)
pmf = poisson.pmf(k, lambda_val)

# 시각화
plt.bar(k, pmf)
plt.xlabel('사건 발생 횟수 (k)')
plt.ylabel('확률')
plt.title(f'포아송분포 Pois({lambda_val})')
plt.show()
```

**개념의 활용**:

- 단위 시간당 고객 도착, 전화 건수, 사고 발생과 같은 이벤트 횟수 모델링 시
- 단위 면적당 입자, 세포, 별과 같은 개체 수 모델링 시
- 서비스 요청 대기열 분석 시
- 네트워크 트래픽 분석 시
- 보험 및 금융에서 클레임 발생 빈도 모델링 시

## 7. 이산형균일분포

**정의**: 유한한 범위의 정수값들이 모두 같은 확률로 발생하는 확률분포입니다.

**수식**:

- 확률질량함수: $P(X=k) = \frac{1}{b-a+1}, k=a,a+1,...,b$
- 평균: $E[X] = \frac{a+b}{2}$
- 분산: $Var(X) = \frac{(b-a+1)^2 - 1}{12}$

**특징**:

- 모든 가능한 값이 동일한 확률을 가집니다.
- 범위 [a,b] 내의 모든 정수값이 취할 수 있는 값입니다.
- 공정한 주사위나 랜덤 숫자 생성기의 기본 모델입니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import randint

# 이산형균일분포 확률질량함수
a, b = 1, 6  # 주사위 (1부터 6까지)
k = np.arange(a, b+1)
pmf = np.ones_like(k) / (b - a + 1)  # 모든 값의 확률은 동일

# 시각화
plt.bar(k, pmf)
plt.xlabel('결과값 (k)')
plt.ylabel('확률')
plt.title(f'이산형균일분포 DU({a}, {b})')
plt.show()
```

**개념의 활용**:

- 공정한 주사위, 룰렛, 카드 선택과 같은 게임의 결과 모델링 시
- 랜덤 숫자 생성 시
- 몬테카를로 시뮬레이션의 기초 분포로 사용할 때
- 동일한 확률을 가진 옵션 중 무작위 선택을 모델링할 때
- 데이터 샘플링 과정에서 무작위 인덱스 생성 시

## 8. 지수분포

**정의**: 연속적인 시간 또는 공간에서, 사건 발생 간의 대기 시간이나 거리를 모델링하는 연속확률분포입니다.

**수식**:

- 확률밀도함수: $f(x) = \lambda e^{-\lambda x}, x \geq 0$
- 평균: $E[X] = \frac{1}{\lambda}$
- 분산: $Var(X) = \frac{1}{\lambda^2}$

**특징**:

- 기억이 없는 특성(memoryless property)을 가집니다: $P(X > s+t | X > s) = P(X > t)$
- 포아송 과정에서 발생 간 대기 시간은 지수분포를 따릅니다.
- 생존분석에서 중요한 분포입니다.
- 파라미터 λ는 단위 시간당 사건 발생률입니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import expon

# 지수분포 확률밀도함수
lambda_val = 0.5  # 사건 발생률
x = np.linspace(0, 10, 1000)
pdf = expon.pdf(x, scale=1/lambda_val)  # scipy에서는 scale=1/lambda 사용

# 시각화
plt.plot(x, pdf)
plt.xlabel('시간/거리 (x)')
plt.ylabel('확률밀도')
plt.title(f'지수분포 Exp({lambda_val})')
plt.grid(True)
plt.show()
```

**개념의 활용**:

- 고객 서비스 시간, 제품 수명, 대기 시간 모델링 시
- 장비 고장 시간 예측 시
- 생존 분석에서 사망이나 실패까지의 시간 모델링 시
- 대기열 이론에서 서비스 시간 모델링 시
- 신뢰성 공학에서 부품 수명 예측 시

## 9. 정규분포

**정의**: 자연계의 많은 현상에서 관찰되는 종 모양의 확률분포로, 평균 μ와 표준편차 σ에 의해 특징지어집니다.

**수식**:

- 확률밀도함수: $f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{1}{2}(\frac{x-\mu}{\sigma})^2}, -\infty < x < \infty$
- 평균: $E[X] = \mu$
- 분산: $Var(X) = \sigma^2$

**특징**:

- 평균, 중앙값, 최빈값이 모두 같은 값 μ입니다.
- 분포는 μ를 중심으로 좌우 대칭입니다.
- 68-95-99.7 규칙: 데이터의 약 68%는 μ±σ 내에, 95%는 μ±2σ 내에, 99.7%는 μ±3σ 내에 있습니다.
- 중심극한정리에 의해 많은 독립적인 확률변수의 합은 정규분포에 가까워집니다.
- 표준정규분포는 μ=0, σ=1인 특수한 경우입니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import norm

# 정규분포 확률밀도함수
mu, sigma = 0, 1  # 평균과 표준편차
x = np.linspace(-4, 4, 1000)
pdf = norm.pdf(x, mu, sigma)

# 시각화
plt.plot(x, pdf)
plt.xlabel('값 (x)')
plt.ylabel('확률밀도')
plt.title(f'정규분포 N({mu}, {sigma}²)')
plt.grid(True)
plt.axvline(mu, color='red', linestyle='--', alpha=0.3)  # 평균선
plt.axvline(mu + sigma, color='green', linestyle='--', alpha=0.3)  # μ+σ
plt.axvline(mu - sigma, color='green', linestyle='--', alpha=0.3)  # μ-σ
plt.show()
```

**개념의 활용**:

- 키, 몸무게, IQ 점수와 같은 자연적 측정값 모델링 시
- 오차와 노이즈 모델링 시
- 통계적 추론 및 가설 검정의 기초로 사용할 때
- 금융에서 자산 수익률 모델링 시
- 품질 관리에서 제조 공정 변동 분석 시

# 추정

## 1. 점추정 (Point Estimation)

**정의**: 표본 데이터를 사용하여 모집단의 미지의 매개변수(모수)에 대한 단일 값을 추정하는 방법입니다.

**수식**:

- 표본평균: $\bar{X} = \frac{1}{n}\sum_{i=1}^{n}X_i$
- 표본분산: $S^2 = \frac{1}{n-1}\sum_{i=1}^{n}(X_i - \bar{X})^2$
- 표본비율: $\hat{p} = \frac{X}{n}$ (X는 성공 횟수)

**특징**:

- 하나의 값으로 모수를 추정합니다
- 추정량의 품질은 불편성(unbiasedness), 일관성(consistency), 효율성(efficiency) 등으로 평가합니다
- 최대우도추정법(MLE), 적률추정법(Method of Moments), 최소제곱법(Least Squares) 등의 방법이 있습니다
- 정확한 값이 아닌 근사치를 제공합니다

**코드 예시**:

```python
import numpy as np

# 데이터 생성
data = np.random.normal(loc=50, scale=5, size=100)

# 점추정 - 평균
mean_estimate = np.mean(data)
print(f"모평균 점추정값: {mean_estimate:.4f}")

# 점추정 - 분산
var_estimate = np.var(data, ddof=1)
print(f"모분산 점추정값: {var_estimate:.4f}")

# 점추정 - 비율 (이진 데이터의 경우)
binary_data = np.random.binomial(n=1, p=0.3, size=100)
prop_estimate = np.mean(binary_data)
print(f"모비율 점추정값: {prop_estimate:.4f}")
```

**개념의 활용**:

- 모집단의 평균 소득, 키, 무게 등을 단일 값으로 추정해야 할 때
- 통계 모델 구축 시 최적의 파라미터를 찾아야 할 때
- 품질 관리에서 제품의 평균 불량률을 추정해야 할 때
- 의약품 임상 시험에서 약효의 크기를 단일값으로 보고해야 할 때
- 선거 예측에서 특정 후보의 득표율을 추정할 때

## 2. 구간추정 (Interval Estimation)

**정의**: 모수가 특정 신뢰수준으로 포함될 것으로 예상되는 값의 범위를 추정하는 방법입니다.

**수식**:

- 일반적인 형태: $\hat{\theta} \pm \text{(신뢰계수)} \times \text{(표준오차)}$
- 신뢰구간: $[L, U]$, L은 하한, U는 상한

**특징**:

- 모수의 불확실성을 명시적으로 나타냅니다
- 신뢰수준(일반적으로 95%)은 유사한 표본을 계속 추출할 때 해당 비율만큼 실제 모수를 포함하는 구간이 얻어짐을 의미합니다
- 표본 크기가 클수록 구간이 좁아집니다
- 통계적 가설 검정과 밀접한 관련이 있습니다

**코드 예시**:

```python
import numpy as np
import scipy.stats as stats

# 데이터 생성
data = np.random.normal(loc=50, scale=5, size=30)

# 평균에 대한 95% 신뢰구간
mean_estimate = np.mean(data)
std_err = stats.sem(data)  # 표준오차
ci_95 = stats.t.interval(0.95, len(data)-1, loc=mean_estimate, scale=std_err)

print(f"95% 신뢰구간: ({ci_95[0]:.4f}, {ci_95[1]:.4f})")
```

**개념의 활용**:

- 연구 결과의 불확실성을 정량화해야 할 때
- 정책 결정에서 가능한 결과 범위를 고려해야 할 때
- 품질 관리에서 제품 특성의 변동 범위를 설정할 때
- 의학 연구에서 치료 효과의 가능한 범위를 제시할 때
- 여론 조사 결과 보고 시 오차 범위를 표시해야 할 때

## 3. 모평균에 대한 구간추정 (Confidence Interval for Population Mean)

**정의**: 모집단의 평균 μ를 특정 신뢰수준으로 포함할 것으로 예상되는 구간을 추정하는 방법입니다.

**수식**:

- 모분산을 알 때: $\bar{X} \pm z_{\alpha/2} \frac{\sigma}{\sqrt{n}}$
- 모분산을 모를 때: $\bar{X} \pm t_{\alpha/2, n-1} \frac{s}{\sqrt{n}}$

**특징**:

- 표본크기가 30 이상이면 보통 정규분포 사용
- 표본크기가 작으면 t분포 사용
- 모집단이 정규분포를 따른다고 가정하거나 중심극한정리를 적용
- 신뢰수준이 높을수록 구간이 넓어짐

**코드 예시**:

```python
import numpy as np
import scipy.stats as stats

# 데이터 생성
data = np.random.normal(loc=50, scale=5, size=25)

# 표본 통계량 계산
mean = np.mean(data)
std = np.std(data, ddof=1)
n = len(data)

# 95% 신뢰구간 계산 (t분포 사용)
alpha = 0.05
t_critical = stats.t.ppf(1 - alpha/2, df=n-1)
margin_error = t_critical * (std / np.sqrt(n))

ci_lower = mean - margin_error
ci_upper = mean + margin_error

print(f"모평균의 95% 신뢰구간: ({ci_lower:.4f}, {ci_upper:.4f})")
```

**개념의 활용**:

- 제조업체의 제품 평균 수명 추정 시
- 약물 임상시험에서 평균 효과 크기 추정 시
- 학생들의 평균 시험 점수 추정 시
- 소비자의 제품 평균 만족도 측정 시
- 공정 관리에서 평균 생산량 예측 시

## 4. 모비율에 대한 구간추정 (Confidence Interval for Population Proportion)

**정의**: 모집단의 비율 p를 특정 신뢰수준으로 포함할 것으로 예상되는 구간을 추정하는 방법입니다.

**수식**: $\hat{p} \pm z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$

**특징**:

- $n\hat{p} \geq 5$와 $n(1-\hat{p}) \geq 5$일 때 정규근사 사용 가능
- 이항분포의 근사로 정규분포 사용
- 추정 비율이 0.5에 가까울수록 더 넓은 구간이 생성됨
- 표본크기가 클수록 구간이 좁아짐

**코드 예시**:

```python
import numpy as np
import scipy.stats as stats

# 데이터 생성 (300명 중 120명이 찬성)
n = 300
successes = 120
p_hat = successes / n

# 95% 신뢰구간 계산
alpha = 0.05
z_critical = stats.norm.ppf(1 - alpha/2)
std_err = np.sqrt((p_hat * (1 - p_hat)) / n)
margin_error = z_critical * std_err

ci_lower = max(0, p_hat - margin_error)  # 0보다 작을 수 없음
ci_upper = min(1, p_hat + margin_error)  # 1보다 클 수 없음

print(f"모비율의 95% 신뢰구간: ({ci_lower:.4f}, {ci_upper:.4f})")
```

**개념의 활용**:

- 선거 여론조사에서 후보 지지율 추정 시
- 마케팅에서 광고 전환율 측정 시
- 의학 연구에서 치료 성공률 분석 시
- 품질 관리에서 제품 불량률 추정 시
- 사회조사에서 특정 의견 지지 비율 파악 시

## 5. 모분산에 대한 구간추정 (Confidence Interval for Population Variance)

**정의**: 모집단의 분산 σ²를 특정 신뢰수준으로 포함할 것으로 예상되는 구간을 추정하는 방법입니다.

**수식**: $\frac{(n-1)s^2}{\chi^2_{\alpha/2, n-1}} \leq \sigma^2 \leq \frac{(n-1)s^2}{\chi^2_{1-\alpha/2, n-1}}$

**특징**:

- 모집단이 정규분포를 따른다고 가정
- 분산의 신뢰구간은 비대칭적
- 표준편차의 신뢰구간은 분산 신뢰구간의 제곱근
- 다른 구간추정보다 더 엄격한 가정이 필요함

**코드 예시**:

```python
import numpy as np
import scipy.stats as stats

# 데이터 생성
data = np.random.normal(loc=50, scale=5, size=20)

# 표본 분산 계산
var = np.var(data, ddof=1)
n = len(data)

# 95% 신뢰구간 계산
alpha = 0.05
chi2_lower = stats.chi2.ppf(1 - alpha/2, df=n-1)
chi2_upper = stats.chi2.ppf(alpha/2, df=n-1)

var_upper = (n-1) * var / chi2_upper
var_lower = (n-1) * var / chi2_lower

print(f"모분산의 95% 신뢰구간: ({var_lower:.4f}, {var_upper:.4f})")
```

**개념의 활용**:

- 금융 자산의 위험(변동성) 평가 시
- 제조 공정의 변동성 관리 시
- 측정 도구의 정밀도 평가 시
- 실험 결과의 일관성 분석 시
- 여러 집단 간 변동성 비교 시

## 6. 표본크기 결정 (Sample Size Determination)

**정의**: 원하는 정확도와 신뢰수준을 달성하기 위해 필요한 최소 표본 크기를 결정하는 방법입니다.

**수식**:

- 모평균 추정을 위한 표본크기: $n = \left(\frac{z_{\alpha/2} \sigma}{E}\right)^2$
- 모비율 추정을 위한 표본크기: $n = \frac{z^2_{\alpha/2} p(1-p)}{E^2}$

**특징**:

- 더 높은 신뢰수준이나 더 작은 오차 범위를 원할수록 더 큰 표본 필요
- 모집단 변동성이 클수록 더 큰 표본 필요
- 비율 추정 시 p=0.5가 가장 보수적인(최대 표본 크기) 접근
- 비용, 시간 등 현실적 제약과 통계적 정확도 사이의 균형이 중요

**코드 예시**:

```python
import numpy as np
import math
from scipy import stats

# 모평균 추정을 위한 표본크기 계산
def sample_size_mean(margin_error, std_dev, confidence=0.95):
    # 신뢰계수 z 계산
    z = stats.norm.ppf(1 - (1 - confidence)/2)
    # 표본 크기 계산
    n = (z * std_dev / margin_error)**2
    return math.ceil(n)  # 올림하여 정수로 변환

# 모비율 추정을 위한 표본크기 계산
def sample_size_proportion(margin_error, proportion=0.5, confidence=0.95):
    # 신뢰계수 z 계산
    z = stats.norm.ppf(1 - (1 - confidence)/2)
    # 표본 크기 계산
    n = (z**2 * proportion * (1 - proportion)) / (margin_error**2)
    return math.ceil(n)  # 올림하여 정수로 변환

# 예시: 모평균 추정 (표준편차가 10이라 가정, 오차 범위 ±2)
n_mean = sample_size_mean(margin_error=2, std_dev=10, confidence=0.95)
print(f"모평균 추정을 위한 필요 표본 크기: {n_mean}")

# 예시: 모비율 추정 (비율이 0.5라 가정, 오차 범위 ±0.03)
n_prop = sample_size_proportion(margin_error=0.03, proportion=0.5, confidence=0.95)
print(f"모비율 추정을 위한 필요 표본 크기: {n_prop}")
```

**개념의 활용**:

- 연구 계획 및 실험 설계 단계에서
- 여론조사 및 시장조사 설계 시
- 임상 시험 규모 결정 시
- 품질 검사 샘플링 계획 수립 시
- 정확도 요구사항과 제한된 자원 사이 균형을 맞출 때

