---
title: PostgreSQL Docker Container 실행 및 pgAdminGUI 사용
author: BeomBeomJoJo
date: 2022-04-11 12:33:00 +0800
date: 2022-04-11 12:33:00 +0800
categories: [PostgreSQL]
tags: [PostgreSQL]
math: true
mermaid: true
---

{% include adsense.html %}

## **목적**
* PostgreSQL 을 Docker Container 로 실행한 후, PostgreSQL GUI 도구인 pgAdminGUI 를 이용하여 테스트 진행할 테이블 생성 및 데이터 추가 합니다.

<br/>

## **PostgreSQL Docker Container 실행**
* 다음 명령어를 통해 PostgreSQL Database 를 Docker Container로 실행합니다.

```console
> docker run --name pg -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres
```

<br/>

## **pgAdmin GUI 설치 및 실행**
* [pgAdmin 공식 홈페이지](https://www.pgadmin.org/download/) 에서 접속하여 최신 버전의 pgADmin GUI 프로그램을 설치 진행합니다.
* 본인 OS 에 맞는 버전에 맞춰서 설치 진행하면 됩니다.

![1](https://user-images.githubusercontent.com/22911504/162698006-5c82dd8b-f25e-4efb-8dd0-3e98b76e6c25.png)

<br/>

## **pgAdmin 실행**
* 정상적으로 pgAdmin GUI 도구가 설치 되었으면 **Add New Server** 메뉴를 선택하여 서버 하나를 추가합니다.
* 여기서 본인의 정보에 맞게 **General**, **Connection** 정보를 입력하면 됩니다.
* 정상적으로 서버가 추가 되었으면 다음과 같이 Server 정보가 표시됩니다.

![2](https://user-images.githubusercontent.com/22911504/162698016-65d4f061-83f9-4170-b4b4-2f752584b519.png)

<br/>

## **Query Tool 사용하기**
* 앞서 **Add New Server** 를 정상적으로 등록하였다면, 이제 Query Tool 을 이용하여 하나의 테스트 테이블 및 데이터를 추가해 보도록 하겠습니다.
* 기본으로 **postgres** 라는 이름의 데이터베이스가 생성되어 있습니다.
* **postgres** 데이터베이스 마우스 우 클릭 후, **Query Tool** 메뉴를 선택합니다.
* 그럼 다음과 같이 Query 를 작성할 수 있는 Editor 가 나오는 것을 확인할 수 있습니다.

![3](https://user-images.githubusercontent.com/22911504/162698019-225c8bfa-2990-47be-bf84-7f35c7c480d6.png)

![4](https://user-images.githubusercontent.com/22911504/162698021-acabe965-1487-4efa-b207-8d29cf6c49c0.png)

<br/>

## **Users 테이블 추가**
* Query Tools를 이용하여 **users** 라는 테이블 하나를 추가 합니다.
* 다음 SQL 문을 입력하고, **F5** 키를 입력하여 SQL 문을 추가시켜 줍니다.
* 실행 결과, users 테이블이 정상적으로 생성된 것을 확인할 수 있습니다.

```sql
create table "users"(
  "name" varchar,
  "age" int
);
```

![5](https://user-images.githubusercontent.com/22911504/162698025-3fca809a-933f-4b43-87fd-c4a323e1a533.png)

<br/>

## **users 테이블에 데이터 추가**
* 앞에서 생성한 users 테이블에 user 데이터를 추가합니다.
* user 데이터를 추가하는 SQL문은 다음과 같습니다.

```sql
insert into users values('JoBeomHee', '30');
```

<br/>

## **데이터 조회**
* SELECT 문을 통해 users 테이블에 데이터가 정상적으로 추가 되었는지 확인합니다.
* 조회 결과, 정상적으로 데이터가 추가된 것을 확인할 수 있습니다.

```sql
SELECT * FROM users
```

![6](https://user-images.githubusercontent.com/22911504/162698028-5e859056-9453-4513-89e9-5d8f595de6d4.png)
