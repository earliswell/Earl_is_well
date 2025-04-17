---
title: 03. Etc.
draft: false
tags:
  - example-tag
---
# 데이터 전처리 파트 정리

## 판다스 (Pandas)

**정의**: 데이터 조작과 분석을 위한 Python 라이브러리로, 구조화된 데이터를 효율적으로 처리할 수 있는 데이터프레임(DataFrame)과 시리즈(Series) 자료구조를 제공합니다.

**수식**: 해당 없음

**특징**:

- 효율적인 데이터 조작을 위한 다양한 함수 제공
- SQL과 유사한 방식으로 데이터 연산 가능
- 결측값 처리, 그룹화, 피벗, 병합 등 데이터 처리 기능
- NumPy 기반으로 빠른 연산 속도

**코드 예시**:

```python
import pandas as pd
import numpy as np

# 데이터프레임 생성
df = pd.DataFrame({
    'A': np.random.randn(5),
    'B': ['foo', 'bar', 'foo', 'bar', 'foo'],
    'C': pd.date_range('20230101', periods=5)
})

# 기본 연산
print(df.head())  # 상위 5개 행 보기
print(df.describe())  # 수치형 열의 통계량
print(df.groupby('B').mean())  # 그룹별 평균

# 결측치 처리
df.fillna(0)  # 결측치를 0으로 채우기
df.dropna()  # 결측치가 있는 행 제거

# 데이터 연결
df2 = pd.DataFrame({'A': [1, 2], 'B': ['foo', 'bar']})
pd.concat([df, df2])  # 수직 연결
df.merge(df2, on='B')  # SQL 조인처럼 합치기
```

**개념의 활용**: 정형 데이터를 다루는 거의 모든 데이터 분석 프로젝트에서 필수적으로 사용됩니다. 데이터 로딩, 전처리, 변환, 탐색적 분석 과정에서 활용되며, 특히 CSV, Excel 등 다양한 포맷의 데이터를 불러오고 처리할 때 기본으로 사용합니다.

## EDA (탐색적 데이터 분석)

**정의**: 수집된 데이터를 다양한 각도에서 관찰하고 분석하여 데이터의 특성, 패턴, 이상치 등을 파악하는 과정입니다.

**수식**: 해당 없음

**특징**:

- 데이터 요약 통계량 확인 (평균, 중앙값, 표준편차 등)
- 데이터 분포 파악 (히스토그램, 박스플롯 등)
- 변수 간 상관관계 분석
- 데이터 품질 확인 (결측치, 이상치 등)

**코드 예시**:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 데이터 로드 (예: 타이타닉 데이터셋)
df = pd.read_csv('titanic.csv')

# 기본 정보 확인
print(df.info())  # 데이터 타입, 결측치 확인
print(df.describe())  # 수치형 데이터 요약 통계

# 시각화
plt.figure(figsize=(10, 6))
sns.histplot(df['Age'].dropna())
plt.title('Age Distribution')
plt.show()

# 상관관계 분석
numeric_cols = df.select_dtypes(include=['float64', 'int64']).columns
corr_matrix = df[numeric_cols].corr()
plt.figure(figsize=(10, 8))
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm')
plt.title('Correlation Matrix')
plt.show()

# 그룹별 분석
survival_by_class = df.groupby('Pclass')['Survived'].mean()
print(survival_by_class)
```

**개념의 활용**: 새로운 데이터셋을 받았을 때 가장 먼저 수행해야 하는 과정으로, 데이터의 전반적인 특성을 이해하고 추후 분석 방향을 설정하는 데 활용합니다. 모델링 전에 데이터 특성을 파악하여 적절한 전처리 방법과 모델을 선택하는 기준이 됩니다.

## 이상치 처리

**정의**: 데이터의 전반적인 패턴에서 크게 벗어난 관측치를 식별하고 처리하는 과정입니다.

**수식**:

- Z-score 방식: $Z = \frac{X - \mu}{\sigma}$ (|Z| > 3인 경우 이상치로 간주)
- IQR 방식: $이상치 < Q1 - 1.5 \times IQR$ 또는 $이상치 > Q3 + 1.5 \times IQR$
- 여기서 IQR = Q3 - Q1 (제3사분위수 - 제1사분위수)

**특징**:

- 통계적 방법(Z-score, IQR 등)과 거리 기반 방법(DBSCAN, Isolation Forest 등)으로 구분
- 이상치 제거, 대체, 별도 처리 등 다양한 방식 존재
- 도메인 지식에 따라 이상치의 기준이 달라질 수 있음

**코드 예시**:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats
from sklearn.ensemble import IsolationForest

# 데이터 준비
df = pd.DataFrame({'value': [1, 2, 2, 3, 3, 4, 5, 5, 6, 100]})

# 방법 1: Z-score 이용
z_scores = stats.zscore(df['value'])
outliers_z = df[abs(z_scores) > 3]
print("Z-score 방식으로 찾은 이상치:", outliers_z)

# 방법 2: IQR 이용
Q1 = df['value'].quantile(0.25)
Q3 = df['value'].quantile(0.75)
IQR = Q3 - Q1
outliers_iqr = df[(df['value'] < (Q1 - 1.5 * IQR)) | (df['value'] > (Q3 + 1.5 * IQR))]
print("IQR 방식으로 찾은 이상치:", outliers_iqr)

# 방법 3: Isolation Forest 이용
model = IsolationForest(contamination=0.1, random_state=42)
model.fit(df[['value']])
df['anomaly'] = model.predict(df[['value']])
outliers_if = df[df['anomaly'] == -1]
print("Isolation Forest 방식으로 찾은 이상치:", outliers_if)

# 이상치 처리 방법
# 1. 제거
df_removed = df[abs(z_scores) <= 3]

# 2. 대체 (중앙값으로)
df['value_fixed'] = df['value'].copy()
df.loc[abs(z_scores) > 3, 'value_fixed'] = df['value'].median()

# 3. 경계값으로 대체 (Capping)
upper_bound = Q3 + 1.5 * IQR
df['value_capped'] = df['value'].copy()
df.loc[df['value'] > upper_bound, 'value_capped'] = upper_bound
```

