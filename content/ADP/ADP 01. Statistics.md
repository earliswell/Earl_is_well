---
title: 01. Statistics
draft: false
tags:
  - "#statistics"
  - "#통계학"
  - "#probability"
  - "#확률"
---
# 기초통계량

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
# 확률
## 조건부 확률

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

---
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
---
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
---
# 가설검정

## 1. 귀무가설과 대립가설 (Null and Alternative Hypotheses)

**정의**:

- 귀무가설(H₀): 검정하고자 하는 주장의 반대 또는 "차이가 없다"는 기본 가정입니다.
- 대립가설(H₁ 또는 H_A): 연구자가 입증하고자 하는 주장으로, 귀무가설을 기각했을 때 채택되는 가설입니다.

**수식**:

- 귀무가설: H₀: θ = θ₀ (모수가 특정 값과 같다)
- 대립가설: H₁: θ ≠ θ₀ (양측검정) 또는 H₁: θ > θ₀ (우측검정) 또는 H₁: θ < θ₀ (좌측검정)

**특징**:

- 귀무가설은 일반적으로 "차이가 없다", "효과가 없다", "연관성이 없다" 등의 형태를 띱니다.
- 통계적 검정은 귀무가설을 기각할 충분한 증거가 있는지 판단하는 과정입니다.
- 귀무가설은 직접 입증되지 않고, 기각되거나 기각되지 않는 형태로만 결론이 내려집니다.
- 대립가설은 연구 목적에 따라 양측 또는 단측 형태를 취할 수 있습니다.

**코드 예시**:

```python
import numpy as np
import scipy.stats as stats

# 귀무가설: 모평균이 100이다 (H₀: μ = 100)
# 대립가설: 모평균이 100보다 크다 (H₁: μ > 100) - 단측검정

# 데이터 생성
data = np.array([102, 105, 109, 101, 98, 104, 105, 103, 106, 104])

# t-검정 수행
t_stat, p_value = stats.ttest_1samp(data, popmean=100)

# 단측검정으로 변환 (기본적으로 scipy는 양측검정 p값을 반환)
# 우측 단측검정이므로 t통계량이 양수일 때만 p값을 반으로 나눔
if t_stat > 0:
    p_value = p_value / 2

alpha = 0.05
print(f"t-통계량: {t_stat:.4f}")
print(f"p-값: {p_value:.4f}")
print(f"결론: {'귀무가설 기각' if p_value < alpha else '귀무가설 기각 실패'}")
```

**개념의 활용**:

- 새로운 약물의 효과를 평가할 때 (H₀: 효과 없음, H₁: 효과 있음)
- 교육 방법의 성과 차이를 검증할 때 (H₀: 차이 없음, H₁: 차이 있음)
- 품질 관리에서 불량률 변화를 확인할 때 (H₀: 변화 없음, H₁: 증가했음)
- 마케팅 캠페인의 전환율 향상을 평가할 때 (H₀: 향상 없음, H₁: 향상됨)
- 두 집단 간 소득 차이를 검증할 때 (H₀: 차이 없음, H₁: 차이 있음)

## 2. 1종오류와 2종오류 (Type I and Type II Errors)

**정의**:

- 1종오류(α): 귀무가설이 사실인데 이를 기각하는 오류(거짓 양성, false positive)
- 2종오류(β): 귀무가설이 거짓인데 이를 기각하지 못하는 오류(거짓 음성, false negative)

**수식**:

- 1종오류의 확률: α = P(귀무가설 기각 | 귀무가설이 참)
- 2종오류의 확률: β = P(귀무가설 기각 실패 | 귀무가설이 거짓)
- 검정력(Power): 1-β = P(귀무가설 기각 | 귀무가설이 거짓)

**특징**:

- 1종오류(α)는 일반적으로 연구자가 통제하는 값으로, 보통 0.05 또는 0.01로 설정합니다.
- 2종오류(β)는 표본 크기, 효과 크기, 유의수준에 따라 결정됩니다.
- α와 β는 서로 반비례 관계에 있어, α를 낮추면 β가 증가합니다.
- 표본 크기를 늘리면 두 오류를 모두 줄일 수 있습니다.
- 실무에서는 상황에 따라 어떤 오류가 더 심각한지 고려하여 적절한 α 값을 선택합니다.

**코드 예시**:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

# 1종오류와 2종오류 시각화
def plot_error_types(mu0, mu1, sigma, alpha=0.05, n=30):
    # 표준오차
    se = sigma / np.sqrt(n)
    
    # 임계값 계산
    critical_value = mu0 + stats.norm.ppf(1-alpha) * se
    
    # x 범위 설정
    x = np.linspace(mu0 - 4*se, mu1 + 4*se, 1000)
    
    # 귀무가설과 대립가설 하의 분포
    y_h0 = stats.norm.pdf(x, mu0, se)
    y_h1 = stats.norm.pdf(x, mu1, se)
    
    # 그래프 그리기
    plt.figure(figsize=(10, 6))
    
    # 분포 곡선
    plt.plot(x, y_h0, 'b-', label='H₀ 분포 (μ='+str(mu0)+')')
    plt.plot(x, y_h1, 'r-', label='H₁ 분포 (μ='+str(mu1)+')')
    
    # 임계값 표시
    plt.axvline(critical_value, color='k', linestyle='--', label='임계값')
    
    # 오류 영역 표시
    # 1종오류 (α)
    x_alpha = np.linspace(critical_value, mu0 + 4*se, 100)
    plt.fill_between(x_alpha, 0, stats.norm.pdf(x_alpha, mu0, se), color='blue', alpha=0.3, label='1종오류(α)')
    
    # 2종오류 (β)
    x_beta = np.linspace(mu1 - 4*se, critical_value, 100)
    plt.fill_between(x_beta, 0, stats.norm.pdf(x_beta, mu1, se), color='red', alpha=0.3, label='2종오류(β)')
    
    plt.title('1종오류(α)와 2종오류(β) 시각화')
    plt.xlabel('표본평균')
    plt.ylabel('확률밀도')
    plt.legend()
    plt.grid(True, alpha=0.3)
    
    # 계산된 1종오류와 2종오류 확률
    alpha_actual = 1 - stats.norm.cdf(critical_value, mu0, se)
    beta_actual = stats.norm.cdf(critical_value, mu1, se)
    
    print(f"1종오류(α) 확률: {alpha_actual:.4f}")
    print(f"2종오류(β) 확률: {beta_actual:.4f}")
    print(f"검정력(1-β): {1-beta_actual:.4f}")
    
    plt.show()

# 예시: H₀: μ=100 vs H₁: μ=103, σ=5
plot_error_types(mu0=100, mu1=103, sigma=5)
```

**개념의 활용**:

- 제약회사의 신약 개발 과정에서 효과가 없는 약을 효과적이라고 잘못 판단하는 위험(1종오류)과 효과적인 약을 효과가 없다고 잘못 판단하는 위험(2종오류) 사이의 균형을 맞출 때
- 의학 진단에서 건강한 사람을 질병이 있다고 잘못 진단하는 위험(1종오류)과 질병이 있는 사람을 건강하다고 잘못 진단하는 위험(2종오류) 중 어느 것이 더 심각한지 평가할 때
- 품질 관리에서 정상 제품을 불량품으로 오판하는 오류(1종오류)와 불량품을 정상으로 오판하는 오류(2종오류)의 비용을 비교할 때
- 범죄 수사에서 무죄인 사람을 유죄로 판단하는 오류(1종오류)와 유죄인 사람을 무죄로 판단하는 오류(2종오류) 중 어느 오류를 더 엄격히 제한할지 결정할 때
- A/B 테스트에서 효과가 없는 변경을 효과적이라고 잘못 판단하는 위험(1종오류)과 효과적인 변경을 효과가 없다고 잘못 판단하는 위험(2종오류) 사이의 균형을 맞출 때

## 3. 대립가설의 형태에 따른 기각역 (Rejection Region Based on Alternative Hypothesis Form)

**정의**: 귀무가설을 기각하게 되는 검정통계량의 값 영역으로, 대립가설의 형태(양측, 우측, 좌측)에 따라 결정됩니다.

**수식**:

- 양측검정(H₁: θ ≠ θ₀): |T| > t_{α/2, df} 또는 p-value < α
- 우측검정(H₁: θ > θ₀): T > t_{α, df} 또는 p-value/2 < α (T > 0인 경우)
- 좌측검정(H₁: θ < θ₀): T < -t_{α, df} 또는 p-value/2 < α (T < 0인 경우)

**특징**:

- 양측검정은 귀무가설 값보다 크거나 작은 방향 모두를 고려합니다.
- 단측검정(우측 또는 좌측)은 한쪽 방향의 변화만 관심이 있을 때 사용합니다.
- 동일한 유의수준에서 단측검정이 양측검정보다 귀무가설을 기각하기 쉽습니다.
- 단측검정을 위해서는 방향성에 대한 명확한 사전 이론적 근거가 필요합니다.
- 검정 형태는 자료 수집 전에 결정해야 하며, 결과를 보고 사후에 변경해서는 안 됩니다.

**코드 예시**:

```python
import numpy as np
import scipy.stats as stats
import matplotlib.pyplot as plt

# 데이터 생성
np.random.seed(42)
data = np.random.normal(loc=105, scale=10, size=30)  # 평균 105, 표준편차 10인 표본

# 귀무가설: 모평균이 100이다 (H₀: μ = 100)

# 검정 함수 정의
def hypothesis_test(data, mu0, alpha=0.05, alternative='two-sided'):
    # t-검정 수행
    t_stat, p_value_two_sided = stats.ttest_1samp(data, popmean=mu0)
    
    # 대립가설 형태에 따른 p-value 조정
    if alternative == 'greater':
        p_value = p_value_two_sided / 2 if t_stat > 0 else 1
        conclusion = "기각" if p_value < alpha else "기각 실패"
    elif alternative == 'less':
        p_value = p_value_two_sided / 2 if t_stat < 0 else 1
        conclusion = "기각" if p_value < alpha else "기각 실패"
    else:  # 'two-sided'
        p_value = p_value_two_sided
        conclusion = "기각" if p_value < alpha else "기각 실패"
    
    return {
        't_stat': t_stat,
        'p_value': p_value,
        'conclusion': conclusion
    }

# 세 가지 대립가설 형태에 대해 검정 수행
results = {}
for alt in ['two-sided', 'greater', 'less']:
    results[alt] = hypothesis_test(data, mu0=100, alternative=alt)
    
    print(f"\n대립가설 형태: {alt}")
    print(f"t-통계량: {results[alt]['t_stat']:.4f}")
    print(f"p-값: {results[alt]['p_value']:.4f}")
    print(f"결론: 귀무가설 {results[alt]['conclusion']}")

# t-분포와 기각역 시각화
def plot_rejection_region(df, alpha=0.05, alternative='two-sided'):
    x = np.linspace(-4, 4, 1000)
    y = stats.t.pdf(x, df)
    
    plt.figure(figsize=(10, 6))
    plt.plot(x, y, 'b-', label='t-분포(df={})'.format(df))
    
    if alternative == 'two-sided':
        # 양측검정 기각역
        t_crit = stats.t.ppf(1-alpha/2, df)
        plt.fill_between(x[x >= t_crit], 0, stats.t.pdf(x[x >= t_crit], df), color='r', alpha=0.3)
        plt.fill_between(x[x <= -t_crit], 0, stats.t.pdf(x[x <= -t_crit], df), color='r', alpha=0.3)
        plt.axvline(t_crit, color='r', linestyle='--', label='임계값 ±{:.4f}'.format(t_crit))
        plt.axvline(-t_crit, color='r', linestyle='--')
        
    elif alternative == 'greater':
        # 우측검정 기각역
        t_crit = stats.t.ppf(1-alpha, df)
        plt.fill_between(x[x >= t_crit], 0, stats.t.pdf(x[x >= t_crit], df), color='r', alpha=0.3)
        plt.axvline(t_crit, color='r', linestyle='--', label='임계값 {:.4f}'.format(t_crit))
        
    else:  # 'less'
        # 좌측검정 기각역
        t_crit = stats.t.ppf(alpha, df)
        plt.fill_between(x[x <= t_crit], 0, stats.t.pdf(x[x <= t_crit], df), color='r', alpha=0.3)
        plt.axvline(t_crit, color='r', linestyle='--', label='임계값 {:.4f}'.format(t_crit))
    
    plt.title('대립가설 형태: {} (α={})'.format(alternative, alpha))
    plt.xlabel('t-통계량')
    plt.ylabel('확률밀도')
    plt.legend()
    plt.grid(True, alpha=0.3)
    plt.show()

# 세 가지 대립가설에 대한 기각역 시각화
for alt in ['two-sided', 'greater', 'less']:
    plot_rejection_region(df=len(data)-1, alternative=alt)
```

**개념의 활용**:

- 신약이 기존 약보다 효과가 있는지(우측), 효과가 없는지(좌측), 또는 단순히 다른지(양측) 검정할 때
- 새로운 교육 방법이 기존 방법보다 더 효과적인지(우측) 검정할 때
- 제품의 품질이 표준보다 낮은지(좌측) 확인할 때
- 두 집단의 평균이 서로 다른지(양측) 비교할 때
- 마케팅 캠페인이 매출을 증가시키는지(우측) 검증할 때

## 4. 검정력과 유의확률 (Statistical Power and p-value)

**정의**:

- 검정력(Power): 귀무가설이 실제로 거짓일 때 이를 기각할 확률로, 1-β로 계산됩니다.
- 유의확률(p-value): 귀무가설이 참이라는 가정 하에, 관측된 통계량과 같거나 더 극단적인 값을 얻을 확률입니다.

**수식**:

- 검정력: Power = 1 - β = P(귀무가설 기각 | 귀무가설이 거짓)
- 유의확률: p-value = P(검정통계량 ≥ |관측값| | H₀가 참) (양측검정의 경우)

**특징**:

- 검정력은 표본 크기, 효과 크기, 유의수준(α)에 따라 증가합니다.
- 일반적으로 0.8(80%) 이상의 검정력이 권장됩니다.
- p-value가 미리 설정한 유의수준(α) 보다 작으면 귀무가설을 기각합니다.
- p-value는 증거의 강도를 나타내며, 작을수록 귀무가설에 반하는 강한 증거입니다.
- p-value가 0.05보다 작다고 해서 효과가 실질적으로 중요하다는 의미는 아닙니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 검정력 계산 함수
def calculate_power(n, effect_size, alpha=0.05, test_type='two-sided'):
    # 비중심 매개변수 계산
    ncp = effect_size * np.sqrt(n)
    
    # 임계값 계산
    if test_type == 'two-sided':
        t_crit = stats.t.ppf(1 - alpha/2, df=n-1)
        power = 1 - stats.nct.cdf(t_crit, df=n-1, nc=ncp) + stats.nct.cdf(-t_crit, df=n-1, nc=ncp)
    elif test_type == 'greater':
        t_crit = stats.t.ppf(1 - alpha, df=n-1)
        power = 1 - stats.nct.cdf(t_crit, df=n-1, nc=ncp)
    elif test_type == 'less':
        t_crit = stats.t.ppf(alpha, df=n-1)
        power = stats.nct.cdf(t_crit, df=n-1, nc=ncp)
    
    return power

# 다양한 표본 크기에 대한 검정력 계산
sample_sizes = np.arange(5, 100, 5)
effect_sizes = [0.2, 0.5, 0.8]  # 작은, 중간, 큰 효과 크기

plt.figure(figsize=(10, 6))

for es in effect_sizes:
    powers = [calculate_power(n, es) for n in sample_sizes]
    plt.plot(sample_sizes, powers, marker='o', label=f'효과 크기 = {es}')

plt.axhline(0.8, color='r', linestyle='--', label='권장 검정력 = 0.8')
plt.title('표본 크기와 효과 크기에 따른 검정력')
plt.xlabel('표본 크기 (n)')
plt.ylabel('검정력 (1-β)')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

# p-value 예시: 두 집단 간 평균 비교
np.random.seed(42)
group1 = np.random.normal(loc=10, scale=2, size=30)
group2 = np.random.normal(loc=11, scale=2, size=30)

# 독립표본 t-검정
t_stat, p_value = stats.ttest_ind(group1, group2, equal_var=True)

print(f"t-통계량: {t_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'귀무가설 기각 (유의한 차이 있음)' if p_value < 0.05 else '귀무가설 기각 실패 (유의한 차이 없음)'}")
```

**개념의 활용**:

- 연구 설계 단계에서 적절한 표본 크기를 결정할 때 검정력 분석을 활용
- 실험 결과의 통계적 유의성을 평가할 때 p-value 활용
- 여러 가설을 동시에 검정할 때 다중검정 문제와 p-value 조정 방법 고려
- 효과 크기가 작을 때 필요한 표본 크기 증가를 정당화할 때
- 연구 결과의 실질적 중요성을 평가할 때 p-value 외에 효과 크기와 신뢰구간 함께 고려

## 5. 정규성 검정 (Tests for Normality)

### Shapiro-Wilk 검정

**정의**: 데이터가 정규분포를 따르는지 검정하는 방법으로, 특히 작은 표본에 효과적입니다.

**수식**: $W = \frac{(\sum_{i=1}^{n} a_i x_{(i)})^2}{\sum_{i=1}^{n} (x_i - \bar{x})^2}$ 여기서 $x_{(i)}$는 오름차순으로 정렬된 데이터이고, $a_i$는 Shapiro-Wilk 계수입니다.

**특징**:

- 귀무가설: 데이터가 정규분포를 따른다.
- 대립가설: 데이터가 정규분포를 따르지 않는다.
- 일반적으로 표본 크기가 3~5,000 사이일 때 적용 가능합니다.
- p-value < α이면 정규성 가정을 기각합니다.
- 작은 표본에서 다른 정규성 검정보다 검정력이 좋습니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 정규분포에서 추출한 데이터
np.random.seed(42)
normal_data = np.random.normal(loc=0, scale=1, size=50)

# 비정규 분포(지수분포)에서 추출한 데이터
non_normal_data = np.random.exponential(scale=1, size=50)

# Shapiro-Wilk 검정 수행
stat_normal, p_normal = stats.shapiro(normal_data)
stat_non_normal, p_non_normal = stats.shapiro(non_normal_data)

print("정규 데이터 Shapiro-Wilk 검정:")
print(f"통계량: {stat_normal:.4f}, p-value: {p_normal:.4f}")
print(f"결론: {'정규성 가정 기각 실패 (정규 분포임)' if p_normal >= 0.05 else '정규성 가정 기각 (정규 분포 아님)'}")

print("\n비정규 데이터 Shapiro-Wilk 검정:")
print(f"통계량: {stat_non_normal:.4f}, p-value: {p_non_normal:.4f}")
print(f"결론: {'정규성 가정 기각 실패 (정규 분포임)' if p_non_normal >= 0.05 else '정규성 가정 기각 (정규 분포 아님)'}")

# 시각적 확인: QQ 플롯
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
stats.probplot(normal_data, dist="norm", plot=plt)
plt.title(f'정규 데이터 QQ 플롯\nShapiro-Wilk p-value: {p_normal:.4f}')

plt.subplot(1, 2, 2)
stats.probplot(non_normal_data, dist="norm", plot=plt)
plt.title(f'비정규 데이터 QQ 플롯\nShapiro-Wilk p-value: {p_non_normal:.4f}')

