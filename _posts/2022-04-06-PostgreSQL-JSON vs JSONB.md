---
title: PostgreSQL - JSON vs JSONB 공통점 및 차이점
author: BeomBeomJoJo
date: 2022-04-06 12:33:00 +0800
date: 2022-04-06 12:33:00 +0800
categories: [PostgreSQL]
tags: [PostgreSQL]
math: true
mermaid: true
---

## **참조**
* [참고 블로그](https://americanopeople.tistory.com/300)
* [참고 블로그](https://www.postgresql.org/docs/9.6/datatype-json.html)
* [참고 블로그](https://www.compose.com/articles/faster-operations-with-the-jsonb-data-type-in-postgresql/)

<br/>

## **JSON, JSONB 공통점**
* JSON과 JSONB의 공통점은 둘 다 JSON 포맷 **유효성 체크** 를 진행합니다.

<br/>

## **JSON, JSONB 차이점**
<br>

### **데이터 저장 방식**
* JSON은 들어온 그대로 값을 저장합니다.
* 하지만, JSONB는 들어온 그대로 저장하지 않습니다.
* 문자열 사이의 공백도 제거해 주고, KEY 순서도 보장하지 않는다는 특징이 있습니다.

```sql
SELECT '{"bar": "baz",     "balance": 7.77, "active":false}'::json;
                      json                       
-------------------------------------------------
 {"bar": "baz",     "balance": 7.77, "active":false}
(1 row)

SELECT '{"bar": "baz",     "balance": 7.77, "active":false}'::jsonb;
                      jsonb                       
--------------------------------------------------
 {"bar": "baz", "active": false, "balance": 7.77}
(1 row)
```

<br/>

### **디스크 사용량**
* 디스크 사용량에 있어서 JSONB 가 JSON 보다는 평균적으로 좀 더 많은 디스크를 사용합니다.
* 하지만, 항상 그런것은 아닙니다.

<br/>

### **인덱싱**
* JSON은 인덱싱이 불가능 하다는 단점이 있습니다.
* 하지만, JSONB는 인덱싱이 가능합니다.
* 다음과 같은 문서가 있습니다.

```json
{
    "guid": "9c36adc1-7fb5-4d5b-83b4-90356a46061a",
    "name": "Angela Barton",
    "is_active": true,
    "company": "Magnafone",
    "address": "178 Howard Place, Gulf, Washington, 702",
    "registered": "2009-11-07T08:53:22 +08:00",
    "latitude": 19.793713,
    "longitude": 86.513373,
    "tags": [
        "enim",
        "aliquip",
        "qui"
    ]
}
```
* 위의 문서를 토대로 다음과 같이 쿼리를 작성할 때 인덱싱을 사용할 수 있습니다.

```sql
SELECT jdoc->'guid', jdoc->'name' FROM api WHERE jdoc @> '{"company": "Magnafone"}';
```

<br/>

### **연산자**
* JSONB에서만 사용 가능한 문법이 있습니다.

```sql
-- String exists as array element:
SELECT '["foo", "bar", "baz"]'::jsonb ? 'bar';

-- String exists as object key:
SELECT '{"foo": "bar"}'::jsonb ? 'foo';

-- Object values are not considered:
SELECT '{"foo": "bar"}'::jsonb ? 'bar';  -- yields false

-- As with containment, existence must match at the top level:
SELECT '{"foo": {"bar": "baz"}}'::jsonb ? 'bar'; -- yields false

-- A string is considered to exist if it matches a primitive JSON string:
SELECT '"foo"'::jsonb ? 'foo';
```

<br/>

## **정리**
* JSON 타입은 입력받은 텍스트 값을 Database에 있는 그대로 저장합니다.
* 때문에 쓰는 비용이 상대적으로 크지 않습니다.
* 하지만, 쓰기에 비해서 읽는 부분에 있어서 비용이 듭니다.
* 반면에 JSONB 타입은 입력받은 데이터를 바이너리 포맷으로 저장합니다.
* 그래서 JSON에 비해 쓰기 비용이 크다는 단점이 있습니다.
* 하지만, JSON과는 가장 큰 차이점 중 하나인 **인덱싱** 이 가능하고, 데이터 파싱 비용이 들지 않기 때문에 읽기 비용이 JSON에 비해 비용히 적다는 장점이 있습니다.
* 때문에 JSON보다 JSONB를 사용하는 것을 권장합니다.