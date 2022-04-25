---
title: EF Core - Fluent API 1:N 관계
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
* [참고 사이트](https://www.entityframeworktutorial.net/efcore/configure-one-to-many-relationship-using-fluent-api-in-ef-core.aspx)

<br/>

## **소개**
* 일반적으로 EF Core 에는 자동으로 구성할 수 있는 충분한 규칙이 포함되어 있으므로 1:N 관계를 구성할 필요가 없습니다.
* 그러나 쉬운 유지 관리를 위해 Fluent API 에 모든 EF 구성을 포함하기로 결정한 경우 Fluent API를 사용하여 1:N 관계를 구성할 수 있습니다.

<br/>

## **예제**
* EF Core를 사용하면 Fluent API를 사용하여 관계를 쉽게 구성할 수 있습니다.
* Grade Entity가 많은 Student Entity를 포함하는 다음 Student 및 Grade 클래스를 보시면 됩니다.

```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }

    public int CurrentGradeId { get; set; }
    public Grade Grade { get; set; }
}

public class Grade
{
    public int GradeId { get; set; }
    public string GradeName { get; set; }
    public string Section { get; set; }

    public ICollection<Student> Students { get; set; }
}
```

* 아래와 같이 Context 클래스에서 OnModelCreating 메서드를 재정의하여 Fluent API를 사용하여 위 Entity에 대한 1:N 관계를 구성할 수 있습니다.

```csharp
using Microsoft.EntityFrameworkCore;

namespace StudentApp.Data
{
    public class SchoolContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseNpgsql(@"Server=localhost;Database=JoBeomHee;Port=5432;User Id=JoBeomHee;Password=1234");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Student>()
                .HasOne<Grade>(s => s.Grade)
                .WithMany(g => g.Students)
                .HasForeignKey(s => s.CurrentGradeId);
        }

        public DbSet<Grade> Grades { get; set; }    
        public DbSet<Student> Students { get; set; }
    }
}

```

* 위 예에서 다음 코드는 1:N 관계를 구성합니다.

```csharp
modelBuilder.Entity<Student>()
            .HasOne<Grade>(s => s.Grade)
            .WithMany(g => g.Students)
            .HasForeignKey(s => s.CurrentGradeId);
```

* 이제 데이터베이스에 반영하기 위해 마이그레이션 명령인 **Add-Migration <name>** 및 **update-database** 를 실행합니다.
* 마이그레이션이 정상적으로 실행 되었다면, 데이터베이스에는 아래와 같이 1:N 관계가 있는 2개의 Student, Grade 테이블이 생성된 것을 확인할 수 있습니다.

![1](https://user-images.githubusercontent.com/22911504/162951034-46f1be2b-3354-443b-946a-c3f8a4e6e34c.png)

<br/>

## **코드 관계 해석**
* 첫째로, Student 또는 Grade 중 하나의 Entity Class로 구성을 시작해야 합니다.
* 따라서 `modelBuilder.Entity<student>()` 는 Student Entity로 시작합니다.
* 두 번째로, `.HasOne<Grade>(s => s.Grade)` 는 Student Entity에 Grade라는 Grade 유형 속성이 포함 되도록 지정합니다.
* 세 번째로, 관계의 다른 쪽 끝인 Grade 엔티티를 구성해야 합니다.
* `.WithMany(g => g.Stduents)` 는 Grade Entity 클래스에 많은 Student Entity가 포함되도록 지정합니다.
* 여기서 WithMany는 컬렉션 탐색 속성을 유추합니다.
* `.HasForeignKey<int>(s => s.CurrentGradeId);` 외래 키 속성 CurrentGradeId의 이름을 지정합니다.
* 외래키 지정은 선택사항 입니다. 종속 클래스에 외래 키 Id 속성이 있는 경우에만 사용합니다.

![2](https://user-images.githubusercontent.com/22911504/162951037-c4fb6f03-5923-4acf-bfc4-fc45fc6150f9.png)