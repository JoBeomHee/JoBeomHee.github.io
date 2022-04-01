---
title: 헤드퍼스트 디자인패턴 - 오리행동 캡슐화 작업
author: BeomBeomJoJo
date: 2022-04-01 12:33:00 +0800
date: 2022-04-01 12:33:00 +0800
categories: [C#, DesignPattern]
tags: [C#, DesignPattern]
math: true
mermaid: true
---

## **소개**
* 안녕하세요. 요새 설계에 대해서 관심을 갖고, 개발자에게 설계는 선택이 아닌 필수라는 사실을 알게 되어서 **HeadFirst 디자인패턴** 책을 구입하여 틈틈이 학습을 하고 있습니다.
* 학습하여 정리한 코드를 정리합니다.

<br/>

## **시나리오**
* 오리에는 여러가지 종류의 오리들이 있습니다.
* 천둥오리, 원앙, 점무늬오리 등등 실제 살아있는 동물들에서도 종류가 다양합니다.
* 또한, 오리라고 해서 살아있는 동물이 아닌 인형이나 고무오리 등의 물체들도 존재합니다.
* 오리는 "꽥꽥" 이라고 소리를 내고, 오리 중에서는 하늘을 날 수 있는 오리와 날지 못하는 오리들이 있습니다.
* 위 내용을 토대로, 책에서 제시하는 방향으로 Csharp 코드로 작성해서 프로젝트 구성을 최대한 유연하게 구현해 보았습니다.

<br/>

## **인터페이스**
* 인터페이스에는 크게 IFlyBehavior.cs, IQuackBehavior.cs 2개의 인터페이스가 있습니다.
* 각 인터페이스의 역할은 하늘은 나는 행동의 인터페이스와, 꽥꽥 소리내는 행동의 인터페이스 입니다.

### **IFlyBehavior.cs**

```csharp
namespace ConsoleApp1
{
    public interface IFlyBehavior
    {
        public void Fly();
    }
}
```

<br/>

### **IQuackBehavior.cs**

```csharp
namespace ConsoleApp1
{
    public interface IQuackBehavior
    {
        public void Quack();
    }
}
```

## **하늘을 나는 행동 구분**
* 다음으로는 하늘을 나는 행위를 구분하는 부분입니다.
* 여기에는 FlyNoWay.cs, FlyWithWing.cs 2개의 클래스를 선언했습니다.

### **FlyNoWay.cs**
```csharp
namespace ConsoleApp1
{
    internal class FlyNoWay : IFlyBehavior
    {
        public void Fly()
        {
            Console.WriteLine("저는 못 날아요.");
        }
    }
}
```

<br/>

### **FlyWithWing.cs**

```csharp
namespace ConsoleApp1
{
    internal class FlyWithWings : IFlyBehavior
    {
        public void Fly()
        {
            Console.WriteLine($"날고 있어요!");
        }
    }
}
```

## **꽥꽥 소리 내는 행동 구분**
* 다음으로 꽥꽥 소리는 내는 행위를 구분하는 부분입니다.
* 여기에는 MuteQuack.cs, Quack.cs 2개의 클래스가 있습니다.

### **MuteQuack.cs**

```csharp
namespace ConsoleApp1
{
    internal class MuteQuack : IQuackBehavior
    {
        public void Quack()
        {
            Console.WriteLine("조용");
        }
    }
}
```

<br/>

### **Quack.cs**

```csharp
namespace ConsoleApp1
{
    internal class Quack : IQuackBehavior
    {
        void IQuackBehavior.Quack()
        {
            Console.WriteLine("꽥");
        }
    }
}
```

<br/>

## **상위 클래스인 Duck 클래스 선언**
* 위에서 인터페이스 및 행위 관련 구현을 마쳤습니다.
* 이제 상위 클래스인 Duck.cs 클래스를 구현하도록 하겠습니다.
* 상위 클래스인 Duck 클래스 안에는 앞에서 선언했던 IFlyBehavior, IQuackBehavior 2개의 인터페이스 변수를 선언합니다.
* 그리고 각각의 행위인 Fly(), Quack() 메서드를 미리 정의해 놓습니다.
* 이렇게 하게되면, 어떤 오리 던지 하늘을 날거나 못날 수도 있고, 꽥꽥 소리를 내거나 안낼 수도 있는데 이런 상황에 보다 유연하게 대처할 수 있습니다.

```csharp
namespace ConsoleApp1
{
    abstract public class Duck
    {
        public IFlyBehavior flyBehavior;
        public IQuackBehavior quackBehavior;

        public Duck() { }

        public abstract void Display();

        public void PerformFly()
        {
            flyBehavior.Fly();
        }

        public void PerformQuack()
        {
            quackBehavior.Quack();
        }

        public void Swim()
        {
            Console.WriteLine($"모든 오리는 물에 뜹니다. 가짜 오리도 뜨죠");
        }
    }
}
```

<br/>

## **MiniDuck 구현**
* 실제로 그럼 오리 종류 중 하나인 MallardDuck 클래스를 선언하고 MallardDuck을 직접 구현해 보도록 하겠습니다.
* 다음과 같이 MallardDuck 클래스는 Duck 상위 클래스를 상속받고 있습니다.
* 때문에 Duck 에 있는 모든 속성을 사용할 수 있습니다.
* 여기서 MallardDuck은 하늘을 날 수 있고, 꽥꽥 소리를 낼 수 있기 때문에 MallardDuck() 생성자 안에 각각 Quack, FlyWithWings 행위를 선언해 줍니다.
* 마지막으로 mallard 객체에서 PerformFly(), PerformQuack() 메서드를 실행하여 어떤 행동들이 실행되는지 확인합니다.

```csharp
using ConsoleApp1;

Duck mallard = new MallardDuck();

mallard.Display();
mallard.PerformFly();
mallard.PerformQuack();

public class MallardDuck : Duck
{
    public MallardDuck()
    {
        quackBehavior = new Quack();
        flyBehavior = new FlyWithWings();
    }

    public override void Display()
    {
        Console.WriteLine("나는 MallardDuck 입니다.");
    }
}
```

<br/>

## **실행 결과**
* 실행 결과, MallardDuck은 하늘을 날 수도 있고, 꽥꽥 소리도 냅니다.
* 만약, 여기서 RubberDuck(고무오리) 클래스를 새롭게 추가한다면, RubberDuck 생성자에서 MuteQuack, FlyNoWay 행위를 선언합니다.
* 그럼 **나는 못 날아요. 조용** 이라는 문구가 출력됩니다.

```console
나는 MallardDuck 입니다.
날고 있어요!
꽥
```