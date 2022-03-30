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

# **참조**
* https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/delegates/
* https://docs.microsoft.com/ko-kr/dotnet/api/system.func-2?view=net-5.0
* https://docs.microsoft.com/ko-kr/dotnet/api/system.action-1?view=net-5.0
* https://docs.microsoft.com/ko-kr/dotnet/api/system.predicate-1?view=net-5.0

<Br>
<Br>

# **1. 콜백(Call-Back)이란?**
```
아빠 : "찬우야, 아빠가 책 읽는 동안 마당의 잔디를 깎아주렴."
아들 : "아빠, 우선 마당 청소를 했어요."
아들 : "아빠, 잔디깎기에 기름을 넣었어요."
아들 : "아빠, 잔디깎기가 동작하지 않아요."
아빠 : "그래 내가 동작시켜볼게."
아들 : "아빠, 다 했어요."
```
* 위의 간단한 대화는 **콜백(Call-Back)** 을 설명하기 위한 대화입니다.
* 아빠는 아들에게 일을 시켰고, 아들은 수차례에 걸쳐 아빠에게 상태 보고를 합니다.
* 아빠는 상태를 확인하기 위해 잠깐씩 책을 읽는 것을 멈추긴 했지만, 책 읽기를 완전히 중단한 채로 아들이 일을 마치기를 기다리지는 않습니다.
* 아들은 중요하다고 생각하는 상태를 아빠에게 알리면서 도움을 요청하기도 했습니다.
* **콜백(Call-Back) 은 서버가 클라이언트에게 비동기적으로 피드백을 주기 위해서 주로 사용하는 방법입니다.**
* 이를 위해 멀티스레딩 기술도 사용되고, 동기적으로 상태를 갱신하는 기법도 활용됩니다.
* C# 에서 콜백은 **델리게이트(delegate)** 를 이용하여 표현됩니다.

<Br>
<Br>

# **2. 델리게이트의 다양한 활용 기법**
* 델리게이트를 이용하면 타입 안정적인 콜백을 정의할 수 있습니다. 
* 대부분의 경우 델리게이트는 event와 함께 사용되지만 **반드시 그래야 하는 것은 또 아닙니다.**
* 여러 클래스가 상호 통신을 수행해야 할 때, 클래스간의 결합도를 낮추기 위해서 인터페이스 말고 델리게이트를 사용할 수도 있습니다.
* 델리게이트는 런타임에 통지 대상을 설정할 수 있습니다.
* 하나의 델리게이트가 여러 메서드에 대한 참조가 가능하기 때문에, 다수의 클라이언트에게 통지를 보낼 수 있습니다.
* 참조 메서드는 Static 메서드일 수도 있고, 인스턴스 메서드일 수도 있습니다.

<Br>
<Br>

# **3. .NET Framework 라이브러리의 델리게이트 형태**
* .NET Framework는 **Predicate\<T>, Action<>, Func<>** 와 같은 형태로 자주 사용하는 델리게이트를 정의해 두었습니다.
  * **Predicate\<T>** : 조건을 검사하여 bool 값을 반환하는 델리게이트
  * **Func<>** : 여러 개의 매개변수를 받아 특정 타입의 단일 결과값을 반환하는 델리게이트
  * **Action<>** : 여러 개의 매개변수를 받지만 결과값의 타입이 void인 델리게이트
* LINQ는 이러한 개념을 기반으로 만들어 졌습니다.

```csharp
List<int> numbers = Enumerable.Range(1,200).ToList();
 
var oddNumbers = numbers.Find(n => n % 2 == 1);
var test = numbers.TrueForAll(n => n < 50);
 
numbers.RemoveAll(n => n % 2 == 0);
numbers.ForEach(item => Console.WriteLine(item));
```

* Find() 메서드는 **Predicate\<int>** 형식의 델리게이트를 사용해서 리스트 내에 포함된 요소에 대하여 테스트를 수행합니다.
* TrueForAll() 메서드 또한 비슷한 방법으로 동작하는데, 각 요소를 개별적으로 테스트하되 모든 항목이 테스트를 통과할 경우에만 True를 반환합니다.
* RemoveAll() 메서드는 델리게이트에서 정의한 테스트를 통과한 항목들을 리스트에서 제거합니다.
* ForEach() 메서드는 리스트 내의 각 요소에 대하여 델리게이트로 지정한 동작을 수행합니다.

