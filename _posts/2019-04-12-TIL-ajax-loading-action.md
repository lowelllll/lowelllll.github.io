---
title: Ajax  요청 후 대기 시 액션 처리 하기
description: Ajax request 후 response data를 받기 전 까지 액션을 취할 수 있습니다.
categories:
 - TIL
tags: [ajax request loding, ajax loding, ajax loading action]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

### 상황

ajax 요청 후 데이터를 불러 올 때 까지 로딩 바를 띄우고 데이터를 불러왔을 때에는 로딩 바가 사라지게 하고 싶었다.

### 해결

```
$(document).ajaxStart(function() {
        // show loader on start 
        // ajax 요청 후 데이터를 불러 올 때 까지 액션을 취한다.
        $("#loader").css("display","block");
    }).ajaxSuccess(function() {
        // hide loader on success
        // ajax 요청 데이터를 성공적으로 받아왔을 때 액션을 취한다.
        $("#loader").css("display","none");
    });
```



### refer

- [https://stackoverflow.com/questions/20095002/how-to-show-progress-bar-while-loading-using-ajax](https://stackoverflow.com/questions/20095002/how-to-show-progress-bar-while-loading-using-ajax)