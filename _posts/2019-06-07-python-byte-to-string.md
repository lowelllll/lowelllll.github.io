---
title: python byte to string
description: byte 문자열을 string으로 바꾸는 방법
categories:
 - python
tags: [python byte to string, 파이썬 바이트 문자열]
---

### 파이썬 Bytes 타입을 str 타입으로 바꾸는 방법

```python
>>> text = b'hello'
>>> type(text)
<class 'bytes'>
>>> text.decode('utf-8')
'hello'
>>> text = text.decode('utf-8')
>>> type(text)
>>> <class 'str'>
```

