---
title: serverless
description: 190108 Today I learned Serverless
categories:
 - TIL
tags: [serverless]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

서버리스란 서버가 없다. 라는 뜻이다. 진짜 서버가 없는 것은 아니고 **특정 작업을 수행하기 위해 컴퓨터나 가상머신에서 서버를 설정해 이를 처리하는 것이 아닌 것**을 뜻한다. 

서버리스를 알아보기 전에 먼저 기존에 있던 기술을 알아보자. 

### IaaS (Infrastructure as a Service)

- AWS, Azure

IaaS를 사용하면 서버 자원, 네트워크 전력 등의 인프라를 모두 구축할 필요가 없다. 단 클릭 몇 번 만으로도 나만의 서버가 생성된다.  이로인해 어플리케이션을 직접 사용자에게 서비스하는 것이 매우 쉬워지고 편리해졌다.

#### PaaS (Platform as a Service)

IaaS에서 한번 더 추상화 된 모델이다. 네트워크, 런타임까지 제공되며 어플리케이션만 배포하면 바로 구동이 가능하다.

### Serverless

서버의 존재에 대해 신경을 쓰지 않아도 된다. 어떤 사양으로 돌아가는지, 서버의 갯수를 늘려야 하는지, 네트워크는 어떤 걸 사용할지 설정하지 않아도 된다.

#### BaaS (Backend as a Service)

- Firebase

앱 개발에 있어 다양한 기능(데이터베이스, 소셜서비스 연동, 파일시스템)을 API로 제공해줌으로써 개발자들이 서버 개발을 하지 않고 필요한 기능을 쉽고 빠르게 구현할 수 있도록 해준다. 비용은 사용한 만큼만 나가게 된다. 또한 서버 확장도 알아서 해주게 된다.

단점은 백앤드 로직이 클라이언트 쪽에 구현이 되기 때문에 클라이언트 코드가 무거워 질 수 도 있다는 점. 복잡한 쿼리가 불가능해진다. 

#### FaaS (Function  as a Service)

- AWS lambda

프로젝트를 여러 개의 함수로, 혹은 한개의 함수로 만들어서 컴퓨팅 자원에 함수를 등록하고 함수를 호출할 수 있다.  함수는 특정 이벤트가 했을 때 실행된다. FaaS는 **주기적 처리, 웹 요청 처리, 콘솔 호출**이 가능하다. 만약 8시간마다 크롤링하는 함수가 있으면 FaaS를 사용하는 것이 적합하다. 

PaaS와의 차이점은 PaaS는 전체 애플리케이션을 배포하고 애플리케이션이 24시간 계속해서 돌아간다면 FaaS는 애플리케이션이 아닌 함수를 배포하고 특정 이벤트가 발생했을 때 실행되고 작업을 마치면 종료가 된다. 



## Refer

https://velopert.com/3543