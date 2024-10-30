---
title: Untitled
draft: false
tags:
  - example-tag
---
 Softmax의 backward 연산에 대해 자세히 분석하겠습니다.

## 수학적 원리

Softmax의 편미분은 다음과 같이 표현됩니다:

$$\frac{\partial s_i}{\partial z_j} = \begin{cases} 
s_i(1-s_i) & \text{if } i = j \\
-s_is_j & \text{if } i \neq j
\end{cases}$$

여기서:
- $s_i$는 i번째 클래스의 softmax 출력
- $z_j$는 j번째 클래스의 입력값

## 코드 분석

```python
def softmax_backward(self, x, upstream_grad):
    softmax_output = x  # softmax의 출력값
    N = softmax_output.shape[0]  # 배치 크기
    
    # 그래디언트 계산
    dx = softmax_output.copy()  # softmax 출력값 복사
    dx -= upstream_grad  # 실제 레이블과의 차이 계산
    dx = dx / N  # 배치 크기로 정규화
    
    return dx
```

## 상세 설명

**1. 입력 파라미터**
- `x`: softmax 함수의 출력값 (확률 분포)
- `upstream_grad`: 상위 레이어에서 전파된 그래디언트 (보통 원-핫 인코딩된 실제 레이블)

**2. 그래디언트 계산 과정**:
```python
# 예시를 통한 설명
import numpy as np

# 가상의 입력 데이터
batch_size = 2
num_classes = 3

# softmax 출력 예시
softmax_output = np.array([
    [0.3, 0.5, 0.2],  # 첫 번째 샘플
    [0.1, 0.7, 0.2]   # 두 번째 샘플
])

# 실제 레이블 (원-핫 인코딩)
true_labels = np.array([
    [0, 1, 0],  # 두 번째 클래스가 정답
    [0, 1, 0]   # 두 번째 클래스가 정답
])

# 그래디언트 계산
dx = softmax_output.copy()
dx -= true_labels
dx = dx / batch_size

print("Gradient:", dx)
```

## 최적화된 구현

더 명확하고 효율적인 구현 버전:

```python
class SoftmaxLayer:
    def __init__(self):
        self.cache = None
        
    def backward(self, dout):
        """
        Parameters:
            dout: 상위 레이어에서의 그래디언트 (보통 원-핫 벡터)
        Returns:
            입력에 대한 그래디언트
        """
        batch_size = dout.shape[0]
        
        # 수치적 안정성을 위한 epsilon 추가
        eps = 1e-8
        
        # 그래디언트 계산
        dx = self.cache.copy()
        dx[dx < eps] = eps  # 로그 연산의 안정성을 위해
        dx -= dout
        dx /= batch_size
        
        return dx
    
    def forward(self, x):
        """
        Forward pass 구현
        """
        exp_x = np.exp(x - np.max(x, axis=1, keepdims=True))
        self.cache = exp_x / np.sum(exp_x, axis=1, keepdims=True)
        return self.cache
```

## 주의사항

1. **수치적 안정성**:
   - 매우 작은 값에 대한 처리 필요
   - overflow/underflow 방지

2. **배치 정규화**:
   - 배치 크기로 나누어 평균 그래디언트 계산
   - 학습 안정성 향상

3. **메모리 효율성**:
   - 불필요한 복사 연산 최소화
   - 중간 결과 재사용

## 성능 개선 팁

```python
def optimized_softmax_backward(self, x, upstream_grad):
    """최적화된 버전의 softmax backward"""
    batch_size = x.shape[0]
    
    # 수치적 안정성을 위한 클리핑
    x = np.clip(x, 1e-7, 1.0 - 1e-7)
    
    # 효율적인 그래디언트 계산
    dx = (x - upstream_grad) / batch_size
    
    return dx
```

이러한 구현은 대규모 데이터셋에서도 안정적으로 동작하며, 수치적 오류를 최소화할 수 있습니다.

Negative Log-Likelihood (NLL) Loss에 대해 수식과 함께 자세히 설명해드리겠습니다.

NLL Loss의 수학적 설명:

1. Forward Pass (손실 계산)

수식:
$$ L = -\frac{1}{N} \sum_{i=1}^N \log(p_{i,y_i}) $$

여기서:
- $$N$$ : 배치 크기
- $$p_{i,y_i}$$ : i번째 샘플의 정답 클래스 $$y_i$$에 대한 예측 확률
- $$\log$$ : 자연로그

코드에서 이는 다음과 같이 구현됩니다:
```python
def nll_loss_forward(self, x, lbl):
    return np.array([-np.log(x[i,l]) for i,l in enumerate(lbl)]).mean()
```

2. Backward Pass (그래디언트 계산)

수식:
$$ \frac{\partial L}{\partial x_{i,j}} = \begin{cases} 
-\frac{1}{N} \cdot \frac{1}{p_{i,j}} & \text{if } j = y_i \\
0 & \text{otherwise}
\end{cases} $$

코드에서 이는 다음과 같이 구현됩니다:
```python
def nll_loss_backward(self, x, lbl):
    dx = np.zeros_like(x)  # 그래디언트 배열 초기화
    for i, l in enumerate(lbl):
        dx[i, l] = -1.0 / x[i, l]  # 정답 클래스에 대해서만 그래디언트 계산
    return dx / len(lbl)  # 배치 크기로 나누어 평균
```

3. 실제 예시:

```python
# 예시 데이터
probs = np.array([
    [0.7, 0.2, 0.1],  # 첫 번째 샘플의 예측 확률
    [0.3, 0.6, 0.1]   # 두 번째 샘플의 예측 확률
])
labels = np.array([0, 1])  # 정답 레이블

# Forward pass 계산:
# L = -(log(0.7) + log(0.6))/2
# ≈ -[log(0.7) + log(0.6)]/2
# ≈ -((-0.357) + (-0.511))/2
# ≈ 0.434

# Backward pass 계산:
# 첫 번째 샘플(i=0)의 그래디언트:
# dx[0,0] = -1/(2*0.7) ≈ -0.714
# dx[0,1] = 0
# dx[0,2] = 0

# 두 번째 샘플(i=1)의 그래디언트:
# dx[1,0] = 0
# dx[1,1] = -1/(2*0.6) ≈ -0.833
# dx[1,2] = 0
```

NLL Loss가 중요한 이유:
1. Softmax와 함께 사용될 때 수치적으로 안정적
2. Cross-Entropy Loss와 동일한 효과
3. 모델이 정답 클래스에 대해 높은 확률을 예측하도록 학습 유도
4. 그래디언트가 예측 확률에 반비례하여, 잘못된 예측에 대해 더 큰 페널티 부여

이 손실 함수는 분류 문제에서 매우 효과적이며, 특히 Softmax 활성화 함수와 함께 사용될 때 가장 일반적인 선택입니다.