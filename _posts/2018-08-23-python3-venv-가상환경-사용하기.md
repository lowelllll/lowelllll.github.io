---
title: python3 venv 가상환경 사용하기
description: venv는 python3에서 기본으로 제공하는 가상환경(virtualenv)을 만들 수 있는 라이브러리이다.
categories:
 - python
tags: [python virtualenv, python venv, python pyenv, window pyenv, python 가상환경]
---

venv는 python3에서 기본으로 제공하는 가상환경(virtualenv)을 만들 수 있는 라이브러리이다.

나는 이 라이브러리를 여태동안 모르고 있었다!

원래 mac에서는 `pyenv`를 사용하여 가상환경을 세팅했는데 윈도우는`pyenv`를 사용할 수 없다.

그래서 그동안 서드파티 라이브러리인 `virtualenv`를 사용하고 있었는데 venv로 갈아탈 예정이다.

기본으로 제공하는데 사용해줘야지! 



만약 `virtualenv` 라이브러리를 사용하고 싶다면 [해당 포스트](http://jamanbbo.tistory.com/10)에서 사용법을 확인하면 된다.

## venv 사용하기

먼저 venv는 python3에 빌트인 되어있기 때문에 설치를 하지 않아도 사용가능하고 python2.7에서는 사용이 불가능하다.

python2를 사용한다면 virtualenv/pyenv 라이브러리를 설치해야한다.



1. 가상환경 생성

   ```shell
   # window 
   # python -m venv [venv 이름]
   > python -m venv ./myenv
   
   # mac
   # python3 -m venv [venv이름]
   $ python3 -m venv ./myenv
   ```

2. 가상환경 활성화

   ```shell
   # window
   > myenv\Scripts\activate
   # mac
   $ source myenv\bin\activate 
   # or 
   . myenv\bin\activate
   ```

   가상환경에 성공적으로 들어간다면 prompt 앞에 현재 가상환경이 표시될 것이다.

   지금부터는 격리된 환경에서 작업이 가능하다!

   `(myenv) $ >`

3. 가상환경 비활성화

   ```shell
   > deactivate
   ```