plt.tight_layout()
plt.show()
```

### Kolmogorov-Smirnov 검정

**정의**: 실증적 분포 함수와 가정된 분포 함수 사이의 최대 차이를 측정하여 분포의 적합성을 검정하는 방법입니다.

**수식**: $D_n = \sup_x |F_n(x) - F(x)|$ 여기서 $F_n(x)$는 경험적 분포 함수이고, $F(x)$는 이론적 분포 함수입니다.

**특징**:

- 귀무가설: 데이터가 특정 분포(예: 정규분포)를 따른다.
- 대립가설: 데이터가 특정 분포를 따르지 않는다.
- 연속형 분포에 대한 적합성 검정에 사용됩니다.
- 특히 큰 표본에 적합합니다.
- 분포의 평균이나 분산을 모를 경우 Lilliefors 수정 버전을 사용합니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 정규분포에서 추출한 데이터
np.random.seed(42)
normal_data = np.random.normal(loc=0, scale=1, size=100)

# 비정규 분포(지수분포)에서 추출한 데이터
non_normal_data = np.random.exponential(scale=1, size=100)

# Kolmogorov-Smirnov 검정 수행
# 정규성 검정에서는 표본 평균과 표준편차를 사용하므로 이는 Lilliefors 검정과 유사
ks_stat_normal, p_normal = stats.kstest(normal_data, 'norm', args=(np.mean(normal_data), np.std(normal_data, ddof=1)))
ks_stat_non_normal, p_non_normal = stats.kstest(non_normal_data, 'norm', args=(np.mean(non_normal_data), np.std(non_normal_data, ddof=1)))

print("정규 데이터 Kolmogorov-Smirnov 검정:")
print(f"통계량: {ks_stat_normal:.4f}, p-value: {p_normal:.4f}")
print(f"결론: {'정규성 가정 기각 실패 (정규 분포임)' if p_normal >= 0.05 else '정규성 가정 기각 (정규 분포 아님)'}")

print("\n비정규 데이터 Kolmogorov-Smirnov 검정:")
print(f"통계량: {ks_stat_non_normal:.4f}, p-value: {p_non_normal:.4f}")
print(f"결론: {'정규성 가정 기각 실패 (정규 분포임)' if p_non_normal >= 0.05 else '정규성 가정 기각 (정규 분포 아님)'}")

# 경험적 분포 함수와 이론적 분포 함수 비교
plt.figure(figsize=(12, 5))

# 정규 데이터 ECDF vs CDF
plt.subplot(1, 2, 1)
x = np.sort(normal_data)
y = np.arange(1, len(x)+1) / len(x)  # ECDF
plt.step(x, y, where='post', label='경험적 분포 함수')

# 이론적 정규 CDF
x_theory = np.linspace(min(x), max(x), 100)
y_theory = stats.norm.cdf(x_theory, loc=np.mean(normal_data), scale=np.std(normal_data, ddof=1))
plt.plot(x_theory, y_theory, 'r-', label='이론적 정규 분포 함수')

plt.title(f'정규 데이터 ECDF vs CDF\nKS 검정 p-value: {p_normal:.4f}')
plt.legend()

# 비정규 데이터 ECDF vs CDF
plt.subplot(1, 2, 2)
x = np.sort(non_normal_data)
y = np.arange(1, len(x)+1) / len(x)  # ECDF
plt.step(x, y, where='post', label='경험적 분포 함수')

# 이론적 정규 CDF
x_theory = np.linspace(min(x), max(x), 100)
y_theory = stats.norm.cdf(x_theory, loc=np.mean(non_normal_data), scale=np.std(non_normal_data, ddof=1))
plt.plot(x_theory, y_theory, 'r-', label='이론적 정규 분포 함수')

plt.title(f'비정규 데이터 ECDF vs CDF\nKS 검정 p-value: {p_non_normal:.4f}')
plt.legend()

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 모수적 검정(t-검정 등)의 정규성 가정을 확인할 때
- 데이터 변환(로그 변환 등)이 정규성을 향상시켰는지 확인할 때
- 비모수 검정의 필요성을 결정할 때
- 회귀분석의 잔차가 정규분포를 따르는지 확인할 때
- 복잡한 통계 모델의 가정을 검증할 때

## 6. 등분산 검정 (Tests for Homogeneity of Variance)

### Levene, Bartlett, Fligner 등분산 검정

**정의**: 여러 집단의 분산이 동일한지 검정하는 방법으로, 모수적 검정의 중요한 가정을 확인하는 데 사용됩니다.

**수식**:

- Levene 검정: $W = \frac{(N-k)}{(k-1)} \frac{\sum_{i=1}^k n_i(Z_{i.} - Z_{..})^2}{\sum_{i=1}^k \sum_{j=1}^{n_i} (Z_{ij} - Z_{i.})^2}$ 여기서 $Z_{ij} = |X_{ij} - \bar{X}_{i.}|$ (중앙값을 사용할 수도 있음)
- Bartlett 검정: $\chi^2 = \frac{(N-k) \ln(s_p^2) - \sum_{i=1}^k (n_i-1) \ln(s_i^2)}{1 + \frac{1}{3(k-1)}(\sum_{i=1}^k \frac{1}{n_i-1} - \frac{1}{N-k})}$

**특징**:

- 귀무가설: 모든 집단의 분산이 동일하다.
- 대립가설: 적어도 한 집단의 분산이 다르다.
- Levene 검정은 정규성 가정에 덜 민감하여 강건합니다.
- Bartlett 검정은 정규성 가정 하에서 더 검정력이 높지만, 정규성 위반에 민감합니다.
- Fligner-Killeen 검정은 비모수적 접근법으로 이상치가 있을 때 강건합니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 데이터 생성
np.random.seed(42)
# 등분산 데이터
group1 = np.random.normal(loc=5, scale=2, size=30)
group2 = np.random.normal(loc=7, scale=2, size=30)
group3 = np.random.normal(loc=9, scale=2, size=30)

# 이분산 데이터
group1_hetero = np.random.normal(loc=5, scale=1, size=30)
group2_hetero = np.random.normal(loc=7, scale=2, size=30)
group3_hetero = np.random.normal(loc=9, scale=3, size=30)

# 등분산 검정 수행
# 1. Levene 검정
stat_levene, p_levene = stats.levene(group1, group2, group3)
stat_levene_hetero, p_levene_hetero = stats.levene(group1_hetero, group2_hetero, group3_hetero)

# 2. Bartlett 검정
stat_bartlett, p_bartlett = stats.bartlett(group1, group2, group3)
stat_bartlett_hetero, p_bartlett_hetero = stats.bartlett(group1_hetero, group2_hetero, group3_hetero)

# 3. Fligner-Killeen 검정
stat_fligner, p_fligner = stats.fligner(group1, group2, group3)
stat_fligner_hetero, p_fligner_hetero = stats.fligner(group1_hetero, group2_hetero, group3_hetero)

print("등분산 데이터 검정 결과:")
print(f"Levene 검정: 통계량={stat_levene:.4f}, p-value={p_levene:.4f}")
print(f"Bartlett 검정: 통계량={stat_bartlett:.4f}, p-value={p_bartlett:.4f}")
print(f"Fligner-Killeen 검정: 통계량={stat_fligner:.4f}, p-value={p_fligner:.4f}")

print("\n이분산 데이터 검정 결과:")
print(f"Levene 검정: 통계량={stat_levene_hetero:.4f}, p-value={p_levene_hetero:.4f}")
print(f"Bartlett 검정: 통계량={stat_bartlett_hetero:.4f}, p-value={p_bartlett_hetero:.4f}")
print(f"Fligner-Killeen 검정: 통계량={stat_fligner_hetero:.4f}, p-value={p_fligner_hetero:.4f}")

# 박스플롯으로 시각화
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
plt.boxplot([group1, group2, group3])
plt.title('등분산 데이터\nLevene p-value: {:.4f}'.format(p_levene))
plt.xticks([1, 2, 3], ['Group 1', 'Group 2', 'Group 3'])
plt.ylabel('값')

plt.subplot(1, 2, 2)
plt.boxplot([group1_hetero, group2_hetero, group3_hetero])
plt.title('이분산 데이터\nLevene p-value: {:.4f}'.format(p_levene_hetero))
plt.xticks([1, 2, 3], ['Group 1', 'Group 2', 'Group 3'])
plt.ylabel('값')

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 독립표본 t-검정 또는 ANOVA 수행 전 등분산 가정을 확인할 때
- 등분산 가정을 위반할 경우 대안적 방법(Welch의 t-검정 등)을 선택할 때
- 실험 조건이 데이터의 변동성에 영향을 미치는지 확인할 때
- 제조 공정의 안정성을 평가할 때
- 여러 측정 방법의 정밀도를 비교할 때

## 7. 독립 표본 t-검정 (Independent Samples t-test)

**정의**: 서로 다른 두 집단의 평균을 비교하여 통계적으로 유의한 차이가 있는지 검정하는 방법입니다.

**수식**:

- 등분산 가정 시: $t = \frac{\bar{X}_1 - \bar{X}_2}{s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}}$, 여기서 $s_p^2 = \frac{(n_1-1)s_1^2 + (n_2-1)s_2^2}{n_1+n_2-2}$
- Welch의 t-검정(이분산 가정 시): $t = \frac{\bar{X}_1 - \bar{X}_2}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}}$

**특징**:

- 귀무가설: 두 집단의 평균이 같다 (μ₁ = μ₂)
- 대립가설: 두 집단의 평균이 다르다 (μ₁ ≠ μ₂), 또는 한 집단이 더 크다 (μ₁ > μ₂ 또는 μ₁ < μ₂)
- 두 집단이 독립적이어야 합니다(동일 대상의 반복 측정이 아님).
- 데이터가 정규분포를 따르거나 표본 크기가 충분히 커야 합니다.
- 등분산 검정 결과에 따라 적절한 t-검정 방법을 선택해야 합니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 데이터 생성
np.random.seed(42)
group1 = np.random.normal(loc=75, scale=5, size=30)  # 첫 번째 그룹 (평균 75)
group2 = np.random.normal(loc=78, scale=5, size=30)  # 두 번째 그룹 (평균 78)

# 등분산 검정
levene_stat, levene_p = stats.levene(group1, group2)
print(f"Levene 등분산 검정: 통계량={levene_stat:.4f}, p-value={levene_p:.4f}")
equal_var = levene_p >= 0.05  # p >= 0.05면 등분산 가정

# 독립표본 t-검정 수행
t_stat, p_value = stats.ttest_ind(group1, group2, equal_var=equal_var)

print(f"독립표본 t-검정 ({('등분산 가정' if equal_var else 'Welch의 t-검정')}):")
print(f"t-통계량: {t_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'평균 차이가 통계적으로 유의함' if p_value < 0.05 else '평균 차이가 통계적으로 유의하지 않음'}")

# 효과 크기 계산 (Cohen's d)
mean_diff = np.mean(group2) - np.mean(group1)
pooled_std = np.sqrt(((len(group1) - 1) * np.var(group1, ddof=1) + 
                       (len(group2) - 1) * np.var(group2, ddof=1)) / 
                      (len(group1) + len(group2) - 2))
cohen_d = mean_diff / pooled_std

print(f"효과 크기 (Cohen's d): {cohen_d:.4f}")

# 시각화
plt.figure(figsize=(10, 6))

# 박스플롯
plt.subplot(1, 2, 1)
box_data = [group1, group2]
plt.boxplot(box_data)
plt.xticks([1, 2], ['그룹 1', '그룹 2'])
plt.ylabel('값')
plt.title('두 그룹의 데이터 분포')

# 평균 비교 막대 그래프
plt.subplot(1, 2, 2)
means = [np.mean(group1), np.mean(group2)]
errors = [stats.sem(group1), stats.sem(group2)]  # 표준오차

plt.bar([1, 2], means, yerr=errors, capsize=10)
plt.xticks([1, 2], ['그룹 1', '그룹 2'])
plt.ylabel('평균 ± 표준오차')
plt.title(f't-검정: p = {p_value:.4f}')

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 두 가지 교육 방법의 효과 비교 시
- 약물 치료군과 대조군의 효과 차이 검정 시
- 남성과 여성의 평균 소득 비교 시
- 두 가지 제조 방법으로 생산된 제품의 품질 비교 시
- 두 지역 간 환경 오염 수준 비교 시

## 8. 대응표본 t-검정 (Paired Samples t-test)

**정의**: 동일한 대상에서 두 번 측정된 값의 평균 차이를 검정하는 방법으로, 짝을 이룬 데이터의 분석에 사용됩니다.

**수식**: $t = \frac{\bar{d}}{s_d / \sqrt{n}}$ 여기서 $\bar{d}$는 대응된 차이의 평균, $s_d$는 차이의 표준편차, n은 쌍의 수입니다.

**특징**:

- 귀무가설: 대응된 측정값들의 평균 차이가 0이다 (μd = 0)
- 대립가설: 대응된 측정값들의 평균 차이가 0이 아니다 (μd ≠ 0)
- 같은 대상에 대한 전후 비교나 짝을 이룬 자료에 적합합니다.
- 독립표본 t-검정보다 검정력이 높을 수 있습니다(개인차에 의한 변동성 제거).
- 차이값이 정규분포를 따른다고 가정합니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 데이터 생성 (예: 치료 전후 환자들의 혈압)
np.random.seed(42)
n = 20  # 환자 수
before = np.random.normal(loc=140, scale=10, size=n)  # 치료 전 혈압
effect = -8  # 치료 효과 (혈압 감소)
noise = np.random.normal(loc=0, scale=5, size=n)  # 개인차
after = before + effect + noise  # 치료 후 혈압

# 대응표본 t-검정 수행
t_stat, p_value = stats.ttest_rel(before, after)

print("대응표본 t-검정 결과:")
print(f"치료 전 평균: {np.mean(before):.2f}, 표준편차: {np.std(before, ddof=1):.2f}")
print(f"치료 후 평균: {np.mean(after):.2f}, 표준편차: {np.std(after, ddof=1):.2f}")
print(f"평균 차이: {np.mean(before - after):.2f}")
print(f"t-통계량: {t_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'치료 전후 차이가 통계적으로 유의함' if p_value < 0.05 else '치료 전후 차이가 통계적으로 유의하지 않음'}")

# 효과 크기 계산 (Cohen's d for paired samples)
d = np.mean(before - after) / np.std(before - after, ddof=1)
print(f"효과 크기 (Cohen's d): {d:.4f}")

# 시각화
plt.figure(figsize=(12, 5))

# 1. 선 그래프로 개인별 변화 표시
plt.subplot(1, 2, 1)
for i in range(n):
    plt.plot([1, 2], [before[i], after[i]], 'o-', alpha=0.3)

plt.plot([1, 2], [np.mean(before), np.mean(after)], 'r-', linewidth=2, label='평균')
plt.xticks([1, 2], ['치료 전', '치료 후'])
plt.ylabel('혈압')
plt.title('개인별 치료 전후 변화')
plt.legend()

# 2. 차이값의 히스토그램
plt.subplot(1, 2, 2)
differences = before - after
plt.hist(differences, bins=10, alpha=0.7, color='skyblue')
plt.axvline(np.mean(differences), color='r', linestyle='--', label=f'평균 차이: {np.mean(differences):.2f}')
plt.xlabel('치료 전 - 치료 후')
plt.ylabel('빈도')
plt.title(f'차이값 분포 (t={t_stat:.2f}, p={p_value:.4f})')
plt.legend()

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 약물 치료 전후의 증상 변화 비교 시
- 교육 프로그램 전후의 학업 성취도 변화 측정 시
- 다이어트 프로그램 전후의 체중 변화 분석 시
- 동일 주제에 대한 두 평가자의 점수 비교 시
- 같은 제품에 대한 두 가지 측정 방법의 결과 비교 시

## 9. 일표본 t-검정 (One-sample t-test)

**정의**: 한 집단의 평균이 특정 기준값(모집단 평균)과 유의하게 다른지 검정하는 방법입니다.

**수식**: $t = \frac{\bar{X} - \mu_0}{s / \sqrt{n}}$ 여기서 $\bar{X}$는 표본평균, $\mu_0$는 검정하려는 기준값, $s$는 표본표준편차, $n$은 표본크기입니다.

**특징**:

- 귀무가설: 모집단 평균이 μ₀와 같다 (μ = μ₀)
- 대립가설: 모집단 평균이 μ₀와 다르다 (μ ≠ μ₀) 또는 크거나(μ > μ₀) 작다(μ < μ₀)
- 데이터가 정규분포를 따르거나 표본 크기가 충분히 커야 합니다.
- 하나의 집단에 대해 알려진 기준값과 비교할 때 사용합니다.
- 통계량은 자유도가 n-1인 t분포를 따릅니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 데이터 생성 (예: 시험 점수)
np.random.seed(42)
n = 25  # 학생 수
scores = np.random.normal(loc=72, scale=8, size=n)  # 평균 72점, 표준편차 8점

# 일표본 t-검정 수행 (귀무가설: 평균 점수는 70점)
mu0 = 70  # 검정할 기준값
t_stat, p_value = stats.ttest_1samp(scores, popmean=mu0)

print("일표본 t-검정 결과:")
print(f"표본 평균: {np.mean(scores):.2f}, 표준편차: {np.std(scores, ddof=1):.2f}")
print(f"검정 기준값: {mu0}")
print(f"t-통계량: {t_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'표본 평균이 기준값과 통계적으로 유의하게 다름' if p_value < 0.05 else '표본 평균이 기준값과 통계적으로 유의한 차이가 없음'}")

# 효과 크기 계산 (Cohen's d for one-sample)
d = (np.mean(scores) - mu0) / np.std(scores, ddof=1)
print(f"효과 크기 (Cohen's d): {d:.4f}")

# 시각화
plt.figure(figsize=(10, 6))

# 히스토그램
plt.hist(scores, bins=10, alpha=0.7, color='skyblue')
plt.axvline(np.mean(scores), color='r', linestyle='-', linewidth=2, label=f'표본 평균: {np.mean(scores):.2f}')
plt.axvline(mu0, color='g', linestyle='--', linewidth=2, label=f'검정 기준값: {mu0}')

# 95% 신뢰구간 계산
sem = stats.sem(scores)  # 표준오차
ci = stats.t.interval(0.95, len(scores)-1, loc=np.mean(scores), scale=sem)
plt.axvspan(ci[0], ci[1], alpha=0.2, color='red', label=f'95% 신뢰구간: ({ci[0]:.2f}, {ci[1]:.2f})')

plt.xlabel('점수')
plt.ylabel('빈도')
plt.title(f'시험 점수 분포 (t={t_stat:.2f}, p={p_value:.4f})')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

**개념의 활용**:

- 신제품의 성능이 기존 표준을 초과하는지 검정할 때
- 학생들의 평균 성적이 목표 점수에 도달했는지 확인할 때
- 어떤 집단의 특성이 국가 평균과 차이가 있는지 검증할 때
- 측정 장비의 정확도가 규격 기준을 만족하는지 확인할 때
- 소비자 만족도가 특정 목표 수준에 도달했는지 평가할 때
---
# 분산분석

## 1. 일원배치 분산분석 (One-way ANOVA)

**정의**: 셋 이상의 독립적인 집단의 평균 간에 통계적으로 유의한 차이가 있는지를 검정하는 방법입니다. 단일 요인(독립변수)이 종속변수에 미치는 영향을 분석합니다.

**수식**:

- F = MSB/MSW
    - MSB(집단 간 평균제곱) = SSB/(k-1)
    - MSW(집단 내 평균제곱) = SSW/(N-k)
    - SSB(집단 간 제곱합) = Σnᵢ(x̄ᵢ-x̄)²
    - SSW(집단 내 제곱합) = ΣΣ(xᵢⱼ-x̄ᵢ)²
    - k = 집단 수, N = 전체 표본 크기

**특징**:

- 귀무가설은 "모든 집단의 평균이 동일하다"입니다 (H₀: μ₁ = μ₂ = ... = μₖ).
- 대립가설은 "적어도 하나의 집단 평균이 다르다"입니다.
- 독립성, 정규성, 등분산성 가정이 필요합니다.
- F-통계량은 자유도가 (k-1, N-k)인 F-분포를 따릅니다.
- 차이가 있는지만 알려주며, 어떤 집단 간에 차이가 있는지는 알려주지 않습니다.
- 사후검정(투키법, 본페로니법 등)을 통해 집단 간 차이를 확인해야 합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import scipy.stats as stats
from statsmodels.stats.multicomp import pairwise_tukeyhsd

# 데이터 생성 (3개 처리군)
np.random.seed(42)
group1 = np.random.normal(loc=5, scale=1, size=30)  # 처리 A
group2 = np.random.normal(loc=6, scale=1, size=30)  # 처리 B
group3 = np.random.normal(loc=6.5, scale=1, size=30)  # 처리 C

# 일원배치 ANOVA 수행
f_stat, p_value = stats.f_oneway(group1, group2, group3)

print(f"F-통계량: {f_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'집단 간 유의한 차이가 있음' if p_value < 0.05 else '집단 간 유의한 차이가 없음'}")

# 데이터 프레임 생성
data = np.concatenate([group1, group2, group3])
labels = np.concatenate([['A']*30, ['B']*30, ['C']*30])
df = pd.DataFrame({'value': data, 'group': labels})

# 사후검정 (Tukey's HSD)
tukey = pairwise_tukeyhsd(df['value'], df['group'], alpha=0.05)
print("\n사후검정 결과:")
print(tukey)

# 시각화
plt.figure(figsize=(10, 6))
plt.boxplot([group1, group2, group3], labels=['처리 A', '처리 B', '처리 C'])
plt.title(f'일원배치 ANOVA: F={f_stat:.2f}, p={p_value:.4f}')
plt.ylabel('값')
plt.grid(True, alpha=0.3)
plt.show()
```

**개념의 활용**:

- 여러 교육 방법의 효과를 비교할 때 (예: 3가지 교수법의 학습 결과 비교)
- 다양한 약물 치료의 효과를 비교할 때 (예: 4가지 약물과 위약 효과 비교)
- 다른 비료 종류가 작물 수확량에 미치는 영향을 분석할 때
- 여러 제조 방법이 제품 품질에 미치는 영향을 평가할 때
- 다양한 마케팅 전략이 판매량에, 미치는 효과를 비교할 때

## 2. 이원배치 분산분석 (Two-way ANOVA)

**정의**: 두 개의 독립적인 범주형 변수(요인)가 하나의 연속형 종속변수에 미치는 영향과 두 요인 간의 상호작용을 분석하는 통계 방법입니다.

**수식**:

- 요인 A와 B에 대한 모형:
    - 총 제곱합(SST) = SS(A) + SS(B) + SS(A×B) + SS(오차)
    - 요인 A의 F-통계량 = MS(A)/MS(오차)
    - 요인 B의 F-통계량 = MS(B)/MS(오차)
    - 상호작용의 F-통계량 = MS(A×B)/MS(오차)

**특징**:

- 세 가지 귀무가설을 검정합니다: 요인 A의 효과 없음, 요인 B의 효과 없음, 상호작용 효과 없음.
- 주효과(main effects)와 상호작용 효과(interaction effects)를 동시에 분석할 수 있습니다.
- 주효과: 한 요인이 종속변수에 미치는 독립적인 영향.
- 상호작용 효과: 한 요인의 효과가 다른 요인의 수준에 따라 달라지는 현상.
- 여러 개의 일원배치 ANOVA보다 통계적 검정력이 더 높습니다.
- 전통적인 분석에서는 균형 설계(모든 셀의 표본 크기가 동일)가 권장됩니다.
- 상호작용 도표(interaction plot)가 결과 해석에 유용합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import statsmodels.api as sm
from statsmodels.formula.api import ols

# 데이터 생성: 비료 종류(A,B)와 물 공급량(저,중,고)이 식물 성장에 미치는 영향
np.random.seed(42)

# 요인 조합 생성
fertilizer = np.repeat(['A', 'B'], 30)
water = np.tile(np.repeat(['Low', 'Medium', 'High'], 10), 2)

# 결과값 생성 (상호작용 효과 포함)
# 비료 A는 물 공급에 덜 민감, 비료 B는 물 공급에 매우 민감
base_growth = 10
fertilizer_effect = {'A': 2, 'B': 0}
water_effect = {'Low': 0, 'Medium': 4, 'High': 7}
interaction_effect = {
    ('A', 'Low'): 0, ('A', 'Medium'): 1, ('A', 'High'): 1,
    ('B', 'Low'): -1, ('B', 'Medium'): 2, ('B', 'High'): 5
}

growth = []
for f, w in zip(fertilizer, water):
    mean_growth = (base_growth + fertilizer_effect[f] + water_effect[w] + 
                   interaction_effect[(f, w)])
    growth.append(np.random.normal(mean_growth, 1.5))

# 데이터프레임 생성
df = pd.DataFrame({
    'growth': growth,
    'fertilizer': fertilizer,
    'water': water
})

# 이원배치 ANOVA 수행
model = ols('growth ~ C(fertilizer) * C(water)', data=df).fit()
anova_table = sm.stats.anova_lm(model, typ=2)
print(anova_table)

# 주효과와 상호작용 시각화
plt.figure(figsize=(12, 5))

# 상호작용 플롯
plt.subplot(1, 2, 1)
interaction_data = df.groupby(['fertilizer', 'water'])['growth'].mean().reset_index()
interaction_pivot = interaction_data.pivot(index='water', columns='fertilizer', values='growth')

for col in interaction_pivot.columns:
    plt.plot(interaction_pivot.index, interaction_pivot[col], marker='o', label=f'비료 {col}')

plt.title('상호작용 플롯: 비료 종류와 물 공급량')
plt.xlabel('물 공급량')
plt.ylabel('평균 성장률')
plt.grid(True, alpha=0.3)
plt.legend()

# 주효과 플롯
plt.subplot(1, 2, 2)
fertilizer_means = df.groupby('fertilizer')['growth'].mean()
water_means = df.groupby('water')['growth'].mean()

plt.bar([1, 2], fertilizer_means, width=0.4, label='비료 종류')
plt.bar([4, 5, 6], water_means, width=0.4, label='물 공급량')

plt.xticks([1.5, 5], ['비료', '물 공급량'])
plt.title('주효과 플롯')
plt.ylabel('평균 성장률')
plt.legend()

plt.tight_layout()
plt.show()

# 사후 분석 (Tukey HSD)
from statsmodels.stats.multicomp import pairwise_tukeyhsd

# 물 공급량에 대한 사후 분석
print("\n물 공급량 사후 분석:")
tukey_water = pairwise_tukeyhsd(df['growth'], df['water'], alpha=0.05)
print(tukey_water)

