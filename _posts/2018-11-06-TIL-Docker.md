---
title: Docker
description: 181106 Today I learned Docker 
categories:
 - TIL
tags: [docker, docker 사용법,docker 명령어]
---


![https://www.docker.com/sites/default/files/Whale%20Logo332_5.png](https://www.docker.com/sites/default/files/Whale%20Logo332_5.png)

## 도커란?

> 컨테이너 기반의 오픈소스 가상화 플랫폼



### 컨테이너

격리된 공간에서 프로세스가 동작하는 기술 (가상화 기술)

이미지를 실행한 상태.

### 이미지

컨테이너 실행에 필요한 파일과 설정 값등을 포함하고 있는 것.

상태 값을 가지지 않고 변하지 않음.

> 이미지가 `클래스`이면 컨테이너는 클래스의 `인스턴스`이다.



같은 이미지에서 여러 개의 컨테이너를 생성할 수 있고 컨테이너의 상태가 바뀌거나 삭제되더라도 이미지는 변하지 않고 그대로 남아있음.

> 하나의 서버에 여러개의 컨테이너를 실행하면 서로 영향을 미치지 않고 독립적으로 실행되어 마치 가벼운 VMVirtual Machine을 사용하는 느낌을 줍니다. 실행중인 컨테이너에 접속하여 명령어를 입력할 수 있고 apt-get이나 yum으로 패키지를 설치할 수 있으며 사용자도 추가하고 여러개의 프로세스를 백그라운드로 실행할 수도 있습니다. CPU나 메모리 사용량을 제한할 수 있고 호스트의 특정 포트와 연결하거나 호스트의 특정 디렉토리를 내부 디렉토리인 것처럼 사용할 수도 있습니다.

정말 가벼운 VM 사용하는 것 같음! 짱 

## Mac에서 도커 설치

[Docker-for-Mac](https://docs.docker.com/docker-for-mac/) 이 곳에서 로그인 후 설치!

설치가 완료되었다면 도커가 잘 설치되었는지 확인해보자.

```shell
$ docker version
Client:
 Version:           18.06.1-ce
 API version:       1.38
 Go version:        go1.10.3
 Git commit:        e68fc7a
 Built:             Tue Aug 21 17:21:31 2018
 OS/Arch:           darwin/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.06.1-ce
  API version:      1.38 (minimum version 1.12)
  Go version:       go1.10.3
  Git commit:       e68fc7a
  Built:            Tue Aug 21 17:29:02 2018
  OS/Arch:          linux/amd64
  Experimental:     true
```



## 명령어

**컨테이너 실행**

```shell
$ docker run ubuntu:16.04
```

`run` ubuntu 이미지가 있으면 docker 컨테이너 실행, 없으면 이미지 다운로드하고 컨테이너 실행 후 바로 종료됨.

```shell
$ docker run -rm -it ubuntu:16.04 /bin/bash
```

`-rm` 컨테이너 실행 후 종료시 컨테이너 삭제 옵션

`-it`  키보드 입력을 위한 옵션

```shell
$ docker run -d -p 1234:6379 redis

# 아래와 같이 접속
$ telnet localhost 1234
```

`-d` 백그라운드 작업

`-p` 컨테이너 포트를 호스트 포트로 연결

```shell
$ docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7
```

`-e` 환경변수 설정

`--name` 컨테이너 이름 설정

**컨테이너 목록 확인** 

```shell
$ docker ps # 실행 중인 컨테이너 목록
$ docker ps -a # 전체 컨테이너 목록
```

**실행 중인 컨테이너 중지 **

```shell
$ docker stop ${container_id or container_name}
```

**컨테이너 완전 삭제 **

```shell
$ docker stop ${container_id or container_name}
```

**이미지 목록 확인 **

```shell
$ docker images
```

**이미지 업데이트 **

```shell
$ docker pull ubuntu:14.04
```

 `pull`  이미지를 최신 버전으로 다시 다운 받을 수 있음.

**이미지 삭제 **

```shell
$ docker rmi ${image_id}
```

컨테이너가 실행 중인 이미지는 삭제되지 않음!

**컨테이너 실행 로그 확인 **

```shell
$ docker logs ${container_id or container_name}
```

컨테이너에서 실행한 로그를 확인.

```shell
$ docker logs --tail 10 ${container_id or container_name}
```

`--tail`컨테이너에서 실행한 로그 마지막 10줄까지 출력.

```shell
$ docker logs -f ${container_id or container_name}
```

`-f`실시간으로 로그가 생성되는 것을 확인함.

**컨테이너 명령어 실행**

```shell
$ docker exec -it mysql /bin/bash # 컨테이너에 접속
$ mysql -uroot 
```

컨테이너에 들어가 보거나 컨테이너의 파일을 실행하고 싶을 때.

`run` 새로 컨테이너를 만들어서 실행.

`exec` 실행 중인 컨테이너에 명령어를 내림.

접속한 이후에는 어떤 작업도 할 수 있고 컨테이너를 마음껏 조작 할 수 있음.



##  그 외

컨테이너를 삭제한다는 것

- 컨테이너에서 생성된 파일이 사라지는 것
- 데이터베이스의 데이터가 사라지는 것
- 웹 애플리케이션의 사용자가 올린 이미지가 사라지는 것

**컨테이너 삭제시 유지해야하는 데이터는 반드시 컨테이너 내부가 아닌 외부 스토리지에 저장해야함.**



### Docker compose

설정 파일을 이용한 도커 설정 

```dockerfile
version: '2'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: wordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     volumes:
       - wp_data:/var/www/html
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_PASSWORD: wordpress
volumes:
    db_data:
    wp_data:
```



## refer

[https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)