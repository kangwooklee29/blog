---
title: jupyter notebook에서 PostgreSQL 사용하기
---


### 1. 외부 URL에서 DB 로드해오기

```python
%load_ext sql

%sql <URL> #postgresql 프로토콜을 사용하는 URL을 입력하면 그 URL에 접속하여 DB를 로드한다.
```


### 2. 로드해온 DB에서 SQL 쿼리 사용하기

```python
%%sql

SELECT * FROM table1
```

### 3. 로드해온 DB에서 테이블을 추출하여 pandas의 DataFrame형 변수로 담기

```python
result = %sql SELECT * FROM TABLE1
df1 = result.DataFrame()
```