# 비료와 물 조합에 대한 사후 분석
df['treatment'] = df['fertilizer'] + '_' + df['water']
print("\n처리 조합 사후 분석:")
tukey_comb = pairwise_tukeyhsd(df['growth'], df['treatment'], alpha=0.05)
print(tukey_comb)
```

**개념의 활용**:

- 성별과 약물 종류가 치료 효과에 미치는 영향 분석 시
- 비료 종류와 물 공급량이 식물 성장에 미치는 영향 평가 시
- 교수법과 학습 환경이 학업 성취도에 미치는 영향 연구 시
- 온도와 습도가 제품 내구성에 미치는 영향 조사 시
- 광고 유형과 가격 수준이 제품 판매량에 미치는 영향 분석 시

### 교호작용, 주효과, 사후분석

**교호작용 (Interaction Effect)**:

- 정의: 한 요인의 효과가 다른 요인의 수준에 따라 달라지는 현상입니다.
- 해석: 상호작용이 유의하면, 한 요인의 효과를 해석할 때 다른 요인의 수준을 고려해야 합니다.
- 시각화: 상호작용 플롯에서 선이 평행하지 않으면 상호작용이 존재함을 시사합니다.

**주효과 (Main Effect)**:

- 정의: 다른 요인의 수준에 관계없이 한 요인이 종속변수에 미치는 독립적인 영향입니다.
- 해석: 상호작용이 유의하지 않을 때 주효과를 직접적으로 해석할 수 있습니다.
- 주의점: 유의한 상호작용이 있다면 주효과만으로 결론을 내리는 것은 오해를 불러일으킬 수 있습니다.

**사후분석 (Post-hoc Analysis)**:

- 목적: ANOVA에서 유의한 차이가 발견됐을 때, 어떤 집단 간에 차이가 있는지 구체적으로 확인합니다.
- 방법:
    - Tukey's Honestly Significant Difference (HSD): 모든 가능한 쌍을 비교하며, 제1종 오류를 통제합니다.
    - Bonferroni: 가장 보수적인 방법으로, 유의수준을 검정 횟수로 나눕니다.
    - Scheffé: 모든 가능한 대비(contrast)를 고려하는 방법으로, 매우 보수적입니다.
    - Duncan's Multiple Range Test: 덜 보수적인 방법으로, 검정력이 높지만 제1종 오류가 증가할 수 있습니다.
- 해석: 각 쌍별 비교의 p-value나 신뢰구간을 통해 유의한 차이가 있는 집단을 확인합니다.

이원배치 분산분석에서 상호작용이 유의한 경우, 분석 순서는 일반적으로 다음과 같습니다:

1. 상호작용 효과 해석 (가장 중요)
2. 필요한 경우 단순 주효과 분석 (한 요인의 수준별로 다른 요인의 효과 분석)
3. 사후검정을 통한 구체적인 차이 확인
---

# 비모수검정 (Non-parametric Tests)

## 1. 카이제곱검정 - 적합성 검정 (Chi-square Goodness of Fit Test)

**정의**: 관측된 범주형 데이터의 분포가 기대되는 이론적 분포와 일치하는지 검정하는 방법입니다. 즉, 실제 관측된 빈도와 기대 빈도 간의 차이를 분석합니다.

**수식**: $\chi^2 = \sum_{i=1}^{k} \frac{(O_i - E_i)^2}{E_i}$ 여기서 $O_i$는 범주 i의 관측 빈도, $E_i$는 범주 i의 기대 빈도, k는 범주의 수입니다.

**특징**:

- 귀무가설은 "관측된 빈도가 기대 빈도와 일치한다"입니다.
- 각 범주의 기대 빈도는 5 이상이어야 합니다.
- 자유도는 (범주 수 - 1)입니다.
- 모든 범주는 상호 배타적이어야 합니다.
- 정규성 가정이 필요 없어 분포에 제약이 없습니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 예시: 주사위 던지기
observed = np.array([15, 20, 18, 16, 12, 19])  # 관측된 각 면의 빈도
n = sum(observed)  # 총 던진 횟수
k = len(observed)  # 범주 수 (주사위 면의 수)
expected = np.array([n/k] * k)  # 공정한 주사위라면 모든 면이 동일한 빈도로 나와야 함

# 카이제곱 적합성 검정 수행
chi2_stat, p_value = stats.chisquare(observed, expected)

print(f"관측 빈도: {observed}")
print(f"기대 빈도: {expected}")
print(f"카이제곱 통계량: {chi2_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'관측 빈도와 기대 빈도가 유의하게 다름' if p_value < 0.05 else '관측 빈도와 기대 빈도 간 유의한 차이 없음'}")

# 시각화
plt.figure(figsize=(10, 6))
categories = [f'면 {i+1}' for i in range(k)]
width = 0.35
x = np.arange(k)

plt.bar(x - width/2, observed, width, label='관측 빈도')
plt.bar(x + width/2, expected, width, label='기대 빈도')

plt.xlabel('주사위 면')
plt.ylabel('빈도')
plt.title(f'카이제곱 적합성 검정: χ²={chi2_stat:.2f}, p={p_value:.4f}')
plt.xticks(x, categories)
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

**개념의 활용**:

- 주사위나 룰렛 같은 게임의 공정성 검정 시
- 유전학에서 멘델의 법칙 검증 시 (예: 3:1 비율 가설)
- 도시의 교통사고 발생 분포가 예상 패턴과 일치하는지 확인할 때
- 설문조사 응답이 특정 기대 분포를 따르는지 검증할 때
- 제품 결함이 랜덤하게 발생하는지 또는 특정 패턴이 있는지 분석할 때

## 2. 카이제곱검정 - 독립성 검정 (Chi-square Test of Independence)

**정의**: 두 범주형 변수 간에 통계적으로 유의한 관계가 있는지 검정하는 방법입니다. 즉, 한 변수의 분포가 다른 변수의 수준에 따라 달라지는지 확인합니다.

**수식**: $\chi^2 = \sum_{i=1}^{r}\sum_{j=1}^{c} \frac{(O_{ij} - E_{ij})^2}{E_{ij}}$ 여기서 $E_{ij} = \frac{R_i \times C_j}{n}$, $R_i$는 i행의 합, $C_j$는 j열의 합, n은 총 관측치 수입니다.

**특징**:

- 귀무가설은 "두 변수는 서로 독립적이다"입니다.
- 각 셀의 기대 빈도는 5 이상이 권장됩니다.
- 자유도는 (행 수 - 1) × (열 수 - 1)입니다.
- 분할표(교차표, contingency table)를 사용하여 데이터를 정리합니다.
- 변수 간 연관성의 존재만 알려주며, 인과관계는 확인할 수 없습니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
from scipy import stats
import matplotlib.pyplot as plt
import seaborn as sns

# 예시: 교육 수준과 정치 성향의 관계
# 데이터 생성: 교육 수준(고졸, 대졸, 대학원졸)과 정치 성향(보수, 중도, 진보)
np.random.seed(42)
n = 500  # 표본 크기

# 약간의 연관성이 있는 데이터 생성
education = np.random.choice(['고졸', '대졸', '대학원졸'], size=n, p=[0.3, 0.5, 0.2])
political_tendency = []

for edu in education:
    if edu == '고졸':
        political_tendency.append(np.random.choice(['보수', '중도', '진보'], p=[0.5, 0.3, 0.2]))
    elif edu == '대졸':
        political_tendency.append(np.random.choice(['보수', '중도', '진보'], p=[0.3, 0.4, 0.3]))
    else:  # 대학원졸
        political_tendency.append(np.random.choice(['보수', '중도', '진보'], p=[0.2, 0.3, 0.5]))

# 데이터 프레임 생성
df = pd.DataFrame({'education': education, 'political': political_tendency})

# 교차표 생성
contingency_table = pd.crosstab(df['education'], df['political'])
print("교차표:")
print(contingency_table)

# 카이제곱 독립성 검정 수행
chi2_stat, p_value, dof, expected = stats.chi2_contingency(contingency_table)

print(f"\n카이제곱 통계량: {chi2_stat:.4f}")
print(f"자유도: {dof}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'변수 간 유의한 연관성이 있음' if p_value < 0.05 else '변수 간 연관성이 없음'}")

print("\n기대 빈도:")
expected_df = pd.DataFrame(expected, index=contingency_table.index, columns=contingency_table.columns)
print(expected_df.round(2))

# 시각화
plt.figure(figsize=(12, 5))

# 1. 교차표 히트맵
plt.subplot(1, 2, 1)
sns.heatmap(contingency_table, annot=True, fmt='d', cmap='Blues')
plt.title('교육 수준과 정치 성향 빈도')

# 2. 100% 누적 막대 그래프
plt.subplot(1, 2, 2)
props = contingency_table.div(contingency_table.sum(axis=1), axis=0)
props.plot(kind='bar', stacked=True, width=0.8)
plt.title('교육 수준별 정치 성향 비율')
plt.xlabel('교육 수준')
plt.ylabel('비율')
plt.legend(title='정치 성향')
plt.ylim(0, 1)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 교육 수준과 정치 성향의 관계 분석 시
- 성별과 특정 질병 발생률의 연관성 검사 시
- 마케팅 전략과 구매 행동 간의 관계 파악 시
- 인구통계학적 특성과 소비 패턴 간의 연관성 연구 시
- 지역과 특정 의견의 연관성 확인 시

## 3. 카이제곱검정 - 동질성 검정 (Chi-square Test of Homogeneity)

**정의**: 서로 다른 집단 간에 특정 범주형 변수의 분포가 동일한지 검정하는 방법입니다. 즉, 여러 집단에서 관측된 범주의 비율이 동일한지 확인합니다.

**수식**: 동질성 검정도 독립성 검정과 동일한 공식을 사용합니다: $\chi^2 = \sum_{i=1}^{r}\sum_{j=1}^{c} \frac{(O_{ij} - E_{ij})^2}{E_{ij}}$

**특징**:

- 귀무가설은 "모든 집단에서 범주의 분포가 동일하다"입니다.
- 수학적으로는 독립성 검정과 동일하지만, 실험 설계와 해석이 다릅니다.
- 독립성 검정에서는 한 표본에서 두 변수의 관계를 검사하지만, 동질성 검정에서는 여러 독립 표본에서 한 변수의 분포를 비교합니다.
- 각 셀의 기대 빈도는 5 이상이 권장됩니다.
- 자유도는 (행 수 - 1) × (열 수 - 1)입니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
from scipy import stats
import matplotlib.pyplot as plt

# 예시: 세 지역에서의 선호 교통수단 비율 비교
# 데이터 생성: 지역 A, B, C에서의 교통수단(자가용, 대중교통, 자전거) 선호도
transportation = {
    '지역 A': [120, 80, 30],  # 자가용, 대중교통, 자전거 선호 인원
    '지역 B': [100, 100, 50],
    '지역 C': [80, 110, 60]
}

# 데이터 프레임 생성
df = pd.DataFrame(transportation, index=['자가용', '대중교통', '자전거'])
print("교차표:")
print(df)

# 카이제곱 동질성 검정 수행
chi2_stat, p_value, dof, expected = stats.chi2_contingency(df)

print(f"\n카이제곱 통계량: {chi2_stat:.4f}")
print(f"자유도: {dof}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'지역 간 교통수단 선호도에 유의한 차이가 있음' if p_value < 0.05 else '지역 간 교통수단 선호도에 유의한 차이가 없음'}")

print("\n기대 빈도:")
expected_df = pd.DataFrame(expected, index=df.index, columns=df.columns)
print(expected_df.round(2))

# 시각화
plt.figure(figsize=(12, 6))

# 1. 빈도 그래프
plt.subplot(1, 2, 1)
df.plot(kind='bar', width=0.7)
plt.title('지역별 교통수단 선호도 (빈도)')
plt.xlabel('교통수단')
plt.ylabel('선호 인원')
plt.grid(True, alpha=0.3)

# 2. 100% 누적 막대 그래프
plt.subplot(1, 2, 2)
props = df.div(df.sum(axis=0), axis=1).T  # 열 기준으로 비율 계산하고 전치
props.plot(kind='bar', stacked=True, width=0.7)
plt.title('지역별 교통수단 선호도 (비율)')
plt.xlabel('지역')
plt.ylabel('비율')
plt.ylim(0, 1)
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 여러 지역 간 특정 제품 선호도 비교 시
- 다양한 인구통계학적 집단의 응답 패턴 비교 시
- 서로 다른 치료법에 대한 환자 반응 분포 비교 시
- 여러 학교 또는 학급 간의 학생 성적 분포 비교 시
- 다양한 시간대 또는 요일별 고객 유형 분포 비교 시

## 4. 맨휘트니 U 검정 (Mann-Whitney U Test / Wilcoxon Rank-Sum Test)

**정의**: 두 독립적인 집단의 분포가 통계적으로 유의하게 다른지 검정하는 비모수적 방법입니다. 독립표본 t-검정의 비모수적 대안으로 사용됩니다.

**수식**: $U = n_1 n_2 + \frac{n_1(n_1+1)}{2} - R_1$ 여기서 $n_1$과 $n_2$는 두 집단의 표본 크기, $R_1$은 첫 번째 집단의 순위합입니다.

**특징**:

- 귀무가설은 "두 집단의 분포가 동일하다"입니다.
- 데이터의 정규성을 가정하지 않습니다.
- 서열 척도 데이터에도 적용 가능합니다.
- 모든 관측값을 통합하여 순위를 매기고, 각 집단의 순위합을 비교합니다.
- 중앙값뿐만 아니라 분포 전체의 차이를 검정합니다.
- 이상치에 강건합니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import seaborn as sns

# 데이터 생성: 두 치료법의 회복 시간 비교
np.random.seed(42)
treatment_A = np.random.exponential(scale=10, size=30)  # 비대칭 분포 (정규성 가정 위배)
treatment_B = np.random.exponential(scale=7, size=30) + 3

# 맨휘트니 U 검정 수행
u_stat, p_value = stats.mannwhitneyu(treatment_A, treatment_B, alternative='two-sided')

print(f"맨휘트니 U 통계량: {u_stat}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'두 집단의 분포가 유의하게 다름' if p_value < 0.05 else '두 집단의 분포에 유의한 차이가 없음'}")

# 참고: 동일한 데이터로 t-검정 수행 (정규성 가정 확인 없이)
t_stat, t_p_value = stats.ttest_ind(treatment_A, treatment_B)
print(f"\nt-검정 p-value: {t_p_value:.4f} (참고용)")

# 시각화
plt.figure(figsize=(12, 5))

# 1. 데이터 분포 비교
plt.subplot(1, 2, 1)
plt.hist(treatment_A, bins=10, alpha=0.5, label='치료법 A')
plt.hist(treatment_B, bins=10, alpha=0.5, label='치료법 B')
plt.xlabel('회복 시간')
plt.ylabel('빈도')
plt.title('두 치료법의 회복 시간 분포')
plt.legend()
plt.grid(True, alpha=0.3)

# 2. 박스플롯
plt.subplot(1, 2, 2)
plt.boxplot([treatment_A, treatment_B], labels=['치료법 A', '치료법 B'])
plt.ylabel('회복 시간')
plt.title(f'맨휘트니 U 검정: p={p_value:.4f}')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 데이터가 정규분포를 따르지 않을 때 두 집단 비교 시
- 척도가 서열형일 때 (예: 리커트 척도 설문 결과)
- 이상치가 있는 데이터 분석 시
- 표본 크기가 작을 때 정규성 가정을 확신할 수 없는 경우
- 고객 만족도와 같은 주관적 평가 데이터 비교 시

## 5. 크루스칼-월리스 검정 (Kruskal-Wallis Test)

**정의**: 세 개 이상의 독립적인 집단 간의 분포 차이를 검정하는 비모수적 방법입니다. 일원배치 ANOVA의 비모수적 대안으로 사용됩니다.

**수식**: $H = (N-1) \frac{\sum_{i=1}^{g} n_i(\bar{r}_i - \bar{r})^2}{\sum_{i=1}^{N} (r_i - \bar{r})^2}$ 여기서 $N$은 전체 표본 크기, $n_i$는 집단 i의 표본 크기, $\bar{r}_i$는 집단 i의 평균 순위, $\bar{r}$은 전체 평균 순위입니다.

**특징**:

- 귀무가설은 "모든 집단의 분포가 동일하다"입니다.
- 데이터의 정규성을 가정하지 않습니다.
- 모든 관측값을 통합하여 순위를 매기고, 집단별 평균 순위를 비교합니다.
- 자유도가 k-1인 카이제곱 분포를 따릅니다 (k는 집단 수).
- 유의한 결과를 얻으면 사후 검정(예: Dunn 검정)을 통해 어떤 집단 간에 차이가 있는지 확인해야 합니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import scikit_posthocs as sp

# 데이터 생성: 세 가지 학습 방법의 성취도 점수
np.random.seed(42)
method_A = np.random.normal(loc=70, scale=10, size=25)
method_B = np.random.normal(loc=75, scale=12, size=25)
method_C = np.random.normal(loc=80, scale=15, size=25)

# 데이터를 비대칭으로 변환 (비모수 검정의 필요성 강조)
method_A = np.exp(method_A / 25)
method_B = np.exp(method_B / 25)
method_C = np.exp(method_C / 25)

# 크루스칼-월리스 검정 수행
H, p_value = stats.kruskal(method_A, method_B, method_C)

print(f"크루스칼-월리스 H 통계량: {H:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'집단 간 유의한 차이가 있음' if p_value < 0.05 else '집단 간 유의한 차이가 없음'}")

# 사후 검정 (Dunn 검정)
if p_value < 0.05:
    # 모든 데이터와 그룹 라벨 준비
    all_data = np.concatenate([method_A, method_B, method_C])
    groups = np.repeat(['A', 'B', 'C'], [len(method_A), len(method_B), len(method_C)])
    
    # Dunn 검정 수행
    dunn_results = sp.posthoc_dunn([method_A, method_B, method_C], p_adjust='bonferroni')
    print("\nDunn 사후 검정 결과 (p-values):")
    print(dunn_results)

# 시각화
plt.figure(figsize=(12, 5))

# 1. 박스플롯
plt.subplot(1, 2, 1)
plt.boxplot([method_A, method_B, method_C], labels=['방법 A', '방법 B', '방법 C'])
plt.ylabel('성취도 점수')
plt.title(f'크루스칼-월리스 검정: H={H:.2f}, p={p_value:.4f}')
plt.grid(True, alpha=0.3)

# 2. 바이올린 플롯
plt.subplot(1, 2, 2)
data = [method_A, method_B, method_C]
violin = plt.violinplot(data, showmeans=True, showmedians=True)
for i, pc in enumerate(violin['bodies']):
    pc.set_facecolor(f'C{i}')
    pc.set_alpha(0.7)

plt.xticks([1, 2, 3], ['방법 A', '방법 B', '방법 C'])
plt.ylabel('성취도 점수')
plt.title('방법별 성취도 분포')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 데이터가 정규분포를 따르지 않을 때 세 개 이상 집단 비교 시
- 서열 척도로 측정된 여러 집단의 성과 비교 시
- 다양한 수업 방식, 치료법, 약물 등의 효과 비교 시
- 이상치가 많아 ANOVA 가정을 충족하지 못하는 경우
- 작은 표본 크기로 정규성을 확신할 수 없는 경우

## 6. 윌콕슨 부호 순위 검정 (Wilcoxon Signed-Rank Test)

**정의**: 두 대응(짝지어진) 표본의 차이가 통계적으로 유의한지 검정하는 비모수적 방법입니다. 대응표본 t-검정의 비모수적 대안입니다.

**수식**: $W = \min(W^+, W^-)$ 여기서 $W^+$는 양의 순위합, $W^-$는 음의 순위합입니다.

**특징**:

- 귀무가설은 "대응된 차이의 중앙값이 0이다"입니다.
- 차이의 크기와 방향을 모두 고려합니다.
- 정규성 가정이 필요 없습니다.
- 동일한 개체나 대상에 대한 전후 비교에 적합합니다.
- 이상치에 강건합니다.
- 0이 아닌 차이에 대해서만 순위를 매깁니다(차이가 0인 쌍은 제외).

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 데이터 생성: 10명의 환자에 대한 치료 전후 통증 점수
np.random.seed(42)
n = 15  # 환자 수
before = np.random.normal(loc=7, scale=1.5, size=n)  # 치료 전 통증 점수 (0-10 척도)
before = np.clip(before, 0, 10)  # 0-10 범위로 클리핑

# 치료 효과에 개인차 (비정규성 추가)
effect = np.random.exponential(scale=2, size=n) * -1  # 음수 효과 (통증 감소)
after = before + effect
after = np.clip(after, 0, 10)  # 0-10 범위로 클리핑

# 윌콕슨 부호 순위 검정 수행
w_stat, p_value = stats.wilcoxon(before, after)

print(f"윌콕슨 부호 순위 통계량: {w_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'치료 전후 통증에 유의한 차이가 있음' if p_value < 0.05 else '치료 전후 통증에 유의한 차이가 없음'}")

# 대응표본 t-검정 (비교용)
t_stat, t_p_value = stats.ttest_rel(before, after)
print(f"\n대응표본 t-검정 p-value: {t_p_value:.4f} (참고용)")

# 시각화
plt.figure(figsize=(12, 5))

# 1. 개인별 전후 변화
plt.subplot(1, 2, 1)
for i in range(n):
    plt.plot([1, 2], [before[i], after[i]], 'o-', alpha=0.5)

plt.plot([1, 2], [np.mean(before), np.mean(after)], 'r-', linewidth=2, label='평균')
plt.xticks([1, 2], ['치료 전', '치료 후'])
plt.ylabel('통증 점수')
plt.ylim(0, 10)
plt.title('환자별 치료 전후 통증 변화')
plt.legend()
plt.grid(True, alpha=0.3)

# 2. 차이 분포
plt.subplot(1, 2, 2)
differences = before - after
plt.hist(differences, bins=8, alpha=0.7, color='skyblue')
plt.axvline(np.median(differences), color='r', linestyle='--', 
            label=f'중앙값: {np.median(differences):.2f}')
plt.axvline(0, color='k', linestyle='-', alpha=0.3)
plt.xlabel('치료 전 - 치료 후')
plt.ylabel('빈도')
plt.title(f'통증 점수 차이 (p={p_value:.4f})')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 약물 치료 전후의 증상 변화가 정규분포를 따르지 않을 때
- 교육 프로그램 전후의 능력 변화 평가 시
- 새로운 훈련 방법 도입 전후의 운동 성과 비교 시
- 환자의 주관적 통증 점수 같은 서열 척도 데이터 분석 시
- 동일 주제에 대한 두 평가자의 점수 차이 분석 시

## 7. 프리드먼 검정 (Friedman Test)

**정의**: 세 개 이상의 대응된(관련된) 집단 간의 차이를 검정하는 비모수적 방법입니다. 반복측정 일원배치 ANOVA의 비모수적 대안입니다.

**수식**: $\chi_r^2 = \frac{12n}{k(k+1)}\sum_{j=1}^{k}(R_j - \frac{k+1}{2})^2$ 여기서 n은 블록(주체) 수, k는 처리(조건) 수, R_j는 j번째 처리의 평균 순위입니다.

**특징**:

- 귀무가설은 "모든 처리의 효과가 동일하다"입니다.
- 대응된 설계에서 사용됩니다 (예: 동일한 대상으로 여러 처리 비교).
- 각 블록 내에서 처리에 순위를 매깁니다.
- 정규성 가정이 필요 없습니다.
- 유의한 결과를 얻으면 사후 검정이 필요합니다.
- 자유도가 k-1인 카이제곱 분포를 따릅니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import pandas as pd
import scikit_posthocs as sp

# 데이터 생성: 10명의 피험자가 4가지 다른 운동을 한 후의 심박수
np.random.seed(42)
n_subjects = 10  # 피험자 수
n_exercises = 4  # 운동 유형 수

# 기본 심박수와 개인차
base_heart_rates = np.random.normal(loc=70, scale=5, size=n_subjects)

# 4가지 운동의 효과 (운동별로 심박수 증가 정도가 다름)
exercise_effects = {
    'A': np.random.normal(loc=20, scale=3, size=n_subjects),  # 가벼운 운동
    'B': np.random.normal(loc=35, scale=5, size=n_subjects),  # 중간 강도
    'C': np.random.normal(loc=50, scale=7, size=n_subjects),  # 고강도
    'D': np.random.normal(loc=30, scale=4, size=n_subjects)   # 중간-고강도
}

# 데이터 생성
data = {}
for ex, effects in exercise_effects.items():
    data[f'운동{ex}'] = base_heart_rates + effects

# 데이터프레임 생성
df = pd.DataFrame(data)
print("피험자별 각 운동 후 심박수:")
print(df)

# 프리드먼 검정 수행
chi2_stat, p_value = stats.friedmanchisquare(*[df[col] for col in df.columns])

print(f"\n프리드먼 카이제곱 통계량: {chi2_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'운동 종류에 따라 심박수에 유의한 차이가 있음' if p_value < 0.05 else '운동 종류에 따른 심박수에 유의한 차이가 없음'}")

# 사후 검정 (Nemenyi 검정)
if p_value < 0.05:
    post_hoc = sp.posthoc_nemenyi_friedman(df.values)
    post_hoc.columns = df.columns
    post_hoc.index = df.columns
    print("\nNemenyi 사후 검정 결과 (p-values):")
    print(post_hoc)

# 시각화
plt.figure(figsize=(12, 6))

# 1. 박스플롯
plt.subplot(1, 2, 1)
df.boxplot()
plt.ylabel('심박수')
plt.title(f'운동별 심박수 비교\n프리드먼 검정: χ²={chi2_stat:.2f}, p={p_value:.4f}')
plt.grid(True, alpha=0.3)

# 2. 주체별 라인 그래프
plt.subplot(1, 2, 2)
for i in range(n_subjects):
    plt.plot(df.columns, df.iloc[i], 'o-', alpha=0.4)

plt.plot(df.columns, df.mean(), 'ro-', linewidth=2, label='평균')
plt.ylabel('심박수')
plt.title('피험자별 운동에 따른 심박수 변화')
plt.grid(True, alpha=0.3)
plt.legend()

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 동일한 참가자가 여러 다른 조건에서 평가되는 실험 분석 시
- 여러 제품에 대한 소비자 선호도 평가 시
- 반복 시행으로 동일한 측정을 여러 번 하는 경우
- 데이터가 정규성 가정을 충족하지 않는 반복측정 설계 분석 시
- 여러 재배 조건에서 동일한 식물 품종의 성장 비교 시

## 8. 맥니마 검정 (McNemar's Test)

**정의**: 대응된 이분형(binary) 자료에서 비율의 변화가 통계적으로 유의한지 검정하는 방법입니다. 전후 설계에서 범주형 변수의 변화를 분석하는 데 사용됩니다.

**수식**: $\chi^2 = \frac{(b - c)^2}{b + c}$ 여기서 b와 c는 불일치 쌍의 빈도입니다 (2×2 분할표에서 off-diagonal 항목).

**특징**:

- 귀무가설은 "두 조건 간 불일치 비율이 같다"입니다.
- 단일 표본에 대한 전후 비교에 사용됩니다.
- 데이터는 반드시 대응되어야 합니다(예: 같은 사람의 전후 측정).
- 이항 분포를 기반으로 하며, 자유도가 1인 카이제곱 분포를 따릅니다.
- b + c ≥ 10이면 카이제곱 근사를 사용하고, 작으면 이항 검정을 권장합니다.
- 방향성(한쪽이 다른 쪽보다 나은지)을 파악할 수 있습니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import pandas as pd

# 데이터 생성: 100명의 환자에 대한 치료 전후 증상 유무
np.random.seed(42)
n = 100

# 치료 전 상태 (1=증상 있음, 0=증상 없음)
before = np.random.binomial(1, 0.7, size=n)  # 70%가 증상 있음

# 치료 후 상태 (효과 시뮬레이션)
after = before.copy()
for i in range(n):
    if before[i] == 1:  # 원래 증상이 있던 경우
        after[i] = np.random.binomial(1, 0.4, size=1)[0]  # 60%가 증상 개선
    else:  # 원래 증상이 없던 경우
        after[i] = np.random.binomial(1, 0.1, size=1)[0]  # 10%만 증상 발생

# 분할표 생성
table = pd.crosstab(before, after, rownames=['Before'], colnames=['After'])
print("치료 전후 분할표:")
print(table)

# 맥니마 검정 수행
result = stats.mcnemar(table, exact=False, correction=True)
print(f"\n맥니마 카이제곱 통계량: {result.statistic:.4f}")
print(f"p-value: {result.pvalue:.4f}")
print(f"결론: {'치료 전후 증상 분포에 유의한 변화가 있음' if result.pvalue < 0.05 else '치료 전후 증상 분포에 유의한 변화가 없음'}")

# 개선 및 악화 비율 계산
improved = sum((before == 1) & (after == 0))  # 증상 있음 → 없음
worsened = sum((before == 0) & (after == 1))  # 증상 없음 → 있음
print(f"\n개선된 환자 수: {improved} ({improved/n*100:.1f}%)")
print(f"악화된 환자 수: {worsened} ({worsened/n*100:.1f}%)")
print(f"변화 없는 환자 수: {n - improved - worsened} ({(n - improved - worsened)/n*100:.1f}%)")

# 시각화
plt.figure(figsize=(12, 5))

# 1. 치료 전후 비율 막대 그래프
plt.subplot(1, 2, 1)
labels = ['증상 없음', '증상 있음']
before_counts = [sum(before == 0), sum(before == 1)]
after_counts = [sum(after == 0), sum(after == 1)]

x = np.arange(len(labels))
width = 0.35

plt.bar(x - width/2, before_counts, width, label='치료 전')
plt.bar(x + width/2, after_counts, width, label='치료 후')

plt.xlabel('증상 상태')
plt.ylabel('환자 수')
plt.title('치료 전후 증상 상태 비교')
plt.xticks(x, labels)
plt.legend()
plt.grid(True, alpha=0.3)

# 2. 상태 변화 흐름도
plt.subplot(1, 2, 2)
states = ['증상 없음 유지', '악화', '개선', '증상 있음 유지']
counts = [
    sum((before == 0) & (after == 0)),
    sum((before == 0) & (after == 1)),
    sum((before == 1) & (after == 0)),
    sum((before == 1) & (after == 1))
]

plt.pie(counts, labels=states, autopct='%1.1f%%', startangle=90)
plt.axis('equal')
plt.title(f'환자 상태 변화\n맥니마 검정: p={result.pvalue:.4f}')

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 약물 치료 전후의 증상 유무 변화 분석 시
- 교육 프로그램 전후의 특정 개념 이해도 변화 측정 시
- 마케팅 캠페인 전후의 브랜드 인지도 변화 평가 시
- 수술 전후의 통증 유무 비교 시
- 정책 변경 전후의 찬성/반대 비율 변화 분석 시

## 9. 피셔의 정확확률검정 (Fisher's Exact Test)

**정의**: 두 범주형 변수의 연관성을 검정하는 방법으로, 카이제곱 검정의 대안으로 특히 표본 크기가 작거나 기대빈도가 작을 때 사용됩니다.

**수식**: $p = \frac{(a+b)!(c+d)!(a+c)!(b+d)!}{a!b!c!d!n!}$ 여기서 a, b, c, d는 2×2 분할표의 셀 빈도이고, n은 총 관측 수입니다.

**특징**:

- 귀무가설은 "행과 열 변수가 독립적이다"입니다.
- 표본 크기가 작거나 기대빈도가 5 미만인 셀이 있을 때 카이제곱 검정 대신 사용합니다.
- 초기하분포를 기반으로 하며, 정확한 p-value를 계산합니다.
- 행과 열 합계가 고정된 것으로 간주합니다(조건부 검정).
- 2×2 분할표에 가장 흔히 사용되지만, 더 큰 분할표에도 확장 가능합니다.
- 계산이 복잡하지만 컴퓨터로 쉽게 수행할 수 있습니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

# 데이터 생성: 새로운 치료법과 기존 치료법의 효과 비교 (작은 표본)
np.random.seed(42)

# 2x2 분할표 데이터
table = np.array([[9, 3],  # 새 치료법: [회복, 비회복]
                  [4, 8]])  # 기존 치료법: [회복, 비회복]

# 피셔의 정확확률검정 수행
oddsratio, p_value = stats.fisher_exact(table)

print("치료법과 회복 여부 분할표:")
print(pd.DataFrame(table, 
                   index=['새 치료법', '기존 치료법'], 
                   columns=['회복', '비회복']))

print(f"\n오즈비: {oddsratio:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'치료법과 회복 여부 간에 유의한 연관성이 있음' if p_value < 0.05 else '치료법과 회복 여부 간에 유의한 연관성이 없음'}")

# 비교: 카이제곱 검정 (참고용)
chi2, p_chi2, dof, expected = stats.chi2_contingency(table)
print(f"\n카이제곱 검정 p-value: {p_chi2:.4f} (참고용)")
print("기대 빈도:")
print(pd.DataFrame(expected, 
                   index=['새 치료법', '기존 치료법'], 
                   columns=['회복', '비회복']).round(2))

# 시각화
plt.figure(figsize=(12, 5))

# 1. 분할표 히트맵
plt.subplot(1, 2, 1)
df_table = pd.DataFrame(table, 
                        index=['새 치료법', '기존 치료법'], 
                        columns=['회복', '비회복'])
sns.heatmap(df_table, annot=True, fmt='d', cmap='Blues')
plt.title(f'치료법과 회복 여부 분할표\n피셔 검정: p={p_value:.4f}')

# 2. 그룹별 회복률 비교
plt.subplot(1, 2, 2)
recovery_rates = [table[0, 0] / sum(table[0]), table[1, 0] / sum(table[1])]
plt.bar(['새 치료법', '기존 치료법'], recovery_rates)
plt.ylim(0, 1)
plt.ylabel('회복률')
plt.title('치료법별 회복률 비교')
plt.grid(True, alpha=0.3)

for i, rate in enumerate(recovery_rates):
    plt.text(i, rate + 0.02, f'{rate:.2f}', ha='center')

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 임상 시험에서 표본 크기가 작을 때 치료 효과 분석 시
- 희귀 질환 연구에서 위험 요인과 질병 간의 연관성 평가 시
- 소규모 설문조사에서 두 범주형 변수 간의 연관성 검정 시
- 기대 빈도가 작은 셀이 있는 분할표 분석 시
- 정확한 p-value가 필요한 중요한 의사결정 상황에서

## 10. 윌콕슨 순위합 검정 (Wilcoxon Rank-Sum Test)

**정의**: 두 독립 표본의 분포가 통계적으로 유의하게 다른지 검정하는 비모수적 방법으로, 맨-휘트니 U 검정과 동일합니다. 여기서는 이미 맨-휘트니 U 검정을 다루었으므로, 추가적인 측면에 중점을 두겠습니다.

**수식**: $W = \sum_{i=1}^{n_1} R_i - \frac{n_1(n_1+1)}{2}$ 여기서 $R_i$는 첫 번째 표본의 순위합, $n_1$은 첫 번째 표본의 크기입니다.

**특징**:

- 맨-휘트니 U 검정과 수학적으로 동등하며, 계산 방식만 다릅니다.
- 두 집단의 중앙값 차이 검정보다는 분포 전체의 위치 차이를 검정합니다.
- 비대칭적 분포에도 적용할 수 있습니다.
- 서로 다른 분포 형태(예: 하나는 정규분포, 다른 하나는 지수분포)인 경우 해석에 주의해야 합니다.
- 동점(tie)이 많을 경우 수정된 검정 통계량을 사용해야 합니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import pandas as pd

# 데이터 생성: 두 가지 진통제의 통증 완화 시간(분) 비교
np.random.seed(42)
drug_A = np.random.weibull(1.5, size=20) * 20 + 10  # 비대칭 분포
drug_B = np.random.weibull(1.5, size=25) * 15 + 15  # 약간 다른 분포

# 윌콕슨 순위합 검정 수행
w_stat, p_value = stats.ranksums(drug_A, drug_B)

print(f"윌콕슨 순위합 통계량: {w_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'두 약물의 효과에 유의한 차이가 있음' if p_value < 0.05 else '두 약물의 효과에 유의한 차이가 없음'}")

# 기술 통계량
print("\n기술 통계량:")
print(f"약물 A (n={len(drug_A)}): 중앙값={np.median(drug_A):.2f}, 평균={np.mean(drug_A):.2f}, 표준편차={np.std(drug_A):.2f}")
print(f"약물 B (n={len(drug_B)}): 중앙값={np.median(drug_B):.2f}, 평균={np.mean(drug_B):.2f}, 표준편차={np.std(drug_B):.2f}")

# 시각화
plt.figure(figsize=(12, 5))

# 1. 히스토그램 비교
plt.subplot(1, 2, 1)
plt.hist(drug_A, alpha=0.5, label='약물 A', bins=10)
plt.hist(drug_B, alpha=0.5, label='약물 B', bins=10)
plt.xlabel('통증 완화 시간(분)')
plt.ylabel('빈도')
plt.title('약물별 통증 완화 시간 분포')
plt.legend()
plt.grid(True, alpha=0.3)

# 2. 박스플롯 비교
plt.subplot(1, 2, 2)
data = [drug_A, drug_B]
plt.boxplot(data, labels=['약물 A', '약물 B'])
plt.ylabel('통증 완화 시간(분)')
plt.title(f'윌콕슨 순위합 검정: p={p_value:.4f}')
plt.grid(True, alpha=0.3)

# 중앙값 표시
medians = [np.median(drug_A), np.median(drug_B)]
for i, median in enumerate(medians):
    plt.text(i+1, median+1, f'중앙값: {median:.1f}', ha='center')

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 약물 효과가 정규분포를 따르지 않을 때 두 약물 비교 시
- 두 집단의 전체 분포 차이를 확인할 때
- 극단값이나 이상치가 있는 데이터 분석 시
- 순서형 척도로 측정된 두 집단의 차이 검정 시
- t-검정의 가정이 위배되지만 두 집단을 비교해야 할 때

## 11. 코크란의 Q 검정 (Cochran's Q Test)

**정의**: 셋 이상의 대응된 이분형(binary) 데이터에서 비율의 차이를 검정하는 비모수적 방법입니다. 맥니마 검정을 세 개 이상의 조건으로 확장한 버전입니다.

**수식**: $Q = \frac{k(k-1)\sum_{j=1}^{k}(c_j - \bar{c})^2}{\sum_{i=1}^{n}r_i(k-r_i)}$ 여기서 k는 조건 수, n은 피험자 수, c_j는 조건 j에서의 성공 수, r_i는 피험자 i의 성공 수입니다.

**특징**:

- 귀무가설은 "모든 조건에서 성공 확률이 동일하다"입니다.
- 동일한 n명의 피험자가 k개의 서로 다른 조건에서 측정되어야 합니다.
- 결과는 이분형(예/아니오, 성공/실패)이어야 합니다.
- 자유도가 k-1인 카이제곱 분포를 따릅니다.
- 유의한 결과가 나오면 사후 검정이 필요합니다.
- 반복 측정된 이분형 자료에 적합합니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import pandas as pd

# 데이터 생성: 동일한 20명의 피험자에게 3가지 다른 약물을 투여한 후 부작용 발생 여부
np.random.seed(42)
n_subjects = 20  # 피험자 수
n_treatments = 3  # 약물 수

# 개인별 부작용 민감도 (0-1 사이의 값)
sensitivity = np.random.beta(2, 5, size=n_subjects)

# 약물별 부작용 유발 가능성 (낮을수록 부작용 가능성이 높음)
drug_effects = [0.7, 0.4, 0.5]  # 약물 A, B, C의 부작용 역치

# 부작용 발생 여부 데이터 생성 (1: 부작용 있음, 0: 부작용 없음)
data = np.zeros((n_subjects, n_treatments), dtype=int)
for i in range(n_subjects):
    for j in range(n_treatments):
        # 개인 민감도가 약물 역치보다 높으면 부작용 발생
        data[i, j] = 1 if sensitivity[i] > drug_effects[j] else 0

# 데이터프레임 생성
df = pd.DataFrame(data, columns=['약물A', '약물B', '약물C'])
print("피험자별 부작용 발생 데이터 (1: 부작용 있음, 0: 부작용 없음):")
print(df.head())

# 약물별 부작용 발생 비율
drug_rates = df.mean()
print("\n약물별 부작용 발생률:")
for drug, rate in drug_rates.items():
    print(f"{drug}: {rate:.2f} ({int(rate * n_subjects)}/{n_subjects}명)")

# 코크란 Q 검정 수행
q_stat, p_value = stats.cochrans_q(df['약물A'], df['약물B'], df['약물C'])

print(f"\n코크란 Q 통계량: {q_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"결론: {'약물 간 부작용 발생률에 유의한 차이가 있음' if p_value < 0.05 else '약물 간 부작용 발생률에 유의한 차이가 없음'}")

# 사후 검정: 약물 쌍별 맥니마 검정
if p_value < 0.05:
    print("\n사후 검정 (맥니마 검정):")
    pairs = [('약물A', '약물B'), ('약물A', '약물C'), ('약물B', '약물C')]
    
    for pair in pairs:
        table = pd.crosstab(df[pair[0]], df[pair[1]])
        try:
            result = stats.mcnemar(table, exact=False, correction=True)
            print(f"{pair[0]} vs {pair[1]}: 통계량={result.statistic:.4f}, p={result.pvalue:.4f}")
        except:
            print(f"{pair[0]} vs {pair[1]}: 분할표 문제로 검정 불가")

# 시각화
plt.figure(figsize=(12, 5))

# 1. 부작용 발생률 비교
plt.subplot(1, 2, 1)
plt.bar(drug_rates.index, drug_rates.values)
plt.ylim(0, 1)
plt.ylabel('부작용 발생률')
plt.title(f'약물별 부작용 발생률\n코크란 Q 검정: p={p_value:.4f}')
plt.grid(True, alpha=0.3)

for i, rate in enumerate(drug_rates):
    plt.text(i, rate + 0.02, f'{rate:.2f}', ha='center')

# 2. 피험자별 패턴 시각화
plt.subplot(1, 2, 2)
plt.imshow(df.values, cmap='Blues', aspect='auto')
plt.colorbar(ticks=[0, 1], label='부작용 여부')
plt.xlabel('약물')
plt.ylabel('피험자')
plt.title('피험자별 부작용 발생 패턴')
plt.xticks(np.arange(n_treatments), df.columns)
plt.yticks(np.arange(n_subjects), np.arange(1, n_subjects+1))

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 여러 약물의 부작용 발생률 비교 시
- 다양한 마케팅 메시지에 대한 소비자 반응 분석 시
- 여러 교육 방법에 따른 학생들의 성공/실패 비율 비교 시
- 다양한 조건에서의 제품 합격/불합격 비율 비교 시
- 반복 측정된 설문조사에서 이분형 응답의 변화 분석 시

## 12. Run 검정 (Runs Test)

**정의**: 데이터의 순서나 시퀀스가 무작위적인지 또는 일정한 패턴이 있는지 검정하는 비모수적 방법입니다. 데이터의 무작위성(randomness)을 평가합니다.

**수식**: $Z = \frac{R - \mu_R}{\sigma_R}$ 여기서 $R$은 관측된 런(run)의 수, $\mu_R = \frac{2n_1n_2}{n_1+n_2}+1$, $\sigma_R = \sqrt{\frac{2n_1n_2(2n_1n_2-n_1-n_2)}{(n_1+n_2)^2(n_1+n_2-1)}}$, $n_1$과 $n_2$는 두 범주의 개수입니다.

**특징**:

- 귀무가설은 "데이터 시퀀스가 무작위적이다"입니다.
- 런(run)은 동일한 값이 연속적으로 나타나는 구간을 의미합니다.
- 런이 너무 적으면 데이터에 군집성(clustering)이 있다는 증거입니다.
- 런이 너무 많으면 데이터에 주기성(periodicity)이 있다는 증거입니다.
- 연속형 데이터는 먼저 중앙값 등의 기준으로 이분화해야 합니다.
- 주로 시계열 데이터나 순차적 데이터의 무작위성 검정에 사용됩니다.

**코드 예시**:

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# 데이터 생성: 주식 가격 변동 (상승 또는 하락)
np.random.seed(42)

# 1. 무작위 시퀀스
random_seq = np.random.choice([1, 0], size=100)  # 1: 상승, 0: 하락

# 2. 패턴이 있는 시퀀스 (군집성이 있는 경우)
pattern_seq = []
current = 1  # 시작값
for i in range(100):
    if i % 15 == 0:  # 15일마다 추세 변경
        current = 1 - current
    # 추세에 맞게 높은 확률로 같은 값, 낮은 확률로 다른 값
    pattern_seq.append(current if np.random.random() < 0.8 else 1 - current)

# 3. 교대로 바뀌는 시퀀스 (주기성이 있는 경우)
alternating_seq = []
for i in range(100):
    # 약간의 랜덤성을 추가하되, 대체로 교대로 바뀌게 함
    if i == 0:
        alternating_seq.append(1)
    else:
        if np.random.random() < 0.75:  # 75% 확률로 이전과 다른 값
            alternating_seq.append(1 - alternating_seq[i-1])
        else:
            alternating_seq.append(alternating_seq[i-1])

# 런 검정 수행
def runs_test(sequence):
    # 런 개수 및 각 값의 개수 계산
    runs, n1, n2 = 1, sum(sequence), len(sequence) - sum(sequence)
    for i in range(1, len(sequence)):
        if sequence[i] != sequence[i-1]:
            runs += 1
    
    # 통계량 계산
    runs_expected = (2 * n1 * n2) / (n1 + n2) + 1
    runs_std = np.sqrt((2 * n1 * n2 * (2 * n1 * n2 - n1 - n2)) / 
                        ((n1 + n2)**2 * (n1 + n2 - 1)))
    
    z = (runs - runs_expected) / runs_std
    p_value = 2 * (1 - stats.norm.cdf(abs(z)))  # 양측 검정
    
    return runs, runs_expected, z, p_value

# 세 시퀀스에 대해 런 검정 수행
results = {}
for name, seq in [('무작위', random_seq), ('군집성', pattern_seq), ('주기성', alternating_seq)]:
    runs, expected, z, p = runs_test(seq)
    results[name] = {'runs': runs, 'expected': expected, 'z': z, 'p': p}
    
    print(f"\n{name} 시퀀스 런 검정:")
    print(f"런 개수: {runs}, 기대 런 개수: {expected:.2f}")
    print(f"Z-통계량: {z:.4f}")
    print(f"p-value: {p:.4f}")
    print(f"결론: {'무작위성 가정 기각' if p < 0.05 else '무작위성 가정 기각 실패'}")

# 시각화
plt.figure(figsize=(15, 10))

# 1. 시퀀스 시각화
for i, (name, seq) in enumerate([('무작위', random_seq), 
                                ('군집성', pattern_seq), 
                                ('주기성', alternating_seq)]):
    plt.subplot(3, 1, i+1)
    
    # 시퀀스 플롯
    x = np.arange(len(seq))
    plt.step(x, seq, where='mid')
    
    # 런 경계 표시
    for j in range(1, len(seq)):
        if seq[j] != seq[j-1]:
            plt.axvline(x=j-0.5, color='r', linestyle='--', alpha=0.5)
    
    plt.ylabel('값 (1: 상승, 0: 하락)')
    plt.title(f'{name} 시퀀스 - 런 개수: {results[name]["runs"]}, p-value: {results[name]["p"]:.4f}')
    plt.ylim(-0.1, 1.1)
    plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 주식 시장이나 경제 지표의 무작위성 검정 시
- 품질 관리에서 제품 불량 발생의 무작위성 확인 시
- 실험 데이터가 진정한 랜덤 샘플인지 검증할 때
- 게임 결과나 도박 결과의 공정성 평가 시
- DNA 서열이나 기타 생물학적 서열의 무작위성 분석 시
---
# 상관분석 (Correlation Analysis)

## 1. 상관관계분석 (Correlation Analysis)

**정의**: 상관관계분석은 두 연속형 변수 간의 선형적 관계의 강도와 방향을 측정하는 통계적 방법입니다. 두 변수가 함께 변화하는 경향이 있는지, 그리고 그 경향이 어느 정도인지를 수치화합니다.

**수식**:

- 피어슨 상관계수(Pearson's correlation coefficient): $r = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n}(x_i - \bar{x})^2 \sum_{i=1}^{n}(y_i - \bar{y})^2}}$
    
- 스피어만 상관계수(Spearman's rank correlation): $r_s = 1 - \frac{6\sum d_i^2}{n(n^2-1)}$ 여기서 $d_i$는 i번째 관측값의 두 변수 간 순위 차이입니다.
    

**특징**:

- 상관계수는 -1에서 1 사이의 값을 가집니다.
- 1에 가까울수록 강한 양의 상관관계, -1에 가까울수록 강한 음의 상관관계를 나타냅니다.
- 0에 가까우면 선형적 관계가 없다는 것을 의미합니다.
- 피어슨 상관계수는 선형 관계만 측정하며, 비선형 관계는 감지하지 못합니다.
- 상관관계는 인과관계를 의미하지 않습니다.
- 이상치에 민감하며, 분포의 형태에 영향을 받을 수 있습니다.
- 스피어만 상관계수는 변수의 순위를 사용하므로 비선형 관계도 측정할 수 있고 이상치에 덜 민감합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats

# 데이터 생성
np.random.seed(42)
n = 50

# 강한 양의 상관관계
x1 = np.random.normal(0, 1, n)
y1 = x1 * 0.8 + np.random.normal(0, 0.5, n)

# 약한 음의 상관관계
x2 = np.random.normal(0, 1, n)
y2 = -x2 * 0.4 + np.random.normal(0, 0.8, n)

# 상관관계 없음
x3 = np.random.normal(0, 1, n)
y3 = np.random.normal(0, 1, n)

# 비선형 관계 (피어슨은 감지하지 못하지만 스피어만은 감지 가능)
x4 = np.random.uniform(-3, 3, n)
y4 = x4**2 + np.random.normal(0, 1, n)

# 피어슨 상관계수 계산
pearson1, p_value1 = stats.pearsonr(x1, y1)
pearson2, p_value2 = stats.pearsonr(x2, y2)
pearson3, p_value3 = stats.pearsonr(x3, y3)
pearson4, p_value4 = stats.pearsonr(x4, y4)

# 스피어만 상관계수 계산
spearman1, sp_value1 = stats.spearmanr(x1, y1)
spearman2, sp_value2 = stats.spearmanr(x2, y2)
spearman3, sp_value3 = stats.spearmanr(x3, y3)
spearman4, sp_value4 = stats.spearmanr(x4, y4)

# 결과 출력
print("강한 양의 상관관계:")
print(f"피어슨 상관계수: {pearson1:.4f}, p-value: {p_value1:.4f}")
print(f"스피어만 상관계수: {spearman1:.4f}, p-value: {sp_value1:.4f}")

print("\n약한 음의 상관관계:")
print(f"피어슨 상관계수: {pearson2:.4f}, p-value: {p_value2:.4f}")
print(f"스피어만 상관계수: {spearman2:.4f}, p-value: {sp_value2:.4f}")

print("\n상관관계 없음:")
print(f"피어슨 상관계수: {pearson3:.4f}, p-value: {p_value3:.4f}")
print(f"스피어만 상관계수: {spearman3:.4f}, p-value: {sp_value3:.4f}")

print("\n비선형 관계:")
print(f"피어슨 상관계수: {pearson4:.4f}, p-value: {p_value4:.4f}")
print(f"스피어만 상관계수: {spearman4:.4f}, p-value: {sp_value4:.4f}")

# 상관관계 시각화
plt.figure(figsize=(14, 10))

plt.subplot(2, 2, 1)
plt.scatter(x1, y1)
plt.title(f'강한 양의 상관관계\n피어슨: {pearson1:.4f}, 스피어만: {spearman1:.4f}')
plt.grid(True, alpha=0.3)

plt.subplot(2, 2, 2)
plt.scatter(x2, y2)
plt.title(f'약한 음의 상관관계\n피어슨: {pearson2:.4f}, 스피어만: {spearman2:.4f}')
plt.grid(True, alpha=0.3)

plt.subplot(2, 2, 3)
plt.scatter(x3, y3)
plt.title(f'상관관계 없음\n피어슨: {pearson3:.4f}, 스피어만: {spearman3:.4f}')
plt.grid(True, alpha=0.3)

plt.subplot(2, 2, 4)
plt.scatter(x4, y4)
plt.title(f'비선형 관계\n피어슨: {pearson4:.4f}, 스피어만: {spearman4:.4f}')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 주가와 환율 같은 경제 지표 간의 관계 분석 시
- 학생의 공부 시간과 시험 성적 간의 연관성 파악 시
- 날씨 변수(온도, 습도 등)와 판매량 간의 관계 분석 시
- 생체 측정치 간의 상관관계 연구(예: 키와 체중, 혈압과 맥박)
- 마케팅 활동과 매출 사이의 관계 평가 시

## 2. 편 상관관계분석 (Partial Correlation Analysis)

**정의**: 편 상관관계분석은 다른 변수들의 영향을 통제한 상태에서 두 변수 간의 순수한 선형 관계를 측정하는 방법입니다. 즉, 제3의 변수(또는 여러 변수)의 효과를 제거한 후 남은 두 변수 간의 상관관계를 의미합니다.

**수식**:

- 단일 통제 변수(z)가 있을 때의 x와 y 간의 편 상관계수: $r_{xy.z} = \frac{r_{xy} - r_{xz}r_{yz}}{\sqrt{(1-r_{xz}^2)(1-r_{yz}^2)}}$

여기서 $r_{xy}$, $r_{xz}$, $r_{yz}$는 각각 변수 쌍 간의 피어슨 상관계수입니다.

**특징**:

- 다른 변수들의 간접적 영향을 제거하여 두 변수 간의 직접적인 관계를 측정합니다.
- 잠재적 혼동 변수(confounding variables)의 효과를 통제할 수 있습니다.
- 편 상관계수도 -1에서 1 사이의 값을 가집니다.
- 원래 상관관계와 편 상관관계가 크게 다를 경우, 통제 변수가 중요한 매개 역할을 하는 것으로 해석할 수 있습니다.
- 여러 변수를 동시에 통제할 수 있습니다.
- 선형 관계만 측정하며, 비선형 관계는 감지하지 못합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats
import pingouin as pg  # 편 상관관계 계산을 위한 라이브러리

# 데이터 생성: 나이(age), 운동량(exercise), 체중(weight)
np.random.seed(42)
n = 100

# 나이가 많을수록 체중이 증가하는 경향
age = np.random.uniform(20, 70, n)
# 운동량은 나이와 약한 음의 상관관계
exercise = 10 - 0.05 * age + np.random.normal(0, 2, n)
# 체중은 나이와 양의 상관관계, 운동량과 음의 상관관계
weight = 50 + 0.3 * age - 1.2 * exercise + np.random.normal(0, 5, n)

# 데이터프레임 생성
df = pd.DataFrame({
    'age': age,
    'exercise': exercise,
    'weight': weight
})

# 기본 상관관계 계산
corr_matrix = df.corr()
print("기본 상관관계 행렬:")
print(corr_matrix)

# 편 상관관계 계산
# 나이를 통제했을 때 운동량과 체중의 상관관계
pcorr_ex_w_age = pg.partial_corr(data=df, x='exercise', y='weight', covar='age')
# 운동량을 통제했을 때 나이와 체중의 상관관계
pcorr_age_w_ex = pg.partial_corr(data=df, x='age', y='weight', covar='exercise')

print("\n편 상관관계 결과:")
print(f"나이를 통제했을 때 운동량과 체중의 편 상관계수: {pcorr_ex_w_age['r'].values[0]:.4f}")
print(f"운동량을 통제했을 때 나이와 체중의 편 상관계수: {pcorr_age_w_ex['r'].values[0]:.4f}")

# 각 상관관계 비교
r_age_weight = corr_matrix.loc['age', 'weight']
r_exercise_weight = corr_matrix.loc['exercise', 'weight']

print("\n상관계수 비교:")
print(f"나이와 체중의 기본 상관계수: {r_age_weight:.4f}")
print(f"운동량을 통제한 후 나이와 체중의 편 상관계수: {pcorr_age_w_ex['r'].values[0]:.4f}")
print(f"운동량과 체중의 기본 상관계수: {r_exercise_weight:.4f}")
print(f"나이를 통제한 후 운동량과 체중의 편 상관계수: {pcorr_ex_w_age['r'].values[0]:.4f}")

# 시각화
plt.figure(figsize=(15, 5))

# 1. 상관관계 히트맵
plt.subplot(1, 3, 1)
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', vmin=-1, vmax=1)
plt.title('기본 상관관계 행렬')

# 2. 나이와 체중의 산점도
plt.subplot(1, 3, 2)
plt.scatter(df['age'], df['weight'])
plt.xlabel('나이')
plt.ylabel('체중')
plt.title(f'나이와 체중\n상관계수: {r_age_weight:.4f}, 편 상관계수: {pcorr_age_w_ex["r"].values[0]:.4f}')
plt.grid(True, alpha=0.3)

# 3. 운동량과 체중의 산점도
plt.subplot(1, 3, 3)
plt.scatter(df['exercise'], df['weight'])
plt.xlabel('운동량')
plt.ylabel('체중')
plt.title(f'운동량과 체중\n상관계수: {r_exercise_weight:.4f}, 편 상관계수: {pcorr_ex_w_age["r"].values[0]:.4f}')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 소득, 교육수준, 건강 상태 간의 직접적인 관계 분석 시
- 여러 예측 변수가 있을 때 다중공선성 문제 진단 시
- 매개 변수의 영향을 제거하여 직접적인 인과 관계 파악 시
- 약물 효과를 평가할 때 환자의 나이, 성별 등의 혼동 변수 통제 시
- 경제 지표 간의 순수한 관계 파악을 위해 다른 변수들의 영향 제거 시
---
# 회귀분석 (Regression Analysis)

## 1. 회귀모형의 기본가정 (Basic Assumptions of Regression Models)

**정의**: 회귀모형의 기본가정은 회귀분석이 올바르게 적용되고 해석되기 위해 충족해야 하는 통계적 전제 조건들입니다. 이러한 가정들은 추정치의 신뢰성과 유효성을 보장하기 위해 중요합니다.

**수식**: 일반적인 선형 회귀 모형은 다음과 같이 표현됩니다. $Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + ... + \beta_pX_p + \epsilon$

여기서 회귀모형의 기본가정은 주로 오차항 $\epsilon$에 관한 것입니다.

**특징**:

- 선형성(Linearity): 독립변수와 종속변수 간의 관계가 선형적이어야 합니다.
- 독립성(Independence): 관측치들이 서로 독립적이어야 합니다(자기상관이 없어야 함).
- 등분산성(Homoscedasticity): 오차항의 분산이 모든 독립변수 값에서 일정해야 합니다.
- 정규성(Normality): 오차항이 정규분포를 따라야 합니다.
- 다중공선성 부재(No Multicollinearity): 독립변수들 간에 높은 상관관계가 없어야 합니다.
- 완전한 다중공선성이 존재하지 않음(Full Rank): 독립변수들이 선형적으로 독립적이어야 합니다.
- 오차항의 평균이 0: $E(\epsilon) = 0$
- 이상치의 영향이 제한적이어야 합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
from statsmodels.stats.outliers_influence import variance_inflation_factor
from scipy import stats

# 데이터 생성
np.random.seed(42)
n = 100

# 독립변수
X1 = np.random.normal(0, 1, n)
X2 = np.random.normal(0, 1, n)
X3 = 0.8 * X1 + 0.2 * np.random.normal(0, 1, n)  # X1과 높은 상관관계를 가짐

# 종속변수 (선형 관계에 오차 추가)
Y = 2 + 3 * X1 - 1.5 * X2 + 0.5 * X3 + np.random.normal(0, 1, n)

# 데이터프레임 생성
df = pd.DataFrame({
    'X1': X1,
    'X2': X2, 
    'X3': X3,
    'Y': Y
})

# 회귀 모델 적합
X = sm.add_constant(df[['X1', 'X2', 'X3']])
model = sm.OLS(df['Y'], X).fit()
print(model.summary())

# 가정 검정
residuals = model.resid
fitted_values = model.fittedvalues

# 1. 선형성 검정: 독립변수와 잔차 간의 산점도
plt.figure(figsize=(15, 10))
plt.subplot(2, 2, 1)
plt.scatter(fitted_values, residuals)
plt.axhline(y=0, color='r', linestyle='-')
plt.title('잔차 vs 예측값 (선형성 검정)')
plt.xlabel('예측값')
plt.ylabel('잔차')
plt.grid(True, alpha=0.3)

# 2. 정규성 검정: QQ 플롯
plt.subplot(2, 2, 2)
sm.qqplot(residuals, line='45', fit=True, ax=plt.gca())
plt.title('잔차의 QQ 플롯 (정규성 검정)')
plt.grid(True, alpha=0.3)

# 3. 등분산성 검정: 잔차의 분포
plt.subplot(2, 2, 3)
plt.scatter(fitted_values, np.abs(residuals))
plt.title('절대 잔차 vs 예측값 (등분산성 검정)')
plt.xlabel('예측값')
plt.ylabel('절대 잔차')
plt.grid(True, alpha=0.3)

# 4. 다중공선성 검정: VIF 계산
plt.subplot(2, 2, 4)
vif_data = pd.DataFrame()
vif_data["변수"] = X.columns
vif_data["VIF"] = [variance_inflation_factor(X.values, i) for i in range(X.shape[1])]
plt.bar(vif_data["변수"], vif_data["VIF"])
plt.axhline(y=5, color='r', linestyle='--', label='VIF=5')
plt.axhline(y=10, color='r', linestyle='-', label='VIF=10')
plt.title('다중공선성 검정 (VIF)')
plt.ylabel('VIF')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 가정 검정 결과 출력
print("\n회귀모형 가정 검정 결과:")

# 1. 선형성: Ramsey RESET 검정
reset_test = sm.stats.diagnostic.linear_reset(model, power=3)
print(f"선형성 (RESET 검정): F={reset_test[0]:.4f}, p={reset_test[1]:.4f}, {'가정 위배' if reset_test[1] < 0.05 else '가정 충족'}")

# 2. 정규성: Jarque-Bera 검정
jb_test = stats.jarque_bera(residuals)
print(f"정규성 (Jarque-Bera 검정): JB={jb_test[0]:.4f}, p={jb_test[1]:.4f}, {'가정 위배' if jb_test[1] < 0.05 else '가정 충족'}")

# 3. 등분산성: Breusch-Pagan 검정
bp_test = sm.stats.diagnostic.het_breuschpagan(residuals, X)
print(f"등분산성 (Breusch-Pagan 검정): LM={bp_test[0]:.4f}, p={bp_test[1]:.4f}, {'가정 위배' if bp_test[1] < 0.05 else '가정 충족'}")

# 4. 다중공선성: VIF 값
print("\n다중공선성 (VIF):")
print(vif_data)
```

