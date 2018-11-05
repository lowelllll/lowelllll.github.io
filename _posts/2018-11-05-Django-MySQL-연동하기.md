---
title: Django-MySQL 연동하기
description: django에서 mysql을 연동해 데이터베이스를 연동합니다.
categories:
 - django
tags: [django mysql연동, django mysql]
---

이 포스팅은 mac과 linux 기반의 Django-MySQL 연동 글이다.

윈도우 환경에서 작업을 진행한다면, [이 포스팅](http://jamanbbo.tistory.com/28) 과 함께 보기 추천한다. (별로 다른 건 없지만 윈도우 환경에서 오류 해결 법이 좀 더 추가되어있다)

MySQL이 설치되어있다는 가정하에 진행하므로 꼭 설치 후 아래 항목들을 차례로 진행하길 바란다. (설치가 되어있지 않으면 오류가 난다)



### Django와 MySQL을 연동하기 위한 연동 드라이버를 설치

django에서 제공하는 MySQL 연동 드라이버 모듈은 3가지가 있다.

- MySQLdb - 제일 안정된 드라이버, python3은 지원하지 않음
- Mysqlclient - MySQLdb를 개선한 패키지, Python3.3 이상의 버전도 지원하고있음, **장고에서 추천함**
- MySQL Connector/Python - MySQL 개발사 오라클에서 제공하는 드라이버

위 모듈 중 자신에게 맞는 드라이버를 설치하면 되는데 우리는 mysqlclient를 사용할 것이다.

mysqlclient를 설치해준다.

`$ pip sintall mysqlclient`

centos/rhel fedora OS에서 설치시 

`mysqlclient OSError: mysql_config not found`

이런 오류가 발생한다면 mysql-devel을 설치해주면 해결된다.

`$ sudo yum install mysql-devel`



### Django에서 MySQL 연동 세팅

정상적으로 설치가 되었다면 django project의 `settings.py` 파일에서 DATABASE 항목을 설정해준다.

```python
# settings.py
DATABASES = {
	'default': {
		'ENGINE':'django.db.backends.mysql', # mysql 엔진 설정
		'NAME':'mysite', # 데이터베이스 이름 
		'USER':'root', # 데이터베이스 연결시 사용할 유저 이름
		'PASSWORD':'password',# 유저 패스워드
        'HOST':'l27.0.0.1', # 데이터베이스 서버 주소
        'PORT':'3306' # 데이터베이스 서버 포트
    }
}
```

`ENGINE`을 제외한 항목은 자신의 mysql 설정에 맞게 수정해야한다.

지금까지 설정이 올바르게 되었다면 Django와 MySQL이 연동이 되었을 것이다.



### MySQL의 데이터(테이블)를 반영

만약 현재 MySQL의 데이터베이스에 저장되어있는 테이블들을 Django project에 반영하고 싶다면 

`$ python manage.py inspectdb > ./app/models.py`

명령어를 사용하면 된다.

```mysql
mysql> desc board_tbl;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| idx     | int(6)       | NO   | PRI | NULL    | auto_increment |
| writer  | int(11)      | YES  |     | NULL    |                |
| subject | varchar(255) | YES  |     | NULL    |                |
| content | text         | YES  |     | NULL    |                |
| date    | datetime     | YES  |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+
```

MySQL 테이블이 model로 적용된 모습

```python
# app/models.py
...

class BoardTbl(models.Model):
    idx = models.AutoField(primary_key=True)
    writer = models.IntegerField(blank=True, null=True)
    subject = models.CharField(max_length=255, blank=True, null=True)
    content = models.TextField(blank=True, null=True)
    date = models.DateTimeField(blank=True, null=True)

    class Meta:
        managed = False
        db_table = 'board_tbl'
```



### Django migration을 MySQL에 반영

Django의 migration을 MySQL에 반영하는 것은 sqllite3에 migration을 반영하는 것과 같다.

` $ python manage.py migrate`

이 명령어를 실행하게 되면 MySQL 테이블에 auth_user 등 장고의 기본 테이블들이 생성된 것을 확인할 수  있을 것이다.



이렇게 마지막까지 잘 되었다면 MySQL을 사용할 준비를 마친 것이다.

이제 ORM, raw SQL 등을 기존 sqlite3 처럼 사용할 수 있다.

