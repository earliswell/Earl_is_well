---
title: Presentation 01. Discriminative Sample-Guided and Parameter-Efficient Feature Space Adaptation for Cross-Domain Few-Show Learning
draft: false
tags:
  - "#paper-review"
  - "#컴퓨터비전심화"
---
## 사전 지식
- 본 논문의 목적은 "교차 도메인 소수 샘플 학습(Cross-Domain Few-Shot Learning)"을 위한 효율적인 특저 공간 적응 방법을 제안함. 
	- 그렇다면 교차 도메인 소수 샘플 학습이 왜 필요할까 ?
	1. **데이터 수집의 현실적 제약**
	2. **도메인 적응의 실질적 필요성**
	3. **인간의 학습 방식과의 유사성 추구**
	4. **계산 및 저장 효율성**
	5. **실시간 또는 온라인 학습 시나리오 지원**
	- 이러한 제한된 데이터에서 새로운 환경에 빠르게 적응해야 하는 현실 세계의 요구를 반영하며, 인간과 같은 적응형 학습 시스템을 향한 중요한 진전을 나타낸다. → 이러한 도전 과제를 효율적이고 효과적으로 해결하는 데 기여를 해보자 !!
- 논문의 핵심은 적은 데이터(Few-Shot)만으로도 이전에 보지 못한 새로운 도메인에서 효과적으로 학습할 수 있는 방법을 개발하는 것 !
## Abstract
- **Background**
	- 우리가 관측하지 못한(few labelled example)을 분류를 잘 하는 cross-domain을 해결하고자 하는 연구인 듯 싶네
	- 기존 방식도 좋지만 → 몇 가지 한계가 존재 → 그 중 두 가지 중요한 개선 사항을 통해 완화함.
- **Method**
	1. ==lightweight parameter-efficient adaptation strategy== → 작은 숫자의 데이터셋에 적용되는 너무 많은 파라미터를 통해 발생하는 과적합 문제 해결
		- linear transformation of pre-trained features을 통해서 학습 파라미터 수를 유의하게 줄임.
	2. 전통적 방식인 최근접 중심 분류기 → ==discriminative sample-aware loss function==으로 대체
		- inter-class variance (클래스 간 분산) : **서로 다른 클래스 간의 차이 또는 변동성**
		- intra-class variance (클래스 내 분산) : **동일한 클래스 내의 샘플들 간의 차이 또는 변동성**
		- 이 두 가지의 민감성을 강화시켜서 Feature의 공간을 더욱 명확하게 구조화 하는 것 !!
- **Result** 
	- Dataset: Meta-Dataset → not only improves accuracy up to 7.7% and 5.3% on previously seen and unseen datasets, but also achieves the above performance while being at least ~ 3% $\times$ more parameter-efficient than existing methods
	- Establishing a new state-of-the-art in cross-domain few-show learning.


## Conclusions and Limitations
- Extremely lightweight linear transformations & Optimized by a discriminative sample-aware loss function → To learn new classes and domains with a limited number of labelled samples. → ==state-of-the-art== !! (Meta-Dataset benchmark) while ensuring parameter efficiency.
- <mark style="background: #FF5582A6;">Our method is not without limitations.</mark>
- The current approach applies a fixed linear transformation to every layer of the pre-trained model. → Future improvements could enable these transformations to be defined flexibly, layer by layer, to suit the specific requirements of the target task.
	- 나중에는 유한한 linear transformation을 사용할 거야~~
- Moreover, instead of restricting tuning depth to only two values for seen and unseen datasets, which may lead to suboptimal outcomes, → future research could explore defining optimal tuning depths customized for each dataset and task.
	- 오직 두 개의 value(seen and unseen)에만 depth 차이를 뒀는데, 이후에는 각각 데이터셋과 Task마다 최적 튜닝 뎁스를 찾겠다?
---
- 그러니까 머신러닝과 딥러닝과 같은 기계학습 분야의 가장 취약한 점이 학습되지 않은 도메인에 대한 정확도가 매우 떨어진다는 단점이 있음 → 즉 한 도메인에 적합한 모델들이 만들어짐.
- 한 도메인에서만 예측을 잘하는 머신러닝과 딥러닝은 현실 세계에 투영하기에는 여러 문제점이 발생함.
	- 데이터셋이 희소한 경우 (의료 데이터, 개인 데이터 등등..)
	- 실제 사진과 픽셀 단위의 사진의 Vector 차이 등
- 이와 같은 문제점은 컴퓨터가 잘 받아들여 예측하는 데 다른 오류를 만듬. 따라서 이러한 문제점들을 해결하기 위한 여러 기존 방법이 존재했음 !!
- 여러 기존 방법론들을 살펴봤는데,  군집 내 와 군집 간의 Feature Space를 명확히 구조화하면서 학습 파라미터수를 조금 유연하게 가져갈 수 있다면 ?