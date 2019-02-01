---
title: mysql count()의 default를 0으로 되게 하기
description: mysql select시 해당되는 결과가 없으면 count의 값을 0으로 나오게 해보자.
categories:
 - mysql
tags: [mysql count default 0]
---

count() 함수를 쓴 쿼리 조회시 해당되는 결과가 없을 시 0이 나오게 하고 싶을 경우가 있다. 

그럴때에는 `LEFT JOIN` 을 사용하면 원하는 값을 얻을 수 있다.

```sql
SELECT s.id, count(hi.id) as 'history_num'
FROM student s LEFT JOIN history hi ON s.id = hi.sid
WHERE s.id IN (1, 2, 3) GROUP BY u.id;

# 결과
s.id   history_num
1	   0
2 	   10
3      22
```

추가로 ` LEFT JOIN`할 테이블에 조건을 추가 하고 싶을 경우 **서브쿼리**를 사용하면 된다.

