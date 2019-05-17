---
title: MySQL ORDER BY 정렬 시 조건 걸기, CASE 사용하기
description: order by 구문을 통해 정렬할 때 조건에 맞게 정렬을 할 수 있습니다. 또 CASE로 원하는 값을 추출할 수 있습니다.
categories:
 - TIL
tags: [order by case, order by field, order by 정렬]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

쿼리를 통해 데이터를 가져올 때 데이터를 기반으로 조건을 걸어서 정렬하고 싶을 때가 있다. 그때 `CASE`나 `FIELD`를 사용하면 가능 조건에 따라 데이터의 정렬 우선순위를 정해줄 수 있다.

### FIELD

`ORDER BY FIELD (column, 1순위, 2순위, 3순위, n순위...)`

어떤 데이터의 값의 정렬을 정해주고 싶을 때 사용하면 된다. 

```
> SELECT id, title, status FROM movie
id title status 
1	아바타 	2 
2	써니 		 0
3	극한직업	1
4	캡틴마블 	1

> SELECT * FROM movie ORDER BY FIELD(status, 1, 2, 0) 
id title status
3	극한직업	1
4	캡틴마블 	1
1	아바타 	2 
2	써니 		 0
```

위 예제는 status의 값 별로 정렬 순위를 정해준 것이다. 값 1이 1순위고 2가 2순위, 0이 3순위인 것을 확인할 수 있다. 

### CASE

`CASE
    WHEN <condition1> THEN <result1>
    WHEN <condition2> THEN <result2>
    WHEN <conditionN> THEN <resultN>
    ELSE <result>
END;`

if문과 비슷하게 동작하는 CASE문은 condition에 조건을 넣으면 되고 then 뒤에는 정렬 순위를 넣으면 된다.

```
> SELECT id, title, start_at, end_at
  CASE
  WHEN start_at <= NOW() AND NOW() < end_at THEN 1
    ELSE 2
  END movie_order
  FROM movie
  ORDER BY movie_order
```

위 예제는 오늘이 영화 상영 기간에 포함되어있다면 1순위, 아니면 2순위로 정렬된다.
