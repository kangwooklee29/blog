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


### 3. 속성명 쓰기

#### 1) DISTINCT field1, field2

\- field1, field2의 값이 중복된 경우, 중복된 튜플을 제거한다.

#### 2) 그룹함수(COUNT, SUM, MAX, MIN, ...)

\- 그룹함수(=aggregate 함수)는 보통 다른 속성명을 쓰지 않고 단독으로 쓰지만, 그룹화 쿼리(GROUP BY)를 쓰는 경우 다른 속성명과 함께 쓰이기도 한다.

(a) COUNT(field1)

\- field1 속성의 값들을 만약 가져온다면 가져오게 될 튜플의 개수를 세서 **그 개수를 그 열의 유일한 튜플이 갖는 값으로 하는 열을 만들어 가져온다.**

\- field1 속성의 값 중 그 값이 null인 튜플이 있다면 그 튜플은 제외하고 개수를 센다. 단, 인자로 속성명이 아니라 **\*을 넣는다면 값이 null인 튜플을 포함**해 개수를 센다.

\- COUNT()에 인자로 속성명 대신 1을 넣으면 전체 행의 개수를 세서 리턴한다.


(b) SUM(field1), MAX(field1), MIN(field1)

\- field1 속성의 값들을 모두 합한 값/최댓값/최솟값을 그 열의 유일한 튜플이 갖는 값으로 하는 열을 만들어 가져온다.




#### 3) 문자열 관련 함수(LEFT, UPPER, LOWER, LEN, REPLACE, LPAD, RPAD, SUBSTRING, TO_CHAR, ...)

\- 속성명을 함수의 인자로 전달하면 그 함수에 해당하는 값으로 이루어진 열을 만들어 가져온다.

(a) LEFT(field1, 7)

\- field1 속성의 값을 왼쪽에서부터 7글자를 잘라낸 값으로 이루어진 열을 만들어 가져온다.

(b) TO_CHAR(field1, 'YYYY-MM')

\- field1 속성의 값을 두 번째 인자로 전달된 포맷으로 변환한 값(여기서는 'YYYY-MM' 포맷으로 변환한 값)으로 이루어진 열을 만들어 가져온다.


#### 4) field1 * 2

\- 속성명 위치에 그 속성명에 관한 연산식을 적을 수도 있다. 이 경우 각 튜플들에는 원래 그 위치에 반환됐어야 할 값의 연산값이 담기게 된다.

\- 연산에 사용하는 수로 NULL을 사면 그 결과값이 전부 NULL값이 된다.


#### 5) CASE WHEN ... THEN ... END

```sql
CASE 
    WHEN field1 % 3 = 0 THEN '나머지 0' 
    WHEN field1 % 3 = 1 THEN '나머지 1'
    ELSE '나머지 2'
END
```

\- 각 튜플의 그 속성값이 조건식을 충족할 경우 **그에 해당하는 값(THEN 뒤에 쓰인 값)을 그 튜플이 그 열에서 갖는 값으로 하는 열을 만들어** 가져온다.

#### 6) IF(field1%2 = 0, '짝수', '홀수')

\- 각 튜플이 조건식을 충족하는 경우와 충족하지 않는 경우로 구분하여 각각 그에 해당하는 값을 그 튜플이 그 열에서 갖는 값으로 하는 열을 만들어 가져온다.

#### 7) 날짜 관련 함수들

(a) DATE_FORMAT(field1, '%Y-%m-%d %H:%i:%s')

\- 각 튜플의 field1 값을 DATE_FORMAT 함수의 인자로 넣어 마찬가지로 인자로 전달된 **포맷 문자열에 대응되는 값**을 그 튜플이 그 열에서 갖는 값으로 하는 열을 만들어 가져온다.

(b) GETDATE()

\- 현재 시간을 리턴한다.

(c) CONVERT_TIMEZONE('America/Los_Angeles', GETDATE())

\- 현재 시간을 미국 서부 시간으로 변환하여 리턴한다.



#### 8) field1::FLOAT, CAST(field1 AS FLOAT)

\- 예를 들어 field1 속성이 INTEGER형으로 정의돼 있고 어떤 튜플의 값에 1이 저장돼 있다면, field1에 1/2을 곱해 가져오도록 하면 0이 리턴된다. 이때 field1 속성의 값을 FLOAT형으로 형변환하여 가져오도록 하면 0이 아니라 0.5를 가져온다.


#### 9) 그외 함수들

(a) ROUND(field1, 2)

\- field1의 값이 실수일 때, 소수점 2번째 자리까지만 가져온다.

