---
title: heroku를 사용해 django 프로젝트 배포하기
description: PaaS 서비스 중 하나인 heroku를 이용해 django 프로젝트를 배포합니다.
categories:
 - django
tags: [django, django 배포, django heroku, heroku]
---

git이 설치되어있다고 가정한다.

### 1. heroku 회원가입

### 2. 헤로쿠 사용을 위한 패키지 다운  
`virtualenv` 활성화 상태에서 패키지 다운로드

```
$ pip install dj-database-url gunicorn whitenoise
```

### 3. `requirements.txt` 파일 생성

```
$ pip freeze > requirements.txt
```

생성된 requirements.txt 맨 아래에 아래 내용 추가

```
$ psycopg2==2.7.1
```

### 4.` Procfile `파일 생성
헤로쿠에게 웹 사이트를 시작시키기 위해 실행되어야할 명령어의 순서를 알려주기 위해 procfile 파일 생성 (확장자 x, cmd에서 vi나 nano로 생성)

```
web: gunicorn <mysite>.wsgi --log-file -
```

웹 애플리케이션을 배포할 때 `gunicorn <mysite>.wsgi` 명령을 실행하는 것을 의미  
(gunicorn은 강력한 버전의 runserver 명령어)

### 5. `runtime.txt` 파일 생성
헤로쿠에게 어떤 버전의 파이썬을 사용하는지 알려줌.

```
# djangoproject/runtime.txt
python-3.6.5 # 프로젝트 파이썬 버전
```

### 5. `settings.py` 설정
로컬 컴퓨터 설정 파일 `local_settigns.py`생성 

```python
# local_settings.py

import os
BASE_DIR = os.path.dirname(os.path.dirname(__file__))

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}

DEBUG = True
```

이 파일은 로컬에서 프로젝트를 돌릴 때 사용을 하기 위함인 것 같다.

### 6. 헤로쿠 배포를 위한 `settings.py` 설정

```python
# settings.py
import dj_database_url

...

DEBUG = False

ALLOWED_HOSTS = ['127.0.0.1', '.herokuapp.com']

...

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'lowell',
        'USER': 'name',
        'PASSWORD': '',
        'HOST': 'localhost',
        'PORT': '',
    }
}
...

db_from_env = dj_database_url.config(conn_max_age=500)
DATABASES['default'].update(db_from_env)
```

### 7. `wsgi.py` 설정  
프로젝트의 wsgi.py 파일의 끝에 다음 라인 추가

````python
# wsgi.py
...
from whitenoise.django import DjangoWhiteNoise
application = DjangoWhiteNoise(application)
````

### 8. `gitignore` 파일 생성  

```
*.pyc
db.sqlite3
myvenv
__pycache__
local_settings.py # 로컬 환경을 위한 파일이기 때문에 등록
```

### 9. heroku 로그인   
아까 만들었던 계정으로 로긘하깅

```shell
$ heroku login
```

### 10. git 저장소 생성,커밋  
현재 장고 프로젝트 루트 디렉토리에 git 저장소를 생성부터 커밋

```shell
$ git init
$ git add . 
$ git status 

$ git commit -m "additional files and changes for Heroku"
```

### 11. 애플리케이션 이름 설정

```shell
$ heroku create <appname>
```

이 이름은 도메인이 된다. `<appname>.herokuapp.com`

### 12. 이제 배포하면 된다.

```shell
$ git push heroku master
```

### 13. 애플리케이션 접속  
헤로쿠에 웹 프로세스를 시작하라고 말한다.

```shell
$ heroku ps:scale web=1
```

앱에 들어가보장. 아래 명령어를 입력하면 브라우저가 뜰 것이당.

```shell
$ heroku open
```

### 14. migrate , createsuperuser 

```shell
$ heroku run python manage.py migrate

$ heroku run python manage.py createsuperuser
```

###  오류 발생시   

오류가 생겼다면 다음 블로그를 참조해보자.

[오류해결](http://ggilrong.tistory.com/entry/heroku%EC%97%90-django-%EC%95%B1%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0
)

## refer
[https://tutorial-extensions.djangogirls.org/ko/heroku/](http://ggilrong.tistory.com/entry/heroku%EC%97%90-django-%EC%95%B1%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0
)

[http://ggilrong.tistory.com/entry/heroku%EC%97%90-django-%EC%95%B1%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0](http://ggilrong.tistory.com/entry/heroku%EC%97%90-django-%EC%95%B1%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0
)

[http://morningbird.tistory.com/23](http://morningbird.tistory.com/23)