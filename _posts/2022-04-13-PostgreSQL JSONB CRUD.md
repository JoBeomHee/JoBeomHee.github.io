---
title: PostgreSQL 에서 JSONB 사용한 CRUD
author: BeomBeomJoJo
date: 2022-04-13 12:33:00 +0800
date: 2022-04-13 12:33:00 +0800
categories: [PostgreSQL]
tags: [PostgreSQL]
math: true
mermaid: true
---

## **참조**
* [참고 사이트](https://www.idalko.com/crud-operations-postgres-jsonb/)

<br/>

## **소개**
* 요즘 데이터를 전송하는 가장 일반적인 방법은 JSON(JavaScript Object Notation) 을 사용하는 것입니다.
* Postgres 9.5 이상 부터는 JSON을 저장 및 가져올 수 있을 뿐만 아니라 JSON 구조를 기반으로 작업을 수행할 수 있도록 새로운 **JSONB** 유형의 Column이 새롭게 도입되었습니다.

<br/>

## **Use case**
* 다음 User를 등록하는 간단한 애플리케이션이 있습니다.
* 응용프로그램을 개선하고 사용자 인터페이스(테마, 아이콘, 텍스트 크기)를 구성하는 기능을 추가한다고 가정합니다.
* 각 사용자는 여러 개의 구성을 가질 수 있으며 이러한 구성을 전환할 수도 있습니다.
* 위 내용을 토대로 작업할 JSON은 다음과 같습니다.

```json
{
    "userid":"artur@gmail.com",
    "configurations":[
        {
            "name":"myconf",
            "theme":"light",
            "icons":"small",
            "textsize":"large"
        },
        {
            "name":"myconf2",
            "theme":"dark"
        }
    ]
}
```

<br/>

## **Creating a table**
* JSON 구성을 저장할 테이블을 생성합니다.
* 저는 pgAdmin GUI 도구를 가지고 테이블을 생성하였습니다.
* 만약 SQL 구문을 통해 테이블을 생성하려면 다음과 같이 SQL 쿼리를 작성하면 됩니다.

```sql
CREATE TABLE USER_CONFIGURATIONS (
  ID         BIGSERIAL PRIMARY KEY,
  DATA       JSONB
);
```

![1](https://user-images.githubusercontent.com/22911504/163142426-533996ac-7088-448a-8a26-094ed2fcc5d0.png)

![2](https://user-images.githubusercontent.com/22911504/163142429-b4205225-3823-45e0-b0c3-e1b33a9573b8.png)

![3](https://user-images.githubusercontent.com/22911504/163142431-85f59a4b-3580-419b-b66e-fe48cb8ff936.png)

<br/>

## **CRUD. Create**
* 사용자가 응용 프로그램에 등록할 때 JSON 배열로 `USER_CONFIGRATIONS` 테이블에 레코드를 생성한다고 가정합니다.

```sql
INSERT INTO USER_CONFIGURATIONS
(DATA)
VALUES('{"userid":"artur@gmail.com", "configurations":[]}'::jsonb);
```

* 데이터가 성공적으로 추가 되었는지 아래 SQL 문을 통해 확인합니다.

```sql
SELECT * FROM USER_CONFIGURATIONS;
```

![4](https://user-images.githubusercontent.com/22911504/163142444-5dbd726c-b04a-4e07-92c8-3c55a99138e8.png)

* 테이블에 entity를 하나 더 추가해 보도록 하겠습니다.

```sql
INSERT INTO USER_CONFIGURATIONS
(DATA)
VALUES('{"userid":"ihor@gmail.com",
"configurations":[{ "name":"myconf", "theme":"light", "icons":"small", "textsize":"large" }, { "name":"myconf2", "theme":"dark" }]}'::jsonb);
```

* 데이터가 성공적으로 추가된 것을 확인할 수 있습니다.

![5](https://user-images.githubusercontent.com/22911504/163142447-b7420613-fb46-4870-b432-cdf81c9f2396.png)

<br/>

## **CRUD. Read**
* 때에 따라서 사용자 ID 별로 모든 구성을 가져 올 수 있어야 합니다.
* 예를 들어, **ihor@gmail.com** 과 관련된 모든 구성을 가져오고 싶으면 다음과 같이 SQL문을 작성하면 됩니다.

```sql
SELECT DATA -> 'configurations' AS configs
FROM USER_CONFIGURATIONS
WHERE 1 = 1
AND (DATA ->> 'userid') = 'ihor@gmail.com'
```

* 실행 결과, userid 가 `ihor@gmail.com` 사람의 정보가 검색되어 나오는 것을 확인할 수 있습니다.

![6](https://user-images.githubusercontent.com/22911504/163142448-fa800d0d-86f6-427d-97c5-0db3e8009cef.png)

### **Read 쿼리 분석**
* 앞서 아래 쿼리문을 통해 JSONB 데이터를 검색하였습니다.
* 좀 더 자세히 SQL 문을 분석해 보도록 하겠습니다.

```sql
SELECT DATA -> 'configurations' AS configs
FROM USER_CONFIGURATIONS
WHERE 1 = 1
AND (DATA ->> 'userid') = 'ihor@gmail.com'
```

![7](https://user-images.githubusercontent.com/22911504/163142449-c4bf738f-2545-46ca-a9b6-9dff3bf606ac.png)

* JSONB 열 유형으로 작업할 때 `->` 및 `->>` 와 같은 추가 기능을 사용할 수 있습니다.
* 둘 다 오른쪽에 지정된 이름으로 JSON 객체의 내용을 반환합니다.
* 이러한 함수의 차이점은 반환의 유형입니다.
* `->` 는 텍스트를 반환하고 `->>` 는 JSONB 를 반환합니다.
* 이 외에도 이름으로 사용자의 구성을 찾을 수 있어야 합니다.
* 아래와 같이 SQL 문을 작성할 수도 있습니다.

```sql
SELECT config as congifuration
FROM USER_CONFIGURATIONS
  CROSS JOIN jsonb_array_elements(DATA -> 'configurations') config
  WHERE (DATA ->> 'userid') = 'ihor@gmail.com'
  AND (config ->> 'name') = 'myconf';
```

<br/>

## **CRUD. Update**
* 다음은 Update 구문 입니다.
* `artur@gmail.com` 을 추가해 보도록 하겠습니다.

```sql
UPDATE USER_CONFIGURATIONS
SET DATA =
jsonb_set(DATA, '{configurations}'::text[], DATA ->'configurations' || '{"name":"firstconf", "theme":"dark", "textsize":"large"}'::jsonb)
WHERE 1 = 1
AND (DATA ->> 'userid') = 'artur@gmail.com';
```

* 데이터 조회 결과, 정상적으로 데이터가 Update 된 것을 확인할 수 있습니다.

![8](https://user-images.githubusercontent.com/22911504/163142450-1058db31-5bdc-4a2f-9f0d-624b879ad94d.png)

<br/>

### **Update 쿼리 분석**

```sql
UPDATE USER_CONFIGURATIONS
SET DATA =
jsonb_set(DATA, '{configurations}'::text[], DATA ->'configurations' || '{"name":"firstconf", "theme":"dark", "textsize":"large"}'::jsonb)
WHERE 1 = 1
AND (DATA ->> 'userid') = 'artur@gmail.com';
```

* 위에서 `jsonb_set(target jsonb, path text[], new_value jsonb, create_missing boolean)` 함수를 사용하고 있습니다.
* Data를 대상 JSONB 객체로 전달하고 'configurations' JSON 객체를 정보를 교체하려는 전달로 지정하고 '구성' JSON 객체에 대한 새 값을 정의합니다.
* 함수 `||` 는 연결 함수 입니다.
* 따라서 여기에서 이전 'configurations' 값을 가져와 새 값과 연결합니다.
* 그 후, 초기 DATA JSON에서 'configurations' 객체를 UPDATE 하고 DATA JSON을 업데이트 된 것으로 교체합니다.

<br/>

## **CRUD. Delete**
* 삭제 작업은 간단합니다.

```sql
DELETE FROM USER_CONFIGURATIONS
WHERE (DATA ->> 'userid') = 'ihor@gmail.com'
```