**개념의 활용**: 회귀 분석, 통계 검정 등 이상치에 민감한 모델을 사용할 때 반드시 필요합니다. 예측 모델의 정확도를 높이고 왜곡된 결과를 방지하기 위해 활용되며, 특히 재무 데이터, 센서 데이터 등에서 자주 나타나는 문제를 해결하는 데 중요합니다.

## 결측치 처리

**정의**: 데이터셋에 값이 누락된 부분을 식별하고, 분석 목적에 맞게 처리하는 과정입니다.

**수식**: 해당 없음

**특징**:

- 결측 메커니즘: MCAR(완전 무작위 결측), MAR(무작위 결측), MNAR(비무작위 결측)
- 처리 방법: 제거(Deletion), 대체(Imputation), 예측 모델 기반 대체
- 결측 패턴 분석을 통해 적절한 처리 방법 선택

**코드 예시**:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.impute import SimpleImputer, KNNImputer
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

# 결측치가 있는 데이터 생성
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4, 5],
    'B': [np.nan, 2, 3, 4, 5],
    'C': [1, 2, 3, np.nan, 5]
})

# 결측치 확인
print("결측치 개수:\n", df.isnull().sum())
print("결측치 비율:\n", df.isnull().mean())

# 결측치 패턴 시각화
plt.figure(figsize=(10, 5))
sns.heatmap(df.isnull(), cbar=False, cmap='viridis')
plt.title('Missing Value Pattern')
plt.show()

# 처리 방법 1: 제거
df_dropped_rows = df.dropna()  # 결측치가 있는 행 제거
df_dropped_cols = df.dropna(axis=1)  # 결측치가 있는 열 제거

# 처리 방법 2: 단순 대체
df_mean = df.fillna(df.mean())  # 평균으로 대체
df_median = df.fillna(df.median())  # 중앙값으로 대체
df_ffill = df.fillna(method='ffill')  # 이전 값으로 대체
df_bfill = df.fillna(method='bfill')  # 이후 값으로 대체

# 처리 방법 3: scikit-learn 임퓨터 사용
# 평균 대체
imputer_mean = SimpleImputer(strategy='mean')
df_imputed_mean = pd.DataFrame(
    imputer_mean.fit_transform(df),
    columns=df.columns
)

# KNN 대체
imputer_knn = KNNImputer(n_neighbors=2)
df_imputed_knn = pd.DataFrame(
    imputer_knn.fit_transform(df),
    columns=df.columns
)

# MICE (Multiple Imputation by Chained Equations)
imputer_mice = IterativeImputer(random_state=42)
df_imputed_mice = pd.DataFrame(
    imputer_mice.fit_transform(df),
    columns=df.columns
)
```

**개념의 활용**: 거의 모든 실제 데이터셋에는 결측치가 존재하므로, 데이터 분석의 필수 단계입니다. 특히 의료 데이터, 설문조사 데이터 등에서 자주 발생하며, 모델의 정확도와 신뢰성에 직접적인 영향을 미치므로 적절한 처리가 중요합니다.

## 클래스불균형

**정의**: 분류 문제에서 특정 클래스의 데이터가 다른 클래스에 비해 현저히 적거나 많은 상태를 말합니다.

**수식**: 불균형 비율(Imbalance Ratio) = 다수 클래스 샘플 수 / 소수 클래스 샘플 수

**특징**:

- 모델이 다수 클래스에 편향되어 소수 클래스 예측 성능이 저하됨
- 정확도(Accuracy)보다 F1 점수, AUC 등 다른 평가 지표가 중요해짐
- 해결 방법: 리샘플링, 비용 민감 학습, 앙상블 기법 등

**코드 예시**:

```python
import pandas as pd
import numpy as np
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report
from imblearn.over_sampling import SMOTE, RandomOverSampler
from imblearn.under_sampling import RandomUnderSampler
from imblearn.combine import SMOTEENN, SMOTETomek

# 불균형 데이터 생성
X, y = make_classification(
    n_samples=1000, n_classes=2, weights=[0.9, 0.1],
    n_features=20, random_state=42
)

print("클래스 분포:", np.bincount(y))

# 데이터 분할
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42, stratify=y
)

# 방법 1: 오버샘플링 (SMOTE)
smote = SMOTE(random_state=42)
X_train_smote, y_train_smote = smote.fit_resample(X_train, y_train)
print("SMOTE 적용 후 클래스 분포:", np.bincount(y_train_smote))

# 방법 2: 언더샘플링
under_sampler = RandomUnderSampler(random_state=42)
X_train_under, y_train_under = under_sampler.fit_resample(X_train, y_train)
print("언더샘플링 적용 후 클래스 분포:", np.bincount(y_train_under))

# 방법 3: 결합 방법 (SMOTE + ENN)
smote_enn = SMOTEENN(random_state=42)
X_train_combined, y_train_combined = smote_enn.fit_resample(X_train, y_train)
print("SMOTE+ENN 적용 후 클래스 분포:", np.bincount(y_train_combined))

# 모델 훈련 및 평가 (원본 데이터)
clf_original = RandomForestClassifier(random_state=42)
clf_original.fit(X_train, y_train)
y_pred_original = clf_original.predict(X_test)
print("원본 데이터 결과:\n", classification_report(y_test, y_pred_original))

