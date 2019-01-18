---
title: mysql-python 설치 시 에러 해결
description: mysql-python, mysqlclient 설치 시 발생하는 에러를 해결합니다
categories:
 - python
tags: [command 'clang' failed with exit status 1, mysql-python 설치 에러, mysqlclient 설치 에러]

---

mysql-python 설치 시 `command 'clang' failed with exit status 1` 이런 오류를 볼 수 있는데 아래와 같이 해결할 수 있다.

```
# brew install mysql 
brew unlink mysql
# brew install mysql-connector-c 
brew link mysql-connector-c

# 설치
pip install mysqlclient 
pip install mysql-python

brew unlink mysql-connector-c
brew link mysql
```

나는 이 방법으로` configparser` 에러 등 mysql 파이썬 드라이버를 설치할 때 나는 오류를 해결 할 수 있었다!



## refer

https://stackoverflow.com/questions/50940302/installing-mysql-python-causes-command-clang-failed-with-exit-status-1-on-mac