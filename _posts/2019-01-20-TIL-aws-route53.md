---
title: AWS Route 53
description: 190120 Today I learned AWS route 53
categories:
 - TIL
tags: [router53, aws router53]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

## Route 53

> 높은 가용성과 확장성이 뛰어난 클라우드 [Domain Name System (DNS)](https://aws.amazon.com/route53/what-is-dns/)웹 서비스 

DNS를 서비스화 시킨 것.

- 도메인 네임 서버를 임대해줌. (도메인 네임서버 장만)
- AWS 컴퓨팅 서비스와 연동이 가능함.
- 도메인 등록을 할 수 있음.

### 프로세스

- 도메인 구입
- 산 도메인에 서버 IP 연결
- NS 설정

Route 53은 도메인 구매대행, 등록 대행자라고 생각하면 됨.

## refer

[생활코딩 Route 53](https://www.youtube.com/watch?v=AnViePe2mj8)