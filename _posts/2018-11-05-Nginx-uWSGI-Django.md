---
title: AWS(rhel fedora)Nginx-uWSGI-Django 연동하기
description: AWS EC2 환경에서 Nginx-uWSGI-Django를 연동해 배포해보자.
categories:
 - django
tags: [django,nginx uwsgi django 연동, nginx uwsgi django, aws nginx django, aws django 배포]
---

이 포스팅에서는 AWS에서 서버를 셋팅할 것이기 때문에 AWS EC2 설정이 되어있다는 가정하에 진행한다.

또한 글쓴이의 OS 환경은 레드햇 페도라 환경에서 진행했다.

---

먼저 간단하게 서버 구조를 말하면  Nginx 80번 포트로 접속하면 uWSGI 8000번 포트로 요청을 보내 우리가 만든 Django 프로젝트에 접속하게 될 것이다.

우리가 사용자에게 listen할 포트(80)는 열어놔야하기 때문에 AWS 사용자 지정 포트에서 80번 포트를 열어줘야한다.

포트를 변경하고 싶으면 변경해도 되지만 설정한 포트에따라 사용자 지정 포트를 열어줘야 하는 것도 명심하자.

![nginx_wsgi_django](https://pic3.zhimg.com/v2-9bc6cfcdb7b946a728ccfe26f4eb5c01_1200x500.jpg)

### 1. Django

django 프로젝트는 배포될 준비가 다 되어있다는 가정 하에 진행한다.

- 가상환경 셋팅 

- static root 설정
- media root 설정
- `$ python manage.py collectstatic` 

### 2.Nginx

글쓴이는 Nginx 설정에 정말 삽질을 했다.

블로그를 보며 배포를 하려고 하는데 블로그에서는 sites-available 폴더에 conf 파일을 넣으면 된다고하는데.. 해당 폴더가없어서 삽질을 엄청했다. 

이 포스팅은 `sites-available`, `sites-enabled` 폴더가 없는 개발자들에게 유용한 포스트가 될 것이다.(그러길 바란다..)



먼저 uWSGI와 nginx 서버 등을 설정하기 위해 conf 파일을 생성한다.

etc/nginx/conf.d/testprj.conf 생성 (nginx의 경로는 OS 별로 다를 수  있기 때문에 꼭 확인해보자.)

`[..] 대괄호 안에있는 것은 자신의 프로젝트의 이름이나 경로에 따라 알맞게 변경한다.`

```
# etc/nginx/conf.d/testprj.conf 

server {
    listen                   80; # nginx가 listen할 포트
    server_name              [www.your.domain.com 123.23.56.5] #서버 도메인이나 아이피 입력
    client_max_body_size     10M;
    access_log               /var/log/testprj.access.log; # 성공 로그를 기록할 파일
    error_log                /var/log/testprj.error.log; # 에러 로그를 기록할 파일 

    # -------------
    # Handle Django
    # -------------

    location / {
    	# 외부에서 특정 포트로 Nginx를 통해 http 요청을 받았을 때 요청을 uWSGI를 통해 Django로 넘김
        proxy_pass       http://localhost:8000; # uWSGI가 nginx 요청을 받을 주소와 포트
        proxy_set_header Upgrade            $http_upgrade;
        proxy_set_header Connection         "upgrade";
        proxy_set_header Host               $host;
        proxy_set_header X-Real-IP          $remote_addr;
        proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto  $scheme;
    }

    # ------------------
    # serve static files
    # ------------------


    # here we assume the STATIC_ROOT inside your django project is
    # set to /static/
    location /static/ {
    	# /static/ 으로 들어 올 때 경로 설정.
        alias   [/home/user/testprj/static/]; # django project settings.py에 설정되어있는 STATIC_ROOT 주소
    }
}

```

여기서 

```
$ sudo nginx -t
nginx: [emerg] could not build server_names_hash, you should increase server_names_hash_bucket_size: 64
nginx: configuration file /etc/nginx/nginx.conf test failed
```

nginx 서버 확인 시 이런 오류가 난다면

`nginx.conf` 파일 수정을 해줘야 한다.

아래 설정을 `http {} block`에 설정 안에 넣는다.

```
# etc/nginx/nginx.conf

htttp{
    server_names_hash_bucket_size 512;
	server_names_hash_max_size 512;
	...
}

```

오류에는 64라고 되어있어 64로 했더니 오류가 사라지지 않아  512로하니까 되었다.

이렇게 하고` $ sudo nginx -t `수행 -> 성공하면 nginx 설정 끝!

### 3. uWSGI

가상환경이 활성화된 상태에서 uWSGI 설치 

`$ pip install uwsgi`

잘 깔렸는지 확인해보자

`$ wsgi --http :8000 --module [your-project-name].wsgi` (manage.py 디렉토리에서)

잘 되면 ini파일 생성

`$ sudo vi [testprj].ini`

```
# home/user/testprj/[testprj].ini

[uwsgi] 
module          =  [testprj].wsgi:application
master          =  true
pidfile         =  [testprj].uwsgi.pid
enable-threads  = true
# uWSGI 포트 설정 
http            =  127.0.0.1:8000 
processes       =  5
harakiri        =  50
max-requests    =  5000
# clear environment on exit
vacuum          =  true
# 가상환경 설정
home            =  [/home/user/.pyenv/versions/3.6.5/envs/devenv]
# background the process
daemonize       =  [testprj].uwsgi.log
```

이렇게 하구 저장! 

- 실행 `$ uwsgi testprj.ini (가상환경 안에서 )`
- uWSGI 로그확인 `$ vi testprj.uwsgi.log`
- uWSGI 중지 `$ uwsgi --stop [testprj].uwsgi.pid`

uWSGI 가 실행된 상태에서 nginx 서버를 시작한다.

`$ sudo service nginx start`

이후 맨 처음 nginx에서 생성한 conf 파일의 server_name에 설정한 주소(AWS 주소)로 들어가보면 Django project가 띄어져 있을 것이다! 그러면 연동 끝! 



설정 중 오류가 있으면 댓글 남겨주세요 💁‍

## refer

[https://medium.com/@charlesthk/deploy-nginx-django-uwsgi-on-aws-ec2-amazon-linux-517a683163c6](https://medium.com/@charlesthk/deploy-nginx-django-uwsgi-on-aws-ec2-amazon-linux-517a683163c6)

[https://www.savour-it.com/posts/2018-02-06-nginx-uwsgi-django-setting/](https://www.savour-it.com/posts/2018-02-06-nginx-uwsgi-django-setting/)

[https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-uwsgi-and-nginx-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-uwsgi-and-nginx-on-ubuntu-14-04)



[https://blog.leop0ld.org/posts/use-python3-django-uwsgi-nginx/](https://blog.leop0ld.org/posts/use-python3-django-uwsgi-nginx/)

[http://charles.lescampeurs.org/2008/11/14/fix-nginx-increase-server_names_hash_bucket_size](http://charles.lescampeurs.org/2008/11/14/fix-nginx-increase-server_names_hash_bucket_size)