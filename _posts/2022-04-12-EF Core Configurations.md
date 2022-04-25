---
title: EF Core - Configurations
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
* [참고 사이트](https://www.entityframeworktutorial.net/efcore/configuration-in-entity-framework-core.aspx)

<br/>

## **목적**
* EF Core 를 사용하면 EF 모델을 데이터베이스 매핑으로 사용자 지정하기 위해 도메인 클래스를 구성할 수 있습니다.
* 이 프로그래밍 패턴을 `Convention over Configuration` 이라고 합니다.

<br/>

## **EF Core 도메인 구성 방법**
* EF Core 에서 도메인 클래스를 구성하는 방법에는 2가지 방법이 있습니다.
  * **By using Data Annotation Attributes**
  * **By using Fluent API**

<br/>

## **Data Annotation Attributes**
* Data Annotation은 다른 .NET 속성을 도메인 클래스 및 속성에 적용하여 모델을 구성할 수 있는 간단한 속성 기반 구성 방법입니다.
* Data Annotation은 ASP.NET MVC에서도 사용되므로 Entity Framework 전용이 아닙니다.
* 다음 예제에서 Data Annotation 속성을 도메인 클래스 및 속성에 적용하여 규칙을 재정의하는 방법을 보여 줍니다.

```csharp
[Table("StudentInfo")]
public class Student
{
    public Student() { }
        
    [Key]
    public int SID { get; set; }

    [Column("Name", TypeName="ntext")]
    [MaxLength(20)]
    public string StudentName { get; set; }

    [NotMapped]
    public int? Age { get; set; }
        
        
    public int StdId { get; set; }

    [ForeignKey("StdId")]
    public virtual Standard Standard { get; set; }
}
```

<br/>