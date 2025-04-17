---
title: 02. Machine Learning
draft: false
tags:
  - "#자격증"
  - "#ML"
  - "#DL"
  - "#Certification"
---
# 지도학습 알고리즘 정리

## 로지스틱 회귀

**정의**: 종속변수가 범주형인 경우 사용하는 회귀 분석 방법으로, 입력 데이터와 가중치의 선형 결합을 시그모이드 함수에 통과시켜 0~1 사이의 확률값으로 변환하는 알고리즘

**수식**: $P(y=1|x) = \frac{1}{1 + e^{-(\beta_0 + \beta_1x_1 + ... + \beta_nx_n)}} = \frac{1}{1 + e^{-z}}$

**특징**:

- 해석력이 뛰어나 각 변수의 영향력을 파악 가능
- 다중공선성에 취약함
- 과적합 방지를 위한 규제(L1, L2) 적용 가능
- 선형 결정 경계를 가짐

**코드 예시**:

```python
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

# 모델 정의
model = LogisticRegression(
    penalty='l2',       # 규제 유형: 'l1', 'l2', 'elasticnet', 'none'
    C=1.0,              # 규제 강도의 역수 (작을수록 강한 규제)
    solver='lbfgs',     # 최적화 알고리즘
    max_iter=100,       # 최대 반복 횟수
    multi_class='auto', # 다중 클래스 처리 방식: 'auto', 'ovr', 'multinomial'
    class_weight=None   # 클래스 가중치
)

# 모델 학습
model.fit(X_train, y_train)

# 예측 및 평가
y_pred = model.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)
print(f"정확도: {accuracy:.4f}")

# 회귀계수 확인
print("절편:", model.intercept_)
print("계수:", model.coef_)
```

**개념의 활용**:

- 이진 분류 문제(예/아니오, 합격/불합격 등)
- 다중 범주형 분류 문제
- 결과 해석이 중요한 의료, 금융 분야 분석
- 확률값이 필요한 상황(예: 고객 이탈 확률 예측)

## 나이브 베이즈

**정의**: 베이즈 정리를 기반으로 독립성 가정을 통해 각 특성이 서로 독립적이라고 가정하고 조건부 확률을 계산하여 분류하는 알고리즘

**수식**: $P(y|x_1,...,x_n) = \frac{P(y)P(x_1,...,x_n|y)}{P(x_1,...,x_n)} \propto P(y)\prod_{i=1}^{n}P(x_i|y)$

**특징**:

- 계산이 단순하고 빠름
- 적은 양의 학습 데이터로도 효과적
- 고차원 데이터에서도 효율적
- 특성 간 독립성 가정으로 인한 한계 존재
- 텍스트 분류에 매우 효과적

**코드 예시**:

```python
from sklearn.naive_bayes import GaussianNB, MultinomialNB, BernoulliNB

# 가우시안 나이브 베이즈 (연속형 데이터)
gnb = GaussianNB(
    priors=None,        # 클래스 사전 확률
    var_smoothing=1e-9  # 안정성을 위한 분산 평활화 파라미터
)
gnb.fit(X_train, y_train)

# 다항 나이브 베이즈 (텍스트 분류에 적합)
mnb = MultinomialNB(
    alpha=1.0,          # 라플라스/Lidstone 평활화 파라미터
    fit_prior=True      # 클래스 사전 확률 학습 여부
)
mnb.fit(X_train, y_train)

# 베르누이 나이브 베이즈 (이진 특성에 적합)
bnb = BernoulliNB(
    alpha=1.0,          # 평활화 파라미터
    binarize=0.0,       # 이진화 임계값
    fit_prior=True      # 클래스 사전 확률 학습 여부
)
bnb.fit(X_train, y_train)
```

**개념의 활용**:

- 텍스트 분류 및 감정 분석
- 스팸 필터링
- 대용량 데이터셋에서의 빠른 분류
- 실시간 예측이 필요한 경우
- 다중 클래스 분류 문제

## 서포트 벡터 머신

**정의**: 데이터를 가장 잘 구분하는 최적의 결정 경계(초평면)를 찾아 분류하는 알고리즘으로, 마진을 최대화하는 방식으로 학습

**수식**: 선형 SVM: $\min_{\mathbf{w}, b} \frac{1}{2}||\mathbf{w}||^2 \text{ subject to } y_i(\mathbf{w}^T\mathbf{x}_i + b) \geq 1, \forall i$

**특징**:

- 고차원 데이터에서 효과적
- 커널 트릭을 통한 비선형 분류 가능
- 과적합에 강한 편
- 이상치에 민감
- 훈련 시간이 오래 걸릴 수 있음

**코드 예시**:

```python
from sklearn.svm import SVC, LinearSVC

# 비선형 SVM (커널 사용)
svm = SVC(
    C=1.0,                   # 규제 파라미터 (작을수록 강한 규제)
    kernel='rbf',            # 커널 종류: 'linear', 'poly', 'rbf', 'sigmoid'
    degree=3,                # 다항 커널의 차수
    gamma='scale',           # 커널 계수 ('scale', 'auto' 또는 실수값)
    probability=False,       # 확률 추정 여부
    class_weight=None,       # 클래스 가중치
    decision_function_shape='ovr'  # 다중 클래스 전략
)
svm.fit(X_train, y_train)

# 선형 SVM (더 빠른 구현)
linear_svm = LinearSVC(
    penalty='l2',            # 규제 유형
    loss='hinge',            # 손실 함수 ('hinge', 'squared_hinge')
    C=1.0,                   # 규제 파라미터
    multi_class='ovr',       # 다중 클래스 전략
    max_iter=1000            # 최대 반복 횟수
)
linear_svm.fit(X_train, y_train)
```

**개념의 활용**:

- 비선형 분류 문제
- 텍스트 분류 및 이미지 인식
- 고차원 데이터셋
- 명확한 마진 구분이 필요한 경우
- 중간 규모의 데이터셋 (매우 큰 데이터셋에서는 계산 비용이 높음)

## 결정트리

**정의**: 데이터의 특성을 기반으로 질문을 통해 트리 구조로 데이터를 분할하여 분류 또는 회귀를 수행하는 알고리즘

**수식**: 분할 기준으로 지니 불순도: $Gini(t) = 1 - \sum_{j=1}^{c} p(j|t)^2$ 또는 엔트로피: $Entropy(t) = -\sum_{j=1}^{c} p(j|t) \log p(j|t)$

**특징**:

- 결과 해석이 매우 직관적
- 데이터 전처리가 적게 필요 (스케일링 불필요)
- 수치형과 범주형 데이터 모두 처리 가능
- 비선형 관계 모델링 가능
- 과적합 위험이 높음

**코드 예시**:

```python
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor
from sklearn.tree import export_graphviz

# 분류 결정트리
dt_clf = DecisionTreeClassifier(
    criterion='gini',       # 분할 기준: 'gini', 'entropy'
    max_depth=None,         # 최대 깊이
    min_samples_split=2,    # 노드 분할에 필요한 최소 샘플 수
    min_samples_leaf=1,     # 리프 노드에 필요한 최소 샘플 수
    max_features=None,      # 분할 시 고려할 최대 특성 수
    random_state=42,        # 랜덤 시드
    class_weight=None       # 클래스 가중치
)
dt_clf.fit(X_train, y_train)

# 회귀 결정트리
dt_reg = DecisionTreeRegressor(
    criterion='mse',        # 분할 기준: 'mse', 'mae'
    max_depth=None,         # 최대 깊이
    min_samples_split=2,    # 노드 분할에 필요한 최소 샘플 수
    min_samples_leaf=1      # 리프 노드에 필요한 최소 샘플 수
)
dt_reg.fit(X_train, y_train)

# 트리 시각화 (graphviz 필요)
export_graphviz(dt_clf, 
                out_file='tree.dot', 
                feature_names=feature_names,
                class_names=class_names,
                filled=True)
```

**개념의 활용**:

- 결과 해석이 필요한 모든 분류/회귀 문제
- 다양한 유형의 데이터를 함께 분석할 때
- 탐색적 데이터 분석(EDA)
- 특성 중요도 분석
- 계층적 의사결정이 필요한 경우

## 랜덤 포레스트

**정의**: 여러 개의 결정 트리를 임의성을 가미하여 학습시키고 그 결과를 집계(앙상블)하여 예측하는 알고리즘

**수식**: $f(x) = \frac{1}{B} \sum_{i=1}^{B} f_i(x)$ (회귀의 경우) $f(x) = mode(f_1(x), f_2(x), ..., f_B(x))$ (분류의 경우)

**특징**:

- 과적합 위험이 결정트리보다 낮음
- 높은 예측 정확도
- 특성 중요도 제공
- 이상치에 강건함
- 병렬 처리 가능
- 해석력은 결정트리보다 떨어짐

**코드 예시**:

```python
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor

# 분류 랜덤 포레스트
rf_clf = RandomForestClassifier(
    n_estimators=100,       # 트리 개수
    criterion='gini',       # 분할 기준
    max_depth=None,         # 최대 깊이
    min_samples_split=2,    # 노드 분할에 필요한 최소 샘플 수
    min_samples_leaf=1,     # 리프 노드에 필요한 최소 샘플 수
    max_features='auto',    # 분할 시 고려할 최대 특성 수
    bootstrap=True,         # 부트스트랩 샘플링 여부
    oob_score=False,        # Out-of-bag 샘플을 사용한 성능 평가 여부
    n_jobs=-1,              # 병렬 처리에 사용할 코어 수
    random_state=42         # 랜덤 시드
)
rf_clf.fit(X_train, y_train)

# 회귀 랜덤 포레스트
rf_reg = RandomForestRegressor(
    n_estimators=100,       # 트리 개수
    criterion='mse',        # 분할 기준
    max_depth=None,         # 최대 깊이
    min_samples_split=2,    # 노드 분할에 필요한 최소 샘플 수
    min_samples_leaf=1,     # 리프 노드에 필요한 최소 샘플 수
    max_features='auto',    # 분할 시 고려할 최대 특성 수
    bootstrap=True,         # 부트스트랩 샘플링 여부
    oob_score=False,        # Out-of-bag 샘플을 사용한 성능 평가 여부
    n_jobs=-1               # 병렬 처리에 사용할 코어 수
)
rf_reg.fit(X_train, y_train)

# 특성 중요도 확인
feature_importances = rf_clf.feature_importances_
```

**개념의 활용**:

- 높은 예측 정확도가 필요한 분류/회귀 문제
- 고차원 데이터 분석
- 특성 중요도 분석이 필요한 경우
- 이상치가 있는 데이터셋
- 계산 자원이 충분한 환경

## XGBoost

**정의**: 그래디언트 부스팅 결정 트리(GBDT)의 최적화된 구현으로, 손실 함수의 그래디언트를 기반으로 순차적으로 트리를 학습시키는 앙상블 알고리즘

**수식**: $\hat{y_i} = \sum_{k=1}^{K} f_k(x_i), f_k \in F$ 목적 함수: $Obj = \sum_{i=1}^{n} l(y_i, \hat{y_i}) + \sum_{k=1}^{K} \Omega(f_k)$

**특징**:

- 높은 성능과 빠른 실행 속도
- 정규화를 통한 과적합 방지
- 결측치 처리 기능 내장
- 병렬 처리 지원
- 조기 중단(early stopping) 지원
- 다양한 손실 함수 제공

**코드 예시**:

