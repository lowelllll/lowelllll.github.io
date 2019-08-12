---
title: flask SQLAlchemy ORM 사용해보기
description: SQL 쿼리 대신 ORM을 사용하여 데이터베이스를 CURD 할 수 있습니다.
categories:
 - TIL
tags: [flask orm, flask sqlalchemy orm, orm 사용법, flask self join, sqlalchemy selfjoin, sqlalchemy lastrowid, sqlalchemy update]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

## ORM 사용법

### SELECT

```python
session.query(Model).all() # SELECT * FROM model
session.query(Model.id, Model.name, Model.age).all() # SELECT id, name, age FROM model
session.query(Model).first() # SELECT * FROM model LIMIT 1

from sqlalchemy import func # count 함수
session.query(func.count(Model.id))
# SELECT COUNT(id) FROM model

# AS 
session.query(Model.id.label('model_id')).all()
# SELECT id AS model_id FROM model
```

### WHERE

`filter`를 사용한다.

```python
session.query(Model).filter(Model.name == 'lowell').all() 
# SELECT * FROM model WHERE name = 'lowell'

session.query(Model).filter(Model.name == 'lowell', Model.age == 20).all() 
# SELECT * FROM model WHERE name = 'lowell' AND age = 20

from sqlalchemy import or_ # OR 연산자
session.query(Model).filter(or_(Model.mame == 'lowell', Model.age == 20)).all()
# SELECT * FORM model WHERE name = 'lowell' OR age = 20
```

### INSERT

```python
user = Model(name='lowell', age=20)
session.add(user)
session.commit() # session.rollback()

# INSERT INTO model(name, age) VALUES ('lowell', 20)
# COMMIT
```

### UPDATE

방법 1

```python
user = session.query(Model).filter(Model.name == 'lowell').first()
user.age += 1
session.commit()

# select 후 update 함
```

방법 2

```python
user = session.query(Model).filter(Model.name == 'lowell').update({'age': User.age + 1});
session.commit()

# select를 하지 않는 update 방법
# UPDATE model SET age = age + 1 WHERE name = 'lowell'
```

### DELETE

```python
user = session.query(Model).filter(Model.name == 'lowell').first()
session.delete(user)
session.commit()
```

### ORDER BY

```python
session.query(Model).filter(Model.name == 'lowell').order_by(Model.created_at)
# SELECT * FROM model WHERE name = 'lowell' ORDER BY created_at

session.query(Model).filter(Model.name == 'lowell').order_by(Model.created_at.desc(), Model.status) 
# SELECT * FROM model WHERE name = 'lowell' ORDER BY created_at DESC, status

```

### JOIN

#### INNER JOIN

```python
session.query(Model1, Model2).filter(Model1.id == Model2.id).all() 
# SELECT * FROM model1 JOIN model2 ON model1.id = model2.id

session.query(Model1).join(Model2, Model1.id == Model2.id).all()
# SELECT * FROM model1 JOIN model2 ON model1.id = model2.id
```

#### OUTER JOIN

```python
session.query(Model1). \
    outerjoin(Model2, Model1.id == Model2.id).\
    all()
    
# SELECT * FROM model1 LEFT JOIN model2 ON model1.id = model2.id 

```

여러개 조인을 하기 이해선 그냥 이어 붙이면 된다.

```python
session.query(Model1.name, Model2.student_id, Model3.account).\
	outerjoin(Model2, Model1.id == Model2.id).\
	outerjoin(Model3, Model1.id == Model3.id).\
	all()
	
# SELECT model1.name, model2.student_id , model3.account 
# FROM model1
# LEFT JOIN model2 ON model1.id = model2.id
# LEFT JOIN model3 ON model1.id = model3.id
```

#### SELF JOIN

> 하나의 테이블을 조인하는 것

`aliased` 를 사용하면 된다.

```python
model2 = aliased(Model)

self.session.query(Model).\
    join(model2, model2.id == Model.id).\
    all()
    
# SELECT model.* FROM model JOIN model model2 ON model2.id = model.id;
    
```

### GROUP BY

```python
session.query(Model).group_by(Model.id).all()

# SELECT * FROM model GROUP BY id
```

### SUBQUERY

```python
from sqlalchemy import subquery

stmt = query.session(Model2).filter(Model2.grade == 'A').subquery() 
# SELECT id, grade FROM model2 WHERE grade = 'A'

session.query(Model1, stmt.c.id, stmt.c.grade).\
	outerjoin(stmt, stmt.c.id = Model1.id)
	
# SELECT model1.*, model2.id, model2.grade
# FROM mode1l LEFT JOIN (SELECT id, grade FROM model2 WHERE grade = 'A') model2 
# ON model1.id = model2.id
```

### 그 외 팁

#### CASE 문

```python
from sqlalchemy import case
	
session.query(
	case(
        	[
            	(Model.age >= 20, 'adult'),
                (Model.age >= 10, 'teenager')
        	], 
        	else_='not adult, not teenager'
    	)
    ).\
    filter(Model.sex='female').\
    all()

# SELECT CASE WHEN age >= 20 THEN 'adult' WHEN age >= 10 THEN 'teenager' ELSE 'not adult, not teenager' FROM model WHERE sex = 'female';
	
```

#### last_row_id 얻기

```python
user = Model(name='lowell', age=20)
session.add(user)
session.flush() # DB connection 일어남

id = user.id # auto_encrement로 생성된 id

session.commit()
```

#### 검색 (LIKE)

```python
results = session.query(Model).\
	filter(Model.name.like('김%')).all() # 성이 김씨인 사람 찾음
    
# 응용해보긔 

keyword = kwargs.get('keyword', '')
search = True 

if keyword:
    search = Model.name.like(f'{keyword}%')
    
results = session.query(Model).\
	filter(search).all() # 검색어가 있으면 검색, 없으면 모두 가져옴
```

#### IN

```python
session.query(Model).filter(Model.name.in_(('lowell', 'yejin'))).all() 
# SELECT * FROM model WHERE name IN ('lowell', 'yejin');
```

#### NOT IN

```python
session.query(Model).filter(~Model.name.in_(('lowell', 'yejin'))).all() 
# SELECT * FROM model WHERE name NOT IN ('lowell', 'yejin');
```

#### COMMIT, ROLLBACK

```python
session.commit() # commit
session.rollback() # rollback
```

#### DATE 계산하기 (DATE_ADD)

```python
today = datetime.datetime.now().strftime('%Y-%m-%d')
session.query(Model).filter(today >= func.ADDDATE(Model.created_at, 30)).all()
# SELECT * FROM model WHERE NOW() >= DATE_ADD(created_at, INTERVAL 30 DAY);
```

### refer

https://stackoverflow.com/questions/9667138/how-to-update-sqlalchemy-row-entry