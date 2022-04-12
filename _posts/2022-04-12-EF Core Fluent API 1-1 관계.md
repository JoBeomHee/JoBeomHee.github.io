---
title: EF Core - Fluent API 1:1 관계
author: BeomBeomJoJo
date: 2022-04-12 12:33:00 +0800
date: 2022-04-12 12:33:00 +0800
categories: [EF Core]
tags: [EF Core]
math: true
mermaid: true
---

## **참조**
* [참고 사이트](https://www.entityframeworktutorial.net/efcore/configure-one-to-one-relationship-using-fluent-api-in-ef-core.aspx)

<br/>

## **소개**
* 일반적으로 EF Core에는 1:1 관계에 대한 규칙이 포함되어 있으므로 1:1 관계를 수동으로 구성할 필요가 없습니다.
* 그러나 키 또는 외래 키 속성이 규칙을 따르지 않는 경우 데이터 주석 속성 또는 Fluent API를 사용하여 두 Entity 간의 1:1 관계를 구성할 수 있습니다.

<br/>

## **예제**
* 외래 키 규칙을 따르지 않는 다음 Student 및 StudentAddress Entity 간의 1:1 관계를 구성해 보도록 하겠습니다.

```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
       
    public StudentAddress Address { get; set; }
}

public class StudentAddress
{
    public int StudentAddressId { get; set; }
    public string Address { get; set; }
    public string City { get; set; }
    public string State { get; set; }
    public string Country { get; set; }

    public int AddressOfStudentId { get; set; }
    public Student Student { get; set; }
}
```
* EF Core에서 Fluent API를 사용하여 1:1 관계를 구성하려면 아래와 같이 `HasOne`, `WithOne` 및 `HasForeignKey` 메서드를 사용합니다.

```csharp
using Microsoft.EntityFrameworkCore;

namespace StudentApp.Data
{
    public class SchoolContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseNpgsql(@"Server=localhost;Database=JoBeomHee;Port=5432;User Id=JoBeomHee;Password=1234;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Student>()
                .HasOne<StudentAddress>(s => s.Address)
                .WithOne(ad => ad.Student)
                .HasForeignKey<StudentAddress>(ad => ad.AddressOfStudentId);
        }

        public DbSet<Student> Students { get; set; }
        public DbSet<StudentAddress> StudentAddresses { get; set; }
    }
}
```
* 위 코드에서 다음 코드 조각은 1:1 관계 구성을 나타냅니다.

```csharp
modelBuilder.Entity<Student>()
                .HasOne<StudentAddress>(s => s.Address)
                .WithOne(ad => ad.Student)
                .HasForeignKey<StudentAddress>(ad => ad.AddressOfStudentId);
```
* `modelBuilder.Entity<Student>()` 는 학생 Entity 구성을 시작합니다.
* `.HasOne<StudentAddress>(s => s.Address)` 메서드는 람다 식을 사용하여 Student Entity가 하나의 StudentAddress 참조 속성을 포함하도록 지정합니다.
* `WithOne(ad => ad.Student)` 는 관계의 다른 쪽 끝인 StudentAddress Entity를 구성합니다.
* StudentAddress Entity가 학생 유형의 참조 탐색 속성을 포함하도록 지정합니다.
* `.HasForeginKey<StudentAddress>(ad => ad.AddressOfStudentId)` 외래 키 속성 이름을 지정합니다.

<br/>

## **테이블 관계 확인**
* Migration 후, 테이블 관계가 1:1 관계로 생성 되었는지 확인합니다.
* 확인 결과, 1:1 관계로 PostgreSQL 데이터베이스에서 테이블이 생성된 것을 확인할 수 있습니다.

![1](https://user-images.githubusercontent.com/22911504/162951223-3ab4036f-451c-4ab9-93ec-750b18fcf416.png)