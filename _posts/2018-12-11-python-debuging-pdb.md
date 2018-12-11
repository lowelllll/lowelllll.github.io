---
title: python pdb로 디버깅하기
description: 엄청 엄청 편한 파이썬 디버깅 도구 pdb를 소개합니다 🧙
categories:
 - python
tags: [파이썬 디버깅,python debuging, python pdb]
---

오늘 소개할 것은 파이썬 디버깅 도구 `pdb`이다. 

나는 IDE를 통한 디버깅은 뭔가 복잡해서 맘에 안들었는데 `pdb`는 간단하고 가벼워서 정말 좋당. 

`pdb`는 파이썬에서 제공하는 기본 모듈이므로 따로 설치가 필요없다.

사용법 또한 간단하다. 

```python
def sum(x, y):
	import pdb
	pdb.set_trace() # 디버깅을 시작하는 구간 (앤드포인트)
	
	result = x + y
	return result

if __name__ = '__main__':
    print(sum(3+7))
```

위 코드와 같이 디버깅을 하고 싶은 부분에 `pdb`를 임포트하고 `pdb.set_trace()`메서드를 적어준다.

그 다음 코드를 실행을 하면 `pdb.set_trace()`를 작성한 부분 다음부터 디버깅을 할 수 있다.

디버깅은 터미널을 통해서 할 수 있다.

```
-> result = x+y
(Pdb) x
3
(Pdb) type(y)
int
(Pdb) n
> /Users/yejin/dev/sum.py(6)sum()
-> return result
(Pdb) result
10
```

실행하면 앤드포인트에서 실행이 중지되고 `n`을 누르면 한줄 한줄 실행되게 된다.

또한 변수명을 입력하게 되면 변수명에 어떤 값이 들어있는지도 확인이 가능하고 변수의 값을 가공해서 어떤 값이 나오는지 확인도 가능하다. 

이 `pdb`의 단점은 함수 안으로 들어가지 못한다는 것이다. 그렇기 때문에 함수 안에 들어가서도 값을 확인해야한다면 `set_trace()`를 함수 안에다 옮겨 놓아야한다.