(b) NULLIF(field1, 0)

\- 괄호 안 조건식이 참이라면 NULL값을, 거짓이라면 field1을 리턴한다.

(c) COALESCE(field1, field2, ..., 0)

\- 인자의 첫 번째 값부터 NULL이 아니면 그냥 그 값을 그대로 리턴하고, NULL이면 두 번째 값을 리턴하는데 그게 NULL이면 다시 세 번째 값을 리턴하고, ... 하는 작업을 수행한다. 인자 개수는 둘 이상도 가능하다. (정해진 개수는 없다.)

(d) ROW_NUMBER() OVER (PARTITION BY field1 ORDER BY field2)

\- 테이블의 모든 튜플의 순서를 field1의 값이 같은 것들끼리 모아서 바꿔놓고, field1의 값이 같은 튜플들끼리는 field2의 값으로 정렬을 한 후 현재 튜플은 그 튜플들 중에 몇 번째인지 그 순서를 리턴하는 함수. 


|-|-|
|field1|field2|
|1|2022-12-31|
|2|2022-12-31|
|1|2022-06-01|
|2|2022-06-01|
|1|2022-01-01|

```sql
SELECT *, ROW_NUMBER() OVER (PARTITION BY field1 ORDER BY field2) rn FROM table1;
```

\- 예를 들어 위 테이블에 대하여 위 쿼리를 사용한다 하면 아래와 같은 테이블을 얻는다.

|-|-|-|
|field1|field2|rn|
|1|2022-01-01|1|
|1|2022-06-01|2|
|1|2022-12-31|3|
|2|2022-06-01|1|
|2|2022-12-31|2|



### 4. 조건식 WHERE

#### 1) IS NULL, IS NOT NULL

\- NULL의 비교는 IS, IS NOT을 통해서만 해야 한다.

\- NOT TRUE는 FALSE가 아니고, NOT FALSE도 TRUE가 아님을 주의해야 한다. (NOT TRUE에는 FALSE뿐 아니라 NULL 등도 포함된다. 그러나 NULL은 FALSE가 아니며, 따라서 NOT FALSE에는 NULL 등도 포함된다.)

\- NULL을 TRUE나 FALSE와 논리연산 시 UNKNOWN 값이 리턴되므로 주의해야 한다.



#### 2) field1 BETWEEN 1 AND 10

\- field1의 값이 1과 10 사이 정수인 튜플을 가져온다.

#### 3) field1 IN (1, 3, 5, 7, 9) / field1 NOT IN (1, 3, 5, 7, 9)

\- field1의 값이 1, 3, 5, 7, 9 중 어느 하나인 튜플(또는 1, 3, 5, 7, 9가 아닌 튜플)을 가져온다.

\- 괄호 안에는 위와 같이 쉼표로 구분된 정수/문자열 대신 (SELECT field2 FROM table2)와 같은 SELECT 쿼리가 들어갈 수도 있다. 단, 이때 괄호 안에 들어가는 SELECT 쿼리는 속성명을 하나만 가져야 한다.

#### 4) field1 LIKE '%오%'

\- field1의 값이 '오'를 포함하는 문자열인 튜플을 가져온다.

\- LIKE는 대소문자를 구분하므로, 대소문자를 구분하지 않는 문자열을 가져오려면 ILIKE로 대체하면 된다.



### 5. GROUP BY field1

```sql
SELECT field1, count(field1) AS cnt FROM table1 WHERE field1 >= 3 GROUP BY field1 HAVING cnt >= 2 ORDER BY field1 
```

\- 그룹함수와 함께 쓰며, 각 튜플이 그 속성명의 각 값과 그 값에 해당하는 그룹함수 값으로 이루어지는 **새로운 테이블을 만들어 가져온다.** 예를 들어, table1의 각 튜플들이 field1 속성에 대하여 각각 값이 1, 1, 3, 3, 5, 5라 할 때, 위 쿼리는 다음 테이블을 가져온다.

| field1 | cnt |
|---|---|
| 3 | 2 |
| 5 | 2 |

\- HAVING 뒤의 조건식은 GROUP BY로 테이블을 만들기 전에 **SELECT 쿼리로 구해진 결과값 중 해당 조건을 충족하는 튜플만을 새로 만들 테이블의 소스로** 하게 한다.

\- GROUP BY 뒤에 쓰는 field1은 여기 쓴 field1처럼 속성명을 그대로 쓰지 않고, SELECT 바로 뒤에 쓴 속성명들 중 몇 번째 속성명을 기준으로 GROUP BY를 할 것인지 숫자를 적어도 된다. (즉, field1 대신 field1에 해당하는 숫자인 1을 적어도 된다.)

