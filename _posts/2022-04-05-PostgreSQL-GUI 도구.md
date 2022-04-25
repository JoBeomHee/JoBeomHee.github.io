---
title: PostgreSQL - GUI pgAdmin 설치하기
author: BeomBeomJoJo
date: 2022-04-05 12:33:00 +0800
date: 2022-04-05 12:33:00 +0800
categories: [PostgreSQL]
tags: [PostgreSQL]
math: true
mermaid: true
---

{% include adsense.html %}

## **참조**
* [참조 블로그](https://xeppetto.github.io/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4/WSL-and-Docker/15-Docker-PostGreSQL/)
* [에러 참조](https://github.com/sameersbn/docker-postgresql/issues/112)

<br/>

## **목적**
* PostgreSQL GUI 도구인 pgADmin 설치하는 방법에 대해서 알아봅니다.

<br/>

## **GUI 도구 pgAdmin 설치하기**
* PostgreSQL은 GUI 도우인 pgAdmin 을 지원합니다.
* pgAdmin 설치는 **[pgAdmin 홈페이지](https://www.pgadmin.org/)**  에서 가능합니다.
* 저는 OS 를 현재 Windows 를 사용중이기 때문에, Download > Window 를 선택하여 Windows 룔 클라이언트를 다운로드 받았습니다.
* 현재 최신버전은 v6.7 버전이었습니다.
* 최신 버전을 다운로드 받습니다.

![1](https://user-images.githubusercontent.com/22911504/161745062-d81d866b-0606-4474-adec-0d4444884b34.png)

![2](https://user-images.githubusercontent.com/22911504/161745068-64f3d0ea-f2fc-45b1-8744-1e88cd039994.png)

![3](https://user-images.githubusercontent.com/22911504/161745070-3acf0191-7881-4d7b-9476-9c8e276f6b88.png)

<br/>

## **pagadmin4-6.7-x64.exe**
* 현재 날짜 기준, pagadmin4-6.7-x64.exe 가 최신 버전이고, 해당 설치 파일을 실행하여 다운로드 진행합니다.

![4](https://user-images.githubusercontent.com/22911504/161745072-0b449b76-f6e4-4a5f-95f4-6211eb0e7152.png)

![5](https://user-images.githubusercontent.com/22911504/161745074-dc37e398-7775-4828-a2c3-31b11b13c21c.png)

<br/>

## **설치 완료**
* 설치가 완료 되면, 검색란에 **pgAdmin** 입력하게 되면 아이콘이 나오는 것을 확인할 수 있습니다.

![6](https://user-images.githubusercontent.com/22911504/161745075-64a552c1-ac87-4658-bffd-78ddbd921930.png)

<br/>

## **pgAdmin 실행**
* pgAdmin 을 실행하게 되면, root 비밀번호 입력하는 창이 나옵니다.
* 해당 비밀번호는 Docker Container 에서 설정했던 번호를 입력하면 됩니다.

![7](https://user-images.githubusercontent.com/22911504/161745077-1892ec9e-18c1-498d-8e43-7b5ef47e77cc.png)

<br/>

## **연결하기**
* **Add New Server** 아이콘을 선택하여 PostgreSQL01 도커 컨테이너로 실행된 녀석이랑 연결해 보도록 하겠습니다.

![8](https://user-images.githubusercontent.com/22911504/161745079-b46733b5-ec82-4cbb-9558-c574df4c880c.png)

<br/>

### **General 입력**
* Name은 Docker PostgreSQL 이라고 입력 하였습니다.

![9](https://user-images.githubusercontent.com/22911504/161745081-cb52d09d-a45c-4beb-bcd9-021e3372187c.png)

<br/>

### **Connection 입력**
* Host는 Localhost 이기 때문에 **127.0.0.1** 입력 합니다.
* Password 는 컨테이너 생성 시의 암호를 입력합니다.
* 만약, "password authentication failed for user "postgres"" 에러가 발생하게 되면, Docker Container 를 삭제하고 아래 명령어를 통해 다시 실행합니다.

```console
docker run --name pg -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres
```

![10](https://user-images.githubusercontent.com/22911504/161745084-9e4458de-99ba-4165-a998-72662f6300d7.png)

![11](https://user-images.githubusercontent.com/22911504/161745090-df97cef8-79a2-4937-a8fe-43abe2fd6409.png)