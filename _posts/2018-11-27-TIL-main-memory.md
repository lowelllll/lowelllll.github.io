---
title: Main Momery 
description: 181124 Today I learned Memory Management
categories:
 - TIL
tags: [메인 메모리, 메모리 관리, main memory, memory management]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

## 메모리 계층 구조

### 레지스터

> CPU Core 내부에 존재하며 Core에서 연산을 수행할 때 직접 참조할 수 있는 유일한 기억장치

Core 내부에도 몇 개 존재하지 않는 귀한 기억장치이며 보통 한 워드(프로세서가 처리할 수 있는 가장 자연스러운 데이터의 크기)가 레지스터 하나의 크기임.

### 캐시 메모리

> CPU Core 내부에 존재하며 Core와 입출력 버스를 통해 정보를 주고 받음

명령어를 저장하는 `Instruction Cache`와 데이터를 저장하는 `Data Cache`로 구분되어있음.

주로 `Refresh` (메모리의 내용을 보존하기 위해서 주기적으로 데이터를 갱신하는 작업. 이 작업이 수행중일 때는 메모리에 읽거나 쓰는 동작을 하지 못함)가 필요없는 `SRAM(Static RAM)`으로 구성함. 

CPU 내부에서 상당한 공간을 차지함.

#### 캐싱 (Caching)

데이터를 느린 하위 계층(보조기억장치)의 데이터를 미리 가져다 놓는 작업

**캐싱할 데이터는 어떻게 선별할까?**

**시간적 지역성 (Temporal Locality)** : 최근 접근된 내용이 또 다시 접근될 확률이 높다.

**공간적 지역성 (Spatial Locality)** : 한 지역의 내용이 접근되었다면, 그 주위의 내용도 접근될 확률이 높다.

CPU 내부에 있는 캐시 컨트롤러는 위 두가지 원리를 적용해 캐시 메모리에 적재될 내용을 관리함.

### 메인 메모리

> CPU 외부에 존재하며 CPU와 North Bridge를 통한 데이터 버스를 통해 통신함.

데이터를 보존하기 위해서 주기적으로 `Refresh`가 필요한 `DRAM`으로 구성됨.

### 보조기억장치

> 하드디스크, CD, DVD, USB 메모리 등. 메인 메모리에 비해 접근 속도가 현저하게 떨어짐

#### 프로세서에서 접근 속도가 빠른 기억장치의 순서

1. 레지스터
2. 캐시 메모리
3. 메인 메모리
4. 보조 기억장치

접근 속도가 빠르다 = 프로세서와 가깝다.

# 메모리 관리

### Base Register, Limit Register

운영체제는 각 프로세스들이 메모리에 올라왔을 때 다른 프로세스가 메모리 공간에 접근하는 것을 막아야함.

이를 위해 메모리에서 프로세스의 시작 위치를 저장하는 `Base register`와 프로세스에게 할당된 메모리의 크기 `Limit register` 값을 이용해 프로세스들의 메모리 공간(space)의 경계를 나눔.

