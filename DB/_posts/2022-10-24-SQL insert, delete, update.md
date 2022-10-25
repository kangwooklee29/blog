---
title: SQL insert, delete, update
---

### 1. INSERT

```sql
INSERT INTO table1(field1, field2) VALUES (1, 2)
```

\- 지정한 relation에 새로운 tuple을 삽입하는 쿼리다. 

\- (그 relation의 foreign key를 통해) 다른 relation을 참조하는 relation에 tuple을 추가할 경우 (foriegn key와 primary key 사이 관계가 항상 유지돼야 한다는) 참조 무결성이 위배될 수 있다. (그러나 다른 relation의 foreign key에 의해 참조되는 relation에 tuple을 추가하는 경우에는 참조 무결성 위배가 발생하지 않는다.)

```sql
INSERT INTO table1(field1, field2) (SELECT field1, field2 FROM table2 WHERE field2 > 0)
```

\- VALUES 예약어를 생략하고 그 위치에 subquery를 써서 여러 개의 tuple을 한번에 삽입할 수도 있다.



### 2. DELETE

```sql
DELETE FROM table1 WHERE field2 < 0
```

\- 지정한 relation에서 조건에 맞는 tuple을 삭제하는 쿼리다.

\- 다른 relation의 foreign key에 의해 참조되는 relation에서 tuple을 삭제할 경우 그 삭제로 인해 참조 무결성이 위배될 수 있다. (단, 그 relation의 foreign key를 통해 다른 relation을 참조하는 relation에서 tuple을 삭제하는 경우에는 참조 무결성 위배가 발생하지 않는다.)


### 3. UPDATE

```sql
UPDATE table1 SET field2 = field2 * -1 WHERE field2 < 0
```

\- 지정한 relation에서 조건에 맞는 tuple의 attribute 값을 변경하는 쿼리다.

\- foreign key나 primary key에 속하는 attribute의 값을 변경 시 참조 무결성 위배가 발생할 수 있다.