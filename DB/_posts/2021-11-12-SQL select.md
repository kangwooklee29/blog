---
title: SQL SELECT
---


### 1. 쿼리를 대문자로 써야 하는 이유

\- 오라클의 경우 쿼리 입력 시 그 쿼리가 예전에 수행된 적 있는지를 탐색하는데, 이전에 같은 내용의 쿼리가 입력된 적 있어도 대소문자가 다르면 다른 쿼리가 입력된 것으로 보므로 캐시를 참조하지 않고 매번 새로 탐색을 수행하게 된다. 이는 비효율적이므로, 일반적으로 쿼리를 쓸 때 키워드와 속성명을 대문자 아니면 소문자로 일관적으로 쓰도록 하는 규칙을 정해두고 쿼리를 쓰게 된다. 그 외에도 대소문자에 관하여 일관된 규칙으로 쿼리를 쓰는 것이 가독성이 더 좋고 유지보수가 용이하다는 등의 장점이 있다.


### 2. SELECT 쿼리의 기본형태

```sql
SELECT table1.field1, table1.field2 FROM table1 WHERE field1 < 10 ORDER BY field1, field2 DESC LIMIT 10;
```

\- 기본적으로 SELECT + 속성명 + FROM 테이블명의 형태를 갖고 있으며, 해당 테이블의 해당 속성명의 모든 값이 위에서 아래로 죽 나열된 튜플들을 가져오게 된다.


\- FROM 이하를 아예 쓰지 않고, 속성명도 쓰지 않고 SELECT 1, 2, 3처럼 속성명 위치에 그냥 정수나 문자열을 두는 경우도 있다. 이 경우 다음 테이블을 결과로 가져온다.

| 1 | 2 | 3 |
|---|---|---|
| 1 | 2 | 3 |


\- SELECT 뒤에 속성명을 쓸 때나 FROM 뒤에 테이블명을 쓸 때 실제 DB에 담긴 테이블명, 속성명을 쓴 다음에 AS로 새로 속성명, 테이블명을 지정해 쿼리를 쓸 수 있다.

- 속성명을 새로 지정하면, SELECT 쿼리의 결과 테이블에 담긴 속성명이 지정된 그 속성명으로 표시되어 결과 테이블을 가져온다.

- 테이블명을 새로 지정하면, FROM 이하 구문에서 테이블명을 지칭할 때 새로 지정한 테이블명으로 쿼리를 쓸 수 있다.

- AS는 생략 가능하다.

\- 속성명/테이블명에 띄어쓰기나 SQL 쿼리의 예약어가 있다면 ""로 묶어써야 한다.

\- FROM 뒤에 테이블을 쓰는 위치에 () 안에 SELECT 쿼리를 넣어 만든 테이블을 쓸 수도 있다.


### 3. 속성명 쓰기

#### 1) `DISTINCT field1, field2`

\- field1, field2의 값이 중복된 경우, 중복된 튜플을 제거한다.

#### 2) 문자열 관련 함수(`LEFT`, `UPPER`, `LOWER`, `LEN`, `REPLACE`, `LPAD`, `SUBSTRING`, `TO_CHAR`, ...)

\- 속성명을 함수의 인자로 전달하면 그 함수에 해당하는 값으로 이루어진 열을 만들어 가져온다.

(a) `LEFT(field1, 7)`

\- `field1` 속성의 값을 왼쪽에서부터 7글자를 잘라낸 값으로 이루어진 열을 만들어 가져온다.

(b) `TO_CHAR(field1, 'YYYY-MM')`

\- `field1` 속성의 값을 두 번째 인자로 전달된 포맷으로 변환한 값(여기서는 `'YYYY-MM'` 포맷으로 변환한 값)으로 이루어진 열을 만들어 가져온다.


#### 3) 연산식(`field1 * 2`)

\- 속성명 위치에 그 속성명에 관한 연산식을 적을 수도 있다. 이 경우 각 튜플들에는 원래 그 위치에 반환됐어야 할 값의 연산값이 담기게 된다.

\- 연산에 사용하는 수로 NULL을 사면 그 결과값이 전부 NULL값이 된다.


#### 4) 조건식

(a) `IF(field1 % 2 = 0, '짝수', '홀수')`

\- 각 튜플이 조건식을 충족하는 경우와 충족하지 않는 경우로 구분하여 각각 그에 해당하는 값을 그 튜플이 그 열에서 갖는 값으로 하는 열을 만들어 가져온다.


(b) `CASE WHEN ... THEN ... END`

```sql
CASE 
    WHEN field1 % 3 = 0 THEN '나머지 0' 
    WHEN field1 % 3 = 1 THEN '나머지 1'
    ELSE '나머지 2'
END
```

