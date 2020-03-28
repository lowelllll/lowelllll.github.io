---
title: intellij에서 python interpreter 셋팅하기
description: intelij ultimate에서 python interpreter 셋팅하는 방법
categories:
 - python
tags: [intellij python project, how to setting lntellij python interpreter]
---

인텔리제이 울티메이트(유료버전)에서 파이썬 프로젝트를 구동하기 위한 인터프리터 셋팅 방법

![](https://user-images.githubusercontent.com/32219612/77221355-702fdd80-6b8c-11ea-8159-d7a7dde445e7.png)

preferences -> plugins에서 python을 검색 후 install 후 intellij를 재시작한다.

![](https://user-images.githubusercontent.com/32219612/77221357-72923780-6b8c-11ea-9583-7684b38da7e2.png)

file -> product structure 

![](https://user-images.githubusercontent.com/32219612/77221358-73c36480-6b8c-11ea-9621-cd120fc27c58.png)

platform settings -> SDKs 에서 + 클릭 후  Python SDK 클릭

![](https://user-images.githubusercontent.com/32219612/77825571-75bd9280-714d-11ea-9cbd-7dfbcf9945e0.png)

그럼 이렇게 interpreter를 추가할 수 있다. virtualenv가 이미 있다면 `Existing environment` 체크 후 만들어 놓은 virtualenv를 선택하면 되고, 

만약 새로운 virtualenv를 생성해야한다면 `New environment` 체크 후 `Location`에서 virtualenv 파일이 생길 경로를 지정해주고 `Base interpreter`에서 python 버전을 선택해준다.

virtualenv 설정이 끝나면 `OK`

![](https://user-images.githubusercontent.com/32219612/77221361-74f49180-6b8c-11ea-92cd-c6f64d8ba6b1.png)

그럼 SDKs에 interpreter가 추가되어있을 것이다. 

![](https://user-images.githubusercontent.com/32219612/77221364-758d2800-6b8c-11ea-8b49-c22e5648562e.png)

SDKs에 위에 `packages` 탭을 누르고 +를 눌러 패키지를 설치할 수 있다.

![](https://user-images.githubusercontent.com/32219612/77221366-7625be80-6b8c-11ea-8232-f9eefbdc863e.png)

검색창에 설치할 패키지를 검색 후 `Install package`를 눌러 패키지를 설치한다.

만약 프로젝트에 requirements.txt가 있으면 terminal에서 `pip install -r requirements.txt` 로 패키지를 설치한다. (새로 virtualenv를 만든 경우엔 requirement로 설치하는 방법을 못찾았다ㅠ 아시는 분 댓글 부탁드립니다)

그 후에 다시 패키지 목록이 보이는 곳에서 `apply` 후 `OK` 를 눌러주면 virtualenv 설정과 interpreter 추가 끝.

이제 프로젝트에 interpreter를 등록해주면 된다.

![](https://user-images.githubusercontent.com/32219612/77825468-e2845d00-714c-11ea-91dc-c1ab8ba7c64c.png)

project -> project sdk에서 추가한 interpreter를 등록해준다.

추가로 필요에 따라 실행 시킬 파일에 인터프리터를 셋팅하고 싶으면 Run -> Edit configuration에서 인터프리터를 등록해주면 된다. 



그럼 끗 😜