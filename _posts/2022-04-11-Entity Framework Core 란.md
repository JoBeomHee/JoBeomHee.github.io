---
title: Entity Framework Core 란
author: BeomBeomJoJo
date: 2022-04-11 12:33:00 +0800
date: 2022-04-11 12:33:00 +0800
categories: [Entity Framework Core]
tags: [Entity Framework Core]
math: true
mermaid: true
---

## **참고**
* [EF Core MSDN](https://docs.microsoft.com/ko-kr/ef/core/)
* [EF Core 튜토리얼](https://www.entityframeworktutorial.net/efcore/entity-framework-core.aspx)

<br/>

## **Entity Framework Core 란?**
* EF(Entity Framework) Core는 널리 사용되는 Entity Framework 데이터 액세스 기술의 가볍고 확장 가능한 오픈소스 플랫폼 입니다.
* EF Core는 다음과 같은 O/RM(개체 관계형 매퍼) 로 사용될 수 있습니다.
  * .NET 개발자가 .NET 개체를 사용하여 데이터베이스로 작업할 수 있도록 합니다.
  * 개발자가 일반적으로 작성해야 하는 대부분의 데이터 액세스 코드가 필요하지 않습니다.
* EF Core는 여러가지 데이터베이스 엔진을 지원합니다.
* 지원하는 정보는 [지원 정보](https://docs.microsoft.com/ko-kr/ef/core/providers/?tabs=dotnet-core-cli) 사이트를 참고하면 됩니다.

<br/>

## **Entity Framework Model**
* **Code First** : 데이터베이스를 미리 설계하지 않고 필요한 테이블을 코드로 먼저 작성합니다. (각 컬럼 이름, 컨스트레인 등) 그 후, 프로그램을 실행하면서 데이터베이스를 자동으로 생성합니다.
* **Model First** : 데이터베이스가 없을 때 EDMX 파일에 필요한 데이터베이스를 넣어서 작동하는 방식입니다.
* **Database First** : 데이터베이스를 먼저 구성하고 그것을 Visual Studio로 읽어 들여 작동하는 방식입니다.

![1](https://user-images.githubusercontent.com/22911504/162701636-cf837ae6-3751-4f9a-9b29-6dc386da4e68.png)