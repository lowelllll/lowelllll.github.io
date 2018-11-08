---
title: git commit 합치기 (rebase -i)
description: 깃 능력자 되기 - rebase를 사용해서 지저분한 커밋 내역을 정리하자!
categories:
 - git
tags: [git rebase, git commit 합치기]
---

### 상황

회사에서 기능을 구현하고 풀리퀘까지 보낸 상황에서 로직에 수정해야하는 부분을 발견했다.

로직을 수정하고 커밋하려고 하니 지저분한 커밋이 늘어나는 것 같아 굉장히 찝찝해졌다. '이건 커밋 메세지를 또 뭐라하지..'

어떻게 하면 좋을 까 생각해보니 예전에 사수님께서 커밋을 합칠 수 있다고 말씀해주신 것이 생각나 바로 검색을 해보았다.



### 커밋 합치기

깃을 사용하는 사람이라면 **같은 기능 구현에 대한 여러 번의 커밋**을 한적이 있을 것이다.

```
웹 디자인 css를 완성하고 '웹 디자인 구현'이라고 커밋을 했는데 웹 페이지를 다시 보니 폰트 사이즈가 맘에 안들어서 수정한 후 '폰트 사이즈 수정' 이라고 커밋을 했다.
커밋 후 다시 보니 이번에는 폰트 색깔이 맘에 안들어 바꾸고 '폰트 색깔 수정' 이라고 커밋을 했다.
```

디자인을 구현한건 똑같은데 여러 번의 커밋이 이루어졌다.

이렇게 되면 협업을 할 때 다른사람이 커밋의 내용에 대해 의문이 들 수도 있고 이런 커밋들이 많아지면 커밋 이력은 매우 지저분해진다.

이럴 때 커밋을 합치면 이런 문제를 해결할 수 있다.

커밋을 합치는 것은 `rebase -i` 명령어로 합칠 수 있다.

먼저 `git log`를 통해 내가 어디서부터 어디까지 커밋을 합칠 것인지 살펴보자.

```shell
commit e1f09108a51c09919c3488f06e3160c8f57
Author: 
Date:   Thu Nov 8 15:06:01 2018 +0900

    Message 4

commit 405715479d9e97bbe714820dda88e04dbc
Author: 
Date:   Thu Oct 25 12:14:21 2018 +0900

    Message 3
```

 `Message 3` 커밋에  `Message 4` 커밋을 합쳐보도록 하겠다.

```
$ git rebase -i HEAD~2 
```

위 명령어는 최근 커밋 2개를 합치라는 명령어이다.  

head는 현재 브랜치에서 마지막 커밋을 가르킨다.

즉 마지막 커밋과 그 전 커밋을 합치는 것이다.

만약 2개 커밋이 아닌 3개 커밋을 합치고 싶다면 `HEAD~3` 이라고 숫자만 바꿔주면 된다.  



위 명령어를 실행하면 vi 편집기가 뜰 것이다. 

```
pick 4057154 Message 3
pick e1f0910 Message 4
```

vi 상단에는 내가 합치고 싶은 커밋들이 나와있을 건데 여기서 합치고 싶은 커밋의 `pick`을 `squash`로 바꿔준다.

 ```
pick 4057154 Message 3
squash e1f0910 Message 4
 ```

message 4 커밋을 message 3 커밋으로 합칠 거기 때문에 Message 4의 `pick`을 `squash`로 바꿔준다.

`:wq` 저장을 하면 

```
# This is a combination of 2 commits.
# This is the 1st commit message:

Message 3

# This is the commit message #2:

# Message 4

# Please enter the commit message for your changes. Lines starting
```

이렇게 다시 vi 편집기가 뜨는데 **여기서는 어떤 커밋 메세지를 사용할 것이냐**를 묻는 것이므로 

Message 4 커밋 메세지를 지우면 message 3의 커밋 메세지만 남게 된다.

지우지 않고 위와 같이 주석 처리를 할 수 있다.  



위 설정까지 다 했다면 커밋이 합쳐졌을 것이다.

```
commit 405715479d9e97bbe714820dda88e04dbc493282
Author: 
Date:   Thu Oct 25 12:14:21 2018 +0900

    Message 3
```

커밋 합친 것을 원격 저장소에도 반영하고 싶으면 `push`를 하면 된다.

```
$ git push -f origin [branch]
```



커밋 합치기 참 쉽죵? 🤗