```python
import xgboost as xgb
from sklearn.metrics import mean_squared_error, accuracy_score

# 분류 XGBoost
xgb_clf = xgb.XGBClassifier(
    max_depth=3,              # 트리 최대 깊이
    learning_rate=0.1,        # 학습률
    n_estimators=100,         # 트리 개수
    subsample=0.8,            # 샘플링 비율
    colsample_bytree=0.8,     # 특성 샘플링 비율
    objective='binary:logistic', # 목적 함수
    reg_alpha=0,              # L1 규제
    reg_lambda=1,             # L2 규제
    random_state=42,          # 랜덤 시드
    n_jobs=-1                 # 병렬 처리에 사용할 코어 수
)
xgb_clf.fit(X_train, y_train, 
           eval_set=[(X_val, y_val)],
           early_stopping_rounds=10,
           verbose=True)

# 회귀 XGBoost
xgb_reg = xgb.XGBRegressor(
    max_depth=3,              # 트리 최대 깊이
    learning_rate=0.1,        # 학습률
    n_estimators=100,         # 트리 개수
    objective='reg:squarederror', # 목적 함수
    reg_alpha=0,              # L1 규제
    reg_lambda=1,             # L2 규제
    random_state=42           # 랜덤 시드
)
xgb_reg.fit(X_train, y_train,
           eval_set=[(X_val, y_val)],
           early_stopping_rounds=10,
           verbose=True)

# 특성 중요도 시각화
xgb.plot_importance(xgb_clf)
```

**개념의 활용**:

- 구조화된 데이터의 분류/회귀 문제
- 데이터 과학 경진대회
- 고성능 예측 모델이 필요한 비즈니스 문제
- 특성 중요도 분석이 필요한 경우
- 대용량 데이터셋 분석

## LightGBM

**정의**: 그래디언트 부스팅 결정 트리의 또 다른 최적화 구현으로, 리프 중심 트리 성장 방식과 히스토그램 기반 특성 분할을 통해 빠른 학습 및 낮은 메모리 사용

**수식**: XGBoost와 유사한 목적 함수를 사용하지만 트리 구축 방식이 다름

**특징**:

- 매우 빠른 학습 속도
- 대용량 데이터에 효율적
- 메모리 사용량이 적음
- 범주형 변수 자동 처리
- 리프 중심 트리 분할(Leaf-wise)
- 작은 데이터셋에서 과적합 위험 있음

**코드 예시**:

```python
import lightgbm as lgb
from sklearn.metrics import mean_squared_error, accuracy_score

# 분류 LightGBM
lgb_clf = lgb.LGBMClassifier(
    boosting_type='gbdt',     # 부스팅 유형: 'gbdt', 'dart', 'goss'
    num_leaves=31,            # 최대 리프 노드 수
    max_depth=-1,             # 최대 깊이
    learning_rate=0.1,        # 학습률
    n_estimators=100,         # 트리 개수
    subsample=0.8,            # 샘플링 비율
    colsample_bytree=0.8,     # 특성 샘플링 비율
    reg_alpha=0,              # L1 규제
    reg_lambda=1,             # L2 규제
    random_state=42,          # 랜덤 시드
    n_jobs=-1                 # 병렬 처리에 사용할 코어 수
)
lgb_clf.fit(X_train, y_train,
           eval_set=[(X_val, y_val)],
           eval_metric='logloss',
           early_stopping_rounds=10,
           verbose=True)

# 회귀 LightGBM
lgb_reg = lgb.LGBMRegressor(
    boosting_type='gbdt',     # 부스팅 유형
    num_leaves=31,            # 최대 리프 노드 수
    max_depth=-1,             # 최대 깊이
    learning_rate=0.1,        # 학습률
    n_estimators=100,         # 트리 개수
    reg_alpha=0,              # L1 규제
    reg_lambda=1,             # L2 규제
    random_state=42           # 랜덤 시드
)
lgb_reg.fit(X_train, y_train,
           eval_set=[(X_val, y_val)],
           eval_metric='rmse',
           early_stopping_rounds=10,
           verbose=True)

# 특성 중요도 시각화
lgb.plot_importance(lgb_clf)
```

**개념의 활용**:

- 대용량 데이터셋 분석
- 빠른 학습 속도가 필요한 경우
- 제한된 메모리 환경
- 범주형 변수가 많은 데이터셋
- 실시간에 가까운 예측이 필요한 환경

## 신경망

**정의**: 인간 뇌의 신경 구조에서 영감을 받은 모델로, 여러 층의 인공 뉴런을 통해 복잡한 패턴을 학습하는 알고리즘

**수식**: 뉴런 출력: $a = \sigma(Wx + b)$ 여기서 $\sigma$는 활성화 함수, $W$는 가중치, $b$는 편향

**특징**:

- 복잡한 비선형 관계 모델링 가능
- 대량의 데이터에 효과적
- 특성 엔지니어링 필요성 감소
- 다양한 아키텍처(CNN, RNN, 트랜스포머 등) 존재
- 학습 시간이 오래 걸림
- 많은 계산 자원 필요
- 해석이 어려운 블랙박스 모델

**코드 예시** (PyTorch):

```python
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader, TensorDataset

# 데이터 준비
X_tensor = torch.FloatTensor(X_train.values)
y_tensor = torch.FloatTensor(y_train.values)
dataset = TensorDataset(X_tensor, y_tensor)
dataloader = DataLoader(dataset, batch_size=32, shuffle=True)

# MLP(다층 퍼셉트론) 정의
class MLP(nn.Module):
    def __init__(self, input_dim, hidden_dim, output_dim):
        super(MLP, self).__init__()
        self.fc1 = nn.Linear(input_dim, hidden_dim)
        self.relu = nn.ReLU()
        self.dropout = nn.Dropout(0.2)
        self.fc2 = nn.Linear(hidden_dim, hidden_dim // 2)
        self.fc3 = nn.Linear(hidden_dim // 2, output_dim)
        
    def forward(self, x):
        x = self.fc1(x)
        x = self.relu(x)
        x = self.dropout(x)
        x = self.fc2(x)
        x = self.relu(x)
        x = self.fc3(x)
        return x

# 분류 모델일 경우
class ClassificationNet(nn.Module):
    def __init__(self, input_dim, hidden_dim, num_classes):
        super(ClassificationNet, self).__init__()
        self.fc1 = nn.Linear(input_dim, hidden_dim)
        self.relu = nn.ReLU()
        self.fc2 = nn.Linear(hidden_dim, num_classes)
        
    def forward(self, x):
        x = self.fc1(x)
        x = self.relu(x)
        x = self.fc2(x)
        return x

# 모델 초기화
input_dim = X_train.shape[1]
hidden_dim = 64
output_dim = 1  # 회귀의 경우
model = MLP(input_dim, hidden_dim, output_dim)

# 손실 함수 및 옵티마이저 정의
criterion = nn.MSELoss()  # 회귀의 경우
# criterion = nn.CrossEntropyLoss()  # 분류의 경우
optimizer = optim.Adam(model.parameters(), lr=0.001)

# 학습 루프
num_epochs = 100
for epoch in range(num_epochs):
    model.train()
    running_loss = 0.0
    
    for inputs, targets in dataloader:
        # 그래디언트 초기화
        optimizer.zero_grad()
        
        # 순전파
        outputs = model(inputs)
        
        # 손실 계산
        loss = criterion(outputs, targets)
        
        # 역전파 및 최적화
        loss.backward()
        optimizer.step()
        
        running_loss += loss.item()
    
    # 에폭당 평균 손실 출력
    if (epoch+1) % 10 == 0:
        print(f'Epoch {epoch+1}/{num_epochs}, Loss: {running_loss/len(dataloader):.4f}')

# 모델 평가
model.eval()
with torch.no_grad():
    test_inputs = torch.FloatTensor(X_test.values)
    predictions = model(test_inputs)
    predictions = predictions.numpy()
```

**개념의 활용**:

- 이미지, 음성, 텍스트 등 비정형 데이터 분석
- 복잡한 패턴 인식 문제
- 대량의 데이터가 있는 경우
- 높은 정확도가 필요한 복잡한 문제
- 엔드투엔드 학습이 필요한 경우
- 딥러닝 접근법이 효과적인 도메인 (컴퓨터 비전, 자연어 처리 등)

## 분류 모델 평가

**정의**: 분류 모델의 성능을 측정하기 위한 다양한 지표와 방법

**주요 평가 지표**:

1. **정확도(Accuracy)**:
    
    - 정의: 전체 예측 중 올바른 예측의 비율
    - 수식: $Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$
    - 특징: 클래스 불균형 데이터에서는 오해의 소지가 있음
2. **정밀도(Precision)**:
    
    - 정의: 양성으로 예측한 것 중 실제 양성의 비율
    - 수식: $Precision = \frac{TP}{TP + FP}$
    - 특징: 거짓 양성(False Positive)을 최소화해야 할 때 중요
3. **재현율(Recall, 민감도)**:
    
    - 정의: 실제 양성 중 양성으로 예측한 비율
    - 수식: $Recall = \frac{TP}{TP + FN}$
    - 특징: 거짓 음성(False Negative)을 최소화해야 할 때 중요
4. **F1 점수**:
    
    - 정의: 정밀도와 재현율의 조화 평균
    - 수식: $F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall}$
    - 특징: 정밀도와 재현율의 균형을 잡을 때 유용
5. **ROC 곡선과 AUC**:
    
    - 정의: 다양한 임계값에서의 참 양성률(TPR)과 거짓 양성률(FPR)의 곡선과 그 아래 면적
    - 특징: 분류기의 전반적인 성능을 평가, 0.5(랜덤)에서 1.0(완벽) 사이 값

**코드 예시**:

```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score
from sklearn.metrics import confusion_matrix, classification_report, roc_curve, auc

# 기본 평가 지표
accuracy = accuracy_score(y_true, y_pred)
precision = precision_score(y_true, y_pred)
recall = recall_score(y_true, y_pred)
f1 = f1_score(y_true, y_pred)

print(f"Accuracy: {accuracy:.4f}")
print(f"Precision: {precision:.4f}")
print(f"Recall: {recall:.4f}")
print(f"F1 Score: {f1:.4f}")

# 혼동 행렬
conf_matrix = confusion_matrix(y_true, y_pred)
print("Confusion Matrix:")
print(conf_matrix)

# 분류 보고서 (모든 메트릭 한번에 보기)
print(classification_report(y_true, y_pred))

# ROC 곡선 및 AUC
y_scores = model.predict_proba(X_test)[:, 1]  # 양성 클래스 확률
fpr, tpr, thresholds = roc_curve(y_true, y_scores)
roc_auc = auc(fpr, tpr)

# ROC 곡선 시각화
import matplotlib.pyplot as plt
plt.figure()
plt.plot(fpr, tpr, color='darkorange', lw=2, 
         label=f'ROC curve (area = {roc_auc:.2f})')
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
plt.xlim([0.0, 1.0])
plt.ylim([0.0, 1.05])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver Operating Characteristic')
plt.legend(loc="lower right")
plt.show()
```

**개념의 활용**:

- 정밀도: 스팸 필터, 사기 탐지(오탐지 비용이 높을 때)
- 재현율: 질병 진단, 불량품 검출(미탐지 비용이 높을 때)
- F1 점수: 정밀도와 재현율의 균형이 중요한 경우
- ROC-AUC: 다양한 임계값에서의 모델 성능을 전체적으로 평가할 때
- 정확도: 클래스 균형이 잘 맞는 일반적인 분류 문제

## 회귀 모델 평가

**정의**: 회귀 모델의 성능을 측정하기 위한 다양한 지표

**주요 평가 지표**:

1. **평균 제곱 오차(MSE)**:
    
    - 정의: 예측값과 실제값 차이의 제곱 평균
    - 수식: $MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$
    - 특징: 오차를 제곱하므로 큰 오차에 더 민감
2. **평균 제곱근 오차(RMSE)**:
    
    - 정의: MSE의 제곱근
    - 수식: $RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}$
    - 특징: 원래 데이터와 같은 단위로 해석 가능
