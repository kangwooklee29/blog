---
title: embedded SQL
---


### 1. 개요

\- SQL은 DB에서 값을 변경하고 추출해 오는 데는 기능이 충분하지만 C나 Java 같은 언어처럼 다양한 기능을 가진 것은 아니라 DB 시스템을 구축하고 활용할 때 C나 Java 같은 언어를 활용할 필요가 있을 수 있다. 이처럼 보다 다양한 기능을 통해 DB 시스템을 구축하는 데 사용하는 언어를 host language라 하고, 그 언어 내부에서 DB를 활용하기 위해 사용하는 SQL을 embedded SQL이라 한다.

\- 많은 DBMS에서 여러 host language로 작성된 코드 내부에 embedded SQL 쿼리를 작성하는 것을 허용하며, 이렇게 작성된 쿼리들은 embedded SQL 쿼리만을 처리하는 precompiler를 거쳐 host language 코드로 변환되어 host language의 컴파일러를 통해 컴파일이 이루어지게 된다. (embedded SQL은 SQL 쿼리 맨 앞에 `EXEC SQL`이라는 예약어가 붙은 형태로 host language로 작성된 코드에 포함되므로, precompiler는 `EXEC SQL`이라는 예약어가 붙은 부분을 처리하게 된다.)


### 2. 정적 SQL와 host variables

\- host language의 문자열 변수를 빌리지 않고, 일반적으로 쓰이는 SQL 쿼리 맨앞에 단순히 `EXEC SQL` 예약어를 쓰는 식으로 SQL 쿼리를 사용하는 방식을 정적 SQL(static SQL)이라 한다.

\- 정적 SQL에서는 host language에서 선언한 변수(host variables)를 사용할 수 있으며, host variables는 SQL 쿼리 내에서 그 변수명 앞에 `:`을 붙인 형태로 사용된다. 

\- 다음 방식으로 host variables를 선언하고 사용할 수 있다.

```c
EXEC SQL BEGIN DECLARE SECTION;
float avr;
EXEC SQL END DECLARE SECTION;

...

EXEC SQL SELECT * FROM table1 WHERE field2 < :avr;
```


### 3. impedence mismatch와 cursor

\- host language들은 일반적으로 데이터를 하나의 변수, 객체에 담을 뿐 객체와 객체 사이 관계를 고려하는 것 같은 기능은 규정되어 있지 않은 반면, SQL은 데이터를 릴레이션의 구성요소 또는 릴레이션 사이 관계를 통해 얻을 수 있는 개념으로 바라보고 있다. 이와 같은 차이로 인해 host language와 embedded SQL 사이에는 여러 충돌 문제가 발생하며, 이를 impedence mismatch라 한다.

\- 이 mismatch 문제에 대한 보완책으로 embedded SQL에서는 cursor라는 개념을 사용하는데, 이는 SQL의 SELECT 쿼리를 통해 얻어온 릴레이션의 각 튜플을 위에서부터 하나씩 읽어와 그 값들을 host variables에 전달하게 할 때 어떤 튜플의 값들을 host variables에 전달할지를 가리키는 일종의 포인터이다. 


\- 정적 SQL에서는 다음 순서대로 커서를 사용할 수 있다. (1)DECLARE CURSOR FOR 쿼리를 통해 특정 릴레이션에 대한 커서를 선언한다. (2)OPEN 쿼리를 통해 커서를 사용할 것을 선언한다. (3)FETCH INTO 쿼리를 통해 그 커서의 릴레이션으로부터 튜플을 맨 위에서부터 하나씩 읽어온다. (4)CLOSE 쿼리를 통해 커서 사용을 종료한다. 구체적 구현 사례는 다음과 같다.

```c
int sum_math=0, sum_eng=0;
EXEC SQL BEGIN DECLARE SECTION;
int math, eng;
EXEC SQL END DECLARE SECTION;

EXEC SQL DECLARE cursor1 CURSOR FOR SELECT field1, field2 FROM table1;
EXEC SQL OPEN cursor1; 

while(1)
{
    EXEC SQL FETCH cursor1 INTO :math, :eng;
    if(SQLCODE)
    	break;
    sum_math += math;
    sum_eng += eng;
}

EXEC SQL CLOSE cursor1;
```


#### * 커서를 이용한 DB 업데이트

\- 커서를 이용해 DB를 업데이트 할 수 있다. (1)갱신하고자 하는 튜플들이 담긴 릴레이션에 대한 커서를 선언하고(이때 반드시 '어떤 속성에 대한 업데이트 목적의 커서다'라는 표현을 함께 사용해야 한다) (2)UPDATE 쿼리를 실행할 때 '그 커서 위치에 업데이트한다'라는 표현을 함께 사용한다. 구체적 구현 사례는 다음과 같다.

```c
EXEC SQL DECLARE cursor1 CURSOR FOR SELECT field1 FROM table1 WHERE field2=10 FOR UPDATE OF field1;
EXEC SQL OPEN cursor1; 
EXEC SQL UPDATE cursor1 SET field1 = 0 WHERE CURRENT OF cursor1;
EXEC SQL CLOSE cursor1;
```



### 4. SQL 통신 영역(SQL communications area, SQLCA)

\- embedded SQL 쿼리의 실행 결과로 에러가 발생하는 경우가 있을 수 있으며, 이 경우 에러가 발생했다는 사실과 그 에러의 내용을 사용자가 알 수 있게 할 필요가 있다. 이를 위해 사용되는 것이 SQLCA이다. 에러 발생 시 SQLCA의 데이터 구조의 에러 필드에 에러 정보가 저장되며, 사용자는 host language 코드 부분에서 에러 필드를 (host variables를 호출하는 것과 같은 방식으로) 호출하여 그 내용을 알 수 있다.

\- SQLCA의 데이터 구조의 에러 필드는 여러 종류가 있는데, 대표적으로 SQLCODE라는 에러 필드가 있으며 이 에러 필드는 직전에 수행된 embedded SQL 쿼리가 정상적으로 실행됐다면 0이라는 값을 갖는다.

