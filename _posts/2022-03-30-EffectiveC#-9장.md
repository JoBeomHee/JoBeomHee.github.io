---
title: Effective C# 7장. 델리게이트를 이용하여 콜백을 표현하라.
author: BeomBeomJoJo
date: 2022-03-30 12:33:00 +0800
date: 2022-03-30 12:33:00 +0800
categories: [C#, Effective C#]
tags: [C#, Effective C#]
math: true
mermaid: true
---

![1](https://user-images.githubusercontent.com/22911504/160802390-fd482e2b-172c-4db3-a5e8-de93ddf84ce0.png)

# **1. 참조**
* https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/reference-types
* https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/types/boxing-and-unboxing

<br>
<br>

# **2. 소개**
* Value 타입은 주로 값을 저장할 때 쓰는 저장소이며 다형적이지 못합니다.
* System.Object는 .NET 프레임워크에서 모든 타입의 최상위 타입으로 정의하고 있는데 언뜻 보면 Value 타입과 System.Object는 양립하지 못하는 것처럼 보입니다.
* 하지만 .NET 프레임워크는 **박싱(Boxing)** 과 **언박싱(UnBoxing)** 이라는 방법을 통해서 두 타입을 서로 변환하게 해줍니다.

<br>
<br>

# **3. 값 타입(Value Type)**
* **값 타입(Value Type) 은 스택영역** 에 저장합니다. (int, char, double...)
* 값 타입은 Object를 상속받은 System.ValueType을 상속받은 구조체입니다. (System.ValueType)
* int -> System.Int32로 정의. 상속관계는 Object -> ValueType -> Int32
* char -> System.Char로 정의. 상속관계는 Object -> ValueType -> Char

<br>
<br>

# **4. 참조 타입(Reference Type)**
* 모든 타입의 base class인 System.Object를 상속받으며, 힙 영역에 저장됩니다.
* 값 형식은 해당 데이터에 직접 값이 저장된다면, 참조 타입은 데이터에 대한 참조가 저장됩니다.
* 참조 타입은 처음 변수를 선언하면 값 타입과 달리 메모리가 생성되지 않습니다.
  
```csharp
Class A
{

}
A a = null; // 메모리 생성되지 않음.
```

* 이후 A를 생성한다면 실제 생성된 A는 힙에 메모리를 할당, a는 그 값에 대한 참조만 스택에 저장합니다.

```csharp
A a = new A(); // 메모리 생성, 변수 a는 생성된 A의 참조(주소)만 갖게 됨.
```

* a를 새로운 aa에 할당한다면 새로운 메모리를 할당하지 않고 참조하는 값만 복사

```csharp
A aa = a; // 새로운 메모리를 할당하지 않고, a의 참조 값을 갖는 값만 복사함.
```
<br>
<br>

# **5. 박싱(Boxing)**
* 값 타입(Value Type)의 객체를 참조 타입(Reference Type)로 변환하는 작업을 말합니다.

```csharp
int i = 123;
object o = i; // 박싱
Console.WriteLine(o.ToString());
```

![2](https://user-images.githubusercontent.com/22911504/160804092-88a5df49-4402-4504-aff4-318c8069f3e9.png)

* 단순한 형변환 같지만 값 타입은 스택에 저장되어 있고 참조 타입은 힙에 저장되어 있습니다.
* 그래서 위 과정을 수행하기 위해선 스택에 저장된 값 타입을 힙 타입으로 복사가 한번 일어납니다.
* 그리고 힙에 복사된 이 영역을 참조 타입이 가리키게 되는 일을 수행합니다.

<br>
<br>

# **6. 언박싱(UnBoxing)**
* 참조 타입을 값 타입으로 변환하는 작업을 말합니다.
  
```csharp
int i = 123; // a value type
object o = o; // boxing
int j = (int)o; // unboxing
```

![3](https://user-images.githubusercontent.com/22911504/160804100-702bd68a-3b56-479e-9ec7-76eb7541a736.png)

* 박싱과 반대로, 힙에 있던 데이터를 다시 스택으로 복사가 일어납니다.
* 박싱과 언박싱은 System.Object 타입이나 인터페이스 타입이 필요한 부분에 Value 타입의 객체를 적용하기 위해서 필요한 기능이지만 **가능하면 쓰지 않는 것이 좋습니다.**

<br>
<br>

# **7. 박싱과 언박싱을 최소화하는 이유**
* 우선 첫 번째로, 성능에 안좋은 영향을 줍니다.
* 다소 억지로 변환해 주는 방법이다 보니, 예상치 못한 곳에서 버그가 발샐할 수 있습니다.

<br>
<br>

# **8. 박싱과 언박싱을 최소화 하는 방법**
* 대부분의 경우에는 .NET 2.0에 추가된 **제네릭(Generic)** 클래스와 제네릭 메서드를 사용하면 박싱과 언박싱을 피할 수 있습니다.
* 하지만, .NET 프레임워크 곳곳에 System.Object 타입의 객체를 요구하는 API들이 많고 여전히 박싱과 언박싱을 많이 수행합니다.
* 이러한 박싱과 언박싱은 모두 자동으로 이루어지고 reference 타입의 객체가 필요한 곳에 value 타입의 객체를 사용하면 컴퍼일러는 자동으로 박싱과 언박싱을 수행합니다.
* 아래 코드가 그 예입니다.
* firstNumber, secondNumber, thirdNumber는 int 타입이지만 보간 문자열에 대입되면서 System.Object 타입으로 변환하면서 Boxing을 수행하게 됩니다.
* 그리고 보간 문자열의 string화 되기 위해 자동으로 **ToString()** 메서드를 수행합니다.

```csharp
Console.WriteLine($"A few numbers:{firstNumber}, {secondNumber}, {thirdNumber}");
```

* 이러한 방법은 성능에 좋지 않은 영향을 주게 됩니다.
* 이런 성능상의 취약점을 개선하고 싶다면 WritrLine 메서드에 Value 타입의 객체를 직접 전달하지 말고 string 인스턴스를 전달하는 것이 좋습니다.
* 아래 코드가 그 예입니다.

```csharp
Console.WriteLine($"A few numbers:{firstNumber.ToString()}, 
             {secondNumber.ToString()}, {thirdNumber.ToString()}");
```

<br>
<br>

# **9. 박싱을 피하는 규칙**
* 위의 코드가 박싱을 피하는 첫 번째 규칙으로 **System.Object** 타입으로 박싱이 이루어지는지 유심히 살펴보고 이를 개선하라 입니다.
* 그리고 비교적 손쉬운 또 다른 규칙은 **가능한 한 .NET 2.0 이전의 컬렉션 사용을 피하고 .NET 2.0에 추가된 제네릭 컬렉션을 사용하라** 입니다.
* .NET 2.0 이전의 컬렉션은 System.Object 타입의 객체에 대한 참조를 저장하도록 구현되어 있기 때문에 컬렉션에 Value 타입의 객체를 추가하려는 경우 자동으로 박싱을 수행합니다.

<br>
<br>

# **10. 정리**
* Value 타입과 System.Object 타입(혹은 다른 인터페이스 타입) 간의 변환이 가능한 박싱과 언박싱이라는 기능이 있습니다.
* 하지만, 이 기능은 암시적으로 이루어져 변환 작업을 프로그래머가 알기 어렵고, 이로 인하여 알 수 없는 버그들이 발생할 수 있습니다.
* 또한 Value 타입을 다형적으로 처리하는 과정에서 성능 이슈 문제를 발생시킵니다.
* 따라서, Value 타입을 System.Object 타입이나 인터페이스 타입으로 변경하는 코드는 가능한 작성하지 않는 것이 좋습니다.

<br>
<br>