**개념의 활용**:

- 회귀 모델을 적합하기 전에 데이터 특성을 확인할 때
- 가정 위반 시 데이터 변환이나 대안적 모델링 방법 선택 시
- 모델 진단과 잔차 분석을 통한 모델 개선 시
- 예측 정확도와 신뢰성을 향상시키기 위한 모델 검증 과정에서
- 통계적으로 올바른 추론을 위한 기반 확인 시

## 2. 단순 선형 회귀 (Simple Linear Regression)

**정의**: 단순 선형 회귀는 하나의 독립변수(X)와 하나의 종속변수(Y) 간의 선형 관계를 모델링하는 방법입니다. 직선 형태의 수학적 함수로 두 변수 사이의 관계를 추정합니다.

**수식**: $Y = \beta_0 + \beta_1X + \epsilon$ 여기서 $\beta_0$는 절편, $\beta_1$은 기울기, $\epsilon$은 오차항입니다.

계수 추정식(최소제곱법): $\beta_1 = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^{n}(x_i - \bar{x})^2}$ $\beta_0 = \bar{y} - \beta_1\bar{x}$

**특징**:

- 가장 단순한 형태의 회귀 모델입니다.
- X와 Y 사이의 선형 관계만 포착할 수 있습니다.
- 결정계수(R²)는 모델이 설명하는의 분산의 비율을 나타냅니다.
- 회귀선은 잔차 제곱합을 최소화하는 방식으로 결정됩니다(최소제곱법).
- 회귀 계수의 유의성 검정을 통해 관계의 통계적 유의미성을 평가합니다.
- 직관적이고 해석하기 쉬우며, 계산이 간단합니다.
- 회귀모형의 기본가정이 모두 적용됩니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import statsmodels.api as sm
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score, mean_squared_error

