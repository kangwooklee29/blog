---
title: which, whereis
---


1. `which [명령어 실행 파일명]`

```bash
$ which bash
```

`which`는 환경변수 `PATH`에 있는 경로 중 해당 명령어 실행 파일의 경로를 검색하는 리눅스 명령어.


2. `whereis [명령어 실행 파일명]`

```bash
$ whereis bash
```

`whereis`는 `which`와 같은 방식으로 파일에 관한 정보를 얻으나, 소스파일, 매뉴얼에 관한 정보까지 함께 얻어온다.