---
title: EF Core - Fluent API N:N 관계
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
* [참고 사이트](https://www.entityframeworktutorial.net/efcore/configure-many-to-many-relationship-in-ef-core.aspx)

<br/>

## **목적**
* EF Core 에서 Entity 간의 N:N 관계 표현은 어떻게 표현하는지 알아보도록 합니다.

<br/>

## **예제**
* 다음 Student 및 Course Entity 간에 N:N 관계를 구현해 보도록 하겠습니다.
* 여기에서 한 학생이 여러 Course에 등록할 수 있고 같은 방식으로 한 Course 에 많은 학생이 참여할 수 있다고 합니다.

```csharp
public class Student
{
    public int StudentId { get; set; }
    public string Name { get; set; }
}

public class Course
{
    public int CourseId { get; set; }
    public string CourseName { get; set; }
    public string Description { get; set; }
}
```
* 데이터베이스의 N:N 관계는 두 테이블의 외래 키를 포함하는 조인 테이블로 표시합니다.
* 또한, 이러한 외래 키는 복합 기본 키입니다.

![1](https://user-images.githubusercontent.com/22911504/162951397-4f025455-494d-48d6-8330-3c8915c70f23.png)

<br/>

## **Convention**
* N:N 관계를 자동으로 구성하는 Entity Framework Core에서 사용할 수 있는 기본 규칙은 없습니다.
* Fluent API를 사용해 N:N 관계를 구성해야 합니다.

<br/>

## **Fluent API**
* EF Core 6.x 또는 이전 버전에서 EF API는 N:N 관계에 대한 조인 테이블을 만드는 데 사용되었습니다.
* 조인 테이블에 대한 조인 Entity를 만들 필요가 없습니다.
* 조인 테이블에 대한 조인 Entity 클래스를 만들어야 합니다.
* 위의 Student 및 Course Entity에 대한 조인 Entity는 각 Entity에 대한 외래 키 속성과 참조 탐색 속성을 포함해야 합니다.

<br/>

## **N:N 관계 구성하기**
* N:N 관계 관계를 구성하는 단계는 다음과 같습니다.
  * 각 Entity에 대한 외래 키 속성과 탐색 속성을 포함하는 새 조인 Entity 클래스를 정의합니다.
  * 양쪽 Entity(Student <-> Course) 에 컬렉션 탐색 속성을 포함하여 다른 두 Entity와 조인 Entity 간의 1:N 관계를 정의합니다.
  * Fluent API를 사용하여 결합 Entity의 두 외래 키를 복합 키로 구성합니다.
* 따라서, 먼저 아래와 같이 조인 Entity StudentCourse를 정의합니다.

```csharp
public class StudentCourse
{
    public int StudentId { get; set; }
    public Student Student { get; set; }

    public int CourseId { get; set; }
    public Course Course { get; set; }
}
```
* 위의 조인 Entity StudentCourse 에는 참조 탐색 속성인 Student 및 Course 와 해당 외래 키 속성인 StudentId 및 CourseId가 각각 포함됩니다.
* 이제 Student -> StudentCourse 및 Course -> StudnetCourse Entity 간에 두 개의 개별 1:N 관계도 구성해야 합니다.
* 아래와 같이 1:N 관계에 대한 규칙을 구성하면 됩니다.

```csharp
public class Student
{
    public int StudentId { get; set; }
    public string Name { get; set; }

    public IList<StudentCourse> StudentCourses { get; set; }
}

public class Course
{
    public int CourseId { get; set; }
    public string CourseName { get; set; }
    public string Description { get; set; }

    public IList<StudentCourse> StudentCourses { get; set; }
}
```
* 위에서 볼 수 있듯이 이제 Student 및 Course Entity에는 StudentCourse 유형의 컬렉션 탐색 속성이 포함됩니다.
* StudentCourse Entity 는 이미 Student 및 Course 에 대한 외래 키 속성 및 탐색 속성이 포함되어 있습니다.
* 이것은 Student & StudentCourse 와 Course & StudentCourse 사이에 완전히 정의된 1:N 관계를 만듭니다.
* 이제 외래 키는 조인 테이블의 복합 기본 키여야 합니다.
* 이것은 아래와 같이 Fluent API 를 통해서만 구성할 수 있습니다.

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
            modelBuilder.Entity<StudentCourse>()
                .HasKey(sc => new { sc.StudentId, sc.CourseId });
        }

        public DbSet<Student> Students { get; set; }
        public DbSet<Course> Courses { get; set; }
        public DbSet<StudentCourse> StudentCourses { get; set; }
    }
}
```
* 위 코드에서 `modelBuilder.Entity<StudentCourse>().HasKey(sc => new {sc.StudentId, sc.CourseId})` 는 StudentId와 CourseID를 복합 키로 설정합니다.
* Entity가 조인 Entity와의 1:N 관계 규칙을 따르는 경우 이것이 N:N 관계를 구성할 수 있는 방법입니다.
* 외래 키 속성 이름이 규칙을 따르지 않는다고 가정하면 아래와 같이 Fluent API를 사용하여 구성할 수도 있습니다.

```csharp
modelBuilder.Entity<StudentCourse>().HasKey(sc => new { sc.SId, sc.CId });

modelBuilder.Entity<StudentCourse>()
    .HasOne<Student>(sc => sc.Student)
    .WithMany(s => s.StudentCourses)
    .HasForeignKey(sc => sc.SId);

modelBuilder.Entity<StudentCourse>()
    .HasOne<Course>(sc => sc.Course)
    .WithMany(s => s.StudentCourses)
    .HasForeignKey(sc => sc.CId);
```

<br/>

## **PostgreSQL 테이블 확인**
* PostgreSQL 테이블 실제 확인 결과, 정상적으로 테이블 및 제약사항이 생성된 것을 확인할 수 있습니다.

![2](https://user-images.githubusercontent.com/22911504/162951401-6498ca3a-1f57-4e1d-8bc2-d9d4f0145bfa.png)