<Br>
<Br>

# **4. 멀티캐스트란?**
* 하나의 델리게이트가 하나의 메서드 호출을 래핑하지 않고 두 개 이상의 메서드를 래핑하여 호출하는 것을 말합니다.
* 예를 들어, Sum 메서드를 참조하는 델리게이트가 있는데, Minus 메서드를 호출하기 위해 다른 델리게이트를 또 만들지 않고, Sum 메서드를 참조하는 델리게이트에 Minus 메서드를 추가하는 것을 의미합니다.

```csharp
using System;

namespace ConsoleApp6
{
    delegate void Del(int a, int b);

    class Program
    {
        static void Sum(int a, int b) => Console.WriteLine($"Result : {a + b}");
        static void Minus(int a, int b) => Console.WriteLine($"Result : {a - b}");

        static void Main(string[] args)
        {
            int a = 10;
            int b = 5;

            Del dele = new Del(Sum);
            dele += new Del(Minus);

            dele(a, b);
        }
    }
}
```

<Br>
<Br>

# **5. 델리게이트 멀티캐스트**
* 위에서 멀티캐스트의 개념을 알아 보았습니다.
* 모든 델리게이트는 기본적으로 멀티캐스트가 가능합니다.
* 일반적으로 동일한 타입의 매개변수를 취하더라도 **반환 타입이 다른 경우 서로 다른 델리게이트 타입으로 간주하며** , 컴파일러는 이 둘 사이의 형변환을 허용하지 않습니다.
* 멀티개스트 델리게이트는 한 번만 호출하면, 델리게이트 객체에 추가된 모든 대상 함수가 호출됩니다.
* 하지만 이러한 구조에는 두 가지 주의해야 할 부분이 있습니다.

## **5.1. 주의 1 : 예외 안전성이 좋지 않다.**
* 멀티캐스트 델리게이트의 내부 동작 방식은 대상 함수들을 연속적으로 호출하는 형태로 구현됩니다.
* **[-델리게이트는 어떤 예외도 잡지 않으며, 따라서 예외가 발생하면 함수 호출 과정이 중단됩니다.-]**

## **5.2. 주의 2 : 마지막 호출 대상 함수의 반환값만이 델리게이트의 반환값이다.**

```csharp
List<ComplicatedClass> container = new List<ComplicatedClass>();

public void LengthOperation(Func<bool> pred)
{
    foreach(ComplicatedClass cl in container)
    {
        cl.DoLengthyOperation();

        //사용자가 임의로 중단을 요청했는지 확인
        if(false == pred())
            return;
    }
}
```

* 위의 LengthOperation 메서드를 멀티캐스트 델리게이트로 사용하면 문제가 발생합니다.

```csharp
Func<bool> cp = () => CheckWithUser();
cp += () => CheckWithSystem();
c.LengthOperation(cp);
```
* 델리게이트의 반환값은 체인 마지막으로 호출된 함수의 반환값이 되며, 다른 반환값은 모두 무시됩니다.
* 따라서 위의 예제의 경우 CheckWithUser() 의 반환값은 무시됩니다.
* 이러한 문제를 해결하려면, 델리게이트에 포함된 호출 대상 콜백 함수를 직접 다뤄야 합니다.

```csharp
public void LengthOperation2(Func<bool> pred)
{
    bool bContinue = true;
    
    foreach(ComplicatedClass cl in bContinue)
    {
        cl.DoLengthyOperation();
        foreach (Func<bool> pr in pred.GetInvocationList())
            bContinue &= pr();
        if (!bContinue)
            return;
    }
}
```

* 위처럼 코드를 작성하면 델리게이트에 추가된 개별 메서드가 true를 반환하는 경우에만 다음 메서드에 대한 호출을 이어갈 수 있습니다.

<Br>
<Br>

# **6. 정리**
* 델리게이트는 런타임에 콜백을 구성하는 최고의 방법입니다.
* 델리게이트를 사용하면 콜백을 사용해야 하는 클라이언트를 더욱 단순하게 구성할 수 있을 뿐 아니라 런타임에 콜백함수를 구성할 수 있습니다.
* 게다가 하나의 델리게이트에 여러 개의 콜백 함수를 추가할 수도 있습니다.
* .NET 환경에서 콜백이 필요한 경우에는 반드시 델리게이트를 사용합시다.

<Br>
<Br>
