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
