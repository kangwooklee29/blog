<Br><Br><Br><Br>

#### 이 폴더에 YYYY-MM-DD-제목.md 형식의 파일을 올려 포스트를 업로드할 수 있다.
  
\- 포스트에 적어야 할 정해진 YAML front matter 규칙이 있기는 하나, YAML 헤더를 전혀 쓰지 않고 그냥 바로 마크다운 문법에 따라 문서를 작성한다 하더라도 Jekyll 블로그에서 정상적으로 포스트를 인식하기는 한다. 이 경우 파일명에 적은 제목이 포스트의 제목인 포스트로 인식된다. 

\- 다만, 전혀 YAML 헤더를 쓰지 않은 포스트 파일의 경우 로컬 서버로 git repository를 pull해 블로그를 실행해 포스트를 열람할 경우 header와 footer가 포함되지 않고 본문의 마크다운 문법만 HTML로 변환된 문서가 화면에 뜬다는 점을 유의해야 한다.

\- 포스트 파일에 적을 것이 권장되는 YAML front matter의 형식은 다음과 같다.

```
---
layout: post
title: 제목
date: 날짜
categories: 카테고리명
tags: 태그
---
```