# SMOTE 적용 모델
clf_smote = RandomForestClassifier(random_state=42)
clf_smote.fit(X_train_smote, y_train_smote)
y_pred_smote = clf_smote.predict(X_test)
print("SMOTE 적용 결과:\n", classification_report(y_test, y_pred_smote))
```

**개념의 활용**: 사기 탐지, 질병 진단, 이상 징후 감지 등 희귀한 이벤트를 예측하는 분류 문제에서 필수적으로 고려해야 합니다. 특히 소수 클래스의 정확한 예측이 중요한 경우(예: 암 진단), 클래스 불균형 처리가 매우 중요합니다.

## 스케일링

**정의**: 서로 다른 범위를 가진 특성(feature)들의 스케일을 조정하여 모델이 모든 특성을 공정하게 고려할 수 있도록 하는 과정입니다.

**수식**:

- 표준화(Standard Scaling): $z = \frac{x - \mu}{\sigma}$
- 정규화(Min-Max Scaling): $x_{norm} = \frac{x - x_{min}}{x_{max} - x_{min}}$
- 로버스트 스케일링: $x_{robust} = \frac{x - median(x)}{IQR}$

**특징**:

- 거리 기반 알고리즘(KNN, K-means 등)에서 특히 중요
- 경사 하강법 기반 알고리즘의 수렴 속도 향상
- 이상치에 민감한 방법(Min-Max)과 강건한 방법(Robust) 구분
- 트리 기반 모델은 스케일링의 영향을 적게 받음

**코드 예시**:

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler, MinMaxScaler, RobustScaler
from sklearn.preprocessing import MaxAbsScaler, Normalizer
import matplotlib.pyplot as plt

# 예제 데이터 생성
data = {
    'feature1': [100, 200, 300, 400, 500, 5000],  # 이상치 포함
    'feature2': [1, 2, 3, 4, 5, 6]
}
df = pd.DataFrame(data)

# 다양한 스케일러 적용
# 1. 표준화 (StandardScaler)
scaler_standard = StandardScaler()
df_standard = pd.DataFrame(
    scaler_standard.fit_transform(df),
    columns=df.columns
)

# 2. Min-Max 스케일링
scaler_minmax = MinMaxScaler()
df_minmax = pd.DataFrame(
    scaler_minmax.fit_transform(df),
    columns=df.columns
)

# 3. 로버스트 스케일링 (이상치에 강건)
scaler_robust = RobustScaler()
df_robust = pd.DataFrame(
    scaler_robust.fit_transform(df),
    columns=df.columns
)

# 4. MaxAbs 스케일링 (절대값 기준 스케일링)
scaler_maxabs = MaxAbsScaler()
df_maxabs = pd.DataFrame(
    scaler_maxabs.fit_transform(df),
    columns=df.columns
)

# 5. 정규화 (행 단위 스케일링, L2 norm)
scaler_norm = Normalizer()
df_norm = pd.DataFrame(
    scaler_norm.fit_transform(df),
    columns=df.columns
)

# 결과 비교
print("원본 데이터:\n", df)
print("StandardScaler:\n", df_standard)
print("MinMaxScaler:\n", df_minmax)
print("RobustScaler:\n", df_robust)
print("MaxAbsScaler:\n", df_maxabs)
print("Normalizer:\n", df_norm)

# 시각화
plt.figure(figsize=(15, 10))
scalers = ['Original', 'Standard', 'MinMax', 'Robust', 'MaxAbs', 'Normalizer']
dfs = [df, df_standard, df_minmax, df_robust, df_maxabs, df_norm]

for i, (scaler_name, scaled_df) in enumerate(zip(scalers, dfs)):
    plt.subplot(2, 3, i+1)
    plt.scatter(scaled_df['feature1'], scaled_df['feature2'])
    plt.title(scaler_name)
    plt.grid(True)

plt.tight_layout()
plt.show()
```

**개념의 활용**: 다양한 단위와 범위를 가진 특성을 사용하는 모델에서 필수적입니다. 특히 신경망, SVM, KNN, PCA와 같은 알고리즘에서 중요하며, 특성 간 중요도 비교가 필요한 경우에도 활용됩니다. 이상치가 많은 데이터에서는 RobustScaler를 고려해야 합니다.

## 범주형 변수 인코딩

**정의**: 머신러닝 알고리즘이 처리할 수 있도록 범주형(categorical) 데이터를 수치형으로 변환하는 과정입니다.

**수식**: 해당 없음

**특징**:

- 명목형(nominal)과 순서형(ordinal) 변수에 대한 처리 방법이 다름
- 원-핫 인코딩은 차원 증가 문제 발생 가능
- 레이블 인코딩은 순서 관계가 없는 데이터에 부적절할 수 있음
- 타겟 인코딩은 과적합 위험이 있어 교차 검증 필요

**코드 예시**:

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import LabelEncoder, OneHotEncoder, OrdinalEncoder
from category_encoders import TargetEncoder, BinaryEncoder, HashingEncoder

# 예제 데이터 생성
df = pd.DataFrame({
    'color': ['red', 'blue', 'green', 'red', 'green'],
    'size': ['small', 'medium', 'large', 'medium', 'small'],
    'material': ['wood', 'metal', 'plastic', 'wood', 'metal'],
    'price': [10, 20, 15, 12, 18]
})

# 1. 레이블 인코딩 (Label Encoding)
le = LabelEncoder()
df['color_label'] = le.fit_transform(df['color'])
print("레이블 인코딩 결과:\n", df[['color', 'color_label']])
print("매핑:", dict(zip(le.classes_, le.transform(le.classes_))))

# 2. 원-핫 인코딩 (One-Hot Encoding)
# 방법 1: pandas get_dummies
df_onehot_pd = pd.get_dummies(df['color'], prefix='color')
print("\n원-핫 인코딩 (pandas):\n", df_onehot_pd)

