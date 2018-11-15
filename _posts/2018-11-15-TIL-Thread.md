---
title: Thread
description: 181113 Today I learned Thread
categories:
 - TIL
tags: [Thread, 쓰레드, 멀티스레딩]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

# Thread

> 프로그램의 실행단위

- 한 프로세스 내에서 동작되는 여러 실행 흐름으로 프로세스 내의 주소 공간이나 자원을 공유할 수 있음

- 프로세스에 할당 된 자원을 씀!

### 스레드의 구성

- 스레드 ID
- 프로그램 카운터
- 레지스터 집합
- 스택

### 멀티 스레딩

>  하나의 프로세스를 다수의 실행단위로 구분하여 자원을 공유하고 자원의 생성과 관리의 중복성을 최소화하여 수행능력을 향상시키는 것.

- 각각의 스레드는 독립적인 작업을 수행해야 하기 때문에 각자의 스택과 PC Register를 가짐



## refer

[https://magi82.github.io/process-thread](https://magi82.github.io/process-thread)

[http://threestory.tistory.com/3](http://threestory.tistory.com/3)