\- GROUP BY 뒤에 쓰는 속성명은 둘 이상일 수 있다. 이 경우 GROUP BY 뒤에 쓰인 속성명에 해당하는 값이 모두 일치하는 경우를 기준으로 grouping이 된 테이블을 만들어 가져온다.


### 6. JOIN

#### 1) INNER JOIN

```sql
SELECT table1.field1 FROM table1 JOIN table2 ON table1.field1 = table2.field2
```

\- table1의 각 튜플의 field1 값이 table2의 어떤 튜플의 field2 값과 일치할 때 **그 두 튜플을 결합한 튜플들로 이루어진 테이블을 만들고** 그 테이블에서 지정된 속성명의 모든 값이 위에서 아래로 죽 나열된 튜플들을 가져온다. _(한편, ON 부분을 생략하면 table1의 모든 튜플에 table2의 모든 튜플을 각각 결합한 튜플들로 이루어진 새로운 테이블이 만들어진다.)_

\- table1의 field1값과 table2의 field2값이 서로 일치하는 튜플만 가져오며, 일치하는 튜플이 없다면 그 튜플은 가져오지 않는다.

\- table1의 field1값과 table2의 field2값이 일치하는 튜플이 일대 다로 여럿이라면 하나를 복제한 후 거기에 일치하는 모든 튜플을 **모두 결합하여** 가져온다. (이런 속성이 있기 때문에, **실수로 어느 한 테이블에 중복 튜플이 삽입되게 되면 JOIN 연산을 통해 전체 연산의 오차가 기하급수적으로 커질 위험이 있다.** 따라서 DB에서 중복 튜플이 있는지를 점검하는 일은 매우 중요한 문제가 된다.)


\- table1과 table2의 속성명이 일치하는 경우가 있더라도 일단 **두 속성명 모두 join된 결과 테이블에 존재**한다. 이때 각 속성명은 그 속성이 원래 있던 테이블명을 속성명 앞에 붙여 지칭한다.


#### 2) OUTER JOIN(LEFT JOIN, RIGHT JOIN, FULL JOIN)

\- table1의 field1값과 일치하는 table2의 field2값이 없더라도 일단 table1의 모든 튜플을 다 가져오는 게 LEFT JOIN, table2의 field2값과 일치하는 table1의 field1값이 없더라도 일단 table2의 모든 튜플을 다 가져오는 게 RIGHT JOIN이다. table1, table2의 모든 튜플을 일단 다 가져오는 게 FULL JOIN이다. (빈 속성명에는 NULL값이 채워진다.)

#### 3) CROSS JOIN

```sql
SELECT * FROM table1 CROSS JOIN table2;
```

\- table1의 모든 튜플(n개)과 table2의 모든 튜플(m개)을 각각 모두 결합하여 총 n * m개의 튜플을 만드는 것을 CROSS JOIN이라 한다. 이 경우 ON 같은 JOIN의 조건식을 쓰지 않는다.

#### 4) SELF JOIN

```sql
SELECT * FROM table1 t1 JOIN table1 t2 ON t1.field1 = t2.field2
```

\- 자기 자신과 JOIN 연산을 하는 것을 SELF JOIN이라 한다.


### 7. UNION, UNION ALL

\- 서로 다른 두 개의 SELECT 쿼리로 얻은 결과를 두 쿼리 사이에 UNION 연산자를 두어 이들을 결합해 하나의 결과로 만들 수 있다. 이때 UNION을 사용하면 앞의 결과와 뒤의 결과 중 완전히 일치하는 튜플(중복된 튜플)은 하나만 남겨두며, UNION ALL을 사용하면 중복을 버리지 않고 모두 하나의 결과로 결합시킨다.

\- 뒤의 SELECT 쿼리에 나온 속성명들은 무시되며 앞의 SELECT 쿼리에 나온 속성명을 순서대로 그대로 쓰게 된다.

\- 앞의 SELECT 쿼리에 나온 속성명의 개수는 뒤의 SELECT 쿼리에 나온 속성명의 개수와 완전히 일치해야 에러가 발생하지 않는다. 



### 8. WITH table2 AS (SELECT * FROM table1)

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



### 9. CREATE TABLE table2 AS SELECT * FROM table1

\- AS 뒤의 쿼리에 해당하는 테이블을 가져와 table2라는 이름의 테이블로 저장한다.