# 방법 2: scikit-learn OneHotEncoder
ohe = OneHotEncoder(sparse=False)
df_onehot_sk = pd.DataFrame(
    ohe.fit_transform(df[['color']]),
    columns=ohe.get_feature_names_out(['color'])
)
print("\n원-핫 인코딩 (scikit-learn):\n", df_onehot_sk)

# 3. 순서형 인코딩 (Ordinal Encoding)
size_order = ['small', 'medium', 'large']  # 크기 순서 정의
oe = OrdinalEncoder(categories=[size_order])
df['size_ordinal'] = oe.fit_transform(df[['size']])
print("\n순서형 인코딩:\n", df[['size', 'size_ordinal']])

# 4. 타겟 인코딩 (Target Encoding)
te = TargetEncoder()
df['color_target'] = te.fit_transform(df['color'], df['price'])
print("\n타겟 인코딩:\n", df[['color', 'color_target']])

# 5. 바이너리 인코딩 (Binary Encoding)
be = BinaryEncoder()
df_binary = be.fit_transform(df['material'])
print("\n바이너리 인코딩:\n", pd.concat([df['material'], df_binary], axis=1))

# 6. 해싱 인코딩 (Hashing Encoding)
he = HashingEncoder(n_components=2)
df_hash = he.fit_transform(df['material'])
print("\n해싱 인코딩:\n", pd.concat([df['material'], df_hash], axis=1))
```

**개념의 활용**: 머신러닝 모델은 대부분 수치 데이터만 처리할 수 있으므로, 범주형 변수가 있는 거의 모든 데이터셋에서 필요합니다. 특히 많은 고유 값을 가진 범주형 변수를 처리할 때 적절한 인코딩 방식 선택이 모델 성능에 큰 영향을 미칩니다. 트리 기반 모델은 레이블 인코딩으로 충분하지만, 선형 모델이나 신경망은 원-핫 인코딩이 더 적합합니다.

## 날짜데이터

**정의**: 시간 정보를 포함하는 데이터로, 특별한 처리와 특성 추출이 필요한 데이터 유형입니다.

**수식**: 해당 없음

**특징**:

- 다양한 포맷으로 저장되어 있어 표준화 필요
- 연, 월, 일, 시간, 분, 초 등 여러 컴포넌트로 분해 가능
- 계절성, 주기성, 트렌드 등 시간 관련 패턴 분석에 중요
- 시차(time lag), 윈도잉(windowing) 등 특수한 처리 필요

**코드 예시**:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

# 날짜 데이터 생성
date_rng = pd.date_range(start='2023-01-01', end='2023-12-31', freq='D')
df = pd.DataFrame(date_rng, columns=['date'])
df['value'] = np.random.randn(len(date_rng)) * 10 + 100  # 임의의 값

# 1. 날짜/시간 파싱
# 문자열을 날짜로 변환
date_strings = ['2023-01-01', '2023/02/15', 'Mar 30, 2023', '20230415']
parsed_dates = [
    pd.to_datetime('2023-01-01'),
    pd.to_datetime('2023/02/15'),
    pd.to_datetime('Mar 30, 2023'),
    pd.to_datetime('20230415', format='%Y%m%d')
]
print("파싱된 날짜:", parsed_dates)

# 2. 날짜 컴포넌트 추출
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day'] = df['date'].dt.day
df['dayofweek'] = df['date'].dt.dayofweek  # 0=월요일, 6=일요일
df['quarter'] = df['date'].dt.quarter
df['is_weekend'] = df['date'].dt.dayofweek >= 5  # 주말 여부
df['dayofyear'] = df['date'].dt.dayofyear
df['weekofyear'] = df['date'].dt.isocalendar().week

# 사용자 정의 계절 (북반구 기준)
def get_season(month):
    if month in [12, 1, 2]:
        return 'Winter'
    elif month in [3, 4, 5]:
        return 'Spring'
    elif month in [6, 7, 8]:
        return 'Summer'
    else:
        return 'Fall'

df['season'] = df['month'].apply(get_season)

print("\n날짜 컴포넌트 추출 결과 (일부):\n", df.head())

# 3. 시간 간격 계산
df['days_since_start'] = (df['date'] - df['date'].min()).dt.days
df['days_until_end'] = (df['date'].max() - df['date']).dt.days

# 4. 시차 변수 생성 (Time Lag)
for lag in [1, 7, 30]:  # 1일, 1주, 1개월 전
    df[f'value_lag_{lag}'] = df['value'].shift(lag)

# 5. 롤링 윈도우 특성 (Rolling Window Features)
df['rolling_mean_7d'] = df['value'].rolling(window=7).mean()
df['rolling_std_7d'] = df['value'].rolling(window=7).std()

# 결측치 제거 (시차/롤링으로 인한 NaN)
df_clean = df.dropna()
print("\n시차 및 롤링 윈도우 특성 (일부):\n", df_clean.head())

# 6. 날짜 데이터 시각화
plt.figure(figsize=(12, 6))
plt.plot(df['date'], df['value'])
plt.title('Time Series Data')
plt.xlabel('Date')
plt.ylabel('Value')
plt.grid(True)
plt.show()

# 7. 주별/월별 집계
monthly_avg = df.groupby(df['date'].dt.month)['value'].mean()
weekly_avg = df.groupby(df['date'].dt.isocalendar().week)['value'].mean()

plt.figure(figsize=(10, 5))
monthly_avg.plot(kind='bar')
plt.title('Monthly Average')
plt.show()
```

**개념의 활용**: 시계열 데이터 분석, 판매 예측, 주가 분석, 계절성 탐지 등 시간 관련 패턴이 중요한 모든 분석에서 필수적입니다. 특히 날짜/시간 컴포넌트를 추출하여 특성으로 활용함으로써 모델의 예측 성능을 크게 향상시킬 수 있습니다.

