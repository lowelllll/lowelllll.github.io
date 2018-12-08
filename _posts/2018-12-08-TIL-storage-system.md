---
title: storage system
description: 181208 Today I learned storage system
categories:
 - TIL
tags: [storage system, 디스크 스케줄링]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

# 디스크 스케줄링

운영체제는 디스크를 효율적으로(탐색 시간 최소화) 사용하기 위해 여러 기법을 사용함. 

- `seek time` : 탐색시간. 디스크의 접근시간에 가장 큰 영향을 줌.
- 디스크의 입출력 요청에 포함되는 정보
  - 연산의 유형 : 입력 or 출력
  - 디스크 주소
  - 메모리 주소
  - 전송할 바이트의 수
- 디스크 드라이브와 제어기가 여유상태이면 바로 처리되고 아니라면 대기 큐에서 대기함.



현재 대기 큐에 `98, 183, 37, 122, 65, 67 ` 이렇게 들어와있고 현재 디스크의 헤드는 `53`을 가르키고 있다고 가정한다.

### FCFS 

> 대기 큐에서 가장 먼저 요청을 한 것 부터 처리를 함.

헤드의 움직임을 최적화  하지 않기 때문에 성능이 나쁠 수 있음.

![FCFS](https://mblogthumb-phinf.pstatic.net/20130726_4/jevida_1374822482883XyVjE_PNG/2.png?type=w2)

### SSTF (Shortest-Seek-Time-First)

> seek time이 가장 작은 것 부터 먼저 처리하는 방식

- 계속해서 탐색 시간이 작은 요청이 큐에 쌓이게 되면 탐색 시간이 긴 요청들은 처리되지 않는 `starvation(기아현상)`이 발생할 수 있음.

![SSTF](https://mblogthumb-phinf.pstatic.net/20130726_85/jevida_1374822483023IKYq8_PNG/3.png?type=w2)

### SCAN

> 디스크의 한 쪽 끝에서부터 다른 쪽 끝으로 이동한해 처리한 다음 방향을 바꾸어 처리하는 방식

- 끝과 끝을 왕복하는 모습이 엘레베이터와 비슷해서 `elevator algorithm`라고 불림.
- 비정상적으로 양 끝단의 request가 들어오면 문제가 발생할 수 있음.

![scan](https://mblogthumb-phinf.pstatic.net/20130726_158/jevida_1374822483175TOygk_PNG/4.png?type=w2)

### C-SCAN

> 끝을 향해 이동하다가 끝에 도달하게 된다면 아예 처음으로 이동 후 다시 시작함.

- SCAN의 발전된 알고리즘

![c-scan](https://mblogthumb-phinf.pstatic.net/20130726_226/jevida_1374822483303sDvx0_PNG/5.png?type=w2)

### C-LOOK

> C-SCAN과 달리 처음으로 돌아갈 때 맨 처음(0), 맨 끝으로 돌아가는 것이 아닌 가장 작은 request 의 위치와 가장 큰 request 위치로 이동함. 

![c-look](https://mblogthumb-phinf.pstatic.net/20130726_105/jevida_1374822483386IAUq9_PNG/6.png?type=w2)



- 디스크에 부하가 가는 작업은 **SCAN**이나 **C-SCAN**이 유리함.
- 보통 **SSTF**나 **LOOK**을 사용함.