---
title: json ValueError Expecting property name.. error
description: python json valueError를 해결해보자
categories:
 - python
tags: [ValueError: Expecting property name: line 1 column 2 (char 1)]
---

python에서 dictionary 데이터를 json 형태로 dumps하거나 반대로 json 데이터를 dictionary 형태로 loads할 때 발생하는 

**ValueError: Expecting property name: line 1 column 2 (char 1)** 에러는 dumps, loads 하려는 데이터가 double quote(쌍 따옴표) 로 감싸지지 않고 single quote(따옴표)로 감싸져 있을 경우 발생한다.

```
"{"property": "1"}" # 허용 O
"{'property': '1'}" # 허용 X
```

만약에 single quote로 된 데이터를 변경하지 못한다면 모듈 ast를 사용해서 해결 할 수 있다.

```
import ast
import json

data = "{'property': '1'}"

# dumps 
json.dumps(ast.literal_eval(data))
# loads
json.loads(json.dumps(ast.literal_eval(data)))
```



## refer

[https://code.i-harness.com/en/q/1884426](https://code.i-harness.com/en/q/1884426)

