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