3. **평균 절대 오차(MAE)**:
    
    - 정의: 예측값과 실제값 차이의 절대값 평균
    - 수식: $MAE = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$
    - 특징: 이상치에 덜 민감
4. **평균 절대 백분율 오차(MAPE)**:
    
    - 정의: 실제값에 대한 오차의 절대 백분율 평균
    - 수식: $MAPE = \frac{100%}{n} \sum_{i=1}^{n} |\frac{y_i - \hat{y}_i}{y_i}|$
    - 특징: 상대적 오차 측정, 실제값이 0에 가까울 때 불안정
5. **결정 계수(R²)**:
    
    - 정의: 모델이 설명하는 종속 변수의 분산 비율
    - 수식: $R^2 = 1 - \frac{\sum_{i=1}^{n} (y_i - \hat{y}_i)^2}{\sum_{i=1}^{n} (y_i - \bar{y})^2}$
    - 특징: 0~1 사이 값(1이 최적), 음수가 나올 수도 있음

**코드 예시**:

```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
import numpy as np

# 평가 지표 계산
mse = mean_squared_error(y_true, y_pred)
rmse = np.sqrt(mse)
mae = mean_absolute_error(y_true, y_pred)
r2 = r2_score(y_true, y_pred)

# MAPE 계산 (sklearn에 없음)
def mean_absolute_percentage_error(y_true, y_pred):
    # 0으로 나누기 방지
    mask = y_true != 0
    return np.mean(np.abs((y_true[mask] - y_pred[mask]) / y_true[mask])) * 100

mape = mean_absolute_percentage_error(y_true, y_pred)

print(f"MSE: {mse:.4f}")
print(f"RMSE: {rmse:.4f}")
print(f"MAE: {mae:.4f}")
print(f"MAPE: {mape:.2f}%")
print(f"R²: {r2:.4f}")

# 잔차 시각화
import matplotlib.pyplot as plt
plt.figure(figsize=(10, 6))
plt.scatter(y_pred, y_true - y_pred)
plt.axhline(y=0, color='r', linestyle='-')
plt.xlabel('Predicted Values')
plt.ylabel('Residuals')
plt.title('Residual Plot')
plt.show()
```

**개념의 활용**:

- MSE/RMSE: 큰 오차를 강조해야 할 때(이상치에 민감)
- MAE: 이상치의 영향을 줄이고 싶을 때
- MAPE: 상대적 오차가 중요한 경우(예: 매출 예측)
- R²: 모델 설명력 평가, 변수 선택 시
- 잔차 분석: 모델 가정 확인, 패턴 탐색
---
# 비지도학습 알고리즘 정리

## 차원축소

### PCA (주성분 분석)

**정의**: 고차원 데이터를 분산이 최대가 되는 방향(주성분)으로 투영하여 더 낮은 차원의 데이터로 변환하는 기법

**수식**:

- 공분산 행렬: $\Sigma = \frac{1}{n} X^T X$ (중심화된 데이터 X에 대해)
- 주성분: 공분산 행렬의 고유벡터
- 변환: $Z = X W$ (W는 주성분 행렬)

**특징**:

- 선형 차원 축소 기법
- 데이터 분산을 최대로 보존
- 직교하는 새로운 기저(축)를 찾음
- 계산이 비교적 간단하고 효율적
- 특성 간 상관관계 제거
- 비선형 관계 포착 불가능
- 이상치에 민감

**코드 예시**:

```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler
import matplotlib.pyplot as plt

# 데이터 표준화 (PCA 전 권장)
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# PCA 모델 정의 및 학습
pca = PCA(n_components=2)  # 2차원으로 축소
X_pca = pca.fit_transform(X_scaled)

# 결과 확인
print(f"원본 데이터 형태: {X_scaled.shape}")
print(f"축소된 데이터 형태: {X_pca.shape}")

# 설명된 분산 비율
explained_variance = pca.explained_variance_ratio_
print(f"설명된 분산 비율: {explained_variance}")
print(f"누적 설명된 분산: {explained_variance.sum():.4f}")

# 주성분 기여도 (특성 중요도)
components = pca.components_
feature_names = X.columns  # 특성 이름이 있다고 가정
for i, component in enumerate(components):
    sorted_indices = np.argsort(np.abs(component))[::-1]
    print(f"주성분 {i+1}의 주요 특성:")
    for idx in sorted_indices[:3]:  # 상위 3개 특성만 출력
        print(f"  {feature_names[idx]}: {component[idx]:.3f}")

# 시각화
plt.figure(figsize=(10, 6))
plt.scatter(X_pca[:, 0], X_pca[:, 1], alpha=0.7)
plt.xlabel('Principal Component 1')
plt.ylabel('Principal Component 2')
plt.title('PCA Result')
plt.grid(True)
plt.show()

# 최적 주성분 개수 찾기 (스크리 플롯)
pca_full = PCA().fit(X_scaled)
plt.figure(figsize=(10, 6))
plt.plot(np.arange(1, len(pca_full.explained_variance_ratio_) + 1), 
         np.cumsum(pca_full.explained_variance_ratio_), 'o-')
plt.axhline(y=0.95, color='r', linestyle='--')
plt.xlabel('Number of Components')
plt.ylabel('Cumulative Explained Variance')
plt.title('Explained Variance vs. Number of Components')
plt.grid(True)
plt.show()
```

**개념의 활용**:

- 고차원 데이터 시각화 (2D, 3D로 축소)
- 다중공선성 완화
- 계산 효율성 향상
- 특성 추출 및 차원 축소
- 노이즈 제거
- 이미지 압축
- 사전 처리 단계로서 다른 알고리즘의 성능 향상

## 클러스터링

### K-Means

**정의**: 데이터를 K개의 클러스터로 나누는 알고리즘으로, 각 데이터 포인트와 할당된 중심점 간의 거리 제곱합을 최소화하는 방식으로 동작

**수식**:

- 목적 함수: $J = \sum_{j=1}^{k} \sum_{i=1}^{n} ||x_i^{(j)} - c_j||^2$
- 여기서 $x_i^{(j)}$는 클러스터 j에 속한 데이터 포인트, $c_j$는 클러스터 j의 중심점

**특징**:

- 구현이 간단하고 계산이 효율적
- 대용량 데이터에 적용 가능
- 구형(원형) 클러스터를 잘 찾음
- 이상치에 민감
- 클러스터 개수(K)를 사전에 지정해야 함
- 초기 중심점에 따라 결과가 달라질 수 있음
- 클러스터 크기가 다양한 경우 성능 저하

**코드 예시**:

```python
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score
import matplotlib.pyplot as plt
import numpy as np

# 데이터 준비 (가정)
# X = your_data

# K-means 클러스터링
kmeans = KMeans(
    n_clusters=3,           # 클러스터 개수
    init='k-means++',       # 초기화 방법 ('random', 'k-means++')
    n_init=10,              # 다른 초기값으로 알고리즘을 실행할 횟수
    max_iter=300,           # 최대 반복 횟수
    tol=1e-4,               # 수렴 조건
    random_state=42         # 랜덤 시드
)
cluster_labels = kmeans.fit_predict(X)

# 결과 확인
centroids = kmeans.cluster_centers_
print("클러스터 중심점:")
print(centroids)

# 각 클러스터별 포인트 수 확인
unique_labels, counts = np.unique(cluster_labels, return_counts=True)
for label, count in zip(unique_labels, counts):
    print(f"클러스터 {label}: {count}개 포인트")

# 실루엣 점수 계산
silhouette_avg = silhouette_score(X, cluster_labels)
print(f"실루엣 점수: {silhouette_avg:.3f}")

# 엘보우 방법을 통한 최적 클러스터 개수 찾기
inertias = []
silhouette_scores = []
range_n_clusters = range(2, 11)

for n_clusters in range_n_clusters:
    kmeans = KMeans(n_clusters=n_clusters, random_state=42, n_init=10)
    cluster_labels = kmeans.fit_predict(X)
    inertias.append(kmeans.inertia_)
    
    # 실루엣 점수 (2개 이상의 클러스터에 대해서만 계산 가능)
    silhouette_avg = silhouette_score(X, cluster_labels)
    silhouette_scores.append(silhouette_avg)

# 시각화 (엘보우 방법)
plt.figure(figsize=(12, 5))
plt.subplot(1, 2, 1)
plt.plot(range_n_clusters, inertias, 'o-')
plt.xlabel('클러스터 개수')
plt.ylabel('관성(Inertia)')
plt.title('엘보우 방법')
plt.grid(True)

# 시각화 (실루엣 점수)
plt.subplot(1, 2, 2)
plt.plot(range_n_clusters, silhouette_scores, 'o-')
plt.xlabel('클러스터 개수')
plt.ylabel('실루엣 점수')
plt.title('실루엣 점수 방법')
plt.grid(True)

plt.tight_layout()
plt.show()

# 클러스터링 결과 2D 시각화 (PCA로 차원 축소 후)
from sklearn.decomposition import PCA

pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)

plt.figure(figsize=(10, 8))
scatter = plt.scatter(X_pca[:, 0], X_pca[:, 1], c=cluster_labels, cmap='viridis', alpha=0.7)
centroids_pca = pca.transform(centroids)
plt.scatter(centroids_pca[:, 0], centroids_pca[:, 1], marker='X', color='red', s=200, label='중심점')
plt.legend(*scatter.legend_elements(), title="클러스터")
plt.title('K-means 클러스터링 결과 (PCA로 축소)')
plt.grid(True)
plt.show()
```

**개념의 활용**:

- 고객 세분화
- 이미지 압축
- 문서 군집화
- 이상 탐지
- 데이터 전처리 단계로 활용
- 유사한 특성을 가진 그룹 식별
- 추천 시스템 개발

#### 실루엣 계수

**정의**: 클러스터링의 품질을 평가하는 지표로, 객체가 자신이 속한 클러스터와 얼마나 유사하고 다른 클러스터와 얼마나 분리되어 있는지를 측정

**수식**:

- 실루엣 계수: $s(i) = \frac{b(i) - a(i)}{\max{a(i), b(i)}}$
- $a(i)$: 객체 i와 같은 클러스터 내 모든 다른 객체 간의 평균 거리
- $b(i)$: 객체 i와 가장 가까운 다른 클러스터 내 모든 객체 간의 평균 거리

**특징**:

- -1에서 1 사이의 값을 가짐
- 1에 가까울수록 좋은 클러스터링
- 0에 가까우면 클러스터 경계에 있음을 의미
- -1에 가까우면 잘못된 클러스터에 배정됨을 의미
- 클러스터 개수 결정에 도움

**코드 예시**:

```python
from sklearn.metrics import silhouette_score, silhouette_samples
import matplotlib.pyplot as plt
import numpy as np

# 실루엣 점수 계산
silhouette_avg = silhouette_score(X, cluster_labels)
print(f"평균 실루엣 점수: {silhouette_avg:.3f}")

# 각 샘플의 실루엣 점수 계산
sample_silhouette_values = silhouette_samples(X, cluster_labels)

# 실루엣 시각화
plt.figure(figsize=(12, 8))
y_lower = 10

# 각 클러스터별로 실루엣 그래프 그리기
for i in range(kmeans.n_clusters):
    # i번째 클러스터에 속한 샘플의 실루엣 점수 추출
    ith_cluster_values = sample_silhouette_values[cluster_labels == i]
    ith_cluster_values.sort()
    
    size_cluster_i = ith_cluster_values.shape[0]
    y_upper = y_lower + size_cluster_i
    
    color = plt.cm.nipy_spectral(float(i) / kmeans.n_clusters)
    plt.fill_betweenx(np.arange(y_lower, y_upper),
                      0, ith_cluster_values,
                      facecolor=color, edgecolor=color, alpha=0.7)
    
    # 클러스터 레이블 추가
    plt.text(-0.05, y_lower + 0.5 * size_cluster_i, f'Cluster {i}')
    
    # 다음 클러스터의 y_lower 계산
    y_lower = y_upper + 10

plt.axvline(x=silhouette_avg, color="red", linestyle="--")
plt.title("Silhouette plot for K-Means clustering")
plt.xlabel("Silhouette Coefficient")
plt.ylabel("Cluster label")
plt.yticks([])  # y축 눈금 제거
plt.show()
```

