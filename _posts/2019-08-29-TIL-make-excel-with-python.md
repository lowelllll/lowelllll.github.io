---
title: 파이썬 사용해서 엑셀 파일 만들기 
description: 파이썬으로 엑셀 파일을 만드는 스크립트를 짜봅니다. 
categories:
 - TIL
tags: [python excel, python make excel, python excel insert image]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

엑셀 파일을 만들 수 있는 파이썬 라이브러리는 대표적으로  `openpyxl` 과 `xlsxwriter` 가 있는데 저는 `wlsxwriter` 를 사용했다. (보통 `openpyxl`을 더 쓰는 것 같은데 나는` wlsxwriter` 예제를 처음 접해서 사용했다. )

#### 엑셀 파일 만들기

아래 코드는 간단한 엑셀 파일을 만드는 코드이다.

```python
import xlsxwriter

file_name = 'test.xlsx' # 엑셀 파일 이름
workbook = xlsxwriter.Workbook(file_name)
ws = workbook.add_worksheet()

ws.write(0, 0, 'Hello') # A1에 값을 넣음
ws.close() # 저-장 
```

같은 디렉토리에 test.xlsx 파일이 생성된 것을 확인할 수 있다.

#### 이미지 삽입하기

image url로 requests를 통해 이미지를  가져온 뒤 이미지도 삽입되어있는 파일을 만들어 보았다. 

```python
from io import BytesIO
  
import requests
import xlsxwriter


image_url =
'https://d3mcojo3jv0dbr.cloudfront.net/2019/03/23/14/25/d88f24f059169559602c836fc35b995c.jpeg?w=128&h=128&q=65'

workbook = xlsxwriter.Workbook('text.xlsx')
ws = workbook.add_worksheet()

ws.set_column(0, 0, 100) # column width 설정 
ws.set_column(1, 1, 100) # column width 설정 
ws.set_default_row(100) # row height 설정

ws.write(0, 0, '어피치') # A1 

res = requests.get(image_url) # 이미지 가져옴
image_data = BytesIO(res.content) # 이미지 파일 처리

image_size = len(image_data.getvalue()) # 이미지 사이즈 
if image_size > 0: # 이미지가 있으면 
    ws.insert_image('B1', image_url, {'image_data': image_data}) # 엑셀에 삽입함


workbook.close() # 저장 
```

이미지 사이즈를 체크하여 이미지가 있는지 확인한 후 엑셀에 삽입하는 이유는 이미지가 없는데 inser_image를 시도하면 오류가 나기 때문이다. 

```bash
struct.error: unpack requires a bytes object of length 3 image
```

(이 오류때문에 반나절 삽질했다 ㅂㄷㅂㄷ)  [해당 오류 이슈](https://github.com/jmcnamara/XlsxWriter/issues/382)

이렇게 파이썬으로 엑셀 파일 만드는 것을 자동화하여 생산성을 높일 수 있다.

### refer

[xlsxwriter 사용법](https://wikidocs.net/14305)

[xlsxwriter document](https://xlsxwriter.readthedocs.io/worksheet.html)

[엑셀 이미지 삽입하기 ](https://xlsxwriter.readthedocs.io/example_images_bytesio.html)