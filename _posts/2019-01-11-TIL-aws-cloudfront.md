---
title: AWS CloudFront
description: 190111 Today I learned AWS CloudFront service
categories:
 - TIL
tags: [AWS cloudfront, cloudfront]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

## CloudFront

> 빠르고 고도로 안전하며 프로그래밍 가능한 콘텐츠 전송 네트워크(CDN)

CloudFront는 캐시를 제공하는 캐시서버이면서 CDN임. 

- 캐시 서버 역할 제공
- 고속으로 컨텐츠 전송 (CDN)

**CDN (Content Delivery Network)** 전 세계 어디있더라도 컨텐츠를 빠르게 제공할 수 있음.

 ColudFront의 캐시 서버인 `edge location`이 전 세계 다양한 곳에 서비스 제공되어 있어 가능함.

![AWS CloudFront](https://d1.awsstatic.com/global-infrastructure/maps/CloudFront%20Network%20Map%2010.12.18.59e838df2f373247d2efaeb548076e084fd8993e.png)

사용법은 사용자가 클라우드 프론트(distribution)을 생성하고  자신의 서버(origin)를 등록한다. 이후 클라우드 프론트의 주소를 실제 서버 도메인 대신 사용하면 빠르게 접속할 수 있을 것이다.

### 캐시 설정

- **캐시 유효시간 설정** Object caching -> customize -> TTL 시간을 정해준다.
  - Minimum TTL : 캐시 최소 유효시간
  - Maximun TTL : 캐시 최대 유효시간
  - CloudFront의 기본 캐시 유효시간은 24시간

- **querystring 설정** 
  - Query string forwarding and caching을 forward all로 설정해야 querystring이 먹음.
- **price class 설정**
  - price class를 통해 접속 가능한 지역을 선택할 수  있음
  - 미국, 캐나다 유럽 -> 저렴, 호주 아프리카 -> 비싸짐

### 기타

- Router 53 서비스를 통해 클라우드 프론트의 주소를 아름답게 할 수 있음
- 다양한 보안 (AWS ACM)