**개념의 활용**:

- 최적의 클러스터 개수 결정
- 클러스터링 알고리즘 성능 평가
- 서로 다른 클러스터링 알고리즘 비교
- 데이터 내 자연적 클러스터 수 추정
- 이상치 탐지

#### 엘보우 방법

**정의**: 클러스터 내 분산(관성, inertia)을 클러스터 개수에 따라 플롯하여 급격한 변화가 있는 "팔꿈치(elbow)" 지점을 찾아 최적의 클러스터 개수를 결정하는 방법

**수식**:

- 관성(Inertia): $\sum_{i=0}^{n} \min_{c_j \in C} (||x_i - c_j||^2)$
- 각 데이터 포인트와 가장 가까운 클러스터 중심 간의 거리 제곱의 합

**특징**:

- 직관적이고 구현이 쉬움
- 클러스터 개수가 증가할수록 관성은 항상 감소
- 명확한 엘보우 포인트가 없을 수 있음
- 시각적 해석에 주관이 개입될 수 있음
- 다른 방법(실루엣 계수 등)과 함께 사용하면 더 신뢰성 있음

**코드 예시**:

```python
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt
import numpy as np

# 엘보우 방법으로 최적 클러스터 개수 찾기
inertias = []
range_n_clusters = range(1, 11)

for n_clusters in range_n_clusters:
    kmeans = KMeans(n_clusters=n_clusters, random_state=42, n_init=10)
    kmeans.fit(X)
    inertias.append(kmeans.inertia_)

# 시각화
plt.figure(figsize=(10, 6))
plt.plot(range_n_clusters, inertias, 'o-', markersize=8)
plt.grid(True)
plt.xlabel('클러스터 개수 (k)', fontsize=12)
plt.ylabel('관성 (Inertia)', fontsize=12)
plt.title('엘보우 방법으로 최적 클러스터 개수 찾기', fontsize=14)

# 관성의 변화율 계산
inertia_changes = np.diff(inertias)
percent_changes = inertia_changes / np.array(inertias[:-1]) * 100
for i, pct in enumerate(percent_changes):
    plt.annotate(f'{abs(pct):.1f}%', 
                 xy=(i+1.5, (inertias[i] + inertias[i+1])/2),
                 xytext=(10, 0), 
                 textcoords='offset points',
                 fontsize=10)

plt.show()

# 관성 변화율 그래프
plt.figure(figsize=(10, 6))
plt.bar(range(2, 11), abs(percent_changes), color='skyblue')
plt.grid(True, axis='y')
plt.xlabel('클러스터 개수 (k)', fontsize=12)
plt.ylabel('관성 변화율 (%)', fontsize=12)
plt.title('각 클러스터 개수에서의 관성 변화율', fontsize=14)
plt.show()
```

**개념의 활용**:

- K-means 클러스터링의 최적 K 결정
- 계층적 클러스터링에서 적절한 클러스터 수 결정
- 비용 대비 효율을 고려한 의사결정
- 클러스터링 문제에서 자동화된 파라미터 선택
- 데이터셋 내 자연적 그룹 수 추정

### DBSCAN

**정의**: 밀도 기반 클러스터링 알고리즘으로, 고밀도 영역의 점들을 클러스터로 그룹화하고 저밀도 영역의 점들을 노이즈로 간주

**수식**:

- 핵심 개념:
    - ε (epsilon): 이웃을 정의하는 반경
    - minPts: 핵심 포인트로 간주되기 위한 최소 이웃 수

**특징**:

- 클러스터 개수를 사전에 지정할 필요 없음
- 불규칙한 모양의 클러스터도 잘 찾음
- 이상치를 자동으로 식별
- 밀도가 다양한 클러스터 처리 가능
- 고차원 데이터에서는 차원의 저주 문제 발생
- 밀도가 크게 다른 클러스터가 있는 경우 파라미터 설정이 어려움

**코드 예시**:

```python
from sklearn.cluster import DBSCAN
from sklearn.neighbors import NearestNeighbors
import matplotlib.pyplot as plt
import numpy as np

# DBSCAN 클러스터링
dbscan = DBSCAN(
    eps=0.5,              # 이웃 반경
    min_samples=5,        # 핵심 포인트 기준 최소 이웃 수
    metric='euclidean',   # 거리 측정 방식
    algorithm='auto',     # 최근접 이웃 검색 알고리즘
    n_jobs=-1             # 병렬 처리에 사용할 코어 수
)
cluster_labels = dbscan.fit_predict(X)

# 결과 확인
n_clusters = len(set(cluster_labels)) - (1 if -1 in cluster_labels else 0)
n_noise = list(cluster_labels).count(-1)

print(f"클러스터 개수: {n_clusters}")
print(f"노이즈 포인트 개수: {n_noise}")

# 각 클러스터별 포인트 수 확인
unique_labels, counts = np.unique(cluster_labels, return_counts=True)
for label, count in zip(unique_labels, counts):
    if label == -1:
        print(f"노이즈: {count}개 포인트")
    else:
        print(f"클러스터 {label}: {count}개 포인트")

# 최적의 eps 찾기 (K-거리 그래프)
def find_optimal_eps(X, n_neighbors=5):
    neighbors = NearestNeighbors(n_neighbors=n_neighbors)
    neighbors_fit = neighbors.fit(X)
    distances, indices = neighbors_fit.kneighbors(X)
    
    # 각 포인트의 평균 거리 계산
    distances = np.sort(distances[:, n_neighbors-1])
    
    plt.figure(figsize=(10, 6))
    plt.plot(range(len(distances)), distances)
    plt.grid(True)
    plt.xlabel('포인트 (오름차순 정렬)')
    plt.ylabel(f'{n_neighbors}번째 최근접 이웃까지의 거리')
    plt.title('K-거리 그래프')
    
    # 기울기 계산하여 변곡점 찾기
    slopes = np.gradient(distances)
    plt.figure(figsize=(10, 6))
    plt.plot(range(len(slopes)), slopes)
    plt.grid(True)
    plt.xlabel('포인트 (오름차순 정렬)')
    plt.ylabel('기울기')
    plt.title('K-거리 기울기 그래프')
    
    return distances

# 최적 eps 찾기
distances = find_optimal_eps(X, n_neighbors=5)

# 클러스터링 결과 2D 시각화 (PCA로 차원 축소 후)
from sklearn.decomposition import PCA

pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)

plt.figure(figsize=(10, 8))
colors = plt.cm.nipy_spectral(np.linspace(0, 1, len(set(cluster_labels))))
for label, color in zip(set(cluster_labels), colors):
    if label == -1:
        # 노이즈 포인트
        mask = cluster_labels == label
        plt.scatter(X_pca[mask, 0], X_pca[mask, 1], 
                    c='black', marker='x', s=50, label='노이즈')
    else:
        # 클러스터 포인트
        mask = cluster_labels == label
        plt.scatter(X_pca[mask, 0], X_pca[mask, 1], 
                    c=[color], alpha=0.7, label=f'클러스터 {label}')

plt.title('DBSCAN 클러스터링 결과 (PCA로 축소)')
plt.legend()
plt.grid(True)
plt.show()
```

**개념의 활용**:

- 불규칙한 모양의 클러스터 탐색
- 이상치/노이즈 탐지
- 공간 데이터 분석
- 밀도 기반 이상치 탐지
- 교통 패턴 분석
- 이미지 세분화
- 도시 계획 및 지리적 클러스터링

### 계층적 군집 (Hierarchical Clustering)

**정의**: 데이터 포인트를 계층적 트리 구조로 군집화하는 알고리즘으로, 상향식(병합적) 또는 하향식(분할적) 접근 방식 사용

**수식**:

