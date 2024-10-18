---
title: Ubuntu
draft: false
tags:
  - Ubuntu
  - Linux
  - 데이터베이스
---
## 사용 메모리 확인
```bash
$ free -h
```

## Ubuntu 권한 부여
```bash
# 권한 부여
$ ubuntu@ubuntu:~$ sudo su
# Input your password

# 편집기 open
$ crontab -e
```
맨 밑줄에 입력
0 4 * * * sudo sync && echo 3 > /proc/sys/vm/drop_caches