\- 각 튜플의 그 속성값이 조건식을 충족할 경우 **그에 해당하는 값(THEN 뒤에 쓰인 값)을 그 튜플이 그 열에서 갖는 값으로 하는 열을 만들어** 가져온다.

#### 5) 날짜 관련 함수들

(a) `DATE_FORMAT(field1, '%Y-%m-%d %H:%i:%s')`

\- 각 튜플의 `field1` 값을 `DATE_FORMAT` 함수의 인자로 넣어 마찬가지로 인자로 전달된 **포맷 문자열에 대응되는 값**을 그 튜플이 그 열에서 갖는 값으로 하는 열을 만들어 가져온다.

(b) `GETDATE()`

\- 현재 시간을 리턴한다.

(c) `CONVERT_TIMEZONE('America/Los_Angeles', GETDATE())`

\- 현재 시간을 미국 서부 시간으로 변환하여 리턴한다.

(d) `DATE_TRUNC('month', field1)`

\- 각 튜플의 `field1` 값에서 월에 해당되는 정보를 가져온다.

(e) `YEAR(field1)`: 주어진 `DATE`형 값에 해당하는 연도 구하는 함수

(f) `HOUR(field1)`: 주어진 `DATETIME`형 값에 해당하는 시간 구하는 함수


#### 6) 형변환(`field1::FLOAT`, `CAST(field1 AS FLOAT)`)

\- 예를 들어 `field1` 속성이 INTEGER형으로 정의돼 있고 어떤 튜플의 값에 1이 저장돼 있다면, `field1`에 1/2을 곱해 가져오도록 하면 0이 리턴된다. 이때 `field1` 속성의 값을 FLOAT형으로 형변환하여 가져오도록 하면 0이 아니라 0.5를 가져온다.


#### 7) 그외 함수들

(a) `ROUND(field1, 2)`

\- `field1`의 값이 실수일 때, 소수점 2번째 자리까지만 가져온다.

(b) `NULLIF(field1, 0)`

\- 괄호 안 조건식이 참이라면 `NULL`값을, 거짓이라면 `field1`을 리턴한다.

(c) `COALESCE(field1, field2, ..., 0)`

\- **인자의 첫 번째 값이 `NULL`이 아니면 그냥 그 값**을 그대로 리턴하고, `NULL`이면 두 번째 값을 리턴하는데 그게 `NULL`이면 다시 세 번째 값을 리턴하고, ... 하는 작업을 수행한다. 인자 개수는 둘 이상도 가능하다. (정해진 개수는 없다.)

\- '여러 개의 컬럼들을 하나로 합해 유효한 값들로 이루어진 컬럼을 만든다'라는 의미 차원에서 '결합하다'란 뜻을 가진 coalesce라는 이름을 사용한다.


(d) `LEAD(field1, 2)`

\- 현재 튜플보다 2칸 뒤에 있는 튜플의 `field1`값을 가져온다. (앞에 있는 튜플을 가져오려면 `LAG` 함수를 사용한다.)



### 4. 조건식 `WHERE`

#### 1) `IS NULL`, `IS NOT NULL`

\- **`NULL`의 비교는 `IS`, `IS NOT`을 통해서만 해야 한다.**

\- `NOT TRUE`는 `FALSE`가 아니고, `NOT FALSE`도 `TRUE`가 아님을 주의해야 한다. (`NOT TRUE`에는 `FALSE`뿐 아니라 **`NULL` 등**도 포함된다. 그러나 `NULL`은 `FALSE`가 아니며, 따라서 `NOT FALSE`에는 `NULL` 등도 포함된다.)

\- `NULL`을 `TRUE`나 `FALSE`와 논리연산 시 `UNKNOWN` 값이 리턴되므로 주의해야 한다.



#### 2) `field1 BETWEEN 숫자 AND 숫자`

\- field1의 값이 두 숫자 사이 정수인 튜플을 가져온다.

#### 3) `field1 IN (1, 3, 5, 7, 9)` / `field1 NOT IN (1, 3, 5, 7, 9)`

\- field1의 값이 1, 3, 5, 7, 9 중 어느 하나인 튜플(또는 1, 3, 5, 7, 9가 아닌 튜플)을 가져온다.

\- 괄호 안에는 위와 같이 쉼표로 구분된 정수/문자열 대신 `(SELECT field2 FROM table2)`와 같은 `SELECT` 쿼리가 들어갈 수도 있다. 단, 이때 괄호 안에 들어가는 `SELECT` 쿼리는 속성명을 **하나만** 가져야 한다.

#### 4) `field1 LIKE '%오%'` / `field1 LIKE '오_'`