- 거리 측정 방법:
    - 단일 연결법(Single Linkage): $d(C_i, C_j) = \min_{x \in C_i, y \in C_j} d(x, y)$
    - 완전 연결법(Complete Linkage): $d(C_i, C_j) = \max_{x \in C_i, y \in C_j} d(x, y)$
    - 평균 연결법(Average Linkage): $d(C_i, C_j) = \frac{1}{|C_i||C_j|} \sum_{x \in C_i} \sum_{y \in C_j} d(x, y)$
    - 와드 연결법(Ward's Linkage): 클러스터 내 분산을 최소화

**특징**:

- 계층적 구조를 제공하여 다양한 수준의 클러스터링 분석 가능
- 클러스터 개수를 사전에 지정할 필요 없음
- 덴드로그램을 통한 시각적 해석 용이
- 계산 복잡도가 높음 (O(n³))
- 큰 데이터셋에는 비효율적
- 이상치에 민감할 수 있음

**코드 예시**:

```python
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import dendrogram, linkage
import matplotlib.pyplot as plt
import numpy as np

# 계층적 클러스터링 (병합적 방식)
hc = AgglomerativeClustering(
    n_clusters=3,               # 클러스터 개수 (None으로 설정하면 distance_threshold 사용)
    affinity='euclidean',       # 거리 측정 방식
    linkage='ward',             # 연결 방식: 'ward', 'complete', 'average', 'single'
    # distance_threshold=None,  # 클러스터 병합을 중단할 거리 임계값
)
cluster_labels = hc.fit_predict(X)

# 결과 확인
print(f"클러스터 개수: {len(set(cluster_labels))}")

# 각 클러스터별 포인트 수 확인
unique_labels, counts = np.unique(cluster_labels, return_counts=True)
for label, count in zip(unique_labels, counts):
    print(f"클러스터 {label}: {count}개 포인트")

# 덴드로그램 그리기 (샘플 수가 많을 경우 일부만 표시)
def plot_dendrogram(X, max_samples=100):
    # 데이터가 너무 크면 샘플링
    if X.shape[0] > max_samples:
        idx = np.random.choice(X.shape[0], max_samples, replace=False)
        X_sample = X[idx]
    else:
        X_sample = X
    
    # 연결 행렬 계산
    Z = linkage(X_sample, method='ward')
    
    # 덴드로그램 그리기
    plt.figure(figsize=(12, 8))
    dendrogram(
        Z,
        leaf_rotation=90.,  # 잎 레이블 회전
        leaf_font_size=10.,  # 잎 레이블 폰트 크기
    )
    plt.title('Hierarchical Clustering Dendrogram')
    plt.xlabel('Sample index or (cluster size)')
    plt.ylabel('Distance')
    
    # 특정 거리에서 클러스터 표시
    if max_d:
        plt.axhline(y=max_d, c='k', linestyle='--', label=f'threshold: {max_d}')
        plt.legend()
    
    plt.show()

# 덴드로그램 출력
plot_dendrogram(X)

# 클러스터링 결과 2D 시각화 (PCA로 차원 축소 후)
from sklearn.decomposition import PCA

pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)

plt.figure(figsize=(10, 8))
scatter = plt.scatter(X_pca[:, 0], X_pca[:, 1], c=cluster_labels, cmap='viridis', alpha=0.7)
plt.legend(*scatter.legend_elements(), title="클러스터")
plt.title('계층적 클러스터링 결과 (PCA로 축소)')
plt.grid(True)
plt.show()

# 다양한 연결법 비교
linkage_methods = ['single', 'complete', 'average', 'ward']
fig, axes = plt.subplots(2, 2, figsize=(15, 10))
axes = axes.flatten()

for i, method in enumerate(linkage_methods):
    hc = AgglomerativeClustering(n_clusters=3, linkage=method)
    labels = hc.fit_predict(X)
    
    # 산점도 그리기
    axes[i].scatter(X_pca[:, 0], X_pca[:, 1], c=labels, cmap='viridis', alpha=0.7)
    axes[i].set_title(f'연결법: {method}')
    axes[i].grid(True)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 생물학적 분류 체계 구축
- 서열화된 데이터 분석
- 계통도 분석
- 시장 세분화
- 문서 분류
- 유전자 발현 데이터 분석
- 클러스터 관계 구조 이해

#### 덴드로그램

**정의**: 계층적 클러스터링의 결과를 시각화한 트리 다이어그램으로, 클러스터가 어떻게 병합되거나 분할되는지 보여줌

**특징**:

- 클러스터 간 계층적 관계 시각화
- 클러스터 간 거리(유사도) 표현
- 적절한 클러스터 개수 결정에 도움
- 클러스터의 형성 과정 이해 가능
- 가지(branch)의 길이는 클러스터 간 거리를 나타냄
- 수평선으로 자르면 해당 높이에서의 클러스터 구성 확인 가능

**코드 예시**:

```python
from scipy.cluster.hierarchy import dendrogram, linkage, fcluster
import matplotlib.pyplot as plt
import numpy as np

# 연결 행렬 계산
Z = linkage(X, method='ward')

# 덴드로그램 그리기
plt.figure(figsize=(12, 8))
plt.title('Hierarchical Clustering Dendrogram')
plt.xlabel('Sample index')
plt.ylabel('Distance')

# 기본 덴드로그램
dendrogram(
    Z,
    truncate_mode=None,  # 'lastp'로 설정하면 마지막 p개 클러스터만 표시
    p=10,                # truncate_mode='lastp'일 때 표시할 클러스터 수
    leaf_rotation=90.,   # 잎 레이블 회전
    leaf_font_size=8.,   # 잎 레이블 폰트 크기
    show_contracted=True,  # 압축된 하위 클러스터 표시
)

# 클러스터 개수 결정을 위한 임계값 표시
threshold = 5  # 임의의 값, 실제로는 덴드로그램을 보고 결정
plt.axhline(y=threshold, c='crimson', linestyle='--', label=f'threshold: {threshold}')
plt.legend()

plt.tight_layout()
plt.show()

# 임계값에 따른 클러스터 구성 확인
clusters = fcluster(Z, t=threshold, criterion='distance')
n_clusters = len(np.unique(clusters))
print(f"임계값 {threshold}에서의 클러스터 개수: {n_clusters}")

# 다양한 레벨에서의 클러스터 수 확인
thresholds = np.linspace(1, 10, 10)
n_clusters_list = []

for t in thresholds:
    clusters = fcluster(Z, t=t, criterion='distance')
    n_clusters_list.append(len(np.unique(clusters)))

plt.figure(figsize=(10, 6))
plt.plot(thresholds, n_clusters_list, 'o-')
plt.xlabel('Distance Threshold')
plt.ylabel('Number of Clusters')
plt.title('Number of Clusters vs. Distance Threshold')
plt.grid(True)
plt.xticks(thresholds)
plt.show()

# 색상 임계값을 사용한 향상된 덴드로그램
plt.figure(figsize=(12, 8))
plt.title('Enhanced Dendrogram with Color Threshold')
plt.xlabel('Sample index')
plt.ylabel('Distance')

dendrogram(
    Z,
    truncate_mode=None,
    color_threshold=threshold,  # 이 값 이상의 거리는 다른 색상으로 표시
    above_threshold_color='grey',
    leaf_rotation=90.,
    leaf_font_size=8.,
)

plt.axhline(y=threshold, c='crimson', linestyle='--', label=f'threshold: {threshold}')
plt.legend()
plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 계층적 클러스터링 결과 시각화
- 최적 클러스터 개수 결정
- 클러스터 간 관계 분석
- 분류학적 구조 표현
- 유전자 발현 패턴 분석
- 계통 발생학적 트리 구성
- 고객 세분화 결과 시각화
---
# 데이터마이닝기타 알고리즘 정리

## 연관분석 (장바구니분석)

**정의**: 대량의 데이터에서 항목 간의 관계를 파악하여 "만약 A가 있다면 B도 있다"와 같은 패턴을 찾아내는 알고리즘으로, 주로 장바구니 데이터나 트랜잭션 데이터에 적용됨

**수식**:

- 지지도(Support): $Support(A \Rightarrow B) = \frac{N(A \cap B)}{N}$ (전체 트랜잭션 중 A와 B를 모두 포함하는 비율)
- 신뢰도(Confidence): $Confidence(A \Rightarrow B) = \frac{N(A \cap B)}{N(A)}$ (A를 포함하는 트랜잭션 중 B도 포함하는 비율)
- 향상도(Lift): $Lift(A \Rightarrow B) = \frac{Confidence(A \Rightarrow B)}{Support(B)} = \frac{P(A \cap B)}{P(A) \times P(B)}$ (독립성 측정)

**특징**:

- 상품 간 연관성을 수치화하여 분석
- 최소 지지도 기준을 만족하는 빈발 항목집합 탐색
- 향상도가 1보다 크면 양의 상관관계, 1이면 독립, 1보다 작으면 음의 상관관계
- 계산 복잡도가 높을 수 있음(트랜잭션 데이터 크기에 따라)
- 희소 행렬 형태의 데이터에 효과적
- 해석이 직관적이고 비즈니스 인사이트로 연결이 쉬움

**코드 예시**:

```python
from mlxtend.frequent_patterns import apriori, association_rules
import pandas as pd

# 예시 데이터 (장바구니 데이터)
# 각 행은 하나의 트랜잭션, 각 열은 상품 여부
data = {
    'Transaction': [1, 2, 3, 4, 5],
    'bread': [1, 1, 1, 0, 1],
    'milk': [1, 1, 0, 1, 0],
    'butter': [1, 0, 1, 0, 1],
    'beer': [0, 1, 0, 1, 1]
}
df = pd.DataFrame(data)
df = df.set_index('Transaction')

# 빈발 항목 집합 찾기 (Apriori 알고리즘)
frequent_itemsets = apriori(df, 
                           min_support=0.3,      # 최소 지지도
                           use_colnames=True,    # 열 이름 사용
                           max_len=3)            # 최대 항목 집합 크기

print("빈발 항목 집합:")
print(frequent_itemsets)

# 연관 규칙 생성
rules = association_rules(frequent_itemsets, 
                         metric="confidence",    # 규칙 정렬 기준
                         min_threshold=0.7)      # 최소 신뢰도

# 결과 정리
rules = rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']]
rules = rules.sort_values('lift', ascending=False)

print("\n연관 규칙:")
print(rules)

# 시각화
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(10, 6))
sns.scatterplot(x='support', y='confidence', size='lift', data=rules, sizes=(50, 200))

# 향상도가 높은 규칙에 주석 추가
for i, row in rules.head(3).iterrows():
    plt.annotate(f"{list(row['antecedents'])} => {list(row['consequents'])}",
                xy=(row['support'], row['confidence']),
                xytext=(10, 10),
                textcoords='offset points',
                arrowprops=dict(arrowstyle='->', connectionstyle='arc3,rad=.2'))

plt.title('Support vs Confidence with Lift as Size')
plt.xlabel('Support')
plt.ylabel('Confidence')
plt.grid(True, alpha=0.3)
plt.show()
```

**개념의 활용**:

- 소매업의 상품 추천 및 진열 전략
- 온라인 쇼핑몰 "함께 구매한 상품" 추천
- 교차 판매(Cross-selling) 및 상향 판매(Up-selling) 전략 수립
- 웹사이트 방문 경로 분석
- 의료 데이터에서 질병과 증상 간 관계 파악
- 은행 거래에서 이상 패턴 탐지
- 도서관/서점의 도서 추천 시스템

### Apriori 알고리즘

**정의**: 연관 규칙 마이닝을 위한 가장 기본적인 알고리즘으로, 빈발 항목 집합을 찾기 위해 레벨별 너비 우선 탐색(breadth-first search)과 프루닝(pruning)을 사용하는 방법

**수식**:

- 핵심 원리: "모든 빈발 항목 집합의 부분집합도 빈발해야 한다"
- 부분집합 지지도: $Support(X) \geq Support(Y)$ (X가 Y의 부분집합인 경우)

**특징**:

- 단계적 접근 방식(후보 생성 → 지지도 계산 → 필터링)
- 후보 생성 및 검증 과정에서 프루닝을 통한 계산량 감소
- 작은 항목 집합부터 큰 항목 집합으로 점진적 확장
- 여러 번의 데이터 스캔이 필요하여 대용량 데이터에 비효율적
- 구현이 간단하고 이해하기 쉬움
- 최소 지지도 설정에 따라 성능과 결과가 크게 달라짐

**코드 예시**:

```python
from mlxtend.frequent_patterns import apriori, association_rules
import pandas as pd
import time

# 트랜잭션 데이터 생성 (예시)
transactions = [
    ['beer', 'nuts', 'cheese'],
    ['beer', 'nuts', 'jam'],
    ['beer', 'chips'],
    ['nuts', 'cheese', 'jam'],
    ['beer', 'nuts', 'cheese', 'jam'],
    ['beer', 'nuts', 'jam'],
    ['nuts', 'jam'],
    ['beer', 'nuts', 'cheese', 'chips'],
    ['beer', 'chips', 'jam'],
    ['nuts', 'cheese', 'chips']
]

# 원-핫 인코딩으로 변환
def one_hot_encode(transactions):
    items = list(set([item for sublist in transactions for item in sublist]))
    one_hot = []
    for transaction in transactions:
        row = [1 if item in transaction else 0 for item in items]
        one_hot.append(row)
    return pd.DataFrame(one_hot, columns=items)

# 데이터 변환
df = one_hot_encode(transactions)

# Apriori 알고리즘 실행 시간 측정
start_time = time.time()
frequent_itemsets = apriori(df, 
                           min_support=0.2,      # 최소 지지도
                           use_colnames=True,    # 열 이름 사용
                           max_len=4)            # 최대 항목 집합 크기
end_time = time.time()

print(f"Apriori 알고리즘 실행 시간: {end_time - start_time:.4f}초")
print(f"발견된 빈발 항목 집합 수: {len(frequent_itemsets)}")
print(frequent_itemsets.sort_values(by='support', ascending=False).head(10))

# 다양한 최소 지지도에 따른 빈발 항목 집합 수 비교
support_thresholds = [0.1, 0.2, 0.3, 0.4, 0.5]
results = []

for threshold in support_thresholds:
    start = time.time()
    itemsets = apriori(df, min_support=threshold, use_colnames=True)
    end = time.time()
    results.append({
        'min_support': threshold,
        'num_itemsets': len(itemsets),
        'execution_time': end - start
    })

results_df = pd.DataFrame(results)
print("\n최소 지지도별 결과:")
print(results_df)

# 연관 규칙 도출
rules = association_rules(frequent_itemsets, 
                         metric="confidence",    # 규칙 정렬 기준
                         min_threshold=0.6)      # 최소 신뢰도

# 결과 출력 (상위 5개 규칙)
print("\n상위 5개 연관 규칙 (향상도 기준):")
cols = ['antecedents', 'consequents', 'support', 'confidence', 'lift']
print(rules[cols].sort_values('lift', ascending=False).head(5))
```

**개념의 활용**:

- 마켓 바스켓 분석
- 웹 사용 패턴 분석
- 유전자 발현 데이터 분석
- 텍스트 문서의 주제 연관성 분석
- 의료 진단 연관성 분석
- 보안 침입 탐지 패턴 분석
- 소셜 네트워크 관계 분석

### FP-Growth 알고리즘

**정의**: 연관 규칙 마이닝을 위한 효율적인 알고리즘으로, 빈발 패턴 트리(FP-Tree)라는 압축된 데이터 구조를 구축하여 데이터베이스를 여러 번 스캔하지 않고 빈발 항목 집합을 찾는 방법

**수식**:

- FP-Tree는 항목들의 빈도 기반 접두사 트리 구조
- 조건부 패턴 기반(Conditional Pattern Base)을 통한 재귀적 성장

**특징**:

- 데이터베이스 전체를 단 두 번만 스캔 (Apriori보다 효율적)
- 후보 항목 집합을 생성하지 않음
- 압축된 데이터 구조를 사용하여 메모리 효율적
- 희소 데이터셋에 매우 효과적
- 구현이 복잡하지만 대규모 데이터셋에서 성능이 우수
- 빈도 기반 정렬을 통해 자주 발생하는 패턴 우선 탐색

**코드 예시**:

```python
from mlxtend.frequent_patterns import fpgrowth, association_rules
import pandas as pd
import time

# 위의 트랜잭션 데이터와 원-핫 인코딩 함수 재사용
# (이전 코드에서 정의한 transactions와 df 사용)

# FP-Growth 알고리즘 실행 시간 측정
start_time = time.time()
fp_frequent_itemsets = fpgrowth(df, 
                               min_support=0.2,       # 최소 지지도
                               use_colnames=True,     # 열 이름 사용
                               max_len=4)             # 최대 항목 집합 크기
end_time = time.time()

print(f"FP-Growth 알고리즘 실행 시간: {end_time - start_time:.4f}초")
print(f"발견된 빈발 항목 집합 수: {len(fp_frequent_itemsets)}")
print(fp_frequent_itemsets.sort_values(by='support', ascending=False).head(10))

# Apriori와 FP-Growth 비교 (다양한 최소 지지도에서)
support_thresholds = [0.1, 0.2, 0.3, 0.4, 0.5]
comparison = []

for threshold in support_thresholds:
    # Apriori
    start = time.time()
    apriori_itemsets = apriori(df, min_support=threshold, use_colnames=True)
    apriori_time = time.time() - start
    
    # FP-Growth
    start = time.time()
    fpgrowth_itemsets = fpgrowth(df, min_support=threshold, use_colnames=True)
    fpgrowth_time = time.time() - start
    
    comparison.append({
        'min_support': threshold,
        'num_itemsets': len(apriori_itemsets),  # 항목 집합 수는 동일해야 함
        'apriori_time': apriori_time,
        'fpgrowth_time': fpgrowth_time,
        'speedup': apriori_time / fpgrowth_time if fpgrowth_time > 0 else float('inf')
    })

comparison_df = pd.DataFrame(comparison)
print("\nApriori와 FP-Growth 비교:")
print(comparison_df)

# FP-Growth에서 추출한 연관 규칙
fp_rules = association_rules(fp_frequent_itemsets, 
                           metric="confidence",    # 규칙 정렬 기준
                           min_threshold=0.6)      # 최소 신뢰도

# 결과 출력 (상위 5개 규칙)
print("\nFP-Growth: 상위 5개 연관 규칙 (향상도 기준):")
cols = ['antecedents', 'consequents', 'support', 'confidence', 'lift']
print(fp_rules[cols].sort_values('lift', ascending=False).head(5))

# 시각화: Apriori와 FP-Growth 실행 시간 비교
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))
plt.bar(comparison_df['min_support'].astype(str) + ' (Apriori)', comparison_df['apriori_time'], 
        width=0.35, label='Apriori', alpha=0.7)
plt.bar(comparison_df['min_support'].astype(str) + ' (FP-Growth)', comparison_df['fpgrowth_time'], 
        width=0.35, label='FP-Growth', alpha=0.7)
plt.xlabel('최소 지지도')
plt.ylabel('실행 시간 (초)')
plt.title('Apriori vs FP-Growth 실행 시간 비교')
plt.legend()
plt.grid(axis='y', alpha=0.3)
plt.show()
```

**개념의 활용**:

- 대규모 트랜잭션 데이터 분석
- 실시간 추천 시스템
- 빅데이터 환경에서의 패턴 마이닝
- 웹 로그 분석
- 생물정보학 데이터 마이닝
- 네트워크 트래픽 패턴 분석
- 소셜 미디어 데이터에서 행동 패턴 발견

## 요인분석

**정의**: 관측된 변수들 간의 상관관계를 기반으로 직접 관찰할 수 없는 잠재 요인(factor)을 추출하여 데이터의 구조를 파악하는 통계적 기법

**수식**:

- 요인 모델: $X = \Lambda F + \varepsilon$
- $X$는 관측 변수 벡터, $\Lambda$는 요인 적재량 행렬, $F$는 잠재 요인 벡터, $\varepsilon$는 오차 항

**특징**:

- 여러 변수들 간의 상관관계를 설명하는 잠재 요인을 식별
- 데이터의 차원을 줄이면서 변수 간의 구조적 관계 파악
- 회전(rotation) 방법을 통해 해석 가능성 향상
- KMO(Kaiser-Meyer-Olkin) 검정으로 적합성 평가
- 바틀렛(Bartlett)의 구형성 검정으로 상관관계 유의성 확인
- 고유값(eigenvalue) > 1인 요인 추출 (카이저 규칙)
- 공통성(communality)과 특이성(uniqueness)으로 변수 평가

**코드 예시**:

```python
from factor_analyzer import FactorAnalyzer
from factor_analyzer.factor_analyzer import calculate_kmo, calculate_bartlett_sphericity
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from sklearn.datasets import load_iris

# 예시 데이터로 iris 데이터셋 사용
iris = load_iris()
X = pd.DataFrame(iris.data, columns=iris.feature_names)

# 요인분석을 위한 적합성 검정
# 1. 바틀렛의 구형성 검정
chi_square_value, p_value = calculate_bartlett_sphericity(X)
print(f"바틀렛 구형성 검정: chi²값={chi_square_value:.2f}, p값={p_value:.10f}")

# 2. KMO(Kaiser-Meyer-Olkin) 검정
kmo_all, kmo_model = calculate_kmo(X)
print(f"KMO 검정: {kmo_model:.3f}")

# 스크리 플롯을 통한 요인 수 결정
fa = FactorAnalyzer()
fa.fit(X)
ev, v = fa.get_eigenvalues()

plt.figure(figsize=(10, 6))
plt.scatter(range(1, X.shape[1] + 1), ev)
plt.plot(range(1, X.shape[1] + 1), ev, 'ro-')
plt.title('Scree Plot')
plt.xlabel('요인')
plt.ylabel('고유값')
plt.grid(True)
plt.axhline(y=1, color='r', linestyle='--', alpha=0.7)  # 고유값 1 기준선
plt.show()

# 최적 요인 수 적용 (고유값 > 1 기준)
n_factors = sum(ev > 1)
print(f"추출할 요인 수 (고유값 > 1): {n_factors}")

# 요인분석 수행 (회전 적용)
fa = FactorAnalyzer(n_factors=n_factors, rotation='varimax')
fa.fit(X)

# 요인 적재량(factor loadings) 확인
loadings = pd.DataFrame(fa.loadings_, index=X.columns)
print("\n요인 적재량:")
print(loadings)

# 공통성(communality) 확인
communalities = pd.DataFrame(fa.get_communalities(), index=X.columns, columns=['공통성'])
communalities['특이성'] = 1 - communalities['공통성']
print("\n공통성 및 특이성:")
print(communalities)

# 설명된 분산 확인
variance = pd.DataFrame(fa.get_factor_variance(), 
                       index=['SS 적재량', '비율', '누적 비율'],
                       columns=[f'요인 {i+1}' for i in range(n_factors)])
print("\n설명된 분산:")
print(variance)

# 요인점수(factor scores) 계산
factor_scores = fa.transform(X)
factor_scores_df = pd.DataFrame(factor_scores, 
                             columns=[f'요인 {i+1}' for i in range(n_factors)])

# 요인 적재량 시각화 (히트맵)
import seaborn as sns

plt.figure(figsize=(10, 8))
sns.heatmap(loadings, annot=True, cmap='coolwarm', vmin=-1, vmax=1)
plt.title('요인 적재량 히트맵')
plt.show()

# 요인 공간에서의 변수 위치 시각화 (2개 요인인 경우)
if n_factors >= 2:
    plt.figure(figsize=(10, 8))
    for i, feature in enumerate(X.columns):
        plt.arrow(0, 0, loadings.iloc[i, 0], loadings.iloc[i, 1], 
                 head_width=0.05, head_length=0.05, fc='blue', ec='blue')
        plt.text(loadings.iloc[i, 0] * 1.15, loadings.iloc[i, 1] * 1.15, feature, 
                color='green', ha='center', va='center')
    
    plt.xlim(-1, 1)
    plt.ylim(-1, 1)
    plt.grid(True)
    plt.axhline(y=0, color='k', linestyle='-', alpha=0.3)
    plt.axvline(x=0, color='k', linestyle='-', alpha=0.3)
    plt.title('요인 공간에서의 변수 위치')
    plt.xlabel(f'요인 1 ({variance.iloc[1, 0]:.1%})')
    plt.ylabel(f'요인 2 ({variance.iloc[1, 1]:.1%})')
    circle = plt.Circle((0,0), radius=1, fill=False, color='red', linestyle='--')
    plt.gca().add_patch(circle)
    plt.show()
```

**개념의 활용**:

- 심리학적 검사 도구 개발 및 검증
- 설문조사 데이터 분석 및 잠재 구성 개념 파악
- 시장 세분화 및 소비자 행동 분석
- 재무 분석 및 주가 동향 예측
- 사회과학 연구에서 잠재 변인 발견
- 교육 분야에서 학습 성취도 요인 분석
- 생물학적 데이터에서 유전적 요인 식별

# 모델 학습 알고리즘 정리

## 앙상블 투표 (Soft/Hard Voting)

**정의**: 여러 모델의 예측을 결합하여 최종 예측을 생성하는 앙상블 기법으로, 하드 보팅은 다수결 원칙을 적용하고 소프트 보팅은 확률 평균을 이용

**수식**:

- 하드 보팅: $\hat{y} = mode(y_1, y_2, ..., y_n)$ (다수결)
- 소프트 보팅: $\hat{y} = argmax_i \sum_{j=1}^{n} p_j(y = i)$ (확률 평균)

**특징**:

- 다양한 모델의 장점을 결합하여 일반화 성능 향상
- 모델 편향 감소 및 분산 감소 효과
- 과적합 위험 감소
- 이상치나 노이즈에 강건함
- 개별 모델보다 안정적인 예측 제공
- 다양한 알고리즘 조합으로 구성 가능
- 소프트 보팅은 확률을 제공하는 모델에만 적용 가능

**코드 예시**:

```python
from sklearn.ensemble import VotingClassifier, VotingRegressor
from sklearn.linear_model import LogisticRegression, LinearRegression
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor
from sklearn.svm import SVC, SVR
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score, mean_squared_error
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_breast_cancer, load_boston
import numpy as np

# ===== 분류 문제 (하드/소프트 보팅) =====
# 데이터 로드 및 분할
X, y = load_breast_cancer(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 개별 분류기 정의
log_clf = LogisticRegression(random_state=42)
tree_clf = DecisionTreeClassifier(random_state=42)
svm_clf = SVC(probability=True, random_state=42)  # 소프트 보팅 위해 확률 활성화
nb_clf = GaussianNB()

# 하드 보팅 앙상블
hard_voting = VotingClassifier(
    estimators=[
        ('lr', log_clf),
        ('dt', tree_clf),
        ('svm', svm_clf),
        ('nb', nb_clf)
    ],
    voting='hard'  # 하드 보팅 (다수결)
)

# 소프트 보팅 앙상블
soft_voting = VotingClassifier(
    estimators=[
        ('lr', log_clf),
        ('dt', tree_clf),
        ('svm', svm_clf),
        ('nb', nb_clf)
    ],
    voting='soft'  # 소프트 보팅 (확률 평균)
)

# 학습 및 평가
models = {
    'Logistic Regression': log_clf,
    'Decision Tree': tree_clf,
    'SVM': svm_clf,
    'Naive Bayes': nb_clf,
    'Hard Voting': hard_voting,
    'Soft Voting': soft_voting
}

results = {}
for name, model in models.items():
    model.fit(X_train, y_train)
    y_pred = model.predict(X_test)
    accuracy = accuracy_score(y_test, y_pred)
    results[name] = accuracy
    print(f"{name} 정확도: {accuracy:.4f}")

# 소프트 보팅과 하드 보팅 비교 시각화
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.bar(results.keys(), results.values(), color=['blue', 'green', 'red', 'purple', 'orange', 'brown'])
plt.axhline(y=max(results.values()), color='r', linestyle='--', alpha=0.5)
plt.title('모델별 정확도 비교')
plt.xlabel('모델')
plt.ylabel('정확도')
plt.ylim(min(results.values()) - 0.05, max(results.values()) + 0.05)
plt.grid(axis='y', alpha=0.3)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# ===== 회귀 문제 (가중치 적용 보팅) =====
# 데이터 로드 및 분할
X, y = load_boston(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 개별 회귀 모델 정의
lr_reg = LinearRegression()
dt_reg = DecisionTreeRegressor(random_state=42)
svr_reg = SVR(kernel='linear')

# 가중치 적용 보팅 앙상블
weighted_voting = VotingRegressor(
    estimators=[
        ('lr', lr_reg),
        ('dt', dt_reg),
        ('svr', svr_reg)
    ],
    weights=[2, 1, 1]  # 선형 회귀에 더 높은 가중치 부여
)

# 학습 및 평가
reg_models = {
    'Linear Regression': lr_reg,
    'Decision Tree': dt_reg,
    'SVR': svr_reg,
    'Weighted Voting': weighted_voting
}

reg_results = {}
for name, model in reg_models.items():
    model.fit(X_train, y_train)
    y_pred = model.predict(X_test)
    mse = mean_squared_error(y_test, y_pred)
    rmse = np.sqrt(mse)
    reg_results[name] = rmse
    print(f"{name} RMSE: {rmse:.4f}")

# 회귀 모델 비교 시각화
plt.figure(figsize=(12, 6))
plt.bar(reg_results.keys(), reg_results.values(), color=['blue', 'green', 'red', 'orange'])
plt.axhline(y=min(reg_results.values()), color='r', linestyle='--', alpha=0.5)
plt.title('모델별 RMSE 비교 (낮을수록 좋음)')
plt.xlabel('모델')
plt.ylabel('RMSE')
plt.grid(axis='y', alpha=0.3)
plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 분류 문제에서 정확도 향상
- 모델 불확실성 감소를 위한 결합
- 다양한 알고리즘의 강점 활용
- 상이한 데이터 소스를 분석하는 모델 결합
- 노이즈가 많은 데이터에서 안정적 예측
- 이상치에 강건한 모델 구축
- 신뢰도가 중요한 의사결정 분야 (의료, 금융 등)

## 그리드 서치 (Grid Search)

**정의**: 모델의 하이퍼파라미터 최적화를 위해 가능한 모든 하이퍼파라미터 조합을 평가하는 기법으로, 탐색 공간을 격자(그리드) 형태로 지정하여 교차 검증을 통해 최적 조합을 찾음

**수식**:

- 목적: $\theta^* = \arg\min_{\theta \in \Theta} CV(\theta)$
- $\theta$는 하이퍼파라미터 집합, $CV(\theta)$는 교차 검증 점수

**특징**:

- 모든 하이퍼파라미터 조합을 체계적으로 탐색
- K-fold 교차 검증과 함께 사용되어 과적합 방지
- 철저한 탐색으로 최적 조합 발견 가능성 높음
- 파라미터 공간이 커질수록 계산 비용 기하급수적 증가
- 병렬 처리 지원으로 효율성 향상 가능
- 하이퍼파라미터 간 상호작용 파악 가능
- 최적 모델 선택 과정 자동화

**코드 예시**:

```python
from sklearn.model_selection import GridSearchCV, train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC
from sklearn.metrics import classification_report, confusion_matrix
from sklearn.datasets import load_iris
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
import time

# 데이터 로드 및 분할
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# === SVM 모델 그리드서치 ===
# 하이퍼파라미터 그리드 정의
param_grid_svm = {
    'C': [0.1, 1, 10, 100],
    'gamma': [0.001, 0.01, 0.1, 1],
    'kernel': ['rbf', 'poly', 'sigmoid']
}

# 그리드서치 객체 생성
grid_svm = GridSearchCV(
    estimator=SVC(),
    param_grid=param_grid_svm,
    cv=5,                   # 5-fold 교차검증
    scoring='accuracy',     # 평가 지표
    n_jobs=-1,              # 모든 CPU 코어 사용
    verbose=1               # 진행 상황 출력
)

# 그리드서치 수행 (시간 측정)
start_time = time.time()
grid_svm.fit(X_train, y_train)
end_time = time.time()

print(f"SVM 그리드서치 실행 시간: {end_time - start_time:.2f}초")
print(f"최적 하이퍼파라미터: {grid_svm.best_params_}")
print(f"최고 교차 검증 점수: {grid_svm.best_score_:.4f}")

# 최적 모델로 테스트 세트 평가
best_svm = grid_svm.best_estimator_
y_pred = best_svm.predict(X_test)
print("\n테스트 세트 분류 보고서:")
print(classification_report(y_test, y_pred))

# === 랜덤 포레스트 모델 그리드서치 ===
# 하이퍼파라미터 그리드 정의
param_grid_rf = {
    'n_estimators': [50, 100, 200],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10],
    'min_samples_leaf': [1, 2, 4]
}

