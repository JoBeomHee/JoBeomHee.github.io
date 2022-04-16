---
title: EF Core 마이그레이션을 위한 패키지 관리자 콘솔 명령
author: BeomBeomJoJo
date: 2022-04-16 12:33:00 +0800
date: 2022-04-16 12:33:00 +0800
categories: [EF Core, Migration]
tags: [EF Core, Migration]
math: true
mermaid: true
---

## **참고**
* [참고 사이트](https://docs.microsoft.com/ko-kr/ef/core/managing-schemas/migrations/?tabs=dotnet-core-cli)

<br/>

## **소개**
* EF Core의 마이그레이션 명령은 Visual Studio의 패키지 관리자 콘솔(PMC) 을 사용하여 실행할 수 있습니다.
* `Visual Studio 도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔 메뉴` 에서 패키지 관리자 콘솔을 실행하여 명령을 실행하면 됩니다.
* 그럼, EF Core Migration 관련된 명령어들이 어떤 것들이 있는 지에 대해서 조사 합니다.

<br/>

## **PMC 명령어 종류**
* EF Core Migration PMC 명령어 종류는 다음과 같습니다.

|PMC Command|Usage|
|-----------|-----|
|Get-Help entityframework|엔터티 프레임워크 명령에 대한 정보를 표시합니다.|
|Add-Migration `<migration name>` |마이그레이션 스냅샷을 추가하여 마이그레이션을 생성합니다.|
|Remove-Migration|마지막 마이그레이션 스냅샷을 제거합니다.|
|Update-Database|마지막 마이그레이션 스냅샷을 기반으로 데이터베이스 스키마를 업데이트합니다.|
|Script-Migration|모든 마이그레이션 스냅샷을 사용하여 SQL 스크립트를 생성합니다.|
|Scaffold-DbContext|지정된 데이터베이스에 대한 DbContext 및 엔터티 형식 클래스를 생성합니다. 이것을 리버스 엔지니어링이라고 합니다.|
|Get-DbContext|DbContext 형식에 대한 정보를 가져옵니다.|
|Drop-Database|데이터베이스를 삭제합니다.|

<br/>
