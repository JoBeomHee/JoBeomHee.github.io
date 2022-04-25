---
title: EF Core - DbContext
author: BeomBeomJoJo
date: 2022-04-12 12:33:00 +0800
date: 2022-04-12 12:33:00 +0800
categories: [EF Core]
tags: [EF Core]
math: true
mermaid: true
---

{% include adsense.html %}

## **참조**
* [참고 사이트](https://www.entityframeworktutorial.net/efcore/entity-framework-core-dbcontext.aspx)

<br/>

## **DbContext 란?**
* DbContext 클래스는 Entity Framework 의 필수적인 부분입니다.
* DbContext 인스턴스는 데이터베이스와의 세션을 나타내며 Entity의 인스턴스를 쿼리하고 저장하는 데 사용할 수 있습니다.
* DbContext는 작업 단위와 저장소 패턴의 조합입니다.
* EF Core 에서 DbContext는 다음 작업을 수행할 수 있습니다.
  * 데이터베이스 연결 관리
  * 모델 및 관계 구성
  * 데이터베이스 쿼리
  * 데이터베이스 데이터 저장
  * 변경 추적 구성
  * 캐싱
  * 거래 관리

<br/>

## **DbContext 예시**
* 애플리케이션에서 DbContext를 사용하려면 컨텍스트 클래스라고도 하는 DbContext에서 파생되는 클래스를 만들어야 합니다.
* 이 컨텍스트 클래스에는 일반적으로 모델의 각 Entity에 대한 `DbSet<TEntity>` 속성이 포함됩니다.
* EF Core에서 컨텍스트 클래스의 예는 다음과 같습니다.

```csharp
public class SchoolContext : DbContext
{
    public SchoolContext()
    {
  
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
    }
    //entities
    public DbSet<Student> Students { get; set; }
    public DbSet<Course> Courses { get; set; }
} 
```
* 위 예에서 SchoolContext 클래스는 DbContext 클래스에서 파생되며 Student 및 Course 유형의 DbSet<TEntity> 속성을 포함합니다.
* 또한, OnConfiguring 및 OnModelCreation 메서드를 재정의합니다.
* 데이터베이스에 연결하고 Student 또는 Course 데이터를 저장하거나 검색하며녀 **SchoolContext** 의 인스턴스를 만들어야 합니다.
* `OnConfiguring()` 메서드를 사용하면 DbContextOptionsBuilder를 사용하여 컨텍스트와 함께 사용할 데이터 소스를 선택하고 구성할 수 있습니다.
* `OnModelCreating()` 메서드를 사용하면 ModelBuilder Fluent API를 사용하여 모델을 구성할 수 있습니다.