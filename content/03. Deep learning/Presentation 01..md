---
title: Presentation 01. Discriminative Sample-Guided and Parameter-Efficient Feature Space Adaptation for Cross-Domain Few-Show Learning
draft: false
tags:
  - "#paper-review"
  - "#컴퓨터비전심화"
---
## 사전 지식
- 본 논문의 목적은 "교차 도메인 소수 샘플 학습(Cross-Domain Few-Shot Learning)"을 위한 효율적인 특저 공간 적응 방법을 제안함. 
- 논문의 핵심은 적은 데이터(Few-Shot)만으로도 이전에 보지 못한 새로운 도메인에서 효과적으로 학습할 수 있는 방법을 개발하는 것 !
## Abstract
- 우리가 관측하지 못한(few labelled example)을 분류를 잘 하는 cross-domain을 해결하고자 하는 연구인 듯 싶네
- 기존 방식도 좋지만 → 몇 가지 한계가 존재 → 그 중 두 가지 중요한 개선 사항을 통해 완화함.
1. lightweight parameter-efficient adaptation strategy → 작은 숫자의 데이터셋에 적용되는 너무 많은 파라미터를 통해 발생하는 과적합 문제 해결
	- linear transformation of pre-trained features을 통해서 학습 파라미터 수를 유의하게 줄임.
2. 전통적 방식인 최근접 중심 분류기 → discriminative sample-aware loss function으로 대체
	- 