# 데이터 생성
np.random.seed(42)
n = 50
X = np.random.uniform(1, 10, n)
Y = 2 + 1.5 * X + np.random.normal(0, 2, n)  # β₀=2, β₁=1.5

# 데이터프레임 생성
df = pd.DataFrame({'X': X, 'Y': Y})

# statsmodels를 이용한 회귀 분석
X_sm = sm.add_constant(X)  # 절편 추가
model = sm.OLS(Y, X_sm).fit()
print(model.summary())

# 회귀 계수
intercept = model.params[0]
slope = model.params[1]
print(f"회귀식: Y = {intercept:.4f} + {slope:.4f}X")

# 결정계수
r_squared = model.rsquared
adj_r_squared = model.rsquared_adj
print(f"결정계수(R²): {r_squared:.4f}")
print(f"조정된 결정계수(Adjusted R²): {adj_r_squared:.4f}")

# 회귀 계수의 신뢰구간
print("\n회귀 계수의 95% 신뢰구간:")
print(model.conf_int())

# 예측값 계산
predictions = model.predict(X_sm)

# 평균 제곱 오차(MSE)
mse = mean_squared_error(Y, predictions)
print(f"평균 제곱 오차(MSE): {mse:.4f}")

# 시각화
plt.figure(figsize=(12, 8))

# 1. 산점도와 회귀선
plt.subplot(2, 2, 1)
plt.scatter(X, Y, alpha=0.7)
plt.plot(X, predictions, 'r-', linewidth=2)
plt.title('단순 선형 회귀: 산점도와 회귀선')
plt.xlabel('X')
plt.ylabel('Y')
plt.grid(True, alpha=0.3)
plt.text(2, max(Y)-2, f'Y = {intercept:.2f} + {slope:.2f}X\nR² = {r_squared:.4f}', fontsize=10)

# 2. 잔차 플롯
plt.subplot(2, 2, 2)
residuals = Y - predictions
plt.scatter(X, residuals, alpha=0.7)
plt.axhline(y=0, color='r', linestyle='-')
plt.title('잔차 플롯')
plt.xlabel('X')
plt.ylabel('잔차')
plt.grid(True, alpha=0.3)

# 3. 잔차의 히스토그램
plt.subplot(2, 2, 3)
plt.hist(residuals, bins=10, alpha=0.7, edgecolor='black')
plt.title('잔차의 히스토그램')
plt.xlabel('잔차')
plt.ylabel('빈도')
plt.grid(True, alpha=0.3)

# 4. 예측값 vs 실제값
plt.subplot(2, 2, 4)
plt.scatter(predictions, Y, alpha=0.7)
plt.plot([min(predictions), max(predictions)], [min(predictions), max(predictions)], 'r--')
plt.title('예측값 vs 실제값')
plt.xlabel('예측값')
plt.ylabel('실제값')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 광고 지출과 매출 간의 관계 분석 시
- 학습 시간과 시험 점수 간의 관계 예측 시
- 나이에 따른 혈압 변화 모델링 시
- 온도와 에너지 소비 간의 관계 파악 시
- 간단한 예측 모델을 빠르게 구축해야 할 때

## 3. 다중 회귀 (Multiple Regression)

**정의**: 다중 회귀는 둘 이상의 독립변수와 하나의 종속변수 간의 관계를 모델링하는 방법입니다. 여러 예측 변수의 조합을 통해 종속변수를 예측하고 각 독립변수의 상대적 영향력을 평가합니다.

**수식**: $Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + ... + \beta_pX_p + \epsilon$ 여기서 $\beta_0$는 절편, $\beta_1, \beta_2, ..., \beta_p$는 각 독립변수의 회귀 계수, $\epsilon$은 오차항입니다.