# 기타기출 파트 정리

## 관리도

**정의**: 공정의 통계적 관리 및 모니터링을 위해 시간 경과에 따른 공정 특성값의 변동을 그래프로 나타낸 도구입니다.

**수식**:

- 관리 상한선(UCL) = $\mu + k\sigma$
- 관리 하한선(LCL) = $\mu - k\sigma$
- 여기서 $\mu$는 평균, $\sigma$는 표준편차, $k$는 보통 3을 사용

**특징**:

- 공정의 안정성 모니터링 및 이상 원인 감지
- 관리 한계선 내의 변동은 우연 원인, 벗어난 변동은 이상 원인으로 판단
- 변수 관리도(X-bar, R, S 등)와 계수 관리도(p, np, c, u 등)로 구분
- Run 검정, 특정 패턴(연속 상승/하락, 관리선 근처 연속 등) 분석으로 이상 감지

**코드 예시**:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
import statsmodels.api as sm

# 공정 데이터 생성 (정상 공정 + 이상 발생)
np.random.seed(42)
normal_process = np.random.normal(100, 5, 40)  # 평균 100, 표준편차 5인 정상 공정
abnormal_process = np.random.normal(110, 7, 10)  # 평균 110, 표준편차 7인 이상 공정
process_data = np.concatenate([normal_process, abnormal_process])

# 데이터프레임 생성
df = pd.DataFrame({
    'sample_no': range(1, len(process_data) + 1),
    'measurement': process_data
})

# X-bar 관리도 계산
mean = normal_process.mean()  # 정상 공정 데이터로 중심선 계산
std = normal_process.std()
UCL = mean + 3 * std  # 3-시그마 상한선
LCL = mean - 3 * std  # 3-시그마 하한선

# 관리도 그리기
plt.figure(figsize=(12, 6))
plt.plot(df['sample_no'], df['measurement'], marker='o', linestyle='-', color='blue')
plt.axhline(y=mean, color='green', linestyle='-', label='Center Line')
plt.axhline(y=UCL, color='red', linestyle='--', label='UCL')
plt.axhline(y=LCL, color='red', linestyle='--', label='LCL')

# 이상점 표시
out_of_control = df[
    (df['measurement'] > UCL) | (df['measurement'] < LCL)
]
plt.scatter(
    out_of_control['sample_no'], 
    out_of_control['measurement'], 
    color='red', s=100, marker='x', label='Out of Control'
)

plt.title('X-bar Control Chart')
plt.xlabel('Sample Number')
plt.ylabel('Measurement')
plt.legend()
plt.grid(True)
plt.show()

# Run 검정 (Western Electric Rules의 일부)
# Rule 1: 1점이 관리한계선을 벗어남
rule1 = df[(df['measurement'] > UCL) | (df['measurement'] < LCL)]
print("Rule 1 위반 (관리한계선 이탈):", rule1['sample_no'].tolist())

# Rule 2: 9점 연속 중심선 한쪽에 위치
rule2_violations = []
for i in range(9, len(df) + 1):
    window = df['measurement'].iloc[i-9:i]
    if all(value > mean for value in window) or all(value < mean for value in window):
        rule2_violations.append(df['sample_no'].iloc[i-1])
print("Rule 2 위반 (9점 연속 한쪽):", rule2_violations)

# Rule 3: 6점 연속 상승 또는 하락
rule3_violations = []
for i in range(6, len(df) + 1):
    window = df['measurement'].iloc[i-6:i]
    if all(window.iloc[j] < window.iloc[j+1] for j in range(5)) or \
       all(window.iloc[j] > window.iloc[j+1] for j in range(5)):
        rule3_violations.append(df['sample_no'].iloc[i-1])
