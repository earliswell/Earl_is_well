---
title: tmux 기초 사용법
draft: false
tags:
  - example-tag
website: https://velog.io/@piopiop/Linux-tmux를-사용해보자
---
## tmux 구성요소
1. session: 여러 윈도우를 구성하는 세션
## Session 관련 명령어
```
# 새로운 세션 생성
tmux new -t (session name)

# 세션 종료
exit

# 세션 목록
tmux ls

# 세션 불러오기
tmux attach -t session name

# 세션 중단하기
(CTRL + b) + d

# 특정 세션 강제 종료
tmux kill-session -t session name
```

## 스크롤 하기 ~
- 파일 추가, 파일 명 : .tmux.conf
```
# Enable mouse control (clickable windows, panes, resizable panes)

set -g mouse on #For tmux version 2.1 and up

# set -g mode-mouse on #For tmux versions < 2.1
```