행렬 형태로는: $\mathbf{Y} = \mathbf{X\beta} + \mathbf{\epsilon}$ 계수 추정(최소제곱법): $\mathbf{\hat{\beta}} = (\mathbf{X'X})^{-1}\mathbf{X'Y}$

**특징**:

- 여러 독립변수의 영향을 동시에 고려할 수 있습니다.
- 각 독립변수의 영향을 다른 변수들의 효과를 통제한 상태에서 평가합니다.
- 상호작용과 다중공선성 문제가 발생할 수 있습니다.
- 독립변수의 수가 증가할수록 과적합(overfitting)의 위험이 있습니다.
- 표준화 계수를 통해 변수 간 영향력을 비교할 수 있습니다.
- 변수 선택 방법(전진, 후진, 단계적 선택)을 통해 모델을 최적화할 수 있습니다.
- 결정계수(R²)는 독립변수 수가 증가할수록 자동으로 증가하므로, 조정된 R²를 함께 고려해야 합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
from statsmodels.stats.outliers_influence import variance_inflation_factor
from sklearn.preprocessing import StandardScaler

# 데이터 생성
np.random.seed(42)
n = 100

# 독립변수
X1 = np.random.normal(0, 1, n)  # 소득
X2 = np.random.normal(0, 1, n)  # 교육 수준
X3 = 0.3 * X1 + 0.5 * X2 + np.random.normal(0, 0.5, n)  # 직업 경험 (소득과 교육 수준과 상관관계 있음)

# 종속변수: 주택 가격
Y = 3 + 2 * X1 + 1.5 * X2 + 0.8 * X3 + np.random.normal(0, 1, n)

# 데이터프레임 생성
df = pd.DataFrame({
    '소득': X1,
    '교육수준': X2,
    '직업경험': X3,
    '주택가격': Y
})

# 상관관계 확인
correlation = df.corr()
print("변수 간 상관관계:")
print(correlation)

# 다중공선성 확인
X = sm.add_constant(df[['소득', '교육수준', '직업경험']])
vif_data = pd.DataFrame()
vif_data["변수"] = X.columns
vif_data["VIF"] = [variance_inflation_factor(X.values, i) for i in range(X.shape[1])]
print("\n다중공선성 검정(VIF):")
print(vif_data)

# 다중 회귀 모델 적합
model = sm.OLS(df['주택가격'], X).fit()
print("\n회귀 분석 결과:")
print(model.summary())

# 표준화 계수 계산
scaler = StandardScaler()
X_scaled = scaler.fit_transform(df[['소득', '교육수준', '직업경험']])
X_scaled = sm.add_constant(X_scaled)
model_scaled = sm.OLS(df['주택가격'], X_scaled).fit()

print("\n표준화 계수:")
print(model_scaled.params[1:])  # 절편 제외

# 예측값 및 잔차
predictions = model.predict(X)
residuals = df['주택가격'] - predictions

# 시각화
plt.figure(figsize=(15, 10))

# 1. 상관관계 히트맵
plt.subplot(2, 2, 1)
sns.heatmap(correlation, annot=True, cmap='coolwarm', vmin=-1, vmax=1)
plt.title('변수 간 상관관계')

# 2. 실제값 vs 예측값
plt.subplot(2, 2, 2)
plt.scatter(df['주택가격'], predictions)
plt.plot([df['주택가격'].min(), df['주택가격'].max()], 
         [df['주택가격'].min(), df['주택가격'].max()], 'r--')
plt.xlabel('실제 주택가격')
plt.ylabel('예측 주택가격')
plt.title(f'실제값 vs 예측값 (R² = {model.rsquared:.4f})')
plt.grid(True, alpha=0.3)

# 3. 잔차 플롯
plt.subplot(2, 2, 3)
plt.scatter(predictions, residuals)
plt.axhline(y=0, color='r', linestyle='-')
plt.xlabel('예측값')
plt.ylabel('잔차')
plt.title('잔차 플롯')
plt.grid(True, alpha=0.3)

# 4. 표준화 계수 비교
plt.subplot(2, 2, 4)
plt.bar(['소득', '교육수준', '직업경험'], model_scaled.params[1:])
plt.axhline(y=0, color='r', linestyle='-')
plt.ylabel('표준화 계수')
plt.title('각 변수의 상대적 중요도')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 변수 중요도 분석
importance = pd.DataFrame({
    '변수': ['소득', '교육수준', '직업경험'],
    '회귀계수': model.params[1:],
    '표준오차': model.bse[1:],
    't값': model.tvalues[1:],
    'p값': model.pvalues[1:],
    '표준화계수': model_scaled.params[1:]
})
print("\n변수 중요도 분석:")
print(importance)
```

**개념의 활용**:

- 주택 가격 예측 시 위치, 면적, 건축 연도 등 여러 요인 분석
- 기업 수익에 영향을 미치는 다양한 경제 지표와 내부 요인 모델링
- 학생 성적에 영향을 미치는 학습 시간, 출석률, 사전 지식 등의 요인 평가
- 건강 지표에 영향을 주는 여러 생활 습관 및 환경 요인 분석
- 제품 판매량에 영향을 미치는 가격, 프로모션, 경쟁사 활동 등 다양한 요인 모델링

## 4. 더미 변수 회귀분석 (Dummy Variable Regression)

**정의**: 더미 변수 회귀분석은 범주형 독립변수를 회귀 모델에 포함시키기 위해 이진(0 또는 1) 변수로 변환하여 분석하는 방법입니다. 이를 통해 범주 간의 효과 차이를 정량적으로 평가할 수 있습니다.

**수식**: k개의 범주를 가진 범주형 변수의 경우, (k-1)개의 더미 변수를 생성합니다. $Y = \beta_0 + \beta_1X_1 + ... + \beta_pX_p + \beta_{p+1}D_1 + ... + \beta_{p+k-1}D_{k-1} + \epsilon$

여기서 $D_1, D_2, ..., D_{k-1}$은 더미 변수들이며, 기준 범주(reference category)는 모든 더미 변수가 0인 경우입니다.

**특징**:

- 범주형 변수를 회귀 분석에 포함할 수 있게 해줍니다.
- 더미 변수의 계수는 기준 범주와 비교한 효과를 나타냅니다.
- 완전 다중공선성을 피하기 위해 k개 범주에 대해 (k-1)개의 더미 변수만 사용합니다.
- 기준 범주의 선택은 결과 해석에 영향을 줄 수 있지만, 전체 모델의 적합도는 변하지 않습니다.
- 순서가 없는 명목형 변수와 순서가 있는 서열형 변수 모두에 적용 가능합니다.
- 연속형 변수와 범주형 변수를 함께 모델링할 수 있습니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
import statsmodels.formula.api as smf
from sklearn.preprocessing import OneHotEncoder

# 데이터 생성: 직업 유형(IT, 금융, 의료)에 따른 급여 데이터
np.random.seed(42)
n = 300

# 직업 유형 생성
job_types = np.random.choice(['IT', '금융', '의료'], size=n)

# 경력 연수 생성
experience = np.random.uniform(1, 20, n)

# 급여 생성 (직업 유형과 경력에 따라 다름)
salary = np.zeros(n)
for i in range(n):
    if job_types[i] == 'IT':
        base = 5000  # IT 기본 급여
        exp_effect = 300  # 경력 1년당 증가액
    elif job_types[i] == '금융':
        base = 5500  # 금융 기본 급여
        exp_effect = 350  # 경력 1년당 증가액
    else:  # 의료
        base = 6000  # 의료 기본 급여
        exp_effect = 400  # 경력 1년당 증가액
    
    # 급여 = 기본급 + 경력효과 + 랜덤오차
    salary[i] = base + exp_effect * experience[i] + np.random.normal(0, 1000)

# 데이터프레임 생성
df = pd.DataFrame({
    '직업': job_types,
    '경력': experience,
    '급여': salary
})

print("데이터 샘플:")
print(df.head())

# 직업별 평균 급여
job_salary = df.groupby('직업')['급여'].agg(['mean', 'std'])
print("\n직업별 평균 급여:")
print(job_salary)

# 더미 변수 생성 방법 1: pandas get_dummies
df_dummies = pd.get_dummies(df, columns=['직업'], drop_first=True)
print("\n더미 변수 생성 결과:")
print(df_dummies.head())

# 더미 변수를 사용한 회귀 분석 방법 1: 직접 더미 변수 사용
X = sm.add_constant(df_dummies[['경력', '직업_의료', '직업_금융']])
model1 = sm.OLS(df_dummies['급여'], X).fit()
print("\n더미 변수 회귀 분석 결과:")
print(model1.summary())

# 방법 2: 공식(formula) 인터페이스 사용 (자동으로 더미 변수 생성)
model2 = smf.ols('급여 ~ 경력 + C(직업)', data=df).fit()
print("\n공식 인터페이스 회귀 분석 결과:")
print(model2.summary())

# 다른 기준 범주 선택
model3 = smf.ols('급여 ~ 경력 + C(직업, Treatment("의료"))', data=df).fit()
print("\n의료를 기준 범주로 한 회귀 분석 결과:")
print(model3.summary())

# 시각화
plt.figure(figsize=(15, 10))

# 1. 직업별 급여 분포
plt.subplot(2, 2, 1)
sns.boxplot(x='직업', y='급여', data=df)
plt.title('직업별 급여 분포')
plt.grid(True, alpha=0.3)

# 2. 경력과 급여의 관계 (직업별 색상 구분)
plt.subplot(2, 2, 2)
colors = {'IT': 'blue', '금융': 'green', '의료': 'red'}
for job in df['직업'].unique():
    subset = df[df['직업'] == job]
    plt.scatter(subset['경력'], subset['급여'], c=colors[job], alpha=0.6, label=job)
plt.xlabel('경력')
plt.ylabel('급여')
plt.title('경력과 급여의 관계 (직업별)')
plt.legend()
plt.grid(True, alpha=0.3)

# 3. 직업별 회귀선
plt.subplot(2, 2, 3)
sns.lmplot(x='경력', y='급여', hue='직업', data=df, height=5, aspect=1.5)
plt.title('직업별 경력-급여 회귀선')

# 4. 모델 예측값 vs 실제값
plt.subplot(2, 2, 4)
predictions = model2.predict(df)
plt.scatter(predictions, df['급여'], alpha=0.6)
plt.plot([df['급여'].min(), df['급여'].max()], [df['급여'].min(), df['급여'].max()], 'r--')
plt.xlabel('예측 급여')
plt.ylabel('실제 급여')
plt.title(f'예측값 vs 실제값 (R² = {model2.rsquared:.4f})')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 결과 해석
print("\n회귀 분석 결과 해석:")
print(f"1. 경력 효과: 경력이 1년 증가할 때마다 급여가 평균 {model2.params['경력']:.2f}원 증가")

# 기준 범주는 IT
it_intercept = model2.params['Intercept']
print(f"2. IT 직종(기준): 경력 0년일 때 예상 급여는 {it_intercept:.2f}원")

# 다른 직종과 IT의 차이
finance_diff = model2.params['C(직업)[금융]']
print(f"3. 금융 직종: IT보다 {finance_diff:.2f}원 더 높음 (p={model2.pvalues['C(직업)[금융]']:.4f})")

medical_diff = model2.params['C(직업)[의료]']
print(f"4. 의료 직종: IT보다 {medical_diff:.2f}원 더 높음 (p={model2.pvalues['C(직업)[의료]']:.4f})")

# 예시 예측
new_data = pd.DataFrame({
    '경력': [10, 10, 10],
    '직업': ['IT', '금융', '의료']
})
predictions = model2.predict(new_data)
result_df = pd.DataFrame({
    '직업': new_data['직업'],
    '경력': new_data['경력'],
    '예측 급여': predictions
})
print("\n경력 10년일 때 직업별 예측 급여:")
print(result_df)
```

**개념의 활용**:

- 성별, 학력 등 범주형 변수가 임금에 미치는 영향 분석 시
- 지역별 부동산 가격 차이 평가 시
- 다양한 마케팅 전략(A/B/C)의 효과 비교 시
- 여러 치료법의 효과 차이를 정량적으로 분석할 때
- 업종별 투자 수익률 차이 평가 시

## 5. 교호작용 유무에 따른 회귀모형 (Regression Models with Interaction Terms)

**정의**: 교호작용(상호작용)이 있는 회귀모형은 두 개 이상의 독립변수 간 상호작용 효과를 포함시켜, 한 변수의 효과가 다른 변수의 수준에 따라 달라지는 현상을 모델링하는 방법입니다.

**수식**: 두 변수 X₁과 X₂ 간의 교호작용이 있는 회귀모형: $Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + \beta_3(X_1 \times X_2) + \epsilon$

여기서 $\beta_3$는 교호작용 효과를 나타내는 계수입니다.

**특징**:

- 교호작용 항의 포함 여부에 따라 회귀 분석 결과와 해석이 크게 달라질 수 있습니다.
- 교호작용이 있으면 한 변수의 효과가 다른 변수의 값에 따라 달라집니다.
- 교호작용 항이 통계적으로 유의하면, 주효과(main effects)만으로는 관계를 완전히 설명할 수 없습니다.
- 교호작용을 포함한 모델은 더 복잡하지만 현실 세계의 복잡한 관계를 더 정확히 반영할 수 있습니다.
- 다중공선성 문제가 발생할 수 있으므로, 독립변수를 중심화(centering)하는 것이 도움이 될 수 있습니다.
- 교호작용은 연속형 변수 간, 범주형 변수 간, 또는 연속형과 범주형 변수 간에 모두 가능합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
import statsmodels.formula.api as smf
from scipy import stats

# 데이터 생성: 비료 사용량(X1)과 강수량(X2)이 작물 수확량(Y)에 미치는 영향
np.random.seed(42)
n = 100

# 독립변수
X1 = np.random.uniform(10, 50, n)  # 비료 사용량
X2 = np.random.uniform(20, 100, n)  # 강수량

# 교호작용이 있는 경우의 종속변수: 강수량이 많을수록 비료 효과가 증가
# Y = β₀ + β₁X₁ + β₂X₂ + β₃(X₁×X₂) + ε
beta0 = 10    # 절편
beta1 = 0.5   # 비료 주효과
beta2 = 0.3   # 강수량 주효과
beta3 = 0.02  # 교호작용 효과
epsilon = np.random.normal(0, 5, n)  # 오차항

Y = beta0 + beta1 * X1 + beta2 * X2 + beta3 * (X1 * X2) + epsilon

# 데이터프레임 생성
df = pd.DataFrame({
    '비료': X1,
    '강수량': X2,
    '수확량': Y
})

# 중심화된 변수 생성 (다중공선성 감소를 위해)
df['비료_중심화'] = df['비료'] - df['비료'].mean()
df['강수량_중심화'] = df['강수량'] - df['강수량'].mean()
df['교호작용'] = df['비료_중심화'] * df['강수량_중심화']

# 1. 교호작용이 없는 모델
model1 = smf.ols('수확량 ~ 비료 + 강수량', data=df).fit()
print("1. 교호작용이 없는 모델:")
print(model1.summary())

# 2. 교호작용이 있는 모델
model2 = smf.ols('수확량 ~ 비료 + 강수량 + 비료:강수량', data=df).fit()
print("\n2. 교호작용이 있는 모델:")
print(model2.summary())

# 3. 중심화 변수를 사용한 교호작용 모델
model3 = smf.ols('수확량 ~ 비료_중심화 + 강수량_중심화 + 교호작용', data=df).fit()
print("\n3. 중심화 변수를 사용한 교호작용 모델:")
print(model3.summary())

# 모델 비교: 추가된 교호작용 항의 유의성 검정
from statsmodels.stats.anova import anova_lm
anova_table = anova_lm(model1, model2)
print("\n모델 비교 (ANOVA):")
print(anova_table)

# 시각화
plt.figure(figsize=(15, 12))

# 1. 교호작용 시각화: 등고선 플롯
plt.subplot(2, 2, 1)
pivot_table = df.pivot_table(index=pd.cut(df['비료'], 10), 
                             columns=pd.cut(df['강수량'], 10), 
                             values='수확량', 
                             aggfunc='mean')
sns.heatmap(pivot_table, cmap='viridis', annot=False)
plt.title('비료와 강수량에 따른 수확량 (등고선 플롯)')
plt.xlabel('강수량')
plt.ylabel('비료 사용량')

# 2. 3D 표면 플롯
from mpl_toolkits.mplot3d import Axes3D

plt.subplot(2, 2, 2, projection='3d')
ax = plt.gca()
fertilizer_range = np.linspace(df['비료'].min(), df['비료'].max(), 30)
rainfall_range = np.linspace(df['강수량'].min(), df['강수량'].max(), 30)
fertilizer_grid, rainfall_grid = np.meshgrid(fertilizer_range, rainfall_range)

# 모델2 예측 함수
def predict_yield(fertilizer, rainfall):
    X_new = pd.DataFrame({
        '비료': fertilizer.flatten(),
        '강수량': rainfall.flatten()
    })
    return model2.predict(X_new).reshape(fertilizer.shape)

yield_grid = predict_yield(fertilizer_grid, rainfall_grid)

surf = ax.plot_surface(fertilizer_grid, rainfall_grid, yield_grid, cmap='viridis', alpha=0.8)
ax.set_xlabel('비료 사용량')
ax.set_ylabel('강수량')
ax.set_zlabel('수확량')
ax.set_title('3D 표면 플롯: 교호작용 효과')
plt.colorbar(surf, ax=ax, shrink=0.5, aspect=5)

# 3. 강수량별 비료 효과 (교호작용 시각화)
plt.subplot(2, 2, 3)
# 강수량 수준 선택
low_rain = df['강수량'].quantile(0.25)
med_rain = df['강수량'].quantile(0.5)
high_rain = df['강수량'].quantile(0.75)

# 새 데이터 생성
fertilizer_range = np.linspace(df['비료'].min(), df['비료'].max(), 100)
new_data_low = pd.DataFrame({
    '비료': fertilizer_range,
    '강수량': np.full_like(fertilizer_range, low_rain)
})
new_data_med = pd.DataFrame({
    '비료': fertilizer_range,
    '강수량': np.full_like(fertilizer_range, med_rain)
})
new_data_high = pd.DataFrame({
    '비료': fertilizer_range,
    '강수량': np.full_like(fertilizer_range, high_rain)
})

# 예측
pred_low = model2.predict(new_data_low)
pred_med = model2.predict(new_data_med)
pred_high = model2.predict(new_data_high)

# 그래프 그리기
plt.plot(fertilizer_range, pred_low, 'b-', label=f'낮은 강수량 ({low_rain:.1f})')
plt.plot(fertilizer_range, pred_med, 'g-', label=f'중간 강수량 ({med_rain:.1f})')
plt.plot(fertilizer_range, pred_high, 'r-', label=f'높은 강수량 ({high_rain:.1f})')
plt.scatter(df['비료'], df['수확량'], alpha=0.3, color='gray')
plt.xlabel('비료 사용량')
plt.ylabel('예측 수확량')
plt.title('강수량 수준별 비료 효과 (교호작용)')
plt.legend()
plt.grid(True, alpha=0.3)

# 4. 비료별 강수량 효과 (교호작용 시각화)
plt.subplot(2, 2, 4)
# 비료 수준 선택
low_fert = df['비료'].quantile(0.25)
med_fert = df['비료'].quantile(0.5)
high_fert = df['비료'].quantile(0.75)

# 새 데이터 생성
rainfall_range = np.linspace(df['강수량'].min(), df['강수량'].max(), 100)
new_data_low = pd.DataFrame({
    '비료': np.full_like(rainfall_range, low_fert),
    '강수량': rainfall_range
})
new_data_med = pd.DataFrame({
    '비료': np.full_like(rainfall_range, med_fert),
    '강수량': rainfall_range
})
new_data_high = pd.DataFrame({
    '비료': np.full_like(rainfall_range, high_fert),
    '강수량': rainfall_range
})

# 예측
pred_low = model2.predict(new_data_low)
pred_med = model2.predict(new_data_med)
pred_high = model2.predict(new_data_high)

# 그래프 그리기
plt.plot(rainfall_range, pred_low, 'b-', label=f'적은 비료 ({low_fert:.1f})')
plt.plot(rainfall_range, pred_med, 'g-', label=f'중간 비료 ({med_fert:.1f})')
plt.plot(rainfall_range, pred_high, 'r-', label=f'많은 비료 ({high_fert:.1f})')
plt.scatter(df['강수량'], df['수확량'], alpha=0.3, color='gray')
plt.xlabel('강수량')
plt.ylabel('예측 수확량')
plt.title('비료 수준별 강수량 효과 (교호작용)')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 결과 해석
print("\n결과 해석:")
print("1. 교호작용이 없는 모델:")
print(f"   - 비료 효과: 1단위 증가 시 수확량 {model1.params['비료']:.4f} 증가")
print(f"   - 강수량 효과: 1단위 증가 시 수확량 {model1.params['강수량']:.4f} 증가")
print(f"   - 모델 설명력(R²): {model1.rsquared:.4f}")

print("\n2. 교호작용이 있는 모델:")
print(f"   - 비료 주효과: {model2.params['비료']:.4f}")
print(f"   - 강수량 주효과: {model2.params['강수량']:.4f}")
print(f"   - 교호작용 효과: {model2.params['비료:강수량']:.4f}")
print(f"   - 모델 설명력(R²): {model2.rsquared:.4f}")

print("\n교호작용 해석:")
fert_effect_low = model2.params['비료'] + model2.params['비료:강수량'] * low_rain
fert_effect_high = model2.params['비료'] + model2.params['비료:강수량'] * high_rain
print(f"   - 낮은 강수량({low_rain:.1f})에서 비료 1단위 효과: {fert_effect_low:.4f}")
print(f"   - 높은 강수량({high_rain:.1f})에서 비료 1단위 효과: {fert_effect_high:.4f}")
```

**개념의 활용**:

- 비료와 물의 상호작용 효과가 작물 수확량에 미치는 영향 분석 시
- 약물 복용량과 환자의 나이가 치료 효과에 미치는 상호작용 연구 시
- 광고 지출과 제품 가격이 판매량에 미치는 조합 효과 모델링 시
- 교육 방법과 학생의 사전 지식 수준 간의 상호작용이 학습 효과에 미치는 영향 분석 시
- 운동 강도와 식이요법 간의 상호작용이 체중 감소에 미치는 효과 평가 시
---
# 시계열 (Time Series)

## 1. 정상성 (Stationarity)

**정의**: 정상성은 시계열 데이터의 통계적 특성(평균, 분산, 자기 공분산)이 시간에 따라 변하지 않는 특성을 의미합니다. 정상 시계열은 시간에 관계없이 일정한 평균, 분산을 가지며, 시차에만 의존하는 자기 공분산 구조를 갖습니다.

**수식**: 시계열 {Yₜ}이 정상 시계열이라면:

- 평균: E(Yₜ) = μ (상수, 시간에 불변)
- 분산: Var(Yₜ) = σ² (상수, 시간에 불변)
- 자기 공분산: Cov(Yₜ, Yₜ₊ₖ) = γₖ (시차 k에만 의존)

**특징**:

- 정상 시계열은 시간에 따라 평균으로 회귀하는 경향을 보입니다.
- 추세(trend), 계절성(seasonality), 분산 변화가 있으면 비정상 시계열입니다.
- 대부분의 시계열 모델링 기법은 정상 시계열을 가정합니다.
- 비정상 시계열은 차분(differencing), 변환(transformation) 등을 통해 정상화할 수 있습니다.
- 정상성 검정에는 ADF(Augmented Dickey-Fuller), KPSS 검정 등이 사용됩니다.
- 약 정상성(weak stationarity)과 강 정상성(strict stationarity)으로 구분됩니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.stattools import adfuller
from statsmodels.tsa.seasonal import seasonal_decompose
import statsmodels.api as sm

# 다양한 시계열 데이터 생성
np.random.seed(42)
n = 200

# 1. 정상 시계열 (평균과 분산이 일정)
stationary = np.random.normal(0, 1, n)

# 2. 트렌드가 있는 비정상 시계열
trend = np.linspace(0, 5, n) + np.random.normal(0, 1, n)

# 3. 계절성이 있는 비정상 시계열
seasonal = np.sin(np.linspace(0, 4*np.pi, n)) + np.random.normal(0, 0.5, n)

# 4. 분산이 변하는 비정상 시계열
heteroscedastic = np.random.normal(0, np.linspace(0.5, 3, n), n)

# 5. 트렌드와 계절성이 모두 있는 비정상 시계열
combined = trend + 2*seasonal

# 시계열 데이터프레임 생성
dates = pd.date_range(start='2020-01-01', periods=n, freq='D')
df = pd.DataFrame({
    '정상': stationary,
    '트렌드': trend,
    '계절성': seasonal,
    '이분산': heteroscedastic,
    '복합': combined
}, index=dates)

# 정상성 검정 함수 (ADF 검정)
def adf_test(series, title=''):
    result = adfuller(series.dropna())
    print(f"ADF 검정 결과 - {title}")
    print(f"ADF 통계량: {result[0]:.4f}")
    print(f"p-value: {result[1]:.4f}")
    for key, value in result[4].items():
        print(f"임계값 ({key}): {value:.4f}")
    if result[1] <= 0.05:
        print("결론: 정상 시계열 (귀무가설 기각)\n")
    else:
        print("결론: 비정상 시계열 (귀무가설 기각 실패)\n")

# 각 시계열에 대한 정상성 검정
for column in df.columns:
    adf_test(df[column], column)

# 트렌드 시계열의 정상화 (차분)
trend_diff = df['트렌드'].diff().dropna()
adf_test(trend_diff, '트렌드 시계열 1차 차분')

# 시각화
plt.figure(figsize=(15, 12))

# 원본 시계열 시각화
for i, column in enumerate(df.columns):
    plt.subplot(5, 2, 2*i+1)
    plt.plot(df[column])
    plt.title(f'{column} 시계열')
    plt.grid(True, alpha=0.3)
    
    # ACF 플롯
    plt.subplot(5, 2, 2*i+2)
    sm.graphics.tsa.plot_acf(df[column].values.squeeze(), lags=20, ax=plt.gca())
    plt.title(f'{column} ACF 플롯')

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 시계열 모델링 전 데이터 특성 파악 및 전처리 단계에서 정상성 확인
- 금융 데이터의 변동성 분석을 위한 정상성 검정
- 계절성과 트렌드 제거를 통한 시계열 모델 성능 개선
- 비정상 시계열의 적절한 변환 방법 선택(로그 변환, 차분 등)
- 경제 지표나 주가 예측 모델 구축 시 변수 정상화 과정

## 2. 잔차분석 (Residual Analysis)

**정의**: 잔차분석은 시계열 모델 적합 후 잔차(실제값과 예측값의 차이)의 특성을 검토하여 모델의 적합성을 평가하는 방법입니다. 좋은 모델의 잔차는 백색잡음(white noise) 특성을 가져야 합니다.

**수식**: 잔차는 다음과 같이 정의됩니다: $e_t = Y_t - \hat{Y}_t$

여기서 $Y_t$는 실제 관측값, $\hat{Y}_t$는 모델의 예측값입니다.

**특징**:

- 잔차는 정규분포를 따라야 하며, 평균이 0에 가까워야 합니다.
- 잔차 간에 자기상관(autocorrelation)이 없어야 합니다(독립성).
- 잔차의 분산이 일정해야 합니다(등분산성).
- Ljung-Box 검정, Durbin-Watson 검정 등으로 잔차의 독립성을 검정합니다.
- ACF, PACF 플롯을 통해 잔차의 자기상관 패턴을 시각적으로 확인합니다.
- 이상치(outlier)가 존재하는지 검토해야 합니다.
- 모델 개선을 위한 중요한 진단 도구입니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import statsmodels.api as sm
from statsmodels.stats.diagnostic import acorr_ljungbox
from statsmodels.stats.stattools import jarque_bera
from statsmodels.graphics.gofplots import qqplot
from scipy import stats

# 시계열 데이터 생성
np.random.seed(42)
n = 100
dates = pd.date_range(start='2020-01-01', periods=n, freq='D')

# AR(1) 프로세스 생성
ar_params = [0.7]
ma_params = []
ar = np.random.normal(0, 1, n)
for t in range(1, n):
    ar[t] += ar_params[0] * ar[t-1]

# 시계열 데이터프레임
df = pd.DataFrame({'y': ar}, index=dates)

# ARIMA 모델 적합
model = sm.tsa.ARIMA(df['y'], order=(1, 0, 0))
results = model.fit()
print(results.summary())

# 잔차 구하기
residuals = results.resid
fitted_values = results.fittedvalues

# 잔차 분석
print("\n잔차 기술통계량:")
print(residuals.describe())

# 1. Ljung-Box 검정 (자기상관 검정)
lb_test = acorr_ljungbox(residuals, lags=[10], return_df=True)
print("\nLjung-Box 검정 결과:")
print(lb_test)
if lb_test['lb_pvalue'].iloc[0] > 0.05:
    print("결론: 잔차에 자기상관이 없음 (모델 적합)")
else:
    print("결론: 잔차에 자기상관이 있음 (모델 재검토 필요)")

# 2. Jarque-Bera 검정 (정규성 검정)
jb_test = jarque_bera(residuals)
print("\nJarque-Bera 검정 결과:")
print(f"JB 통계량: {jb_test[0]:.4f}")
print(f"p-value: {jb_test[1]:.4f}")
if jb_test[1] > 0.05:
    print("결론: 잔차가 정규분포를 따름")
else:
    print("결론: 잔차가 정규분포를 따르지 않음")

# 시각화
plt.figure(figsize=(15, 10))

# 1. 원본 시계열과 모델 적합값
plt.subplot(2, 2, 1)
plt.plot(df.index, df['y'], label='원본 시계열')
plt.plot(df.index, fitted_values, 'r--', label='모델 적합값')
plt.title('원본 시계열과 모델 적합')
plt.legend()
plt.grid(True, alpha=0.3)

# 2. 잔차 시계열
plt.subplot(2, 2, 2)
plt.plot(df.index, residuals)
plt.axhline(y=0, color='r', linestyle='-')
plt.title('잔차 시계열')
plt.grid(True, alpha=0.3)

# 3. 잔차 ACF 플롯
plt.subplot(2, 2, 3)
sm.graphics.tsa.plot_acf(residuals.values.squeeze(), lags=20, ax=plt.gca())
plt.title('잔차 ACF 플롯')

# 4. 잔차 QQ 플롯
plt.subplot(2, 2, 4)
qqplot(residuals, line='45', fit=True, ax=plt.gca())
plt.title('잔차 QQ 플롯 (정규성 확인)')

plt.tight_layout()
plt.show()

# 추가 시각화: 잔차 히스토그램과 적합값 대비 잔차 플롯
plt.figure(figsize=(12, 5))

# 5. 잔차 히스토그램
plt.subplot(1, 2, 1)
plt.hist(residuals, bins=15, alpha=0.7, density=True, edgecolor='black')
# 정규분포 곡선 추가
x = np.linspace(residuals.min(), residuals.max(), 100)
plt.plot(x, stats.norm.pdf(x, residuals.mean(), residuals.std()), 'r-', linewidth=2)
plt.title('잔차 히스토그램')
plt.grid(True, alpha=0.3)

# 6. 적합값 대비 잔차 플롯 (등분산성 확인)
plt.subplot(1, 2, 2)
plt.scatter(fitted_values, residuals, alpha=0.7)
plt.axhline(y=0, color='r', linestyle='-')
plt.title('적합값 대비 잔차 플롯')
plt.xlabel('적합값')
plt.ylabel('잔차')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 시계열 모델(ARIMA, 지수평활법 등)의 적합성 평가
- 모델의 개선 방향 결정(차수 변경, 계절성 고려 등)
- 예측의 신뢰성 검증
- 이상점 및 구조적 변화 탐지
- 시계열 모델 간 비교 및 최적 모델 선택

## 3. 시계열 분해 (Time Series Decomposition)

**정의**: 시계열 분해는 시계열 데이터를 추세(Trend), 계절성(Seasonality), 순환성(Cycle), 불규칙성(Irregular) 등의 요소로 분리하는 방법입니다. 이를 통해 시계열의 패턴과 특성을 더 명확히 파악할 수 있습니다.

**수식**: 시계열 분해에는 주로 두 가지 모델이 사용됩니다:

- 가법 모델(Additive): $Y_t = T_t + S_t + I_t$
- 승법 모델(Multiplicative): $Y_t = T_t \times S_t \times I_t$

여기서 $T_t$는 추세, $S_t$는 계절성, $I_t$는 불규칙 요소입니다.

**특징**:

- 가법 모델은 계절적 변동이 일정할 때 적합합니다.
- 승법 모델은 계절적 변동이 추세에 비례하여 변할 때 적합합니다.
- 분해 방법으로는 고전적 방법, X-12-ARIMA, STL(Seasonal and Trend decomposition using Loess) 등이 있습니다.
- 분해된 구성요소는 시계열 특성 파악과 예측에 활용됩니다.
- 비정상 시계열을 정상화하는 데 도움이 됩니다.
- 계절 조정(seasonally adjusted) 데이터를 얻을 수 있습니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose
import statsmodels.api as sm

# 시계열 데이터 생성 (추세 + 계절성 + 불규칙성)
np.random.seed(42)
n = 4 * 12  # 4년치 월별 데이터
dates = pd.date_range(start='2018-01-01', periods=n, freq='MS')

# 추세 요소
trend = np.linspace(7, 15, n)

# 계절성 요소 (12개월 주기)
seasonality = 3 * np.sin(np.linspace(0, 2*np.pi*4, n))

# 불규칙 요소
irregular = np.random.normal(0, 0.5, n)

# 가법 모델
additive_ts = trend + seasonality + irregular

# 승법 모델
multiplicative_ts = trend * (1 + seasonality/15) * (1 + irregular/10)

# 데이터프레임 생성
df = pd.DataFrame({
    '가법_모델': additive_ts,
    '승법_모델': multiplicative_ts
}, index=dates)

# 가법 모델 시계열 분해
add_decomposition = seasonal_decompose(df['가법_모델'], model='additive', period=12)

# 승법 모델 시계열 분해
mul_decomposition = seasonal_decompose(df['승법_모델'], model='multiplicative', period=12)

# 시각화: 가법 모델 분해
plt.figure(figsize=(14, 10))
plt.suptitle('가법 모델 시계열 분해', fontsize=16)

plt.subplot(4, 1, 1)
plt.plot(df['가법_모델'], label='원본 시계열')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 2)
plt.plot(add_decomposition.trend, label='추세')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 3)
plt.plot(add_decomposition.seasonal, label='계절성')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 4)
plt.plot(add_decomposition.resid, label='잔차(불규칙성)')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.subplots_adjust(top=0.9)
plt.show()

# 시각화: 승법 모델 분해
plt.figure(figsize=(14, 10))
plt.suptitle('승법 모델 시계열 분해', fontsize=16)

plt.subplot(4, 1, 1)
plt.plot(df['승법_모델'], label='원본 시계열')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 2)
plt.plot(mul_decomposition.trend, label='추세')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 3)
plt.plot(mul_decomposition.seasonal, label='계절성')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 4)
plt.plot(mul_decomposition.resid, label='잔차(불규칙성)')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.subplots_adjust(top=0.9)
plt.show()

# STL 분해 방법 적용 (비모수적 방법)
stl = sm.tsa.STL(df['가법_모델'], period=12).fit()

plt.figure(figsize=(14, 10))
plt.suptitle('STL 시계열 분해', fontsize=16)

plt.subplot(4, 1, 1)
plt.plot(df['가법_모델'], label='원본 시계열')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 2)
plt.plot(stl.trend, label='추세')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 3)
plt.plot(stl.seasonal, label='계절성')
plt.legend()
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 4)
plt.plot(stl.resid, label='잔차(불규칙성)')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.subplots_adjust(top=0.9)
plt.show()

# 계절 조정 시계열
plt.figure(figsize=(12, 6))
plt.plot(df['가법_모델'], label='원본 시계열')
plt.plot(add_decomposition.trend + add_decomposition.resid, label='계절 조정 시계열')
plt.title('계절 조정 시계열 (Seasonally Adjusted)')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

**개념의 활용**:

- 경제 지표의 계절성 조정(예: 실업률, 소비자 물가지수)
- 시계열의 장기 추세와 계절 패턴 파악
- 이상치나 구조적 변화 감지
- 시계열 예측 모델의 정확도 향상
- 다양한 요인이 시계열에 미치는 영향 분석

## 4. 단순 이동평균 모형 (Simple Moving Average Model)

**정의**: 단순 이동평균 모형은 시계열의 각 시점에서 이전 k개 관측값의 평균을 계산하여 시계열의 단기 변동을 평활화하는 방법입니다. 이는 시계열의 추세를 식별하고 노이즈를 줄이는 데 유용합니다.

**수식**: k기간 단순 이동평균: $MA_t = \frac{Y_t + Y_{t-1} + \ldots + Y_{t-k+1}}{k} = \frac{1}{k}\sum_{i=0}^{k-1} Y_{t-i}$

여기서 $Y_t$는 t시점의 시계열 관측값, $MA_t$는 t시점의 이동평균값입니다.

**특징**:

- 계산이 단순하고 직관적입니다.
- 시계열의 단기 변동(노이즈)을 제거하여 기본 패턴을 파악할 수 있습니다.
- 윈도우 크기(k)가 클수록 평활화 효과가 커지고 반응성은 낮아집니다.
- 중심화 이동평균(centered moving average)은 과거와 미래 값을 동시에 사용합니다.
- 예측보다는 추세 파악과 계절성 분석에 주로 사용됩니다.
- 급격한 변화나 구조적 변화에 대한 반응이 느립니다.
- 시계열의 끝 부분에서는 계산할 수 없는 값이 발생합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.holtwinters import SimpleExpSmoothing
from scipy import signal

# 시계열 데이터 생성
np.random.seed(42)
n = 200
dates = pd.date_range(start='2020-01-01', periods=n, freq='D')

# 추세와 노이즈가 있는 시계열
trend = np.linspace(0, 10, n)
noise = np.random.normal(0, 1, n)
ts = trend + noise

# 구조적 변화 추가
ts[100:] += 5

# 데이터프레임 생성
df = pd.DataFrame({'y': ts}, index=dates)

# 다양한 윈도우 크기의 이동평균 계산
ma_windows = [5, 10, 30]
for window in ma_windows:
    df[f'MA_{window}'] = df['y'].rolling(window=window, center=False).mean()

# 중심화 이동평균
df['MA_10_centered'] = df['y'].rolling(window=10, center=True).mean()

# scipy를 사용한 이동평균 (끝 부분의 값도 계산)
def moving_average(x, w):
    return np.convolve(x, np.ones(w), 'valid') / w

# 시각화
plt.figure(figsize=(14, 10))

# 1. 원본 시계열과 다양한 이동평균
plt.subplot(2, 2, 1)
plt.plot(df['y'], label='원본 시계열', alpha=0.7)
for window in ma_windows:
    plt.plot(df[f'MA_{window}'], label=f'{window}일 이동평균')
plt.title('다양한 윈도우 크기의 이동평균')
plt.legend()
plt.grid(True, alpha=0.3)

# 2. 중심화 이동평균 vs 표준 이동평균
plt.subplot(2, 2, 2)
plt.plot(df['y'], label='원본 시계열', alpha=0.7)
plt.plot(df['MA_10'], label='10일 이동평균')
plt.plot(df['MA_10_centered'], label='10일 중심화 이동평균')
plt.title('중심화 이동평균 vs 표준 이동평균')
plt.legend()
plt.grid(True, alpha=0.3)

# 3. 이동평균을 이용한 추세 추출
plt.subplot(2, 2, 3)
plt.plot(df['y'], label='원본 시계열', alpha=0.5)
plt.plot(df['MA_30'], label='30일 이동평균 (추세)')
plt.title('이동평균을 이용한 추세 추출')
plt.legend()
plt.grid(True, alpha=0.3)

# 4. 이동평균과 잔차
plt.subplot(2, 2, 4)
residuals = df['y'] - df['MA_10']
plt.plot(residuals, label='이동평균에서의 잔차')
plt.axhline(y=0, color='r', linestyle='-')
plt.title('이동평균에서의 잔차')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 이동평균을 이용한 간단한 예측 (naïve approach)
forecast_horizon = 20
forecast_index = pd.date_range(start=dates[-1] + pd.Timedelta(days=1), periods=forecast_horizon, freq='D')

# 마지막 10일 이동평균 값을 예측으로 사용
last_ma = df['MA_10'].dropna().iloc[-1]
forecast = np.full(forecast_horizon, last_ma)

# 예측 시각화
plt.figure(figsize=(12, 6))
plt.plot(df['y'], label='원본 시계열')
plt.plot(df['MA_10'], label='10일 이동평균')
plt.plot(forecast_index, forecast, 'r--', label='이동평균 기반 예측')
plt.title('이동평균을 이용한 간단한 예측')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

**개념의 활용**:

- 주가 추세 분석(기술적 분석)에서 이동평균선 활용
- 시계열의 노이즈 제거와 시각적 평활화
- 계절성이 있는 데이터의 추세 파악
- 간단한 예측 모델로 활용(예: 나이브 예측)
- 이상치 탐지를 위한 기준선 설정

## 5. 평균평활 모형 (Exponential Smoothing Model)

**정의**: 지수평활법은 과거 관측값에 지수적으로 감소하는 가중치를 부여하여 시계열을 평활화하는 방법입니다. 최근 데이터에 더 높은 가중치를 부여함으로써 시계열의 최근 패턴을 더 잘 반영할 수 있습니다.

**수식**:

- 단순 지수평활법(SES): $S_t = \alpha Y_t + (1-\alpha)S_{t-1}$, 여기서 0 < α < 1
- 홀트 지수평활법(추세 고려): $S_t = \alpha Y_t + (1-\alpha)(S_{t-1} + T_{t-1})$ $T_t = \beta(S_t - S_{t-1}) + (1-\beta)T_{t-1}$
- 홀트-윈터스(계절성 고려):
    - 가법: $S_t = \alpha(Y_t - I_{t-m}) + (1-\alpha)(S_{t-1} + T_{t-1})$
    - 승법: $S_t = \alpha\frac{Y_t}{I_{t-m}} + (1-\alpha)(S_{t-1} + T_{t-1})$

**특징**:

- 단순 이동평균보다 최근 데이터에 더 민감하게 반응합니다.
- 평활화 매개변수(α)가 클수록 최근 관측값에 더 큰 가중치가 부여됩니다.
- 단순, 추세, 계절성에 따라 다양한 변형이 있습니다.
- 단기 예측에 효과적이지만, 장기 예측은 안정성이 떨어질 수 있습니다.
- 계산이 간단하고 적은 데이터로도 적용 가능합니다.
- 매개변수는 경험적으로 선택하거나 최적화할 수 있습니다.
- 급격한 변화에 점진적으로 적응합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.holtwinters import SimpleExpSmoothing, ExponentialSmoothing

# 시계열 데이터 생성
np.random.seed(42)
n = 100
dates = pd.date_range(start='2020-01-01', periods=n, freq='D')

# 추세와 노이즈가 있는 시계열
trend = np.linspace(0, 5, n)
noise = np.random.normal(0, 0.5, n)
ts = trend + noise

# 시점 변화 추가
ts[50:] += 2

# 데이터프레임 생성
df = pd.DataFrame({'y': ts}, index=dates)

# 1. 단순 지수평활법(SES) - 다양한 알파 값
alphas = [0.1, 0.3, 0.7]
ses_models = {}
ses_forecasts = {}

for alpha in alphas:
    # 모델 적합
    model = SimpleExpSmoothing(df['y']).fit(smoothing_level=alpha, optimized=False)
    ses_models[alpha] = model
    
    # 적합값
    df[f'SES_alpha_{alpha}'] = model.fittedvalues
    
    # 예측
    forecast = model.forecast(5)
    ses_forecasts[alpha] = forecast

# 2. 홀트 지수평활법 (추세 고려)
holt_model = ExponentialSmoothing(df['y'], trend='add').fit()
df['Holt'] = holt_model.fittedvalues
holt_forecast = holt_model.forecast(5)

# 3. 홀트-윈터스 지수평활법 시뮬레이션 (계절성 시계열 생성)
m = 12  # 계절 주기
n_years = 3
n_with_season = m * n_years
dates_season = pd.date_range(start='2020-01-01', periods=n_with_season, freq='MS')

# 추세, 계절성, 노이즈로 구성된 시계열
trend_season = np.linspace(0, 5, n_with_season)
seasonality = 2 * np.sin(np.linspace(0, 2*n_years*np.pi, n_with_season))
noise_season = np.random.normal(0, 0.3, n_with_season)
ts_season = trend_season + seasonality + noise_season

df_season = pd.DataFrame({'y': ts_season}, index=dates_season)

# 홀트-윈터스 모델 적합 (가법 모델)
hw_add_model = ExponentialSmoothing(
    df_season['y'], trend='add', seasonal='add', seasonal_periods=m).fit()
df_season['HW_add'] = hw_add_model.fittedvalues
hw_add_forecast = hw_add_model.forecast(m)  # 1년 예측

# 홀트-윈터스 모델 적합 (승법 모델)
hw_mul_model = ExponentialSmoothing(
    df_season['y'], trend='add', seasonal='mul', seasonal_periods=m).fit()
df_season['HW_mul'] = hw_mul_model.fittedvalues
hw_mul_forecast = hw_mul_model.forecast(m)  # 1년 예측

# 시각화
plt.figure(figsize=(15, 10))

# 1. 단순 지수평활법(SES) - 다양한 알파 값
plt.subplot(2, 2, 1)
plt.plot(df['y'], label='원본 시계열', alpha=0.7)
for alpha in alphas:
    plt.plot(df[f'SES_alpha_{alpha}'], label=f'SES (α={alpha})')
plt.title('단순 지수평활법(SES) - 다양한 알파 값')
plt.legend()
plt.grid(True, alpha=0.3)

# 2. SES 예측 비교
forecast_index = pd.date_range(start=dates[-1] + pd.Timedelta(days=1), periods=5, freq='D')
plt.subplot(2, 2, 2)
plt.plot(df['y'], label='원본 시계열', alpha=0.7)
for alpha in alphas:
    plt.plot(forecast_index, ses_forecasts[alpha], '--', label=f'SES (α={alpha}) 예측')
plt.title('단순 지수평활법(SES) 예측 비교')
plt.legend()
plt.grid(True, alpha=0.3)

# 3. 홀트 지수평활법 (추세 고려)
plt.subplot(2, 2, 3)
plt.plot(df['y'], label='원본 시계열', alpha=0.7)
plt.plot(df['Holt'], label='홀트 지수평활법')
plt.plot(forecast_index, holt_forecast, 'r--', label='홀트 예측')
plt.title('홀트 지수평활법 (추세 고려)')
plt.legend()
plt.grid(True, alpha=0.3)

# 4. 홀트-윈터스 지수평활법 (추세 및 계절성 고려)
plt.subplot(2, 2, 4)
plt.plot(df_season['y'], label='계절성 시계열', alpha=0.7)
plt.plot(df_season['HW_add'], label='홀트-윈터스 (가법)')
plt.plot(df_season['HW_mul'], label='홀트-윈터스 (승법)')
plt.title('홀트-윈터스 지수평활법 (계절성 고려)')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 홀트-윈터스 예측 시각화
plt.figure(figsize=(12, 6))
forecast_index_season = pd.date_range(start=dates_season[-1] + pd.DateOffset(months=1), periods=m, freq='MS')

plt.plot(df_season['y'], label='계절성 시계열')
plt.plot(forecast_index_season, hw_add_forecast, 'r--', label='홀트-윈터스 (가법) 예측')
plt.plot(forecast_index_season, hw_mul_forecast, 'g--', label='홀트-윈터스 (승법) 예측')
plt.title('홀트-윈터스 지수평활법 예측 (1년)')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

**개념의 활용**:

- 수요 예측(재고 관리, 판매 예측)
- 웹사이트 트래픽 예측
- 경제 지표의 단기 예측
- 주가 분석에서 평균선 기법으로 활용
- 이상치 탐지 및 알림 시스템 구축

## 6. 자기회귀 이동평균 모형 (ARMA, Autoregressive Moving Average Model)

**정의**: ARMA 모형은 시계열의 자기회귀(AR) 특성과 이동평균(MA) 특성을 결합한 선형 모델입니다. 이는 과거 관측값과 과거 오차항이 현재 관측값에 미치는 영향을 함께 모델링합니다.

**수식**: ARMA(p, q) 모형은 다음과 같이 표현됩니다: $Y_t = c + \sum_{i=1}^{p} \phi_i Y_{t-i} + \sum_{j=1}^{q} \theta_j \varepsilon_{t-j} + \varepsilon_t$

여기서:

- p: 자기회귀(AR) 차수
- q: 이동평균(MA) 차수
- $\phi_i$: AR 계수
- $\theta_j$: MA 계수
- $\varepsilon_t$: 백색잡음 오차항
- c: 상수항

**특징**:

- 정상 시계열을 모델링하는 데 사용됩니다.
- AR 부분은 시계열의 자기상관을, MA 부분은 과거 오차의 영향을 반영합니다.
- ACF와 PACF 플롯을 통해 적절한 p, q 차수를 결정합니다.
- AIC, BIC와 같은 정보 기준으로 최적 모델을 선택할 수 있습니다.
- 단기 예측에 효과적이지만, 비선형 패턴이나 구조적 변화를 포착하기 어렵습니다.
- 복잡한 계절성이나 추세는 직접 모델링하지 않습니다.
- 모델 식별, 추정, 진단, 예측의 Box-Jenkins 방법론을 따릅니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import statsmodels.api as sm
from statsmodels.tsa.arima.model import ARIMA
from statsmodels.tsa.stattools import acf, pacf
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf

# 시계열 데이터 생성
np.random.seed(42)
n = 200
dates = pd.date_range(start='2020-01-01', periods=n, freq='D')

# AR(1) 프로세스 생성
ar_params = [0.7]
ar = np.random.normal(0, 1, n)
for t in range(1, n):
    ar[t] += ar_params[0] * ar[t-1]

# MA(1) 프로세스 생성
ma_params = [0.6]
errors = np.random.normal(0, 1, n)
ma = errors.copy()
for t in range(1, n):
    ma[t] += ma_params[0] * errors[t-1]

# ARMA(1,1) 프로세스 생성
arma = np.random.normal(0, 1, n)
for t in range(1, n):
    arma[t] += ar_params[0] * arma[t-1] + ma_params[0] * errors[t-1]

# 데이터프레임 생성
df = pd.DataFrame({
    'AR(1)': ar,
    'MA(1)': ma,
    'ARMA(1,1)': arma
}, index=dates)

# ACF와 PACF 플롯을 통한 모델 식별
plt.figure(figsize=(15, 10))

# AR(1) 모델의 ACF와 PACF
plt.subplot(3, 2, 1)
plot_acf(df['AR(1)'], lags=20, ax=plt.gca())
plt.title('AR(1) 모델의 ACF')

plt.subplot(3, 2, 2)
plot_pacf(df['AR(1)'], lags=20, ax=plt.gca())
plt.title('AR(1) 모델의 PACF')

# MA(1) 모델의 ACF와 PACF
plt.subplot(3, 2, 3)
plot_acf(df['MA(1)'], lags=20, ax=plt.gca())
plt.title('MA(1) 모델의 ACF')

plt.subplot(3, 2, 4)
plot_pacf(df['MA(1)'], lags=20, ax=plt.gca())
plt.title('MA(1) 모델의 PACF')

# ARMA(1,1) 모델의 ACF와 PACF
plt.subplot(3, 2, 5)
plot_acf(df['ARMA(1,1)'], lags=20, ax=plt.gca())
plt.title('ARMA(1,1) 모델의 ACF')

plt.subplot(3, 2, 6)
plot_pacf(df['ARMA(1,1)'], lags=20, ax=plt.gca())
plt.title('ARMA(1,1) 모델의 PACF')

plt.tight_layout()
plt.show()

# 모델 적합 및 예측
# 1. AR(1) 모델
ar_model = ARIMA(df['AR(1)'], order=(1, 0, 0)).fit()
print("AR(1) 모델 요약:")
print(ar_model.summary())

# 2. MA(1) 모델
ma_model = ARIMA(df['MA(1)'], order=(0, 0, 1)).fit()
print("\nMA(1) 모델 요약:")
print(ma_model.summary())

# 3. ARMA(1,1) 모델
arma_model = ARIMA(df['ARMA(1,1)'], order=(1, 0, 1)).fit()
print("\nARMA(1,1) 모델 요약:")
print(arma_model.summary())

# 예측 및 시각화
forecast_steps = 20
forecast_index = pd.date_range(start=dates[-1] + pd.Timedelta(days=1), periods=forecast_steps, freq='D')

# AR(1) 모델 예측
ar_forecast = ar_model.forecast(steps=forecast_steps)
ar_conf_int = ar_model.get_forecast(steps=forecast_steps).conf_int()

# ARMA(1,1) 모델 예측
arma_forecast = arma_model.forecast(steps=forecast_steps)
arma_conf_int = arma_model.get_forecast(steps=forecast_steps).conf_int()

# 예측 시각화
plt.figure(figsize=(12, 8))

# AR(1) 모델 예측
plt.subplot(2, 1, 1)
plt.plot(df['AR(1)'], label='실제 AR(1) 시계열')
plt.plot(ar_model.fittedvalues, 'r--', label='AR(1) 모델 적합값')
plt.plot(forecast_index, ar_forecast, 'g-', label='AR(1) 모델 예측')
plt.fill_between(forecast_index, 
                 ar_conf_int.iloc[:, 0], 
                 ar_conf_int.iloc[:, 1], 
                 color='g', alpha=0.2, label='95% 신뢰구간')
plt.title('AR(1) 모델 적합 및 예측')
plt.legend()
plt.grid(True, alpha=0.3)

# ARMA(1,1) 모델 예측
plt.subplot(2, 1, 2)
plt.plot(df['ARMA(1,1)'], label='실제 ARMA(1,1) 시계열')
plt.plot(arma_model.fittedvalues, 'r--', label='ARMA(1,1) 모델 적합값')
plt.plot(forecast_index, arma_forecast, 'g-', label='ARMA(1,1) 모델 예측')
plt.fill_between(forecast_index, 
                 arma_conf_int.iloc[:, 0], 
                 arma_conf_int.iloc[:, 1], 
                 color='g', alpha=0.2, label='95% 신뢰구간')
plt.title('ARMA(1,1) 모델 적합 및 예측')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 모델 진단 (ARMA 모델 예시)
plt.figure(figsize=(12, 10))

# 1. 잔차 시계열
plt.subplot(2, 2, 1)
plt.plot(arma_model.resid)
plt.axhline(y=0, color='r', linestyle='-')
plt.title('ARMA(1,1) 모델 잔차')
plt.grid(True, alpha=0.3)

# 2. 잔차 히스토그램
plt.subplot(2, 2, 2)
plt.hist(arma_model.resid, bins=20, alpha=0.7, density=True, edgecolor='black')
# 정규분포 곡선 추가
x = np.linspace(arma_model.resid.min(), arma_model.resid.max(), 100)
plt.plot(x, stats.norm.pdf(x, arma_model.resid.mean(), arma_model.resid.std()), 'r-', linewidth=2)
plt.title('잔차 히스토그램')
plt.grid(True, alpha=0.3)

# 3. 잔차 ACF
plt.subplot(2, 2, 3)
plot_acf(arma_model.resid, lags=20, ax=plt.gca())
plt.title('잔차 ACF')

# 4. Q-Q 플롯
plt.subplot(2, 2, 4)
sm.graphics.qqplot(arma_model.resid, line='45', fit=True, ax=plt.gca())
plt.title('잔차 Q-Q 플롯')

plt.tight_layout()
plt.show()

# 정보 기준을 이용한 모델 선택
aic_values = []
bic_values = []
orders = [(p, 0, q) for p in range(3) for q in range(3)]

for order in orders:
    try:
        model = ARIMA(df['ARMA(1,1)'], order=order).fit()
        aic_values.append(model.aic)
        bic_values.append(model.bic)
    except:
        aic_values.append(np.nan)
        bic_values.append(np.nan)

# 결과 정리
results_df = pd.DataFrame({
    'Order': [(p, 0, q) for p in range(3) for q in range(3)],
    'AIC': aic_values,
    'BIC': bic_values
})

print("\n모델 선택 결과:")
print(results_df.sort_values('AIC'))
```

**개념의 활용**:

- 금융 시계열 분석 및 예측(주가, 환율)
- 경제 지표의 단기 예측
- 센서 데이터 분석과 이상치 탐지
- 품질 관리 시스템에서의 공정 모니터링
- 복잡한 시계열의 기본 패턴 파악과 예측

## 7. ARIMA (Autoregressive Integrated Moving Average)

**정의**: ARIMA는 ARMA 모델을 비정상 시계열에 확장한 것으로, 차분(differencing)을 통해 비정상 시계열을 정상화한 후 ARMA 모델을 적용합니다. 이는 추세가 있는 시계열을 모델링하는 데 유용합니다.

**수식**: ARIMA(p, d, q) 모형은 다음과 같이 표현됩니다: $(1 - \sum_{i=1}^{p} \phi_i L^i)(1 - L)^d Y_t = (1 + \sum_{j=1}^{q} \theta_j L^j)\varepsilon_t$

여기서:

- p: 자기회귀(AR) 차수
- d: 차분(differencing) 차수
- q: 이동평균(MA) 차수
- L: 지연 연산자 (LYₜ = Yₜ₋₁)
- $\phi_i$: AR 계수
- $\theta_j$: MA 계수
- $\varepsilon_t$: 백색잡음 오차항

**특징**:

- 비정상 시계열을 차분하여 정상화한 후 ARMA 모델을 적용합니다.
- 차분 차수 d는 일반적으로 단위근 검정(ADF, KPSS 등)을 통해 결정합니다.
- Box-Jenkins 방법론(식별, 추정, 진단, 예측)을 따릅니다.
- 선형 추세가 있는 시계열에 적합합니다.
- 계절성을 직접 모델링하지 않습니다(계절성은 SARIMA에서 처리).
- 모델 복잡성이 증가할수록 과적합 위험이 있습니다.
- ACF, PACF 및 정보 기준(AIC, BIC)을 통해 최적 차수를 선택합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.arima.model import ARIMA
from statsmodels.tsa.stattools import adfuller
import statsmodels.api as sm
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
import pmdarima as pm
from sklearn.metrics import mean_squared_error

# 비정상 시계열 데이터 생성 (추세 포함)
np.random.seed(42)
n = 200
dates = pd.date_range(start='2020-01-01', periods=n, freq='D')

# 자기회귀 프로세스에 추세 추가
ar_params = [0.7]
trend = np.linspace(0, 10, n)
ar = np.zeros(n)
errors = np.random.normal(0, 1, n)

for t in range(1, n):
    ar[t] = trend[t] + ar_params[0] * (ar[t-1] - trend[t-1]) + errors[t]

# 데이터프레임 생성
df = pd.DataFrame({'y': ar}, index=dates)

# ADF 검정으로 정상성 확인
def check_stationarity(series, title=''):
    result = adfuller(series.dropna())
    print(f"ADF 검정 결과 - {title}")
    print(f"ADF 통계량: {result[0]:.4f}")
    print(f"p-value: {result[1]:.4f}")
    for key, value in result[4].items():
        print(f"임계값 ({key}): {value:.4f}")
    if result[1] <= 0.05:
        print("결론: 정상 시계열 (귀무가설 기각)")
    else:
        print("결론: 비정상 시계열 (귀무가설 기각 실패)")
    print()

# 원본 시계열과 차분 시계열의 정상성 확인
check_stationarity(df['y'], '원본 시계열')
check_stationarity(df['y'].diff().dropna(), '1차 차분 시계열')

# 1차 차분 시계열 생성
df['diff1'] = df['y'].diff()

# 시각화: 원본 시계열과 차분 시계열
plt.figure(figsize=(12, 8))

plt.subplot(2, 2, 1)
plt.plot(df['y'])
plt.title('원본 시계열 (비정상)')
plt.grid(True, alpha=0.3)

plt.subplot(2, 2, 2)
plt.plot(df['diff1'])
plt.title('1차 차분 시계열')
plt.grid(True, alpha=0.3)

# ACF와 PACF 플롯으로 ARIMA 차수 식별
plt.subplot(2, 2, 3)
plot_acf(df['diff1'].dropna(), lags=20, ax=plt.gca())
plt.title('1차 차분 시계열의 ACF')

plt.subplot(2, 2, 4)
plot_pacf(df['diff1'].dropna(), lags=20, ax=plt.gca())
plt.title('1차 차분 시계열의 PACF')

plt.tight_layout()
plt.show()

# ARIMA 모델 적합
arima_model = ARIMA(df['y'], order=(1, 1, 1)).fit()
print("ARIMA(1,1,1) 모델 요약:")
print(arima_model.summary())

# auto_arima로 최적 차수 선택
auto_model = pm.auto_arima(df['y'], start_p=0, start_q=0, max_p=3, max_q=3, d=None,
                          test='adf', seasonal=False, trace=True,
                          error_action='ignore', suppress_warnings=True, stepwise=True)

print("\nauto_arima 최적 모델:")
print(auto_model.summary())
best_order = auto_model.order
print(f"최적 차수 (p,d,q): {best_order}")

# 최적 모델 적합
best_model = ARIMA(df['y'], order=best_order).fit()

# 훈련/테스트 분리 및 평가
train_size = int(0.8 * len(df))
train, test = df.iloc[:train_size], df.iloc[train_size:]

# 훈련 데이터로 모델 적합
train_model = ARIMA(train['y'], order=best_order).fit()

# 테스트 기간 예측
forecast_steps = len(test)
forecast = train_model.forecast(steps=forecast_steps)

# RMSE 계산
rmse = np.sqrt(mean_squared_error(test['y'], forecast))
print(f"\n테스트 데이터 RMSE: {rmse:.4f}")

# 예측 시각화
plt.figure(figsize=(12, 6))
plt.plot(df.index[:train_size], train['y'], label='훈련 데이터')
plt.plot(df.index[train_size:], test['y'], label='테스트 데이터')
plt.plot(df.index[train_size:], forecast, 'r--', label='ARIMA 예측')
plt.title(f'ARIMA{best_order} 모델 예측 (RMSE: {rmse:.4f})')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

# 전체 데이터로 모델 재적합 및 미래 예측
final_model = ARIMA(df['y'], order=best_order).fit()
future_steps = 30
future_index = pd.date_range(start=dates[-1] + pd.Timedelta(days=1), periods=future_steps, freq='D')
future_forecast = final_model.forecast(steps=future_steps)
forecast_conf_int = final_model.get_forecast(steps=future_steps).conf_int()

# 미래 예측 시각화
plt.figure(figsize=(12, 6))
plt.plot(df['y'], label='관측 데이터')
plt.plot(future_index, future_forecast, 'r-', label='ARIMA 예측')
plt.fill_between(future_index, 
                 forecast_conf_int.iloc[:, 0], 
                 forecast_conf_int.iloc[:, 1], 
                 color='r', alpha=0.2, label='95% 신뢰구간')
plt.title(f'ARIMA{best_order} 모델 미래 예측')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

# 모델 진단
plt.figure(figsize=(12, 10))

# 1. 잔차 시계열
plt.subplot(2, 2, 1)
plt.plot(final_model.resid)
plt.axhline(y=0, color='r', linestyle='-')
plt.title('ARIMA 모델 잔차')
plt.grid(True, alpha=0.3)

# 2. 잔차 히스토그램
plt.subplot(2, 2, 2)
plt.hist(final_model.resid, bins=20, alpha=0.7, density=True, edgecolor='black')
# 정규분포 곡선 추가
x = np.linspace(final_model.resid.min(), final_model.resid.max(), 100)
plt.plot(x, stats.norm.pdf(x, final_model.resid.mean(), final_model.resid.std()), 'r-', linewidth=2)
plt.title('잔차 히스토그램')
plt.grid(True, alpha=0.3)

# 3. 잔차 ACF
plt.subplot(2, 2, 3)
plot_acf(final_model.resid, lags=20, ax=plt.gca())
plt.title('잔차 ACF')

# 4. Q-Q 플롯
plt.subplot(2, 2, 4)
sm.graphics.qqplot(final_model.resid, line='45', fit=True, ax=plt.gca())
plt.title('잔차 Q-Q 플롯')

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 추세가 있는 경제 시계열의 예측(GDP, 물가지수)
- 금융 시계열의 분석 및 예측(주가, 환율)
- 판매량이나 수요 패턴 예측
- 웹사이트 트래픽 예측
- 인구통계학적 시계열 분석

## 8. SARIMA (Seasonal ARIMA)

**정의**: SARIMA는 ARIMA 모델에 계절성 요소를 추가한 확장 모델입니다. 시계열에 존재하는 계절적 패턴(예: 매년, 매월 반복되는 패턴)을 모델링하여 더 정확한 예측을 제공합니다.

**수식**: SARIMA(p, d, q)(P, D, Q)s 모형: $\Phi_P(L^s)\phi_p(L)(1-L)^d(1-L^s)^D Y_t = \Theta_Q(L^s)\theta_q(L)\varepsilon_t$

여기서:

- p, d, q: 비계절성 ARIMA 차수
- P, D, Q: 계절성 ARIMA 차수
- s: 계절 주기 (예: 월별 데이터는 s=12, 분기별 데이터는 s=4)
- $\Phi_P, \phi_p$: 계절성 및 비계절성 AR 연산자
- $\Theta_Q, \theta_q$: 계절성 및 비계절성 MA 연산자
- L: 지연 연산자

**특징**:

- 계절성과 비계절성 패턴을 동시에 모델링합니다.
- 복잡한 시계열 패턴을 포착할 수 있습니다.
- 계절 차분(D)은 계절적 비정상성을 제거합니다.
- 계절 주기(s)는 데이터 빈도에 따라 결정됩니다(일별=7, 월별=12, 분기별=4 등).
- ARIMA보다 추정해야 할 매개변수가 많아 과적합 위험이 높습니다.
- 정보 기준(AIC, BIC)이나 auto_arima와 같은 자동화 도구로 차수를 선택할 수 있습니다.
- 중장기 예측에 효과적입니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.statespace.sarimax import SARIMAX
from statsmodels.tsa.seasonal import seasonal_decompose
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
import pmdarima as pm
from sklearn.metrics import mean_squared_error

# 계절성이 있는 시계열 데이터 생성
np.random.seed(42)
n_years = 5
s = 12  # 월별 데이터
n = n_years * s
dates = pd.date_range(start='2018-01-01', periods=n, freq='MS')

# 추세 + 계절성 + 노이즈
trend = np.linspace(0, 5, n)
seasonality = 3 * np.sin(np.linspace(0, 2*np.pi*n_years, n))
noise = np.random.normal(0, 0.5, n)
ts = trend + seasonality + noise

# 데이터프레임 생성
df = pd.DataFrame({'y': ts}, index=dates)

# 시계열 분해로 계절성 확인
decomposition = seasonal_decompose(df['y'], model='additive', period=s)

# 시각화: 원본 시계열 및 분해 결과
plt.figure(figsize=(12, 10))

plt.subplot(4, 1, 1)
plt.plot(df['y'])
plt.title('원본 시계열 (계절성 포함)')
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 2)
plt.plot(decomposition.trend)
plt.title('추세 요소')
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 3)
plt.plot(decomposition.seasonal)
plt.title('계절성 요소')
plt.grid(True, alpha=0.3)