print("Rule 3 위반 (6점 연속 상승/하락):", rule3_violations)
```

**개념의 활용**: 제조업, 품질 관리, 공정 모니터링 등에서 필수적으로 사용됩니다. 특히 반도체, 자동차, 제약 등 품질이 중요한 산업에서 공정의 안정성을 확보하고 품질 문제를 사전에 탐지하는 데 활용됩니다. 공정 개선과 Six Sigma 등 품질 향상 활동에서도 중요한 도구입니다.

## 생존분석

**정의**: 특정 사건(event)이 발생할 때까지 걸리는 시간을 분석하는 통계적 방법으로, 중도절단(censoring)된 데이터를 다룰 수 있습니다.

**수식**:

- 생존 함수: $S(t) = P(T > t)$ (시간 t 이후까지 생존할 확률)
- 위험 함수(Hazard function): $h(t) = \lim_{\Delta t \to 0} \frac{P(t \leq T < t + \Delta t | T \geq t)}{\Delta t}$ (시간 t까지 생존했을 때, t 시점에서의 즉각적인 사건 발생 위험)
- Kaplan-Meier 추정량: $\hat{S}(t) = \prod_{j:t_j \leq t} (1 - \frac{d_j}{n_j})$ (여기서 $d_j$는 시간 $t_j$에서의 사건 발생 수, $n_j$는 위험 집합)

**특징**:

- 중도절단(censoring) 데이터를 적절히 처리할 수 있음
- Kaplan-Meier 곡선으로 생존 확률을 시각화
- Log-rank 검정으로 집단 간 생존 함수 차이 분석
- Cox 비례 위험 모형으로 다양한 변수의 영향 분석

**코드 예시**:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from lifelines import KaplanMeierFitter, CoxPHFitter
from lifelines.statistics import logrank_test

# 생존 데이터 생성 (예: 환자 치료 후 재발까지 시간)
np.random.seed(42)
n_patients = 100
group = np.random.choice(['Treatment', 'Control'], size=n_patients, p=[0.5, 0.5])
# 치료 그룹은 평균적으로 더 긴 재발 시간을 가짐
times = np.where(
    group == 'Treatment', 
    np.random.exponential(50, n_patients), 
    np.random.exponential(30, n_patients)
)
# 관찰 기간 제한으로 인한 중도절단
censoring_times = np.random.exponential(40, n_patients)
observed = times <= censoring_times
times_observed = np.minimum(times, censoring_times)

# 데이터프레임 생성
df = pd.DataFrame({
    'time': times_observed,
    'event': observed.astype(int),  # 1=사건 발생, 0=중도절단
    'group': group,
    'age': np.random.normal(65, 10, n_patients),
    'sex': np.random.choice(['Male', 'Female'], n_patients)
})

# 1. Kaplan-Meier 생존 곡선
kmf = KaplanMeierFitter()
kmf.fit(df['time'], df['event'], label='Overall')

# 생존 곡선 그리기
plt.figure(figsize=(12, 6))
kmf.plot()
plt.title('Kaplan-Meier Survival Curve')
plt.xlabel('Time')
plt.ylabel('Survival Probability')
plt.grid(True)
plt.show()

# 2. 그룹별 생존 곡선 비교
plt.figure(figsize=(12, 6))
for group_name in df['group'].unique():
    mask = df['group'] == group_name
    kmf = KaplanMeierFitter()
    kmf.fit(df.loc[mask, 'time'], df.loc[mask, 'event'], label=group_name)
    kmf.plot()

plt.title('Kaplan-Meier Survival Curves by Group')
plt.xlabel('Time')
plt.ylabel('Survival Probability')
plt.grid(True)
plt.legend()
plt.show()

# 3. Log-rank 검정으로 그룹 간 생존 함수 차이 분석
treatment = df[df['group'] == 'Treatment']
control = df[df['group'] == 'Control']
results = logrank_test(
    treatment['time'], control['time'],
    treatment['event'], control['event']
)
print("Log-rank 검정 결과:")
print(f"p-value: {results.p_value:.4f}")
print(f"검정 통계량: {results.test_statistic:.4f}")

# 4. Cox 비례 위험 모형
cph = CoxPHFitter()
df['sex_encoded'] = df['sex'].map({'Male': 0, 'Female': 1})
df['group_encoded'] = df['group'].map({'Control': 0, 'Treatment': 1})
cph.fit(
    df[['time', 'event', 'age', 'sex_encoded', 'group_encoded']], 
    duration_col='time', 
    event_col='event'
)
print("\nCox 비례 위험 모형 결과:")
print(cph.summary)

# 모형 시각화
plt.figure(figsize=(10, 6))
cph.plot()
plt.tight_layout()
plt.show()
```

**개념의 활용**: 의학 연구(생존 시간, 재발 시간), 고객 이탈 분석, 기기 고장 시간 분석, 채무 불이행 시간 분석 등 시간에 따른 사건 발생을 분석하는 경우에 활용됩니다. 특히 관찰 기간 중에 사건이 발생하지 않은 중도절단 데이터를 포함하는 연구에서 필수적입니다.

## 수송계획법

**정의**: 복수의 공급지(source)에서 복수의 수요지(destination)로 상품을 최소 비용으로 배송하는 최적 운송 계획을 수립하는 방법입니다.

**수식**:

- 목적함수: Minimize $Z = \sum_{i=1}^{m} \sum_{j=1}^{n} c_{ij} x_{ij}$
- 제약조건:
    - $\sum_{j=1}^{n} x_{ij} = a_i$ (공급지 i의 공급량 제약)
    - $\sum_{i=1}^{m} x_{ij} = b_j$ (수요지 j의 수요량 제약)
    - $x_{ij} \geq 0$ (음수 운송량 불가)
- 여기서 $c_{ij}$는 운송 단위당 비용, $x_{ij}$는 공급지 i에서 수요지 j로의 운송량, $a_i$는 공급지 i의 공급량, $b_j$는 수요지 j의 수요량

**특징**:

- 선형계획법의 특수한 형태로, 빠른 해법 존재
- 균형 문제(총 공급량 = 총 수요량)와 불균형 문제(≠)로 구분
- 초기 해법: 북서쪽 모서리법, 최소 비용법, Vogel 근사법
- 최적해 판정: MODI(Modified Distribution) 방법

**코드 예시**:

