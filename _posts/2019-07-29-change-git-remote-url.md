---
title: git remote url 변경하기
description: 
categories:
 - TIL
tags: [git remote url 변경, change git remote url, git username 변경]

---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

### 상황

사정이 생겨 github username을 변경하고 푸시를 하려고 했는데 `authentication` 오류가 나면서 푸시가 되지 않았다.

```bash
fatal: Authentication failed for 'https://old-username@github.com/account/repo.git/
```

### 해결

usename을 바꾸면서 이전 username의 remote url을 사용할 수 없어 새로운 username이 들어간 remote url로 변경해줬다.

```bash
$ git config --list # remote url 확인 
remote.origin.url orgin https://old-username@github.com/account/repo.git/

$ git remote set-url origin https://new-username@github.com/account/repo.git/
```

