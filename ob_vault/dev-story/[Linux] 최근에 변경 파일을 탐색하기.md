---
date: 2022-08-21 00:08
lastmod: 2022-08-21 00:08
tags:
- Linux
- command
- search
---

# 최근에 변경된 파일을 탐색하기

## 명령어 기본 구조

```shell
# find {PATH} -type f -mtime -30 -ls
# -type f 옵션은 검색 대상의 유형이 파일임을 의미
# -name 을 사용 
# -mtime -30 옵션은 수정된 기간이 이전 30일을 의미
# -ls 는 조건에 맞는 대상을 보여주는 방식으로 ls 를 사용

# / 하위에서 최근 1주일 동안 수정된 파일 검색
$ find / -type f -mtime -7 -ls

# 수정일이 30일을 넘은 log 파일 삭제
$ find / -name '*.log.*' -mtime +29 -delete

# 용량 100 MB 이상의 파일 검색
$ find / -size +100000 -ls
```

## mtime / atime / ctime

-   mtime : 수정 날짜 (modify)
-   atime : 접근 날짜 (access)
-   ctime : 권한 변경 날짜 (change)

## *x*time 옵션의 부호

- +n : n 일 및 n 일 이전 을 모두 포함하는 조건
- n : 정확히 n 일만 조건
- -n : 오늘 부터 n일 전까지 포함하는 조건