```python
import numpy as np
import pandas as pd
from scipy.optimize import linprog
import pulp as pl

# 예제: 3개 공장에서 4개 창고로의 운송 계획
# 공급량
supply = np.array([100, 150, 200])  # 각 공장의 공급 능력
# 수요량
demand = np.array([80, 120, 150, 100])  # 각 창고의 수요
# 단위 운송 비용
cost = np.array([
    [4, 5, 6, 8],  # 공장1 -> 각 창고 비용
    [5, 4, 3, 5],  # 공장2 -> 각 창고 비용
    [6, 7, 5, 4]   # 공장3 -> 각 창고 비용
])

# 검증: 균형 문제인지 확인
total_supply = supply.sum()
total_demand = demand.sum()
print(f"총 공급: {total_supply}, 총 수요: {total_demand}")

if total_supply > total_demand:
    # 더미 수요지 추가
    print("총 공급 > 총 수요: 더미 수요지 추가")
    dummy_demand = total_supply - total_demand
    demand = np.append(demand, dummy_demand)
    # 더미 수요지로의 운송 비용은 0
    cost = np.hstack((cost, np.zeros((len(supply), 1))))
elif total_supply < total_demand:
    # 더미 공급지 추가
    print("총 공급 < 총 수요: 더미 공급지 추가")
    dummy_supply = total_demand - total_supply
    supply = np.append(supply, dummy_supply)
    # 더미 공급지에서의 운송 비용은 0
    cost = np.vstack((cost, np.zeros((1, len(demand)))))
else:
    print("균형 문제: 총 공급 = 총 수요")

# 방법 1: SciPy의 linprog 사용
# 목적함수 계수 (비용 행렬을 1차원으로 펼침)
c = cost.flatten()

# 등식 제약조건 행렬 구성
# 1. 각 공급지의 공급량 제약
A_eq_supply = np.zeros((len(supply), len(supply) * len(demand)))
for i in range(len(supply)):
    for j in range(len(demand)):
        A_eq_supply[i, i * len(demand) + j] = 1

# 2. 각 수요지의 수요량 제약
A_eq_demand = np.zeros((len(demand), len(supply) * len(demand)))
for j in range(len(demand)):
    for i in range(len(supply)):
        A_eq_demand[j, i * len(demand) + j] = 1

# 모든 등식 제약조건 합치기
A_eq = np.vstack((A_eq_supply, A_eq_demand))
b_eq = np.concatenate((supply, demand))

# 음수 운송량 불가 제약
bounds = [(0, None) for _ in range(len(supply) * len(demand))]

# 최적화 해결
result = linprog(c, A_eq=A_eq, b_eq=b_eq, bounds=bounds, method='simplex')

# 결과 해석
print("\nSciPy linprog 결과:")
print(f"최적 목적함수 값 (총 운송 비용): {result.fun}")
print("최적 운송량:")
x_opt = result.x.reshape(len(supply), len(demand))
for i in range(len(supply)):
    for j in range(len(demand)):
        if x_opt[i, j] > 1e-5:  # 수치 오차 고려
            print(f"공급지 {i+1} -> 수요지 {j+1}: {x_opt[i, j]:.1f}")

# 방법 2: PuLP 사용 (좀 더 읽기 쉬운 코드)
problem = pl.LpProblem("Transportation_Problem", pl.LpMinimize)

# 결정 변수 정의
x = {}
for i in range(len(supply)):
    for j in range(len(demand)):
        x[i, j] = pl.LpVariable(f"x_{i}_{j}", lowBound=0)  # 음수 운송량 불가

# 목적 함수 정의
problem += pl.lpSum(cost[i][j] * x[i, j] for i in range(len(supply)) for j in range(len(demand)))

# 제약 조건 정의
# 공급 제약
for i in range(len(supply)):
    problem += pl.lpSum(x[i, j] for j in range(len(demand))) == supply[i]

# 수요 제약
for j in range(len(demand)):
    problem += pl.lpSum(x[i, j] for i in range(len(supply))) == demand[j]

# 문제 해결
problem.solve()

# 결과 출력
print("\nPuLP 결과:")
print(f"최적 목적함수 값 (총 운송 비용): {pl.value(problem.objective)}")
print("최적 운송량:")
for i in range(len(supply)):
    for j in range(len(demand)):
        if pl.value(x[i, j]) > 1e-5:  # 수치 오차 고려
            print(f"공급지 {i+1} -> 수요지 {j+1}: {pl.value(x[i, j]):.1f}")
```

**개념의 활용**: 물류 및 공급망 관리, 생산 계획, 자원 할당, 네트워크 설계 등 다양한 분야에서 활용됩니다. 특히 물류 비용이 큰 비중을 차지하는 제조업에서 운송 비용 최적화를 통한 원가 절감에 중요하게 활용됩니다. 복잡한 운송 문제는 선형 계획법 외에도 휴리스틱 알고리즘, 메타휴리스틱 등 다양한 최적화 기법이 적용됩니다.

## 텍스트마이닝

**정의**: 자연어 처리 기술을 활용하여 비정형 텍스트 데이터에서 의미 있는 정보, 패턴, 지식을 추출하는 분석 기법입니다.

**수식**:

- TF-IDF: $\text{TF-IDF}(t, d, D) = \text{TF}(t, d) \times \text{IDF}(t, D)$
    - $\text{TF}(t, d)$: 문서 d에서 단어 t의 빈도
    - $\text{IDF}(t, D) = \log\frac{N}{|{d \in D: t \in d}|}$: 단어 t가 등장한 문서 수의 역수에 로그를 취한 값
    - N: 전체 문서 수

**특징**:

- 전처리: 토큰화, 불용어 제거, 어간/표제어 추출, 정규화
- 특성 추출: 단어 빈도, TF-IDF, Word2Vec, BERT 등
- 분석 기법: 감성 분석, 토픽 모델링, 텍스트 분류, 개체명 인식 등
- 비지도 학습과 지도 학습 모두 적용 가능

**코드 예시**:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import re
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import PorterStemmer, WordNetLemmatizer
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer
from sklearn.decomposition import LatentDirichletAllocation, TruncatedSVD
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import classification_report, confusion_matrix
from wordcloud import WordCloud

# NLTK 리소스 다운로드 (처음 실행 시 필요)
# nltk.download('punkt')
# nltk.download('stopwords')
# nltk.download('wordnet')

# 예제 텍스트 데이터 (리뷰)
texts = [
    "This movie was amazing! I loved the plot and the acting.",
    "Terrible film, waste of time. The story was boring.",
    "Great performance by the lead actor. Highly recommended!",
    "I didn't enjoy this movie at all. Very disappointed.",
    "The cinematography was beautiful, but the story was weak.",
    "One of the best films I've seen this year. Excellent!",
    "Poor character development and predictable ending.",
    "The soundtrack was fantastic! Really enjoyed it.",
    "This movie wasn't worth the ticket price. Awful.",
    "Brilliant direction and screenplay. A must-watch!"
]