\- `%`와 `_`는 SQL에서 '모든 문자열'/'어느 한 문자열'을 의미하는 와일드카드로, 이 조건식은 `field1`의 값이 '오'를 포함하는/'오'로 시작하는 두 글자 문자열인 튜플을 가져온다.

\- `LIKE`는 대소문자를 구분하므로, 대소문자를 구분하지 않는 문자열을 가져오려면 `ILIKE`로 대체하면 된다.




### 5. WITH table2 AS (SELECT * FROM table1)

#### 1) 개요

\- INSERT 쿼리로 새로운 DB 데이터를 DB에 추가하지 않고, WITH 쿼리를 사용하여 메모리에 임시로 새로운 테이블을 만들고 또 그 테이블에 이름을 붙여 그 테이블을 SELECT 쿼리로 호출하여 사용할 수 있다. 이처럼 메모리에 임시로 만든 테이블을 CTE(common table expression)이라 한다.

#### 2) WITH RECURSIVE table1 AS (...)

```sql
WITH RECURSIVE table1(field1, field2) AS (
    SELECT 0, 1 -- 초기값
    UNION
    SELECT field1+1, field2*2 FROM table1 WHERE field1 < 10
                -- 증감식, 조건식
)
```

\- WITH RECURSIVE 쿼리를 사용하면 반복문을 통해 만든 것과 같은 CTE를 만들 수 있다. 이러한 CTE는 초기값을 선언하는 SELECT 쿼리 하나와 증감식, 조건식을 모두 포함하는 SELECT 쿼리 하나를 사용하여 만든다. 구체적으로, 두 번째 SELECT 쿼리의 속성명에 그 CTE의 속성명을 사용한 연산식을 쓰면 그 SELECT 쿼리의 결과값이 재귀적으로 구해지게 된다. 이 재귀호출은 조건식에서 정한 조건이 참인 동안에만 일어난다.



### 6. CREATE TABLE table2 AS SELECT * FROM table1

\- AS 뒤의 쿼리에 해당하는 테이블을 가져와 table2라는 이름의 테이블로 저장한다.


### 7. SELECT JSON_EXTRACT_PATH_TEXT('{"field1"}', 'field1')

\- 첫 번째 인자로 JSON 파일의 내용을 문자열 형태로 전달하고, 두 번째 이후 인자는 그 JSON 파일의 키를 전달한다. 이때 그 전달된 키에 해당하는 값을 그 JSON 파일에서 찾아내 가져온다.


### 8. subquery

\- SELECT, INSERT, DELETE, UPDATE 쿼리의 안에 요소로서 들어가는 SELECT 쿼리를 subquery 또는 nested query라 하며, 이 쿼리로 얻은 테이블을 derived table이라 한다. derived table의 레코드는 컬럼이 하나뿐인 행 하나일 수도 있고, 컬럼이 하나 이상인 테이블일 수도 있다. 이처럼 nested query를 작성할 때는, derived table을 위한 alias를 반드시 써야 한다.

\- 내부에 subquery를 포함하는 SELECT 쿼리의 결과 테이블과 동일한 테이블을 그 SELECT 쿼리가 참조하는 relation과 그 내부의 subquery가 참조하는 relation 사이 join 연산으로도 얻을 수 있다. 같은 결과를 얻는 방법으로서 각 방식은 서로 다른 장단점이 있다.

\- 내부 subquery가 외부 쿼리가 참조하는 relation을 참조할 수 있다(correlated subquery). 이 경우 외부 쿼리가 참조하는 relation의 tuple 하나의 참/거짓을 판별하기 위해 각 tuple에 대해 매번 subquery 전체를 구하는 연산을 새로 해야 하는 경우가 있을 수 있다. 이는 전체 쿼리의 결과를 구하는 데 많은 시간이 드는 중요한 원인이 된다.

#### 1) subquery가 반환한 값이 attribute가 하나인 relation인 경우

\- 이러한 subquery는 WHERE 조건절 내에서 IN, ANY(SOME), ALL, EXISTS 같은 예약어와 함께만 사용할 수 있다.

- IN: 왼쪽 값이 오른쪽 relation에 포함되면 이 조건절은 참을 리턴한다.

- ANY: 이 relation의 어느 한 요소만이라도 해당 조건식을 만족하면 이 조건절은 참을 리턴한다.

- ALL: 이 relation의 모든 요소가 해당 조건식을 만족해야 이 조건절이 참을 리턴한다.


#### 2) subquery가 반환한 값이 attribute가 둘 이상인 relation인 경우

\- 이러한 subquery는 WHERE 조건절 내에서 그 결과값이 존재하는지 아닌지를 EXISTS 예약어를 통해 검사하는 방식으로 사용할 수 있다. 결과값이 존재한다면 이 조건절이 참을 리턴한다.

