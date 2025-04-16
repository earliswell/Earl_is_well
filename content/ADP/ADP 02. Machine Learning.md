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
