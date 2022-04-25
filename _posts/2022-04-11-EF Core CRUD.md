---
title: EF Core CRUD
author: BeomBeomJoJo
date: 2022-04-11 12:33:00 +0800
date: 2022-04-11 12:33:00 +0800
categories: [Entity Framework Core]
tags: [Entity Framework Core]
math: true
mermaid: true
---

{% include adsense.html %}

## **참고**
* [EF Core 첫 번째 어플리케이션](https://jobeomhee.github.io/posts/EF-Core-%EC%B2%AB-%EB%B2%88%EC%A7%B8-%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98/)

<br/>

## **소개**
* 앞서 Code-First 방식으로 EF Core 첫 번째 어플리케이션을 만들어 보았습니다.
* 다음으로 만들어진 프로젝트에 이어서, CRUD 기능을 추가해 보도록 하겠습니다.

<br/>

## **Insert Data**
* 앞서, School, Course 테이블을 마이그레이션 하여 PostgreSQL 데이터베이스에 생성하였습니다.
* 그럼 실제로 School 테이블에 데이터를 저장해 보도록 하겠습니다.

```csharp
using EFCoreSample;

using (var context = new SchoolContext())
{
    context.Database.EnsureCreated();
    AddStudent(context);
}

static void AddStudent(SchoolContext context)
{
    var student = context.Students.FirstOrDefault();
    if (student != null) return;

    context.Students.Add(new Student
    {
        StudentId = 1,
        Name = "JoBeomHee"
    });

    context.Students.Add(new Student
    {
        StudentId = 2,
        Name = "BeomBeomJoJo"
    });

    context.SaveChanges();
}
```

* PostgreSQL 에서 Student 테이블을 직접 확인 결과, 2개의 데이터가 정상적으로 Insert 된 것을 확인할 수 있습니다.

![1  Insert Data](https://user-images.githubusercontent.com/22911504/162732947-8d7d5225-31ad-4e08-8e8f-2e70da66bb9b.png)

<br/>

## **Update Data**
* 다음은 앞서 추가했던 데이터에서 이름이 "BeomBeomJoJo" 인 사람의 이름을 "Kim" 으로 변경해 보도록 하겠습니다.

```csharp
using EFCoreSample;

using (var context = new SchoolContext())
{
    context.Database.EnsureCreated();
    UpdateStudent(context);
}

static void UpdateStudent(SchoolContext context)
{
    var student = context.Students.Where(x => x.Name == "BeomBeomJoJo").FirstOrDefault();
    student.Name = "Kim";
    context.SaveChanges();
}
```

* 실행 결과, "BeomBeomJoJo" 이름에서 "Kim" 으로 변경된 것을 확인할 수 있습니다.

![2  Update Data](https://user-images.githubusercontent.com/22911504/162732953-cff533e2-8087-450a-958b-ab2f03b11186.png)

<br/>

## **Delete Data**
* 다음으로는 데이터를 삭제해 보도록 하겠습니다.
* 이름이 "Kim" 이라는 학생을 조회하여 해당 학생을 student 테이블에서 삭제해 보도록 하겠습니다.

```csharp
using EFCoreSample;

using (var context = new SchoolContext())
{
    context.Database.EnsureCreated();
    DeleteStudent(context);
}

static void DeleteStudent(SchoolContext context)
{
    var student = context.Students.Where(x => x.Name == "Kim").FirstOrDefault();
    context.Students.Remove(student);

    // or
    // context.Remove<Student>(student);

    context.SaveChanges();
}
```

* 실행 결과, 삭제 된 것을 확인할 수 있습니다.

![3  Delete Data](https://user-images.githubusercontent.com/22911504/162732956-6b9bf661-e261-465a-bd30-ae711d7479bf.png)

<br/>

## **전체 소스 코드**
* program.cs 의 전체 소스코드는 다음과 같습니다.

```csharp
using EFCoreSample;

using (var context = new SchoolContext())
{
    context.Database.EnsureCreated();
    DeleteStudent(context);
}

static void AddStudent(SchoolContext context)
{
    var student = context.Students.FirstOrDefault();
    if (student != null) return;

    context.Students.Add(new Student
    {
        StudentId = 1,
        Name = "JoBeomHee"
    });

    context.Students.Add(new Student
    {
        StudentId = 2,
        Name = "BeomBeomJoJo"
    });

    context.SaveChanges();
}

static void UpdateStudent(SchoolContext context)
{
    var student = context.Students.Where(x => x.Name == "BeomBeomJoJo").FirstOrDefault();
    student.Name = "Kim";
    context.SaveChanges();
}

static void DeleteStudent(SchoolContext context)
{
    var student = context.Students.Where(x => x.Name == "Kim").FirstOrDefault();
    context.Students.Remove(student);

    // or
    // context.Remove<Student>(student);

    context.SaveChanges();
}
```