# 그리드서치 객체 생성
grid_rf = GridSearchCV(
    estimator=RandomForestClassifier(random_state=42),
    param_grid=param_grid_rf,
    cv=5,
    scoring='accuracy',
    n_jobs=-1,
    verbose=1
)

# 그리드서치 수행
start_time = time.time()
grid_rf.fit(X_train, y_train)
end_time = time.time()

print(f"\n랜덤 포레스트 그리드서치 실행 시간: {end_time - start_time:.2f}초")
print(f"최적 하이퍼파라미터: {grid_rf.best_params_}")
print(f"최고 교차 검증 점수: {grid_rf.best_score_:.4f}")

# 그리드서치 결과 분석
results_df = pd.DataFrame(grid_svm.cv_results_)

# C와 gamma에 따른 평균 점수 시각화 (커널 = rbf인 경우)
rbf_results = results_df[results_df['param_kernel'] == 'rbf']
pivot_table = rbf_results.pivot_table(
    values='mean_test_score', 
    index='param_gamma', 
    columns='param_C'
)

plt.figure(figsize=(10, 8))
sns.heatmap(pivot_table, annot=True, cmap='viridis', fmt='.3f')
plt.title('C와 gamma에 따른 교차 검증 점수 (커널 = rbf)')
plt.ylabel('gamma')
plt.xlabel('C')
plt.show()

