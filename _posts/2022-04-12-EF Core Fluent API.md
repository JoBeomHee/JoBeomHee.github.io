---
title: EF Core - Fluent API
author: BeomBeomJoJo
date: 2022-04-12 12:33:00 +0800
date: 2022-04-12 12:33:00 +0800
categories: [EF Core]
tags: [EF Core]
math: true
mermaid: true
---


## **참조**
* [참고 사이트](https://www.entityframeworktutorial.net/efcore/fluent-api-in-entity-framework-core.aspx)

<br/>

## **소개**
* Entity Framework Fluent API는 규칙을 재정의하도록 도메인 클래스를 구성하는 데 사용됩니다.
* EF Fluent API는 결과가 메서드 체인으로 공식화되는 Fluent API 디자인 패턴을 기반으로 합니다.
* Entity Framework Core에서 ModelBuilder 클래스는 Fluent API 역할을 합니다.
* 이를 사용하여 데이터 주석 속성보다 더 많은 구성 옵션을 제공하므로 다양한 것을 유연하게 구성하여 사용할 수 있습니다.

<br/>

## **Entity Framework Core 모델 구성 요소**
* Entity Framework Core Fluent API는 모델의 다음 측면을 구성합니다.
  * 모델 구성 : 데이터베이스 매핑에 대한 EF 모델을 구성합니다. 매핑에서 제외할 기본 스키마, DB 기능, 추가 데이터 주석 속성 및 엔티티를 구성합니다.
  * 엔티티 구성 : 엔티티를 데이틀 및 관계 매핑으로 구성합니다. PrimaryKey, AlternateKey, 인덱스, 테이블 이름, 일대일, 일대다, 다다다 관계 등등
  * 속성 구성 : 속성을 Column 매핑에 구성합니다. Column 이름, 기본값, null 허용 여부, 외래 키, 데이터 유형, 동시성 열 등등

<br/>

### **구성 유형 정보**
* 다음 표에는 각 구성 유형에 대한 중요한 방법이 나열되어 있습니다.


| Configurations         | Fluent API Methods            | Usage                                                                                                                                                                 |   |   |
|------------------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|---|
| Model Configurations   | HasDbFunction()               | Configures a database function when targeting a relational database.                                                                                                  |   |   |
|                        | HasDefaultSchema()            | Specifies the database schema.                                                                                                                                        |   |   |
|                        | HasAnnotation()               | Adds or updates data annotation attributes on the entity.                                                                                                             |   |   |
|                        | HasSequence()                 | Configures a database sequence when targeting a relational database.                                                                                                  |   |   |
| Entity Configuration   | HasAlternateKey()             | Configures an alternate key in the EF model for the entity.                                                                                                           |   |   |
|                        | HasIndex()                    | Configures an index of the specified properties.                                                                                                                      |   |   |
|                        | HasKey()                      | Configures the property or list of properties as Primary Key.                                                                                                         |   |   |
|                        | HasMany()                     | Configures the Many part of the relationship, where an entity contains the reference collection property of other type for one-to-Many or many-to-many relationships. |   |   |
|                        | HasOne()                      | Configures the One part of the relationship, where an entity contains the reference property of other type for one-to-one or one-to-many relationships.               |   |   |
|                        | Ignore()                      | Configures that the class or property should not be mapped to a table or column.                                                                                      |   |   |
|                        | OwnsOne()                     | Configures a relationship where the target entity is owned by this entity. The target entity key value is propagated from the entity it belongs to.                   |   |   |
|                        | ToTable()                     | Configures the database table that the entity maps to.                                                                                                                |   |   |
| Property Configuration | HasColumnName()               | Configures the corresponding column name in the database for the property.                                                                                            |   |   |
|                        | HasColumnType()               | Configures the data type of the corresponding column in the database for the property.                                                                                |   |   |
|                        | HasComputedColumnSql()        | Configures the property to map to computed column in the database when targeting a relational database.                                                               |   |   |
|                        | HasDefaultValue()             | Configures the default value for the column that the property maps to when targeting a relational database.                                                           |   |   |
|                        | HasDefaultValueSql()          | Configures the default value expression for the column that the property maps to when targeting relational database.                                                  |   |   |
|                        | HasField()                    | Specifies the backing field to be used with a property.                                                                                                               |   |   |
|                        | HasMaxLength()                | Configures the maximum length of data that can be stored in a property.                                                                                               |   |   |
|                        | IsConcurrencyToken()          | Configures the property to be used as an optimistic concurrency token.                                                                                                |   |   |
|                        | IsRequired()                  | Configures whether the valid value of the property is required or whether null is a valid value.                                                                      |   |   |
|                        | IsRowVersion()                | Configures the property to be used in optimistic concurrency detection.                                                                                               |   |   |
|                        | IsUnicode()                   | Configures the string property which can contain unicode characters or not.                                                                                           |   |   |
|                        | ValueGeneratedNever()         | Configures a property which cannot have a generated value when an entity is saved.                                                                                    |   |   |
|                        | ValueGeneratedOnAdd()         | Configures that the property has a generated value when saving a new entity.                                                                                          |   |   |
|                        | ValueGeneratedOnAddOrUpdate() | Configures that the property has a generated value when saving new or existing entity.                                                                                |   |   |
|                        | ValueGeneratedOnUpdate()      | Configures that a property has a generated value when saving an existing entity.       


<br/>

## **Fluent API Configurations**
* **`OnModelCreating`** 메서드를 재정의하고 ModelBuilder 유형의 매개 변수 modelBuilder를 사용하여 아래와 같이 도메인 클래스를 구성할 수 있습니다.

```csharp
public class SchoolDBContext: DbContext 
{
    public DbSet<Student> Students { get; set; }
        
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        //Write Fluent API configurations here

        //Property Configurations
        modelBuilder.Entity<Student>()
                .Property(s => s.StudentId)
                .HasColumnName("Id")
                .HasDefaultValue(0)
                .IsRequired();
    }
}
```

* 위의 예에서 ModelBuilder Fluent API 인스턴스는 체인에서 여러 메서드를 호출하여 속성을 구성하는데 사용됩니다.
* Student 엔티티의 StudentID 속성을 구성합니다.
* HasColumnName을 사용하여 이름을 구성하고 HasDefaultValue를 사용하여 기본값을 구성하고 여러 명령문이 아닌 단일 명령문에서 IsRequired 메서드를 사용하여 null 허용 여부를 구성합니다.
* 위와 같이 작성하게 되면 가독성이 향상되고 아래와 같이 여러 문과 비교하여 작성하는 데 시간이 적게 든다는 장점이 있습니다.

```csharp
//Fluent API method chained calls
modelBuilder.Entity<Student>()
        .Property(s => s.StudentId)
        .HasColumnName("Id")
        .HasDefaultValue(0)
        .IsRequired();

//Separate method calls
modelBuilder.Entity<Student>().Property(s => s.StudentId).HasColumnName("Id");
modelBuilder.Entity<Student>().Property(s => s.StudentId).HasDefaultValue(0);
modelBuilder.Entity<Student>().Property(s => s.StudentId).IsRequired();
```