---
title: PostgreSQL - Docker Container로 실행하기
author: BeomBeomJoJo
date: 2022-04-05 12:33:00 +0800
date: 2022-04-05 12:33:00 +0800
categories: [PostgreSQL]
tags: [PostgreSQL]
math: true
mermaid: true
---

## **참조**
* [참조 블로그](https://xeppetto.github.io/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4/WSL-and-Docker/15-Docker-PostGreSQL/)

<br/>

## **PostgreSQL Docker 컨테이너 실행**
* 다음 명령어를 입력하여 PostgreSQL Docker 컨테이너를 실행합니다.

```console
> docker run -d -p 5432:5432 -e POSTGRES_PASSWORD="<YourStrong@Passw0rd>" --name PostgreSQL01 postgres
```

* 위의 명령어 내용은 다음과 같습니다.
  * docker run : docker image에서 container를 생성
  * --name PostgreSQL01 : container 의 이름
  * -p 5432:5432 : 해당 container의 port forwarding에 대해 inbound/outbound port 모두 5432 설정
  * -e : Container 내 변수 설정
  * POSTGRES_PASSWORD="암호" : ROOT 암호를 설정
  * -d postgres : postgres라는 이미지에서 분리하여 container 를 생성

<br/>

## **컨테이너 생성 및 실행 완료**
* 다음과 같이 정상적으로 PostgreSQL 컨테이너가 다운 및 생성 된 것을 확인할 수 있습니다.

```console
Unable to find image 'postgres:latest' locally
latest: Pulling from library/postgres
c229119241af: Already exists
3ff4ca332580: Pull complete
5037f3c12de6: Pull complete
0444ef779945: Pull complete
47098a4166e7: Pull complete
203cca980fab: Pull complete
a479b6c0e001: Pull complete
1eaa9abe8ca4: Pull complete
cad613328fe3: Pull complete
1ce5087aacfa: Pull complete
b133d2355caa: Pull complete
b2694eb85faf: Pull complete
503b75e1e236: Pull complete
Digest: sha256:e3d8179786b8f16d066b313f381484a92efb175d1ce8355dc180fee1d5fa70ec
Status: Downloaded newer image for postgres:latest
7751d5684ea397baea2b74340ab668ab68fa5742bebef8e02f40b817dde2c530

> docker ps -a

CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS                     PORTS                               NAMES
7751d5684ea3   postgres        "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes               0.0.0.0:5432->5432/tcp              PostgreSQL01
```

<br/>

## **PostgreSQL 컨테이너 진입**
* 만약, 컨테이너가 실행 중인 상태일 때 컨테이너로 진입 시 다음 명령을 사용합니다.

```console
> docker exec --user="root" -it PostgreSQL01 bash
root@7751d5684ea3:/#
```

<br/>

## **PostgreSQL을 실행하고 데이터 만들어 보기**
* 다음 명령어를 통해 DB를 실행합니다.

```console
> psql -U postgres
```

```console
root@7751d5684ea3:/# psql -U postgres
psql (14.2 (Debian 14.2-1.pgdg110+1))
Type "help" for help.

postgres=#
```

* 진입 하였다면, 서버 내 생성되어 있는 모든 데이터베이스 이름을 확인합니다.

```sql
postgres=# SELECT datname FROM pg_database; --전체 데이터베이스 이름 출력
  datname
-----------
 postgres
 template1
 template0
(3 rows)
```

```sql
postgres=# SELECT datname FROM pg_database WHERE datistemplate = false; -- 사용자가 생성한 데이터베이스 이름만 출력
 datname
----------
 postgres
(1 row)
```

* PostgreSQL 내에 사용자 Database를 하나 생성해 봅니다.
* 사용 방법은 **CREATE DATABASE 데이터베이스 이름;** 하면 됩니다.

```sql
postgres=# CREATE DATABASE postgresTestDB;
CREATE DATABASE
```

* 생성된 Database 의 Schema를 보려면 다음과 같이 입력합니다.

```sql
postgres=# SELECT nspname FROM pg_catalog.pg_namespace;
      nspname
--------------------
 pg_toast
 pg_catalog
 public
 information_schema
(4 rows)
```

* 생성한 데이터베이스들 중 하나를 사용하기 위해서는 **\c 데이터베이스이름** 을 실행하면 됩니다.

```sql
postgrestestdb=# \c postgres
You are now connected to database "postgres" as user "postgres".
postgres=# \c postgrestestdb
You are now connected to database "postgrestestdb" as user "postgres".
```

* **postgrestestdb** 를 선택한 후, 특정 DB 내 모든 Table을 확인해 봅니다.

```sql
SELECT * FROM PG_TABLES; -- PostgreSQL 내 모든 테이블 이름 조회
SELECT * FROM PG_TABLES WHERE schemaname='public'; -- 사용자가 생성한 테이블 이름 조회
SELECT table_name FROM information_schema.tables WHERE table_schema = 'public' ORDER BY table_name; -- 사용자가 생성한 테이블의 이름 정보만 조회
```

* 다음과 같이 SQL 구문을 사용하여 간단히 데이터테이블을 하나 생성 후, 데이티 입력 후 조회해 보도록 하겠습니다.

```sql
postgrestestdb=# CREATE TABLE TestTable (ID INT, TestString VARCHAR(30));
CREATE TABLE
postgrestestdb=# INSERT INTO TestTable VALUES(1, 'number one');
INSERT 0 1
postgrestestdb=# INSERT INTO TestTable VALUES(2, 'number two');
INSERT 0 1
postgrestestdb=# SELECT * FROM TestTable;
 id | teststring
----+------------
  1 | number one
  2 | number two
(2 rows)
```

<br/>

## **PostgreSQL Bash 빠져나오기**
* PostgreSQL에서 빠져나올 때는 아래와 같이 입력하면 됩니다.

```console
\q
```