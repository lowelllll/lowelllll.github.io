---
title: Http Cache
description: 181220 Today I learned Cache
categories:
 - TIL
tags: [http cache]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

## Cache

> 데이터의 전송 속도를 높이기 위해서 고안된 것

이미 다운로드 받은 파일을 왜 다시 받아야하지?

- 다운로드 받은 파일은 캐시(저장)했다가 같은 파일을 요청해 그 파일을 열게 되면 훨씬 빠르게 접근할 수 있음.

### cache 설정

**no-store** : 캐시 금지

**max-age** : 캐시 만기 시간

- `max-age=300` 이라면 5분마다 캐시가 갱신됨.

**no-cache** : 캐시가 유효한지 확인

- `response header`에 보면 `last modified`가 있는데 이것은 서버에 위치한 파일이 마지막으로 수정된 시간을 뜻함. 캐시가 만료될 시 클라이언트와 서버의 파일의 `last modified` 값을 비교해 시간이 같다면 파일이 수정되지 않았으므로 응답 헤더만 주고 받음.
- `no-cache`는 `max-age=0`과 같음.

**ETag** : `last modified`보다 정확한 파일 수정 비교 값

- 웹 서버는 `ETag`와 `last modified`를 비교해 둘 중 하나만 달라도 다시 파일을 응답함.



## refer

[생활코딩 http cache](https://www.youtube.com/watch?v=fczpUczepS4&list=PLuHgQVnccGMAM6VAWEKtaUnvzePCxnUVo&index=1)