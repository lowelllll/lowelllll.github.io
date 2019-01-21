---
title: mysql upsert
description:  190120 Today I learned upsert
categories:
 - db
tags: [upsert, db upsert]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

### upsert

데이터베이스에서 만약 특정 값이 없다면 **INSERT**를, 있다면 **update**를 해야하는 경우가 있을 것이다. 나는 원래직접 특정 값을 **select**해서 가져온 값이 있다면 update 없다면 insert 를 했었는데 mysql에서는 **UPSERT**를 지원한다.

```mysql
INSERT INTO table (id, comment) 
VALUES (1, 'hello')
ON DUPLICATE KEY UPDATE # update 
    comment = 'hello'
```

위 쿼리에서 `id` 는 테이블에 해당 값이 있는지 확인하는 컬럼이다. 이 id가 있다면 UPDATE를 수행하고 없다면 INSERT를 수행하게 된다.

