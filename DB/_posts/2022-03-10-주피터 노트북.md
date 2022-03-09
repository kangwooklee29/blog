---
title: jupyter notebook에서 PostgreSQL 사용하기
---

```python
%load_ext sql

%sql <URL> #postgresql 프로토콜을 사용하는 URL을 입력하면 그 URL에 접속하여 DB를 로드한다.

%%sql

SELECT * FROM table1
```