# 파라미터 중요도 분석 (랜덤 포레스트)
rf_results = pd.DataFrame(grid_rf.cv_results_)

# 각 파라미터별 평균 점수 비교
param_names = ['param_n_estimators', 'param_max_depth', 
               'param_min_samples_split', 'param_min_samples_leaf']

plt.figure(figsize=(15, 10))
for i, param in enumerate(param_names):
    plt.subplot(2, 2, i+1)
    param_values = rf_results[param].astype(str)
    sns.boxplot(x=param_values, y=rf_results['mean_test_score'])
    plt.title(f'{param} 값에 따른 교차 검증 점수')
    plt.xlabel(param.replace('param_', ''))
    plt.ylabel('교차 검증 점수')
    plt.xticks(rotation=45)
    plt.grid(axis='y', alpha=0.3)

plt.tight_layout()
plt.show()
```

**개념의 활용**:

- 최적 하이퍼파라미터 탐색
- 모델 성능 최대화
- 여러 알고리즘 비교 및 선택
- 자동화된 모델 조정
- 하이퍼파라미터 민감도 분석
- 상호작용 효과 탐색
- 모델 선택 프로세스 표준화

## K-Fold 교차 검증

**정의**: 데이터를 K개의 폴드(fold)로 나누어 순차적으로 각 폴드를 테스트 세트로 사용하고 나머지를 훈련 세트로 사용하는 모델 평가 기법으로, 모든 데이터 포인트가 한 번씩 테스트 세트에 포함되도록 함

**수식**:

- 교차 검증 점수: $CV_k = \frac{1}{k} \sum_{i=1}^{k} Score_i$
- $Score_i$는 i번째 폴드를 테스트 세트로 사용했을 때의 성능 점수

**특징**:

- 모든 데이터를 효율적으로 활용
- 과적합 감지 및 방지에 효과적
- 모델 일반화 성능 더 정확하게 추정
- 데이터 분포 변동에 강건한 평가
- 표준 편차 계산을 통한 모델 안정성 평가
- 계산 비용이 K배로 증가
- 층화(stratified) 방식으로 클래스 비율 유지 가능

**코드 예시**:

```python
from sklearn.model_selection import KFold, StratifiedKFold, cross_val_score, cross_validate
from sklearn.metrics import accuracy_score, roc_auc_score, mean_squared_error
from sklearn.linear_model import LogisticRegression, LinearRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC
from sklearn.datasets import load_breast_cancer, load_boston
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

# ===== 분류 문제 K-Fold 교차 검증 =====
# 데이터 로드
X, y = load_breast_cancer(return_X_y=True)

# 교차 검증 객체 정의
kf = KFold(n_splits=5, shuffle=True, random_state=42)
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)  # 클래스 비율 유지

# 모델 정의
models = {
    'Logistic Regression': LogisticRegression(max_iter=1000, random_state=42),
    'Random Forest': RandomForestClassifier(random_state=42),
    'SVM': SVC(probability=True, random_state=42)
}

# 다양한 평가 지표로 교차 검증 수행
metrics = ['accuracy', 'roc_auc', 'f1']
results = {}

for name, model in models.items():
    # 다중 지표 교차 검증
    cv_results = cross_validate(
        model, X, y,
        cv=skf,                       # 층화 K-폴드
        scoring=metrics,              # 여러 평가 지표
        return_train_score=True,      # 훈련 점수도 반환
        n_jobs=-1                     # 병렬 처리
    )
    
    # 결과 정리
    results[name] = {
        'test_accuracy': cv_results['test_accuracy'].mean(),
        'test_accuracy_std': cv_results['test_accuracy'].std(),
        'test_roc_auc': cv_results['test_roc_auc'].mean(),
        'test_f1': cv_results['test_f1'].mean(),
        'train_accuracy': cv_results['train_accuracy'].mean(),
        'train_test_diff': cv_results['train_accuracy'].mean() - cv_results['test_accuracy'].mean()
    }
    
    # 각 폴드별 점수 출력
    print(f"\n{name} 교차 검증 결과:")
    print(f"정확도: {cv_results['test_accuracy'].mean():.4f} (±{cv_results['test_accuracy'].std():.4f})")
    print(f"ROC AUC: {cv_results['test_roc_auc'].mean():.4f}")
    print(f"F1 점수: {cv_results['test_f1'].mean():.4f}")
    print(f"훈련-테스트 정확도 차이: {results[name]['train_test_diff']:.4f}")
    
    # 각 폴드별 점수 출력
    fold_scores = cv_results['test_accuracy']
    print(f"폴드별 정확도: {', '.join([f'{score:.4f}' for score in fold_scores])}")

