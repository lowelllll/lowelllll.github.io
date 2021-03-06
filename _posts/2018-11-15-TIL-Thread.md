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
- 각각의 스레드는 독립적인 작업을 수행해야 하기 때문에 각자의 스택과 PC Register를 가짐

### 스레드의 구성

- 스레드 ID
- 프로그램 카운터
- 레지스터 집합
- 스택  



###멀티 스레딩

>  하나의 프로세스를 다수의 실행단위로 구분하여 자원을 공유하고 자원의 생성과 관리의 중복성을 최소화하여 수행능력을 향상시키는 것.

여러 프로세스로 할 수 있는 작업들을 하나의 프로세스에서 스레드로 나누어 수행함.

#### 장점

- 프로세스를 이용하여 동시에 처리하던 일을 스레드로 구현할 경우 메모리 공간과 시스템 자원소모가 줄어듬
- 스레드간 통신이 필요한 경우에도 별도의 자원을 이용하는 것이 아닌 `Heap` 영역을 이용해 데이터를 주고받음.
- 스레드의 `context switching`은 캐시 메모리를 비울 필요가 없기 때문에 빠름.  

#### 문제점

- 멀티 프로세싱 기반의 프로그래밍을 할 땐 프로세스간 공유하는 자원이 없어 동일한 자원에 동시에 접근하는 일이 없지만 멀티스레딩은 서로 다른 스레드가 데이터와 힙 영역을 공유하기 때문에 어떤 스레드가 다른 스레드에서 사용중인 변수나 자료구조에 접근해 엉뚱한 값을 읽어오거나 수정할 수 있음

  #### 해결 

  멀티 스레딩 환경에서는 동기화 작업이 필요함!

  - 동기화를 통해 작업 처리 순서 컨트롤, 공유 자원에 대한 접근을 컨트롤 해야함. 이 때 병목현상이 발생해 성능이 저하될 가능성이 있기 때문에 과도한 락을 걸지 않도록 주의함.



## refer

[https://magi82.github.io/process-thread](https://magi82.github.io/process-thread)

[http://threestory.tistory.com/3](http://threestory.tistory.com/3)