---
title: 파이썬에서 PostgreSQL 사용하기
---


### 1. Google Colab에서 PostgreSQL 쓰기

#### 1) 외부 URL에서 DB 로드해오기

```python
%load_ext sql

%sql <URL> #postgresql 프로토콜을 사용하는 URL을 입력하면 그 URL에 접속하여 DB를 로드한다.
```


#### 2) 로드해온 DB에서 SQL 쿼리 사용하기

```python
%%sql

SELECT * FROM table1
```

#### 3) 로드해온 DB에서 테이블을 추출하여 pandas의 DataFrame형 변수로 담기


```python
result = %sql SELECT * FROM table1
df1 = result.DataFrame()
```



### 2. 파이썬에서 psycopg2 라이브러리를 import해서 쓰기

#### 1) 외부 URL에서 DB 로드해오기

```python
import psycopg2

conn1 = psycopg2.connect(f"dbname={dbname} user={user} host={host} password={password} port={port}")
cur1 = conn1.cursor()
```

#### 2) 로드해온 DB에서 SQL 쿼리 사용하기

```python
cur1.execute("SELECT * FROM table1")
result = cur1.fetchall() #튜플들이 리스트에 담겨 리턴돼 result 변수에 담긴다.
```

#### 3) 로드해온 DB에서 테이블을 추출하여 pandas의 DataFrame형 변수로 담기

```python
import pandas.io.sql as psql
result = psql.read_sql("SELECT * FROM table1", conn1)
```

#### 4) psycopg2에서 트랜잭션 다루기

(a) autocommit=False로 세팅하고 트랜잭션 다루기

```python
conn1.set_session(automcommit = False)

try:
    cur1.execute("DELETE FROM table1"); #TRUNCATE도 테이블의 모든 레코드를 삭제하기는 하지만 트랜잭션을 지원하지 않으므로 여기서는 쓸 수 없다.
    cur1.execute("INSERT INTO table1 VALUES ('fruit')")
    conn1.commit()
except (Exception, psycopg2.DatabaseError) as error:
    print(error)
    conn1.rollback()
finally:
    conn1.close()
```

(b) autocommit=True인 상태에서 트랜잭션 다루기

\- autocommit=True일 때에도 여러 쿼리를 하나의 트랜잭션으로 다룰 수 있다.

```python
cur1.execute("BEGIN;");
cur1.execute("DELETE FROM table1;");
cur1.execute("INSERT INTO table1 VALUES ('fruit');")
cur1.execute("END;")
```