# 결과 시각화: 정확도 비교
plt.figure(figsize=(12, 6))
models_list = list(results.keys())
acc_means = [results[m]['test_accuracy'] for m in models_list]
acc_stds = [results[m]['test_accuracy_std'] for m in models_list]

plt.bar(models_list, acc_means, yerr=acc_stds, capsize=10, alpha=0.7)
plt.title('모델별 교차 검증 정확도')
plt.ylabel('정확도')
plt.ylim(0.8, 1.0)  # 적절한 범위로 조정
plt.grid(axis='y', alpha=0.3)
plt.show()

# 훈련-테스트 점수 차이 비교 (과적합 확인)
plt.figure(figsize=(12, 6))
x = np.arange(len(models_list))
width = 0.35

train_scores = [results[m]['train_accuracy'] for m in models_list]
test_scores = [results[m]['test_accuracy'] for m in models_list]

plt.bar(x - width/2, train_scores, width, label='훈련 점수', alpha=0.7)
plt.bar(x + width/2, test_scores, width, label='테스트 점수', alpha=0.7)

plt.title('훈련-테스트 점수 비교')
plt.xticks(x, models_list)
plt.ylabel('정확도')
plt.legend()
plt.grid(axis='y', alpha=0.3)
plt.show()

# ===== 회귀 문제 K-Fold 교차 검증 =====
# 데이터 로드
X_reg, y_reg = load_boston(return_X_y=True)

# 교차 검증 객체 정의
kf_reg = KFold(n_splits=5, shuffle=True, random_state=42)

# 회귀 모델 정의 및 교차 검증
reg_model = LinearRegression()

# MSE와 R^2로 교차 검증
mse_scores = -cross_val_score(reg_model, X_reg, y_reg, 
                            cv=kf_reg, 
                            scoring='neg_mean_squared_error')
r2_scores = cross_val_score(reg_model, X_reg, y_reg, 
                          cv=kf_reg, 
                          scoring='r2')

# 결과 출력
print("\n선형 회귀 교차 검증 결과:")
print(f"MSE: {mse_scores.mean():.4f} (±{mse_scores.std():.4f})")
print(f"RMSE: {np.sqrt(mse_scores.mean()):.4f}")
print(f"R^2: {r2_scores.mean():.4f} (±{r2_scores.std():.4f})")
print(f"폴드별 MSE: {', '.join([f'{score:.4f}' for score in mse_scores])}")
```

**개념의 활용**:

- 제한된 데이터셋에서 신뢰할 수 있는 모델 평가
- 하이퍼파라미터 튜닝과 그리드서치
- 모델 선택 및 비교
- 과적합 탐지
- 모델 일반화 성능 추정
- 편향-분산 트레이드오프 평가
- 시계열 데이터 분석 (시간적 교차 검증)

## Hold-Out 검증

**정의**: 데이터를 훈련, 검증, 테스트 세트로 나누는 가장 기본적인 모델 평가 방법으로, 훈련 세트로 모델을 학습하고 검증 세트로 하이퍼파라미터를 튜닝한 후 최종적으로 테스트 세트로 성능을 평가

**수식**:

- 일반적인 분할 비율: 훈련(60-80%), 검증(10-20%), 테스트(10-20%)
- 테스트 오차: $Error_{test} = \frac{1}{n_{test}} \sum_{i=1}^{n_{test}} L(y_i, \hat{y}_i)$

**특징**:

- 간단하고 직관적인 방법
- 계산 효율성이 높음 (K-Fold보다 빠름)
- 대규모 데이터셋에 적합
- 모델 개발과 평가의 명확한 분리
- 데이터 낭비 가능성 (K-Fold에 비해)
- 결과가 분할 방식에 민감할 수 있음
- 층화 샘플링으로 클래스 불균형 문제 해결 가능

**코드 예시**:

```python
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report, mean_squared_error, r2_score
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression, LinearRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC
from sklearn.pipeline import Pipeline
from sklearn.datasets import load_breast_cancer, load_boston
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

# ===== 분류 문제 Hold-Out 검증 =====
# 데이터 로드
X, y = load_breast_cancer(return_X_y=True)

# Hold-Out 분할: 훈련(60%), 검증(20%), 테스트(20%)
X_train_val, X_test, y_train_val, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
X_train, X_val, y_train, y_val = train_test_split(X_train_val, y_train_val, test_size=0.25, random_state=42)  # 0.25 x 0.8 = 0.2

print(f"훈련 세트: {X_train.shape[0]} 샘플")
print(f"검증 세트: {X_val.shape[0]} 샘플")
print(f"테스트 세트: {X_test.shape[0]} 샘플")

# 모델 정의
models = {
    'Logistic Regression': LogisticRegression(max_iter=1000, random_state=42),
    'Random Forest': RandomForestClassifier(random_state=42),
    'SVM': SVC(probability=True, random_state=42)
}

# 하이퍼파라미터 후보
hyperparams = {
    'Logistic Regression': {'C': [0.1, 1, 10]},
    'Random Forest': {'n_estimators': [50, 100, 200]},
    'SVM': {'C': [0.1, 1, 10], 'gamma': [0.01, 0.1, 1]}
}

# Hold-Out 검증을 통한 모델 선택
best_models = {}
validation_results = {}

for name, model in models.items():
    best_score = 0
    best_params = None
    best_model = None
    
    print(f"\n=== {name} 하이퍼파라미터 튜닝 ===")
    
    # 하이퍼파라미터 튜닝 (간단한 그리드 서치)
    if name == 'Logistic Regression':
        for C in hyperparams[name]['C']:
            model.set_params(C=C)
            model.fit(X_train, y_train)
            val_score = model.score(X_val, y_val)
            print(f"C={C}, 검증 정확도: {val_score:.4f}")
            
            if val_score > best_score:
                best_score = val_score
                best_params = {'C': C}
                best_model = model.__class__(**best_params, max_iter=1000, random_state=42)
    
    elif name == 'Random Forest':
        for n_estimators in hyperparams[name]['n_estimators']:
            model.set_params(n_estimators=n_estimators)
            model.fit(X_train, y_train)
            val_score = model.score(X_val, y_val)
            print(f"n_estimators={n_estimators}, 검증 정확도: {val_score:.4f}")
            
            if val_score > best_score:
                best_score = val_score
                best_params = {'n_estimators': n_estimators}
                best_model = model.__class__(**best_params, random_state=42)
    
    elif name == 'SVM':
        for C in hyperparams[name]['C']:
            for gamma in hyperparams[name]['gamma']:
                model.set_params(C=C, gamma=gamma)
                model.fit(X_train, y_train)
                val_score = model.score(X_val, y_val)
                print(f"C={C}, gamma={gamma}, 검증 정확도: {val_score:.4f}")
                
                if val_score > best_score:
                    best_score = val_score
                    best_params = {'C': C, 'gamma': gamma}
                    best_model = model.__class__(**best_params, probability=True, random_state=42)
    
    # 최적 모델 저장
    best_models[name] = best_model
    validation_results[name] = {
        'best_params': best_params,
        'validation_score': best_score
    }
    
    print(f"최적 파라미터: {best_params}")
    print(f"최고 검증 점수: {best_score:.4f}")

# 테스트 세트에서 최종 평가
test_results = {}

for name, model in best_models.items():
    # 최적 모델 재학습 (훈련 + 검증 세트)
    model.fit(X_train_val, y_train_val)
    
    # 테스트 세트 예측
    y_pred = model.predict(X_test)
    test_acc = accuracy_score(y_test, y_pred)
    
    # 확률 기반 예측
    try:
        y_prob = model.predict_proba(X_test)[:, 1]
    except:
        y_prob = None
    
    # 결과 저장
    test_results[name] = {
        'accuracy': test_acc,
        'predictions': y_pred,
        'probabilities': y_prob
    }
    
    print(f"\n=== {name} 테스트 결과 ===")
    print(f"정확도: {test_acc:.4f}")
    print("분류 보고서:")
    print(classification_report(y_test, y_pred))

# 결과 비교 시각화
plt.figure(figsize=(10, 6))
models_list = list(models.keys())
val_scores = [validation_results[m]['validation_score'] for m in models_list]
test_scores = [test_results[m]['accuracy'] for m in models_list]

x = np.arange(len(models_list))
width = 0.35

plt.bar(x - width/2, val_scores, width, label='검증 점수', alpha=0.7)
plt.bar(x + width/2, test_scores, width, label='테스트 점수', alpha=0.7)

plt.title('검증 vs 테스트 정확도 비교')
plt.xticks(x, models_list)
plt.ylabel('정확도')
plt.ylim(0.9, 1.0)  # 적절한 범위로 조정
plt.legend()
plt.grid(axis='y', alpha=0.3)
plt.show()

# ===== 회귀 문제 Hold-Out 검증 =====
# 데이터 로드
X_reg, y_reg = load_boston(return_X_y=True)

# Hold-Out 분할
X_train_val_reg, X_test_reg, y_train_val_reg, y_test_reg = train_test_split(X_reg, y_reg, test_size=0.2, random_state=42)
X_train_reg, X_val_reg, y_train_reg, y_val_reg = train_test_split(X_train_val_reg, y_train_val_reg, test_size=0.25, random_state=42)

# 회귀 모델 파이프라인 (스케일링 포함)
reg_pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('regressor', LinearRegression())
])

# 모델 학습 및 검증
reg_pipeline.fit(X_train_reg, y_train_reg)
val_pred = reg_pipeline.predict(X_val_reg)
val_mse = mean_squared_error(y_val_reg, val_pred)
val_r2 = r2_score(y_val_reg, val_pred)

print("\n=== 선형 회귀 검증 결과 ===")
print(f"검증 MSE: {val_mse:.4f}")
print(f"검증 RMSE: {np.sqrt(val_mse):.4f}")
print(f"검증 R²: {val_r2:.4f}")

# 최종 평가 (훈련 + 검증 세트로 재학습)
reg_pipeline.fit(X_train_val_reg, y_train_val_reg)
test_pred = reg_pipeline.predict(X_test_reg)
test_mse = mean_squared_error(y_test_reg, test_pred)
test_r2 = r2_score(y_test_reg, test_pred)

print("\n=== 선형 회귀 테스트 결과 ===")
print(f"테스트 MSE: {test_mse:.4f}")
print(f"테스트 RMSE: {np.sqrt(test_mse):.4f}")
print(f"테스트 R²: {test_r2:.4f}")

# 예측 vs 실제 시각화
plt.figure(figsize=(10, 6))
plt.scatter(y_test_reg, test_pred, alpha=0.7)
plt.plot([y_reg.min(), y_reg.max()], [y_reg.min(), y_reg.max()], 'k--', lw=2)
plt.xlabel('실제 값')
plt.ylabel('예측 값')
plt.title('테스트 세트: 예측 vs 실제')
plt.grid(True, alpha=0.3)
plt.show()
```

**개념의 활용**:

- 대용량 데이터셋 모델링
- 빠른 프로토타이핑 및 모델 개발
- 시간 효율성이 중요한 상황
- 하이퍼파라미터 튜닝
- 온라인 학습 환경에서의 모델 평가
- 시계열 데이터 모델링 (시간 기반 분할)
- 다단계 모델 선택 프로세스