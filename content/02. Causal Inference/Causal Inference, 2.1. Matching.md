---
title: 2.1. Basic of Matching and Various Effects
draft: false
tags:
  - "#nonparametric"
---
- 매칭이라는게 그러니까 → 결국에는 Hidden bias를 제거해낼 수 있는데, 그 이유는 결국 비슷한 성질을 갖고 있는 Treatment 그룹과 control 그룹을 비교해서 → 결국 차분을 통해서 Confounding 변수를 제거할 수 있다는 컨셉이다.
- propensity score → Treatment Group과 Control Group의 공변량 균형을 맞춰서 → 이제 남은 차이는 Treatment이니까 이에 대한 차이를 보는 것이고
- Progonostic Score → 이는 Control Group에 대한 정보를 통해서 만약 Treatment 그룹이 처치를 받지 않았을 것을 상정한 차이를 통해서 Treatment의 effect 크기를 추출함. (만약 Treatment가 없었더라면?)