plt.subplot(4, 1, 4)
plt.plot(decomposition.resid)
plt.title('잔차 요소')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 계절 차분 및 일반 차분
df['seasonal_diff'] = df['y'] - df['y'].shift(s)
df['diff'] = df['seasonal_diff'].diff()

# 시각화: 차분 결과
plt.figure(figsize=(12, 8))

plt.subplot(3, 1, 1)
plt.plot(df['y'])
plt.title('원본 시계열')
plt.grid(True, alpha=0.3)

plt.subplot(3, 1, 2)
plt.plot(df['seasonal_diff'])
plt.title('계절 차분 (D=1)')
plt.grid(True, alpha=0.3)

plt.subplot(3, 1, 3)
plt.plot(df['diff'])
plt.title('계절 차분 후 일반 차분 (D=1, d=1)')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# ACF와 PACF 플롯으로 SARIMA 차수 식별
plt.figure(figsize=(12, 8))

plt.subplot(2, 2, 1)
plot_acf(df['y'].dropna(), lags=36, ax=plt.gca())
plt.title('원본 시계열의 ACF')

plt.subplot(2, 2, 2)
plot_pacf(df['y'].dropna(), lags=36, ax=plt.gca())
plt.title('원본 시계열의 PACF')

plt.subplot(2, 2, 3)
plot_acf(df['seasonal_diff'].dropna(), lags=36, ax=plt.gca())
plt.title('계절 차분 시계열의 ACF')