![메모리](https://t1.daumcdn.net/cfile/tistory/9950C73359C4A5E416)

**Base Register <= 프로세스가 접근할 수 있는 공간 <= Base Register+ Limit Register**

### Address Binding

> 프로세스가 접근해야 하는 값과 함수에 대한 주소가 정해지는 것

`Logical Address`, `Virtual Address` 

- 논리적 주소. CPU가 생성하는 주소

`Physical Address` 

- 물리적 주소. 실제 접근해야하는 메모리 주소.
- 메모리에 나타나는 주소
-  `Logical Address`와 다를 수 있음.

#### 바인딩 시점

1. 컴파일 시점 주소 바인딩

   > **컴파일이 될 때 컴파일러에서 멍령어들과 변수의 메모리 주소가 정해지는 것**

   - 여기서 정해지는 주소는 실제 메모리 주소. 
   - CPU에서 명령어를 실행할 때 이미 주소가 정해져있으므로 따로 주소 계산을 하지 않고 바로 접근해야하는 주소가 되므로 컴파일 시점 바인딩에서는 **Logical Address  = Physical Address**
   - 주소가 결정나고 프로그램을 다른 시스템에서 실행했을 때 해당 시스템의 메모리를 어디서부터 하ㄹ당해주는지 나타내는 `stating point`가 다르거나 이미 그 주소에 다른 프로그램이 실행 중이라면  이 프로그램은 실행할 수 없음.

2. 로드 시점 주소 바인딩

   > 프로그램이 컴파일 되고 **메모리에 올라갈 때 CPU에서 주소가 계산되는 것**

   - 컴파일 된 프로그램 내의 각 변수나 명령어의 주소는 **특정 값**을 기준으로 정해짐.
   - 메모리에 올라가 CPU에서 주소를 계산할 때 프로그램이 올라간 메모리 주소의 Base Register 값에 이를 더하여 주소를 만들어냄. 
   - **CPU 내에서 계산되고 발생한 Logical 주소는 해당 메모리의 실제 주소 Physical Address와 같게됨.**
   - 한번 메모리에 올라가면 종료될 때 까지 그 위치에 존재하게됨.

3. 실행 시점 주소 바인딩

   > **메모리에 올라가 매번 명령어가 실행될 때 마다 주소가 계산되는 것**

   - 프로그램의 메모리 위치가 매번 변하기 때문에 이 방법을 사용.
   - 프로세스는 여러 이유로 인해 메모리 공간 안에서 위치가 바뀔 수 있음. -> 시작 위치는 계속 바뀌고 프로세스 내의 주소들도 바뀌게 되는데 위 두가지 방법으로는 매번 바뀌는 메모리 주소에 접근을 할 수 없게됨.
   - CPU에서 나온 임의의 주소 `MMU`를 걸쳐 실제 주소로 바뀌어 접근함.
   - **CPU에서 발생한 주소가 Logical Address이고 MMU를 통해 바뀐 주소가 메모리의 실제 주소인 Physical Address이므로 두 주소가 다름** (Logical Address는 가상 주소)

### MMU (Memory Management Unit)

> 가상 주소를 물리 주소로 매핑하는 하드웨어 장치

- 재배치  레지스터를 사용함.

![메모리](https://cdn-images-1.medium.com/max/1600/0*ADwXjKP1LMrEDxcB)

##### 프로그램이 메모리에 올라가는 과정

1. 로드과정 
   - 프로그램을 메모리에 올림
2. 링킹 과정
   - 라이브러리 파일과 연결해줌

메모리에 올라가서도 지속적으로 메모리 공간을 차지하는 것이 아닌 운영체제의 필요에 의해 `swap-in`되거나 `swap-out` 되기도 함

#### linking

> 프로그래머가 작성한 소스코드를 컴파일하여 생성된 목적파일과 이미 컴파일된 라이브러리 파일들을 묶어 하나의 실행 파일을 생성하는 과정

### Dynamic Linking

> 프로그램이 실행되기까지 링킹이 지연됨.

#### Static linking 

매번 프로세스 메모리에 사용하는 라이브러리를 확보하여 프로세스와 함께 올림. -> **메모리 낭비**

다이나믹 링킹은 `shared libraries`라고도 불림.

### Swapping

ready queue에 있는 우선순위가 높은 프로세스가 메모리에 있는 프로세스와 자리를 바꾼다고 생각하면 됨.

메모리에 있던 프로세스는 `backing store`라는 곳에 머물다가 다시 메모리에 할당되어 실행됨.

메모리에서 backing store로 가는 것을 `swap out`이라고 하고 다시 메모리에 할당되는 것을 `swap in`이라고 함.

![메모리](https://techdifferences.com/wp-content/uploads/2017/01/Swapping.jpg)

## 메모리 할당 방법

### 연속적인 할당

프로세스에게 메모리를 연속적으로 할당하는 방법.

**hole(구멍)**은 비워져있는 메모리 공간으로 프로세스에게 할당할 수 있는 메모리 공간(avaliable)임.

- `first-fit`
  - 최초로 할당받을 수 있는 크기의 공간에 무조건 할당됨. 
- `best-fit`
  - 크기가 가장 근접한 공간에 할당됨. 속도가 가장 빠름.
- `worst-fit`
  - 크기가 가장 많이 차이가 나는 공간에 할당됨.

![메모리 할당 방법](https://edux.pjwstk.edu.pl/mat/270/lec/lecture7/rys-7-4.png)

이 연속적인 할당 방법에는 단편화 문제가 생기는데 단편화에는 두 종류에 단편화가 있음.

- **외부 단편화 (external fragmentation)**
  - 메모리 공간에는 요청에 만족하는 크기의 할당할 수 있는 공간이 존재하나, **연속적이지 못해 할당할 수 없음.**
- **내부 단편화 (internal fragmentation)**
  - 메모리를 할당해주는 단위가**시스템의 block 단위**인데, 만약 요청하는 메모리 크기가 14.5kByte이고 시스템의 block 단위가 1kByte이면 15kByte를 할당해주게 되고 이 때 0.5 kByte의 메모리가 낭비됨.

이 단편화 문제를 해결할 방법에는 압축과 비연속적인 할당이 있음.

압축 방법의 경우 상당히 제한적임.

### 비지속적인 할당

### Paging

> 논리 메모리는 `page`라 불리는 고정 크기의 블록으로 나누어지고 물리 메모리는 `frame` 이라는 페이지와 **같은 크기**의 블록들로 나누어짐. 

page와 frame를 매핑할 `page table`이 존재하는데 이 page table에는 각 페이지 번호와 그에 해당하는 frame의 시작 물리 주소를 저장함.

![paging](https://t1.daumcdn.net/cfile/tistory/2446D83555FB8B6127)

### Segmentation

>  페이징과 비슷하나 논리 메모리와 물리 메모리를 같은 크기의 블록이 아닌 서로 다른 크기의 논리적 단위인 `segment`로 분할함.

블록의 크기를 모르기 때문에 `segment table`에는 해당 segment의 시작점인 `base`와 segment의 길이인 `limit`을 가지고 있음.

![segmentation](http://courses.teresco.org/cs432_f02/lectures/15-io/sgg-seg-ex.gif)



## refer

- [oprating system concepts 8장 ](http://codex.cs.yale.edu/avi/os-book/OS9/slide-dir/index.html)
- [http://baked-corn.tistory.com/16?category=718232](http://baked-corn.tistory.com/16?category=718232)
- [https://medium.com/@lyoungh2570/%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC-e770ff60db8f](https://medium.com/@lyoungh2570/%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC-e770ff60db8f)
- [https://m.blog.naver.com/s2kiess/220149980093](https://m.blog.naver.com/s2kiess/220149980093)