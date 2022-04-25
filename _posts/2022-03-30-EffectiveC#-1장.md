---
title: Effective C# 1장. 지역변수를 선언할 때는 var를 사용하는 것이 낫다.
author: BeomBeomJoJo
date: 2022-03-30 12:33:00 +0800
date: 2022-03-30 12:33:00 +0800
categories: [C#, Effective C#]
tags: [C#, Effective C#]
math: true
mermaid: true
---

{% include adsense.html %}

![1](https://user-images.githubusercontent.com/22911504/160802390-fd482e2b-172c-4db3-a5e8-de93ddf84ce0.png)

# **참조**
* [참고 사이트](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/var)

<br>

## **1. 소개**
* C# 3.0 부터 메서드 범위에서 선언된 변수에 **암시적 형식 `var`** 를 사용할 수 있습니다.
* 암시적 형식 지역 변수는 형식을 직접 선언하는 것처럼 강력한 형식이지만 **컴파일러가 형식을 결정합니다.**
* Effective C# 에서는 개발자가 코딩 할 때, 지역변수를 선언할 시 var 사용을 권장합니다.

<br>

## **2. var 사용 예제**
* var는 **암시적 타입 지역변수(implicitly typed local variable)** 입니다.
* var는 데이터 타입을 개발자가 아닌 **컴파일러(Compiler)** 가 결정합니다.
* 다음은 var 로 지역변수를 선언한 예제 코드입니다.
* 아래 예제에서 **var idx의 값은 int형 정수 10으로 설정됨으로 컴파일러에 의해 int형으로 타입이 결정됩니다.**

```c#
var idx = 10; //Implicitly typed.
int idx = 10; //Explicitly typed.
```

* 다음은 C#에서 사용하는 **var 사용 예제코드** 입니다.

```c#
using System.Linq;

namespace VarTest
{
    class Program
    {
        static void Main(string[] args)
        {
            // idx 변수 int로 컴파일 
            var idx = 5;

            // str 변수 string로 컴파일
            var str = "Hello";

            // arr 변수 int[]로 컴파일
            var arr = new[] { 0, 1, 2 };

            // wordQuery 변수 IEnumerable<Word> 혹은 IQueryable<Word> 로 컴파일
            string[] words = { "사과", "딸리", "포도", "복숭아", "바나나" };
            var wordQuery = from word in words
                            where word == "사과"
                            select word;
        }
    }
}
```

<br>

## **3. var 사용 제약 사항**
* **지역변수에만 사용 가능합니다.**

![2](https://user-images.githubusercontent.com/22911504/160802423-139269fe-b869-49b6-b887-49e6d44ed395.png)

* **변수 선언과 동시에 반드시 초기화 되어야 합니다.**

![3](https://user-images.githubusercontent.com/22911504/160802432-f1bee428-b3f3-4f90-a46e-2fb8609192f4.png)

* **null, 메서드 그룹, 익명 함수로 초기화 불가능 합니다.**

![4](https://user-images.githubusercontent.com/22911504/160802448-d35d24d0-be20-4388-9d09-65b6daf0b1f0.png)

<br>

## **4. 지역변수 사용할 때 var를 권장하는 이유**
* 개발자가 올바른 변환타입을 알지 못해 잘못된 타입을 명시적으로 지정하여 사용하는 경우를 방지합니다.
* var를 사용함으로써 코드를 간결하게 하고 가독성을 높일 수 있습니다. **[-그러나 지역변수내 var 사용이 절대적인건 아닙니다.-]**
* 내장 숫자타입(int, float, double..) 사용 시 var는 **주의** 해야 합니다.
* 내장 숫자 타입들은 다양하게 형변환이 가능합니다. 이들은 각각의 정밀도가 다르기 때문에 var를 사용할 경우 가독성과 정밀도에 있어 오류가 발생할 수 있기 때문에 지역변수라도 **숫자타입은 명시적으로 선언하는 것이 낫습니다.**

<br>

## **5. 내장 숫자 타입과 var를 함께 사용한 경우**
* 다음은 내장 숫자 타입과 var를 함께 사용한 예제 코드 입니다.

```c#
var f = GetMagicNumber();
var total = 100 * f / 6;
Console.WriteLine($"Declared Type : {total.GetType().Name}, Value : {total}");

static double GetMagicNumber()
{
    double num = 2;
    return num;
}
```

* 위의 코드에서 total은 무슨 타입일까요?
* total의 정확한 타입은 GetMagicNumber() 의 반환 타입에 의해 결정됩니다.
* GetMagicNumber()의 반환 타입을 5가지의 숫자 타입으로 바꿔가며 출력한 결과 내용입니다.

```c#
Declared Type : Double, Value : 33.333333333333336
Declared Type : Single, Value : 33.333332
Declared Type : Decimal, Value : 33.333333333333333333333333333
Declared Type : Int32, Value : 33
Declared Type : Int64, Value : 33
```

* 컴파일러는 GetMagicNumber() 메서드의 반환 타입으로 f의 타입을 결정합니다.
* total 계산 시에 사용한 상수는 모두 리터럴 이므로 컴파일러가 이 상수들을 f와 동일한 타입으로 변환한 후 계산하게 되는데 이런 이유로 결과 값에 차이가 생기게 된 것입니다.
* **[-이로 인해 숫자 타입과 var를 함께 사용하면 가독성 문제 뿐 아니라 정밀도와 관련된 혼돈스러운 문제를 유발할 가능성이 생깁니다.-]**
* **{+때문에, 숫자타입은 지역변수에서 선언하더라도 var타입이 아닌 명시적 으로 선언하는 것을 권장합니다.+}**

<br>

## **6. IEnumerable\<T>, IQueryable\<T> 반환 예제**
* 아래와 같이 **IEnumerable\<string>** 변수 q를 Linq를 이용하여 받아오는 예제 코드가 있습니다.

```c#
List<string> fruitList = new List<string> { "Apple", "Grape", "Banana", "Orange", "Mango" };

IEnumerable<string> q =
    from fruit in fruitList
    select fruit;

var query = q.Where(s => s.StartsWith("A"));
```

* 위의 코드는 **심각한 성능 문제** 를 유발할 수 있습니다.
* 결과를 받아들일 변수 q를 **IEnumerable\<string> 타입으로 명시적으로 선언** 했기 때문입니다.
* q의 원래 반환값은 **IQeuryable\<string> 타입** 을 반환하지만, 개발자가 q를 명시적으로 **IEnumerable\<string>** 으로 선언해 버렸기에 **IQueryable\<string> 과 관련된 장점을 모두 잃게 됩니다.**
* 이렇듯, 개발자가 Linq를 사용하여 해당 반환값을 정확하게 모르는 상태에서 잘못된 타입을 명시적으로 선언할 경우 **성능저하** 라는 문제를 직면하게 됩니다.
* **{+때문에 Linq 구문을 이용할때 var 변수로 반환값을 받는 것을 권장합니다.+}**

<br>

## **7. 정리**
* 지역변수에서는 var를 사용하는것이 좋습니다.
* 그러나 무분별하게 사용하는 것 보단 적당히 코드 가독성을 해치지 않는 선에서 사용하는 걸 권장합니다.
* 또한, 정확한 정밀도를 요구하는 **숫자형 타입** 들 경우에는 지역변수 선언일지라 하더라도 **명시적으로** 타입을 선언하는 것이 좋습니다.
* Linq를 사용할 때 반환값이 IEnumerable\<T>, IQueryable\<T> 인지 정확하게 모르는 경우, `var` 타입으로 Linq의 반환값을 받는것이 좋습니다.