plt.subplot(2, 2, 4)
plot_pacf(df['seasonal_diff'].dropna(), lags=36, ax=plt.gca())
plt.title('계절 차분 시계열의 PACF')

plt.tight_layout()
plt.show()

# SARIMA 모델 적합
sarima_model = SARIMAX(df['y'], order=(1, 1, 1), seasonal_order=(1, 1, 1, s)).fit(disp=False)
print("SARIMA(1,1,1)(1,1,1,12) 모델 요약:")
print(sarima_model.summary())

# auto_arima로 최적 차수 선택
auto_model = pm.auto_arima(df['y'], start_p=0, start_q=0, max_p=2, max_q=2, d=None,
                          start_P=0, start_Q=0, max_P=1, max_Q=1, D=None, m=s,
                          seasonal=True, test='adf', trace=True,
                          error_action='ignore', suppress_warnings=True, stepwise=True)

print("\nauto_arima 최적 모델:")
print(auto_model.summary())
best_order = auto_model.order
best_seasonal_order = auto_model.seasonal_order
print(f"최적 차수 (p,d,q)(P,D,Q,s): {best_order}{best_seasonal_order}")

# 최적 모델 적합
best_model = SARIMAX(df['y'], order=best_order, seasonal_order=best_seasonal_order).fit(disp=False)

# 훈련/테스트 분리 및 평가
train_size = int(0.8 * len(df))
train, test = df.iloc[:train_size], df.iloc[train_size:]

# 훈련 데이터로 모델 적합
train_model = SARIMAX(train['y'], order=best_order, seasonal_order=best_seasonal_order).fit(disp=False)

# 테스트 기간 예측
forecast_steps = len(test)
forecast = train_model.forecast(steps=forecast_steps)

# RMSE 계산
rmse = np.sqrt(mean_squared_error(test['y'], forecast))
print(f"\n테스트 데이터 RMSE: {rmse:.4f}")

# 예측 시각화
plt.figure(figsize=(12, 6))
plt.plot(df.index[:train_size], train['y'], label='훈련 데이터')
plt.plot(df.index[train_size:], test['y'], label='테스트 데이터')
plt.plot(df.index[train_size:], forecast, 'r--', label='SARIMA 예측')
plt.title(f'SARIMA{best_order}{best_seasonal_order} 모델 예측 (RMSE: {rmse:.4f})')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

# 전체 데이터로 모델 재적합 및 미래 예측
final_model = SARIMAX(df['y'], order=best_order, seasonal_order=best_seasonal_order).fit(disp=False)
future_steps = 24  # 2년 예측
future_index = pd.date_range(start=dates[-1] + pd.DateOffset(months=1), periods=future_steps, freq='MS')
future_forecast = final_model.forecast(steps=future_steps)
forecast_conf_int = final_model.get_forecast(steps=future_steps).conf_int()

# 미래 예측 시각화
plt.figure(figsize=(12, 6))
plt.plot(df['y'], label='관측 데이터')
plt.plot(future_index, future_forecast, 'r-', label='SARIMA 예측')
plt.fill_between(future_index, 
                 forecast_conf_int.iloc[:, 0], 
                 forecast_conf_int.iloc[:, 1], 
                 color='r', alpha=0.2, label='95% 신뢰구간')
plt.title(f'SARIMA{best_order}{best_seasonal_order} 모델 미래 예측')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

# 모델 진단
plt.figure(figsize=(12, 10))

# 1. 잔차 시계열
plt.subplot(2, 2, 1)
plt.plot(final_model.resid)
plt.axhline(y=0, color='r', linestyle='-')
plt.title('SARIMA 모델 잔차')
plt.grid(True, alpha=0.3)

# 2. 잔차 히스토그램
plt.subplot(2, 2, 2)
plt.hist(final_model.resid, bins=20, alpha=0.7, density=True, edgecolor='black')
# 정규분포 곡선 추가
x = np.linspace(final_model.resid.min(), final_model.resid.max(), 100)
plt.plot(x, stats.norm.pdf(x, final_model.resid.mean(), final_model.resid.std()), 'r-', linewidth=2)
plt.title('잔차 히스토그램')
plt.grid(True, alpha=0.3)

# 3. 잔차 ACF
plt.subplot(2, 2, 3)
plot_acf(final_model.resid, lags=36, ax=plt.gca())
plt.title('잔차 ACF')

# 4. Q-Q 플롯
plt.subplot(2, 2, 4)
sm.graphics.qqplot(final_model.resid, line='45', fit=True, ax=plt.gca())
plt.title('잔차 Q-Q 플롯')

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 계절성이 있는 소매 판매 데이터 예측
- 월별 관광객 수 예측
- 계절적 패턴이 있는 에너지 소비량 분석
- 분기별 경제 지표의 예측
- 계절성이 있는 웹 트래픽이나 앱 사용량 분석

## 9. VAR 모형 (Vector Autoregression Model)

**정의**: VAR 모형은 다변량 시계열 분석 방법으로, 여러 시계열 변수 간의 상호 의존성을 모델링합니다. 각 변수는 자신과 다른 모든 변수의 과거 값의 선형 함수로 표현됩니다.

**수식**: p차 VAR 모형 VAR(p): $\mathbf{Y}_t = \mathbf{c} + \mathbf{A}_1 \mathbf{Y}_{t-1} + \mathbf{A}_2 \mathbf{Y}_{t-2} + ... + \mathbf{A}_p \mathbf{Y}_{t-p} + \mathbf{\varepsilon}_t$

여기서:

- $\mathbf{Y}_t$: t시점의 k×1 시계열 벡터
- $\mathbf{c}$: k×1 상수항 벡터
- $\mathbf{A}_i$: k×k 계수 행렬
- $\mathbf{\varepsilon}_t$: k×1 오차항 벡터 (백색잡음)
- p: 시차(lag) 차수

**특징**:

- 다수의 시계열 변수가 서로 어떻게 영향을 주는지 분석할 수 있습니다.
- 그랜저 인과성 검정을 통해 변수 간 인과 관계를 파악할 수 있습니다.
- 충격 반응 함수(Impulse Response Function)를 통해 한 변수의 충격이 다른 변수에 미치는 영향을 분석할 수 있습니다.
- 분산 분해(Variance Decomposition)로 한 변수의 변동이 다른 변수에 기인하는 정도를 파악할 수 있습니다.
- 모든 변수는 내생적(endogenous)으로 취급됩니다.
- 정보 기준(AIC, BIC, HQ)을 통해 최적 시차를 선택합니다.
- 모든 시계열은 정상성을 만족해야 합니다.

**코드 예시**:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.api import VAR
from statsmodels.tsa.stattools import adfuller, grangercausalitytests
from statsmodels.tsa.vector_ar.irf import plot_irf
import statsmodels.api as sm

# 다변량 시계열 데이터 생성
np.random.seed(42)
n = 200
dates = pd.date_range(start='2020-01-01', periods=n, freq='D')

# 상관된 시계열 변수 생성
e1 = np.random.normal(0, 1, n)
e2 = np.random.normal(0, 1, n)
e3 = np.random.normal(0, 1, n)

y1 = np.zeros(n)
y2 = np.zeros(n)
y3 = np.zeros(n)

# y1은 자신의 지연값과 오차항의 영향을 받음
# y2는 y1과 자신의 지연값, 오차항의 영향을 받음
# y3는 y1, y2와 자신의 지연값, 오차항의 영향을 받음
for t in range(1, n):
    y1[t] = 0.6 * y1[t-1] + e1[t]
    y2[t] = 0.3 * y1[t-1] + 0.5 * y2[t-1] + e2[t]
    y3[t] = 0.4 * y1[t-1] + 0.2 * y2[t-1] + 0.3 * y3[t-1] + e3[t]

# 데이터프레임 생성
df = pd.DataFrame({
    'y1': y1,
    'y2': y2,
    'y3': y3
}, index=dates)

# 시각화: 다변량 시계열
plt.figure(figsize=(12, 8))

for i, col in enumerate(df.columns):
    plt.subplot(3, 1, i+1)
    plt.plot(df[col])
    plt.title(f'시계열 변수: {col}')
    plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 정상성 검정
for col in df.columns:
    result = adfuller(df[col])
    print(f"ADF 검정 결과 - {col}")
    print(f"ADF 통계량: {result[0]:.4f}")
    print(f"p-value: {result[1]:.4f}")
    if result[1] <= 0.05:
        print("결론: 정상 시계열 (귀무가설 기각)")
    else:
        print("결론: 비정상 시계열 (귀무가설 기각 실패)")
    print()

# 그랜저 인과성 검정
max_lag = 5
print("그랜저 인과성 검정 결과:")
for i in range(len(df.columns)):
    for j in range(len(df.columns)):
        if i != j:
            test_result = grangercausalitytests(df[[df.columns[j], df.columns[i]]], maxlag=max_lag, verbose=False)
            p_values = [round(test_result[lag+1][0]['ssr_ftest'][1], 4) for lag in range(max_lag)]
            min_p_value = min(p_values)
            min_p_lag = p_values.index(min_p_value) + 1
            print(f"{df.columns[i]} -> {df.columns[j]}: 최소 p-value {min_p_value} (시차 {min_p_lag})")
            if min_p_value <= 0.05:
                print(f"  결론: {df.columns[i]}가 {df.columns[j]}에 그랜저 인과성 있음")
            else:
                print(f"  결론: {df.columns[i]}가 {df.columns[j]}에 그랜저 인과성 없음")
    print()

# 최적 시차 선택
model = VAR(df)
lag_order_results = model.select_order(maxlags=10)
print("최적 시차 선택:")
print(lag_order_results.summary())
best_lag = lag_order_results.aic

# VAR 모델 적합
var_model = model.fit(maxlags=best_lag)
print("\nVAR 모델 요약:")
print(var_model.summary())

# 훈련/테스트 분리 및 평가
train_size = int(0.8 * len(df))
train, test = df.iloc[:train_size], df.iloc[train_size:]

# 훈련 데이터로 모델 적합
train_model = VAR(train)
train_model_fitted = train_model.fit(maxlags=best_lag)

# 예측 기간 설정
forecast_steps = len(test)
forecast_input = train.values[-best_lag:]
forecast = train_model_fitted.forecast(y=forecast_input, steps=forecast_steps)

# 예측 결과를 데이터프레임으로 변환
forecast_df = pd.DataFrame(forecast, index=test.index, columns=test.columns)

# RMSE 계산
mse = ((test - forecast_df) ** 2).mean()
rmse = np.sqrt(mse)
print("\n각 변수별 RMSE:")
for col in test.columns:
    print(f"{col}: {rmse[col]:.4f}")

# 예측 시각화
plt.figure(figsize=(15, 12))

for i, col in enumerate(df.columns):
    plt.subplot(3, 1, i+1)
    plt.plot(train.index, train[col], label='훈련 데이터')
    plt.plot(test.index, test[col], label='테스트 데이터')
    plt.plot(test.index, forecast_df[col], 'r--', label='VAR 예측')
    plt.title(f'{col} VAR 모델 예측 (RMSE: {rmse[col]:.4f})')
    plt.legend()
    plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 전체 데이터로 모델 재적합 및 미래 예측
final_model = VAR(df)
final_model_fitted = final_model.fit(maxlags=best_lag)

# 미래 예측 기간
future_steps = 30
forecast_input = df.values[-best_lag:]
future_forecast = final_model_fitted.forecast(y=forecast_input, steps=future_steps)

# 예측 결과를 데이터프레임으로 변환
future_index = pd.date_range(start=dates[-1] + pd.Timedelta(days=1), periods=future_steps, freq='D')
future_forecast_df = pd.DataFrame(future_forecast, index=future_index, columns=df.columns)

# 미래 예측 시각화
plt.figure(figsize=(15, 12))

for i, col in enumerate(df.columns):
    plt.subplot(3, 1, i+1)
    plt.plot(df.index, df[col], label='관측 데이터')
    plt.plot(future_index, future_forecast_df[col], 'r-', label='VAR 예측')
    plt.title(f'{col} VAR 모델 미래 예측')
    plt.legend()
    plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# 충격 반응 함수(IRF) 분석
irf = final_model_fitted.irf(10)  # 10기간 IRF
plot_irf(irf, impulse=None, response=None)
plt.suptitle('충격 반응 함수(IRF) 분석', fontsize=16)
plt.tight_layout()
plt.subplots_adjust(top=0.9)
plt.show()

# 예측 오차 분산 분해(FEVD)
fevd = final_model_fitted.fevd(10)  # 10기간 FEVD

# FEVD 시각화
plt.figure(figsize=(15, 12))

for i, col in enumerate(df.columns):
    plt.subplot(3, 1, i+1)
    fevd.plot(impulse=None, response=col, ax=plt.gca())
    plt.title(f'{col}의 예측 오차 분산 분해')
    plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 거시경제 변수 간의 상호작용 분석(GDP, 실업률, 인플레이션 등)
- 금융 시장 간의 상호연관성 분석(주가, 금리, 환율)
- 마케팅 채널별 매출 영향 분석
- 온라인 플랫폼에서 다양한 활동 지표 간의 관계 파악
- 여러 제품 카테고리 간의 판매량 상호 의존성 분석