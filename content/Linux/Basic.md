---
title: Basic
draft: false
tags:
  - example-tag
---

## 환경 실행
```
conda activate <환경명>
```

## 환경 삭제
```
conda deactivate 
conda env remove -n <환경명>
```

## 파일 전송
#file
```
# 서버 to 서버
sshpass -p "password" scp -P Port번호 -o StrictHostKeyChecking=no -r /보낼폴더경로 사용자명@서버주소:/저장경로/

# 로컬 to 서버

# 서버 to 로컬

```