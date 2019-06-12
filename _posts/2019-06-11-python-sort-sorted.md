---
title: Python sort와 sorted
description: sort와 sorted의 차이점을 알고 키 값으로 정렬을 하는 방법을 알 수 있습니다.
categories:
 - python
tags: [python sort sorted, 파이썬 sort sorted 차이, python sort key, python sort key two, python 다중 조건 정렬]
---

### list.sort()

```python
>>> score = [92, 34, 56, 100, 80, 97, 20, 65, 70, 55]
>>> score.sort() # 오름차순
>>> score
[20, 34, 55, 56, 65, 70, 80, 92, 97, 100]
```

`list.sort()`는 리스트를 정렬하는데 기존 객체를 정렬한다. 따라서 반환 값이 없다.(None) `score`를 정렬 후 프린트 해보면 요소들이 오름차순으로 된 것을 볼 수 있다.

### sorted(list)

```python
>>> score = [92, 34, 56, 100, 80, 97, 20, 65, 70, 55]
>>> sorted(score)
[20, 34, 55, 56, 65, 70, 80, 92, 97, 100]
>>> score
[92, 34, 56, 100, 80, 97, 20, 65, 70, 55]
```

`sorted(list)`는 리스트를 정렬하는 것은 똑같지만 기존 객체를 정렬하는 것이 아닌 기존 객체의 요소를 정렬한 새로운 객체를 반환한다. 따라서 기존 객체인 score는 요소들의 위치가 변하지 않은 것을 확인할 수 있다.

`list.sort()`, `sorted(list)`는 성능의 차이가 없으므로 둘 중 필요한 것을 사용하면 된다. 내림차순을 사용하려면 `reverse=True` 를 인자로 넘겨주면 된다.

```python
>>> score = [92, 34, 56, 100, 80, 97, 20, 65, 70, 55]
>>> sorted(score, reverse=True) # 내림차순
[100, 97, 92, 80, 70, 65, 56, 55, 34, 20]

>>> score.sort(reverse=True) # 내림차순
>>> score
[100, 97, 92, 80, 70, 65, 56, 55, 34, 20]
```

### 어떤 값을 기준으로 정렬하기

여러개의 딕셔너리가 들어있는 리스트를 딕셔너리의 어떤 값을 이용해 정렬하고 싶다면 `key` 를 사용한다.

```python
>>> users
[{'age': 0}, {'age': 12}, {'age': 42}, {'age': 6}, {'age': 48}, {'age': 18}, {'age': 30}, {'age': 54}, {'age': 36}, {'age': 24}]
>>> users.sort(key=lambda x: x['age']) # age 값을 기준으로 오름차순 정렬
>>> users
[{'age': 0}, {'age': 6}, {'age': 12}, {'age': 18}, {'age': 24}, {'age': 30}, {'age': 36}, {'age': 42}, {'age': 48}, {'age': 54}]

>>> users.sort(key=lambda x: x['age'], reverse=True) # age 값을 기준으로 내림차순 정렬
>>> users
[{'age': 54}, {'age': 48}, {'age': 42}, {'age': 36}, {'age': 30}, {'age': 24}, {'age': 18}, {'age': 12}, {'age': 6}, {'age': 0}]

```

만약 키 값이 있다 없다 한다면 `dict.get()`을 사용하자.

```python
>>> users
[{'age': 30}, {}, {'age': 6}, {'age': 24}, {'age': 48}, {}, {'age': 42}, {'age': 54}, {'age': 36}, {'age': 12}, {}, {}, {'age': 18}, {}]
>>> users.sort(key=lambda x: x.get('age', 0), reverse=True) # 키 값이 없으면 0
>>> users
[{'age': 54}, {'age': 48}, {'age': 42}, {'age': 36}, {'age': 30}, {'age': 24}, {'age': 18}, {'age': 12}, {'age': 6}, {}, {}, {}, {}, {}]
```

#### 다중 조건으로 정렬하기

만약 정렬을 하고 싶은 기준이 2개 이상이라면 아래 방법을 사용한다. (나이 오름차순 정렬 후,  점수는 내림차순으로 정렬)

```python
>>> students
[{'age': 0, 'score': 0}, {'age': 48, 'score': 1}, {'age': 40, 'score': 6}, {'age': 24, 'score': 8}, {'age': 16, 'score': 6}, {'age': 48, 'score': 2}, {'age': 64, 'score': 0}, {'age': 0, 'score': 3}, {'age': 56, 'score': 2}, {'age': 72, 'score': 6}, {'age': 72, 'score': 0}, {'age': 24, 'score': 5}, {'age': 8, 'score': 2}, {'age': 32, 'score': 1}, {'age': 16, 'score': 4}, {'age': 56, 'score': 9}, {'age': 32, 'score': 9}, {'age': 8, 'score': 1}, {'age': 40, 'score': 3}, {'age': 64, 'score': 2}]

>>> sort_students = sorted(sorted(students, key=lambda x: x['score'], reverse=True), key=lambda x: x['age'])
>>> sort_students
[{'age': 0, 'score': 3}, {'age': 0, 'score': 0}, {'age': 8, 'score': 2}, {'age': 8, 'score': 1}, {'age': 16, 'score': 6}, {'age': 16, 'score': 4}, {'age': 24, 'score': 8}, {'age': 24, 'score': 5}, {'age': 32, 'score': 9}, {'age': 32, 'score': 1}, {'age': 40, 'score': 6}, {'age': 40, 'score': 3}, {'age': 48, 'score': 2}, {'age': 48, 'score': 1}, {'age': 56, 'score': 9}, {'age': 56, 'score': 2}, {'age': 64, 'score': 2}, {'age': 64, 'score': 0}, {'age': 72, 'score': 6}, {'age': 72, 'score': 0}]

```



### refer

<https://stackoverflow.com/questions/4233476/sort-a-list-by-multiple-attributes>