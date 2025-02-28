---
title: tmux 기초 사용법
draft: false
tags:
  - example-tag
---
## tmux 구성요소
1. session: 여러 윈도우를 구성하는 세션
2. window: 터미널 화면, 세션 내에서 탭처럽 사용 가능
3. pane: 하나의 윈도우 내에서의 화면 분할?

## Session 관련 명령어
```
# 새로운 세션 생성
tmux new -s (session name)

# 세션 만들면서 윈도우랑 같이 생성
tmux new -s (session name) -n (window name)

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