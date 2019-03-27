---
title: local storage, session storage
description: 로컬 스토리지, 세션 스토리지를 사용해서 브라우저에 데이터를 저장합니다.
categories:
 - TIL
tags: [local storage, session storage]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

### 상황

게시판을 구현하는 중 새로 올라온 게시글을 읽지 않았으면 빨간 닷 표시를 해주고 읽었다면 닷 표시를 없애는 읽음 처리 작업을 해야했다. 카카오톡에서 유저가 프로필을 변경하면 프로필 사진 옆에 빨간 닷 표시가 생기는 것 처럼? 만들고 싶었는데 이를 서버에서 처리하기에는 계속 읽었는지 안읽었는지 확인해야해서 클라이언트 상에서 처리할 수 있는 방안을 찾던 중 회사 프론트 개발자분께서 웹 스토리지를 알려주셨다. 

## 스토리지

스토리지는 key-value 쌍으로 클라이언트 데이터를 저장할 수 있는 공간이다. 이 스토리지는 두 종류로 되어있는데 로컬 스토리지와 세션 스토리지로 분류되어있다. 각 스토리지는 차이점이 있어서 용도에 따라 알맞게 사용하면 된다.

##### 장점 

- 용량이 크다.
- HTTP요청 때 전달되지 않아 자원낭비 적음.

스토리지는 환경을 많이 타기 때문에 제대로 작동하지 않을 가능성이 있으므로 엄청 중요하진 않지만 있으면 좋은 기능을 구현할 때 사용하면 좋다.

아래와 같은 경우 등에서 사용한다고 한다.

- 팝업
- 글 작성 시 임시저장용 

### 로컬 스토리지

로컬 스토리지에 데이터를 저장하면 브라우저를 껐다 키거나 컴퓨터를 껐다 켜도 직접 지우지 않는 이상 절대 지워지지 않는다. 데이터가 **영구성**을 띈다. 내 경우도 한번 읽은 글에 표시를 할 필요가 없으므로 로컬 스토리지에 데이터를 저장해서 이 데이터로 읽음    처리를 했다. 로컬 스토리지에 쌓인 데이터는 개발자도구 - application - localstorage 에서 확인할 수 있다.

### 세션 스토리지

세션 스토리지에 데이터를 저장하면 로컬 스토리지와 반대되게 브라우저, 컴퓨터를 껐다 키면 데이터가 날라간다. 세션 스토리지에는 **일회성**을 띄는 데이터를 저장하면 된다. 세션 스토리지에 쌓인 데이터는 개발자 도구 - applicaation - sessionstorage에서 확인할 수 있다.

### Javascript 에서 사용하기

#### 로컬 스토리지

```javascript
// set data
localStorage.setItem('lowell', 'value');
// get data 
localStorage.getItem('lowell'); //vallue

// JSON 데이터도 사용가능하다.
var data = {'lowell': 20, 'yejin': 23};
localStorage.setItem('people', JSON.stringify(data));

people = JSON.parse(localStorage.getItem('people'));

// remove data
localStorage.removeItem();
```

#### 세션 스토리지

로컬 스토리지 사용법에서 `localStroage`를 `sessionStorage`로 변경하면 된다. 메소드는 똑같다!

```javascript
// set data
sessionStorage.setItem('lowell', 'value') 
// get data
sessionStorage.getItem('lowell')
```

### Refer

- https://isme2n.github.io/devlog/2017/06/21/storage-cookie/

- https://www.zerocho.com/category/HTML&DOM/post/5918515b1ed39f00182d3048