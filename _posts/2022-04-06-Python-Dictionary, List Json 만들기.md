---
title: Python - Dictionary, List Json 파일 만들기
author: BeomBeomJoJo
date: 2022-04-06 12:33:00 +0800
date: 2022-04-06 12:33:00 +0800
categories: [Python]
tags: [Python]
math: true
mermaid: true
---

{% include adsense.html %}

## **소개**
* 오늘은 파이썬에서 Dictionary 타입으로 이루어진 데이터를 Json 파일로 만드는 방법에 대해서 알려 드리려고 합니다.
* 해당 문법을 익히게 되면, Json 파일을 다룰 때 매우 유용 할거라고 생각합니다.

<br/>

## **파이썬 코드로 만들 Json Data 형태**
* 다음은 파이썬 코드로 직접 만들 Json Data 형태 모습입니다.
* 크게 Image 라는 키 안에는 다시 Image_name, Image_number 의 키와 Value 들이 모여 있는 형태 입니다.
* 아래 데이터와 동일한 모습으로 파이썬 코드로 Json 파일을 직접 만들어 보도록 하겠습니다.

```json
{
    "Image": [
        {
          "image_name": 1,
          "image_number": 1
        },
        {
          "image_name": 2,
          "image_number": 2
        },
        {
          "image_name": 3,
          "image_number": 3
        }
      ]
}
```

<br/>

## **파이썬 코드**
* 아래 코드에서 핵심은 처음에는 dictionary 타입의 nested_dictionary 변수를 선업합니다.
* 다음으로 rest_list 라는 list 타입의 변수를 추가하여, 실제 데이터들은 해당 리스트에 append 하여 데이터를 추가합니다.
* 다음으로 nested_dictionary Value 값에 rest_list 데이터를 추가하면 원하는 Json 파일 포맷대로 출력됩니다.

```python
import json

if __name__ == "__main__":
    nested_dictionary = dict()
    rest_list = list()
    
    for idx in range(5):
        rest_list.append({"image_name" : idx, "image_number" : idx})
        
    nested_dictionary['Image'] = rest_list

    with open('data.json', 'w') as fp:
        json.dump(nested_dictionary, fp, 
                  sort_keys=True, 
                  indent=4)

```

<br/>

## **실행 결과**
* 위에서 작성한 파이썬 코드를 실행한 결과, 앞에서 미리 만들어 놓았던 Json 파일 모습 그대로 실제 JSON 데이터가 생성되어진 것을 확인할 수 있습니다.

```json
{
    "Image": [
        {
            "image_name": 0,
            "image_number": 0
        },
        {
            "image_name": 1,
            "image_number": 1
        },
        {
            "image_name": 2,
            "image_number": 2
        },
        {
            "image_name": 3,
            "image_number": 3
        },
        {
            "image_name": 4,
            "image_number": 4
        }
    ]
}
```