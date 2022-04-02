---
title: Effective C# 3장. 캐스트보다는 is,as가 좋다.
author: BeomBeomJoJo
date: 2022-03-30 12:33:00 +0800
date: 2022-03-30 12:33:00 +0800
categories: [C#, Effective C#]
tags: [C#, Effective C#]
math: true
mermaid: true
---

![1](https://user-images.githubusercontent.com/22911504/160802390-fd482e2b-172c-4db3-a5e8-de93ddf84ce0.png)

# **참조**
* [참고 사이트](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/user-defined-conversion-operators)
* [참고 사이트](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/language-specification/conversions#implicit-conversions)

<br>
<br>

# **1. C# 에서의 형변환**
* C# 에서 형변환을 수행하는 방법에는 **is/as 연산자** 를 사용하는 방법과 컴파일러의 **cast 연산자** 구문을 사용하는 **두 가지 방법** 이 있습니다.
* 안정적인 코드를 작성하려는 경우, 우선 is 연산자로 형변환 유무 확인 후 실제 형변환을 수행하도록 코드를 작성할 수도 있습니다.

## **1.1. as 형변환 예제**

```csharp
object o = Factory.GetObject(); 

// as 변환 
MyType t = o as MyType; 

if (t != null) 
{ 
    //MyType 타입의 t 객체 사용 
} 
else
{ 
    //형변환 실패 시 
}
```

## **1.2. cast 연산자 예제**

```csharp
object o = Factory.GetObject(); 

// cast 변환 
try 
{ 
    MyType t; 
    t = (MyType) o; 
    //MyType 타입의 t 객체 사용 
} 
catch (InvalidCastException) 
{ 
    //형변환 실패 오류 
}
```

* **as 연산자** 를 사용할 경우, 강제 형변환 사용하는 것 보다 사용 방법도 쉽고 가독성도 좋습니다.
* 또한, try/catch 문을 사용하지 않아도 되기 때문에 성능도 강제 형 변환보다 더 좋습니다.

<br>
<br>

# **2. as 연산자와 캐스팅의 가장 큰 차이 - '사용자 정의 형 변환을 어떻게 다루는가'**
## **2.1. is/as 연산자**
* **as 연산자** 는 런타임에 객체의 타입을 확인하고 필요에 따라 박싱을 수행하는 것을 제외하고는 어떤 작업도 수행하지 않습니다.
* 객체를 다른 타입으로 형 변환 하려면 이 객체는 지정한 타입이거나 혹은 지정한 타입을 상속한 타입이어야 합니다.
* 그 외의 경우는 **모두 실패** 합니다.

## **2.2. cast 연산자**
* **cast 연산자** 는 타입 변환 시 **형 변환 연산자** 가 개입합니다.
* 대표적인 **cast 연산자** 는 숫자 타입에 대한 cast 연산자가 있습니다.
* long 타입을 short 타입으로 cast하면 일부 정보를 잃을 수도 있는데, 이러한 변환이 **cast 연산자** 에서 발생할 수 있는 **문제** 입니다.

## **2.3. 사용자 정의 연산자(user-defined type)의 cast가 실패하는 경우**
* 다음 예제는 SecondType에서 implicit으로 사용자 정의 연산자를 정의한 예제입니다.

```csharp
public class SecondType
{
   private MyType _value;
 
   public static implicit operator MyType(SecondType t)
   {
      return t._value;
   }
}
 
object o = Factory.GetObject();
// o is a SecondType:
 
MyType t = o as MyType; // Fails. o is not MyType
 
if (t != null)
{
    // work with t, it's a MyType.
}
else
{
   // report the failure.
}
 
// Version two:
try
{
    MyType t1;
    t1 = (MyType)o; // Fails. o is not MyType
    // work with t1, it's a MyType.
}
catch (InvalidCastException)
{
   // report the conversion failure.
}
```

* 위의 형 변환은 **둘 다 실패합니다.**
* **첫 번째 버전** 에서 as는 런타임의 타입으로 결정이 되므로, 런타임에 o가 SecondType이므로 MyType 또는 MyType에서 파생된 클래스가 아니므로 실패하게 됩니다.
* 사용자 정의 연산자가 존재 하기는 하지만, **as 연산자는 사용자 정의 연산자를 허용하지 않기 때문에 실패합니다.**
* **두 번째 버전** 은 o가 SecondType이므로 사용자 정의 연산자에 의해 MyType으로 변환이 가능한 것 처럼 보입니다.
* 하지만, 위의 방법도 실패합니다. 
* cast 연산자는 컴파일 타임에 코드를 생성하기 때문에, 변환 시도시 코드상에 있는 Object Type 에서 MyType으로 변환을 시도합니다.
* Object에서는 MyType으로 변환하는 사용자 정의 연산자가 정의 되지 않기 때문에 실패합니다.
* 컴파일 타임에 실패하게 되면, 컴파일러는 런타임의 o가 MyType인지만 검사하는 코드를 생성합니다.
* 따라서 런타임에는 o가 SecondType 이므로 SecondType은 MyType이 아니기 때문에 변환 실패합니다.

<br>
<br>

# **3. 정리**
* **형 변환 시 is/as 연산자를 사용하는 것이 더 안정적이고 런타임에 효율적입니다.**
* is/as 연산자를 사용하여 try/catch 문을 없앨 수 있어 코드가 간결하며 부하가 줄어듭니다.
* value type 변환은 cast 연산자가 필요합니다.
* **사용자 정의 변환(user-defined operator)은 cast 연산자만 사용 가능합니다.**
* cast 연산자는 컴파일 타임 코드를 생성하기 때문에 컴파일 시점에 선언된 타입으로 결정합니다. 런타임 시점의 타입은 모릅니다.