---
title: Effective C# 5장. 문화권별로 다른 문자열을 생성하려면 FormattableString을 사용하라.
author: BeomBeomJoJo
date: 2022-03-30 12:33:00 +0800
date: 2022-03-30 12:33:00 +0800
categories: [C#, Effective C#]
tags: [C#, Effective C#]
math: true
mermaid: true
---

![1](https://user-images.githubusercontent.com/22911504/160802390-fd482e2b-172c-4db3-a5e8-de93ddf84ce0.png)

## 참조
* https://docs.microsoft.com/en-us/dotnet/api/system.formattablestring?view=net-5.0
* https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/tokens/interpolated
* https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=net-5.0

<br>
<br>

## **1. FormattableString 이란?**
* C# 6.0에서 추가된 클래스 입니다.
* **문자열 보간 기능('$')** 을 이용하여 생성된 문자열은 단순 문자열일 수도 있지만, **FormattableString** 을 상속할 타입일 수도 있습니다.
* **FormattableString** 은 문자열의 연결을 돕는 기능이 있어, 문화권과 언어를 지정하여 문자열을 생성하는데 활용할 수 있습니다.
* **아래 예제에서 var로 선언하면 변수 vs는 string 객체가 될 수도 있겠지만, FormattableString을 상속한 타입의 객체가 될 수도 있습니다.**
```csharp
string s = $"It's the {DateTime.Now.Day} of the {DateTime.Now.Month} month";

var vs = $"It's the {DateTime.Now.Day} of the {DateTime.Now.Month} month"; 

FormattableString fs = $"It's the {DateTime.Now.Day} of the {DateTime.Now.Month} month";
```

<br>
<br>

## **2. FormattableString 활용**
* **FormattableString 객체** 라면 컴퓨터에서 지정된 문화권을 고려하여 문자열을 생성할 수 있습니다.
* **예를들어, 소수를 출력할 때 미국이라면 소수점 기호 '.', 유럽 대부분의 국가라면 소수점 기호 ',' 가 적용되도록 구현할 수 있습니다.**
```csharp
using System;
using System.Globalization;
 
namespace GlobalString
{
    class Program
    {
        static void Main(string[] args)
        {
            FormattableString fs = $"{Math.PI}";
            Console.WriteLine(fs);
            Console.WriteLine(ToGerman(fs));
        }
 
        public static string ToGerman(FormattableString src)
        {
            return string.Format(
                CultureInfo.CreateSpecificCulture("de-De"),
                src.Format,
                src.GetArgument(0));
        }
    }
}
```

* **실행 결과**
```
3.1415
3,1415
```

<br>
<br>

## **3. 정리**
* **문자열 보간 기능('$')** 은 글로벌화나 로컬화에 필요한 모든 기능을 갖추고 있습니다.
* 문화권을 고려하여 문자열을 생성하는 복잡한 기능을 잘 감추고 있기도 합니다.
* 문화권 지정이 필요한 경우, **FormattableString 객체를 생성하도록 코드를 작성하는 방법을 이용합니다.**