---
title: flask SQLAlchemy ORM 사용해보기
description: SQL 쿼리 대신 ORM을 사용하여 데이터베이스를 CURD 할 수 있습니다.
categories:
 - TIL
tags: [flask orm, flask sqlalchemy orm, orm 사용법]


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

```python
user = session.query(Model).filter(Model.name == 'lowell').first()
user.age += 1
session.commit()

# select 후 update 함
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

