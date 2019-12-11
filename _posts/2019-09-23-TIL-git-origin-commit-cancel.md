---
title: git reset과 revert
description: 원격 저장소에 올린 커밋을 취소하는 방법을 알아봅니다.
categories:
 - TIL
tags: [git, git revert, git reset, git force push]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

원격 저장소에 올라간 커밋을 되돌리는 방법은 2가지가 있다.

## 로컬에서 커밋을 되돌린 후 강제로 force push 하는 방법

`git reset` 명령어를 사용해서 커밋을 되돌린다.

예시로 아래 보이는 `comit: C` 커밋을 취소해보자.

![git reset](https://user-images.githubusercontent.com/32219612/70634564-cb8d2e00-1c75-11ea-9a08-8b04a10eeb79.png)

```
git reset --hard HEAD~1
```

위 명령어는 커밋을 되돌리는데, 커밋한 코드도 다 날아가기때문에 조심해야한다.

![git reset2](https://user-images.githubusercontent.com/32219612/70634568-d1830f00-1c75-11ea-8142-bea0d6a13130.png)

로컬 저장소에서 커밋을 취소했기 때문에 원격에도 반영을 시켜주기 위해 push를 해줘야하는데 현재 로컬 저장소의 히스토리가 원격 저장소의 커밋 히스토리보다 뒤쳐져 있기 때문에 push할 경우 에러가 발생한다. 그렇기 때문에 이러한 상황에서는 `force push`를 해줘야한다.

```
git push -f origin <branch-name>
```

**주의 사항**

- 이 방법은 내 로컬 저장소의 히스토리를 원격 저장소 히스토리로 강제 덮어쓰기 하는 것이므로 원격 저장소에 **흔적도 없이** 내가 만들었던 커밋을 제거한다. 
- 만약 해당 브랜치가 혼자 사용하는 브랜치가 아닌 팀원들과 공유하는 브랜치이고 커밋을 되돌리기 전, 원격에 올라간 내 커밋을 다른 팀원이 pull 땡겼을 경우 원격 저장소에서 커밋을 되돌려도 다른 팀원의 로컬 저장소에는 커밋이 남아있게 된다. (이 상황에서 내가 원격 저장소에 force push 한 것을 팀원이 pull을 해도 `origin/master`의 커밋으로 돌아가지 않는다. `origin/master`보다 새로운 커밋을 가지고 있는 것으로 인식하기 때문)
- 커밋을 되돌려진 것을 모르고 팀원은 자신이 작업한 커밋과 되돌린 커밋을 push하게 되면 되돌렸던 커밋들이 다시 원격 저장소에 추가됨. 🤦‍♀

따라서 `reset + force push` 방법은 

- 나혼자만 사용하는 브랜치에 커밋을 push 했고, 이를 되돌리고 싶은 경우
- 팀원들과 커뮤니케이션을 통해 내가 되돌린 커밋을 pull로 땡겨간 팀원이 없다고 확인된 경우

에만 사용해야한다. (왠만하면 공용 브랜치에는 사용하지 않는다)



## Revert를 통해 커밋 되돌리는 방법

`git revert` 명령어는 revert 커밋을 히스토리에 쌓는 방식이다.

`reset`과의 차이점은 reset은 커밋을 되돌리면서 **해당 커밋 이력은 사라진다**, revert는 커밋을 되돌리는 커밋을 찍어 

**커밋과 커밋을 되돌린 이력이 모두 남는다.** 특정 커밋을 되돌리는 작업도 하나의 커밋으로 간주하는 것이다. 

reset을 사용하면 커밋을 되돌려서 push 했기 때문에 원격-로컬간의 히스토리가 맞지 않아 에러가 나고 팀원들의 커밋과 꼬일 가능성이 큰데 이건 되돌리는 커밋을 하나 더 생성하기 때문에 에러도 나지 않고 커밋이 꼬일 확률이 적다.

revert를 할 때에는 커밋을 한 순서 거꾸로 revert를 해야한다. (가장 나중에 한 커밋을 제일 먼저 revert 해야함)

```
# git revert <commit hash>
# 여러개의 커밋을 한꺼번에 revert할 경우 : git revert HEAD~3
git revert 1347972
```

위 명령어를 사용하면 `1347972` commit을 되돌리는 커밋이 찍힌다. 

![git revert](https://user-images.githubusercontent.com/32219612/70634588-d8118680-1c75-11ea-891f-a911de73c947.png)

`--no-commit` 옵션을 사용하면 자동으로 revert 커밋이 생성되는게 아닌 working tree와 index에만 변경사항이 적용된다. 이걸 사용해 여러개의 커밋을 하나의 커밋으로 revert 할 수 있다.

```
git revert --no-commit HEAD~3 # revert 커밋이 자동으로 생성되지 않음.
git commit -m "Revert Commit C, B, A
git push origin master
```

나는 실제로 팀원들과 협업하여 개발할 때에는 안전한 Revert를 이용한 되돌리기를 사용한다.



### 정리 

> 나름 정리한거다 

![git reset, revert](https://user-images.githubusercontent.com/32219612/70634551-c3cd8980-1c75-11ea-870d-90404be029c4.jpg)



### refer

https://behonestar.tistory.com/200

https://jupiny.com/2019/03/19/revert-commits-in-remote-repository/