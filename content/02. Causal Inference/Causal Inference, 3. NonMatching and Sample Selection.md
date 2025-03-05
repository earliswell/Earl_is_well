---
title: 3. NonMatching and Sample Selection
draft: false
tags:
  - nonparametric
---
- Matching is not the only way to control for covariates. 'Weighting' divides each group response by the covariate-conditioned group choice probability to identify the treatment and control group means separately.
- 'Regression imputation' integrates out the covariates in each group's regression function to get the two group means separately.
- 'Complete pairing' forms a weighted average of all possible pair differences across the two groups with the weight depending on the covariate differnces;
- it requires hardly and decision to be made by the user and has a built-in mechanism to deal with the support problem plaguing matching.
- Treatment Effects for sample selection models often arise when the treatment can affect participation in an activity and the performance there, and separating these effects has interesting policy implications.
---
- 매칭은 공변량을 제어하는 유일한 방법이 아니다. 'Weighting'은 각 그룹의 반응을 공변량의 조건의 그룹 선택 확률로 나누어 treatment 그룹과 통제 그룹의 평균을 별도로 식별한다. [[Causal Inference, 3.1. NonMatching and Sample Selection]]
- 'Regression imputation'은 각 그룹의 회귀 함수에서 공변량을 통합하여 두 그룹의 평균을 별도로 얻는다.
- 'Complete paring'은 공변량의 차이의 따른 가중치를 사용하여 두 그룹의 가능한 모든 쌍 차이의 가중 평균을 형성한다.
- 사용자 결정을 내릴 필요가 거의 없으며 매칭을 괴롭히는 Support problem을 다루기 위한 매커니즘이 내장되어 있다.
- Sample Selection 모델을 위한 Treatment Effect는 종종 떠오른다. treatment가 활동 참여 및 성과에 영향을 미칠 수 있을 때, 그리고 이들의 효과를 분리하는 것은 흥미로운 정책적 의미를 갖는다.

#### 알면 좋은 영단어
1. integrate, Verb : 통합시키다\[되다], 통합되다
2. plague, Noun : 전염병
3. plague, Verb : 괴롭히다(=trouble), 성가시게하다(=hound)
4. deal with something : ~을 다루다, 처리하다.
5. bulit-in : 내장되 있는 