# 레이블 (긍정=1, 부정=0)
labels = [1, 0, 1, 0, 0, 1, 0, 1, 0, 1]

# 데이터프레임 생성
df = pd.DataFrame({
    'text': texts,
    'sentiment': labels
})

# 1. 텍스트 전처리 함수
def preprocess_text(text):
    # 소문자 변환
    text = text.lower()
    # 특수 문자 제거
    text = re.sub(r'[^\w\s]', '', text)
    # 토큰화
    tokens = word_tokenize(text)
    # 불용어 제거
    stop_words = set(stopwords.words('english'))
    tokens = [word for word in tokens if word not in stop_words]
    # 어간 추출 (Stemming)
    stemmer = PorterStemmer()
    stemmed_tokens = [stemmer.stem(word) for word in tokens]
    # 표제어 추출 (Lemmatization)
    lemmatizer = WordNetLemmatizer()
    lemmatized_tokens = [lemmatizer.lemmatize(word) for word in tokens]
    
    return {
        'original': text,
        'tokens': tokens,
        'stemmed': stemmed_tokens,
        'lemmatized': lemmatized_tokens,
        'processed': ' '.join(lemmatized_tokens)  # 처리된 텍스트
    }

# 전처리 적용
preprocessed_data = [preprocess_text(text) for text in texts]
df['processed_text'] = [data['processed'] for data in preprocessed_data]

# 2. 특성 추출
# a. 단어 빈도 (Bag of Words)
count_vectorizer = CountVectorizer()
X_count = count_vectorizer.fit_transform(df['processed_text'])
count_df = pd.DataFrame(
    X_count.toarray(),
    columns=count_vectorizer.get_feature_names_out()
)
print("단어 빈도 행렬 (일부):")
print(count_df.head(3))

# b. TF-IDF
tfidf_vectorizer = TfidfVectorizer()
X_tfidf = tfidf_vectorizer.fit_transform(df['processed_text'])
tfidf_df = pd.DataFrame(
    X_tfidf.toarray(),
    columns=tfidf_vectorizer.get_feature_names_out()
)
print("\nTF-IDF 행렬 (일부):")
print(tfidf_df.head(3))

# 3. 텍스트 분석
# a. 워드 클라우드
positive_text = ' '.join(df[df['sentiment'] == 1]['processed_text'])
negative_text = ' '.join(df[df['sentiment'] == 0]['processed_text'])

plt.figure(figsize=(16, 8))
# 긍정 워드 클라우드
plt.subplot(1, 2, 1)
wordcloud_pos = WordCloud(
    width=800, height=400,
    background_color='white',
    max_words=100
).generate(positive_text)
plt.imshow(wordcloud_pos, interpolation='bilinear')
plt.axis('off')
plt.title('Positive Reviews')

# 부정 워드 클라우드
plt.subplot(1, 2, 2)
wordcloud_neg = WordCloud(
    width=800, height=400,
    background_color='white',
    max_words=100
).generate(negative_text)
plt.imshow(wordcloud_neg, interpolation='bilinear')
plt.axis('off')
plt.title('Negative Reviews')

plt.tight_layout()
plt.show()

# b. 토픽 모델링 (LDA)
lda_model = LatentDirichletAllocation(
    n_components=2,  # 토픽 수
    random_state=42
)
lda_topics = lda_model.fit_transform(X_count)

# 토픽별 상위 단어 추출
def print_top_words(model, feature_names, n_top_words):
    topics = []
    for topic_idx, topic in enumerate(model.components_):
        topic_words = [feature_names[i] for i in topic.argsort()[:-n_top_words - 1:-1]]
        topics.append((topic_idx, topic_words))
        print(f"Topic #{topic_idx}: {' '.join(topic_words)}")
    return topics

print("\n토픽 모델링 결과 (LDA):")
topics = print_top_words(
    lda_model, 
    count_vectorizer.get_feature_names_out(), 
    n_top_words=5
)

# c. 텍스트 분류 (나이브 베이즈)
X_train, X_test, y_train, y_test = train_test_split(
    X_tfidf, df['sentiment'], test_size=0.3, random_state=42
)

classifier = MultinomialNB()
classifier.fit(X_train, y_train)
y_pred = classifier.predict(X_test)

print("\n텍스트 분류 결과 (나이브 베이즈):")
print(classification_report(y_test, y_pred))

# d. 감성 분석 시각화
df['prediction'] = classifier.predict(X_tfidf)
sentiment_counts = df.groupby(['sentiment', 'prediction']).size().unstack(fill_value=0)
print("\n감성 분석 결과 카운트:")
print(sentiment_counts)

# 직접 새로운 텍스트에 대한 감성 예측
new_texts = [
    "I really enjoyed this movie, it was fantastic!",
    "This was the worst film I've ever seen."
]

# 전처리 및 벡터화
new_processed = [preprocess_text(text)['processed'] for text in new_texts]
new_tfidf = tfidf_vectorizer.transform(new_processed)
new_predictions = classifier.predict(new_tfidf)

print("\n새로운 텍스트 감성 예측:")
for text, prediction in zip(new_texts, new_predictions):
    sentiment = "Positive" if prediction == 1 else "Negative"
    print(f"Text: '{text}'\nPredicted sentiment: {sentiment}\n")
```

**개념의 활용**: 소셜 미디어 분석, 감성 분석, 고객 리뷰 분석, 문서 분류, 주제 추출, 정보 검색, 질의응답 시스템 등 다양한 분야에서 활용됩니다. 기업은 텍스트 마이닝을 통해 고객 의견을 분석하고, 마케팅 전략을 수립하며, 신제품 개발 아이디어를 얻을 수 있습니다. 또한 뉴스 기사 요약, 이메일 스팸 필터링, 문서 추천 시스템 등에도 적용됩니다.