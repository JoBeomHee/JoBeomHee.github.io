---
title: C# 코딩 규칙(C# 프로그래밍 가이드)
author: BeomBeomJoJo
date: 2022-03-25 13:33:00 +0800
categories: [C#]
tags: [C#]
math: true
mermaid: true
---

# MSDN | C# 코딩 규칙(C# 프로그래밍 가이드)

<br/>

# **참조**
* https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/inside-a-program/coding-conventions

<br/>

# **명명 규칙**
* **using 지시문이 포함되지 않는 간단한 예제** 에서 **네임스페이스 한정자** 를 사용합니다.
* 프로젝트에서 네임스페이스를 기본적으로 가져오는 경우에는 해당 네임스페이스의 이름을 정규화하지 않아도 됩니다.
* 정규화된 이름은 한 줄에 표시하기가 너무 길면 다음 예제에 나와 있는 것 처럼 점(.) 으로 분할할 수 있습니다.
```csharp
var currentPerformanceCounterCategory = new System.Diagnostics.
                                            PerformanceCounterCategory();
```

<br/>

# **레이아웃 규칙**
* 기본 코드 편집기 설정(스마트 들여쓰기, 4자 들여쓰기, 탭을 공백으로 저장) 을 사용합니다.
* **문을 한 줄에 하나씩만** 작성합니다.
* **선언을 한 줄에 하나씩만** 작성합니다.
* 연속 줄이 자동으로 들여쓰기 되지 않으면 **탭 정지 하나(공백4개) 만큼** 들여 씁니다.
* **메서드 정의와 속성 정의 간에는 빈 줄을 하나 이상** 추가합니다.
* 다음 코드에 나와 있는 것 처럼 **괄호** 를 사용하여 식의 절을 명확하게 구분합니다.
```csharp
if((val1 > val2) && (val1 > vale))
{
    // Take appropriate action.
}
```

<br/>

# **주석 규칙**
* 코드 줄의 끝이 아닌 **별도의 줄** 에 주석을 배치합니다.
* 주석 텍스트는 **대문자로 시작** 합니다.
* 주석 텍스트 끝에는 **마침표** 를 붙입니다.
* 다음 코드에 나와 있는 것처럼 주석 구분 기호(//)와 주석 텍스트 사이에 **공백을 하나 삽입** 합니다.
```csharp
// The following declaration creates a query. It does not run
// the query.
```

<br/>

# **문자열 데이터 형식**
* **문자열 보간** 을 사용하여 짧은 문자열을 연결합니다.
```csharp
string displayName = $"{nameList[n].Lastname}, {nameList[n].FirstName}";
```
* 특히 많은 양의 텍스트를 사용할 때, 문자열을 루프에 추가하려면 **StringBuilder 객체** 를 사용합니다.
```csharp
var phrase = "lalalalalalalalalalalalalalalalalalalalalalalalalalalalalala";
var manyPhrases = new StringBuilder();
for(var index = 0; index < 10000; index++)
{
    manyPhrases.Append(phrase);
}
```

<br/>

# **암시적으로 형식화한 지역 변수**
* 오른쪽에서 할당 된 변수 형식이 명확하거나 정확한 형식이 중요하지 않으면 **[+지역 변수에 대해 암시적 형식+]** 을 사용
```csharp
// When the type of a variable is clear from the context, use var
// in the declaration.
var var1 = "This is clearly a string.";
var var2 = 27;
```
* 오른쪽에서 할당 된 변수 형식이 명확하지 않으면 **[-var를 사용하지 않습니다.-]**
```csharp
// When the type of a variable is not clear from the context, use an
// explicit type. You generally don't assume the type clear from a method name.
// A variable type is considered clear if it's a new operator or an explicit cast.
int var3 = Convert.ToInt32(Console.ReadLine());
int var4 = ExampleClass.ResultSoFar();
```
* **dynamic** 대신 `var`를 사용하지 않습니다.
* for루프의 루프 변수 형식을 결정하려면 **암시적 형식** 을 사용합니다.
```csharp
var phrase = "lalalalalalalalalalalalalalalalalalalalalalalalalalalalalala";
var manyPhrases = new StringBuilder();
for(var index = 0; index < 10000; index++)
{
    manyPhrases.Append(phrase);
}
```
* **foreach** 루프의 루프 변수 형식을 결정하기 위해 **[-암시적 형식을 사용하지 않습니다.-]**
* 다음 예제에서는 `foreach` 문에서 명시적 형식을 사용합니다.
```csharp
foreach(char ch in laugh)
{
    if(ch == 'h')
        Console.Write("H");
    else
        Console.Write(ch);
}
Console.WriteLine();
```

> **NOTE :** 반복 가능한 컬렉션의 요소 형식을 실수로 변경하지 않도록 주의해야 합니다. 예를 들어 `foreach` 문에서 System.Linq.IQueryable을 System.Collections.IEnumerable 으로 전환하기 쉬운데 그러면 쿼리 실행이 변경됩니다.

<br/>

# **배열**
* 선언 줄에서 배열을 초기화할 때는 **간결한 구문** 을 사용합니다.
```csharp
// Preferred syntax. Note that you cannot use var here instead of string[].
string[] vowels1 = { "a", "e", "i", "o", "u" };

// If you use explicit instantiation, you can use var.
var vowels2 = new string[] { "a", "e", "i", "o", "u"};

// If you specify an array size, you muse initialize the elenents one at a time.
var vowels3 = new string[5];
vowels3[0] = "a";
voewls3[1] = "e";
```

<br/>

# **대리자**
* 대리자 형식의 인스턴스를 만들려면 **간결한 구문** 을 사용합니다.
```csharp
// First, in class Program, define the delegate type and a method that
// has a matching signature.

// Define the type.
public delegate void Del(string message);

//Define a method that has a matching signature.
public static void DelMethod(string str)
{
    Console.WriteLine($"DelMethod argument: {str}");
}
```

```csharp
// In the Main method, create an instance of Del.

// Preferred: Create an instance of Del by using condensed syntax.
Del exampleDel2 = Delmethod;

// The following declaration uses full syntax.
Del exampleDel1 = new Del(DelMethod);
```

# **예외 처리의 try-catch 및 using 문**
* 대부분의 예외 처리는 **try-catch** 문을 사용합니다.
```csharp
static string GetValueFromArray(string[] array, int index)
{
    try
    {
        return array[index];
    }
    catch(System.IndexoutOfRangeException ex)
    {
        Console.WriteLine($"Index is out of range : {index}");
        throw;
    }
}
```
* C# using 문을 사용하면 코드를 간소화 할 수 있습니다.
* `finally` 블록의 코드가 Dispose 메서드 호출뿐인 try-finally 문이 있는 경우에는 **using** 문을 대신 사용합니다.

```csharp
// This tye-finally statement only calls Dispose in the finally block.
Font font1 = new Font("Arial", 10.0f);
try
{
    byte charset = font1.GdiCharSet;
}
finally
{
    if(font1 != null)
    {
        ((IDisposable)font1).Dispose();
    }
}

//You can do the same thing with a using statement.
using (Font font2 = new Font("Arial", 10.0f))
{
    byte charset = font2.GdiCharSet;
}
```

<br/>

# **&& 및 || 연산자**
* 예외를 방지하고 불필요한 비교를 건너뛰어 성능을 개선하려면 **& 대신 &&를 사용하고 | 대신 ||를 사용** 합니다.
```csharp
Console.Write("Enter a dividend: ");
var divident = Convert.ToInt32(Console.ReadLine());

Console.Write("Enter a divisor: ");
var divisor = Convert.ToInt32(Console.ReadLine());

// If the divisor is 0, the second clause in the following condition
// causes a run-time error. The && operator short circuits when the
// first expression is false. That is, it does not evaluate the
// second expression. The & operator evaluates both, and causes
// a run-time error when divisor is 0.
if ((divisor != 0) && (dividend / divisor > 0))
{
    Console.WriteLine($"Quotient: {dividend / divisor}");
}
else
{
    Console.WriteLine("Attempted division by 0 ends up here.");
}
```

<br/>

# **New 연산자**
* 암시적 형식이 포함된 간결한 형태의 개체 인스턴스화를 사용합니다.
```csharp
var instance1 = new ExampleClass();
```
* 위의 줄은 다음 선언과 동일합니다.
```csharp
ExampleClass instance2 = new ExampleClass();
```
* 객체를 간편하게 만들려면 **객체 이니셜라이저** 를 사용합니다.
```csharp
// Object initializer.
var instance3 = new ExampleClass { Name = "Desktop", ID = 123, Location = "Seoul", Age = 29 };

//Default constructor and assignment statements.
var instance4 = new ExampleClass();
instance4.Name = "Desktop";
instance4.ID = 123;
instance4.Location = "Seoul";
instance4.Age = 29;
```

<br/>

# **이벤트 처리**
* 나중에 제거할 필요가 없는 이벤트 처리기를 정의하는 경우 **람다 식** 을 사용합니다.
```csharp
public Form2()
{
    // You can use a lambda expression to define an event handler.
    this.Click += (s, e) =>
        {
            MessageBox.Show((MouseEventArge)e).Location.ToString();
        }
}
```
```csharp
// Using a lambda expression shortens the following traditional definition.
public Form1()
{
    this.Click += new EventHandler(Form1_Click);
}
void Form1_Click(object sender, EventArgs e)
{
    MessageBox.Show(((MouseEventArgs)e).Location.ToString());
}
```

<br/>

# **LINQ 쿼리**
* 쿼리 변수에 **의미 있는** 이름을 사용합니다.
```csharp
var seoulCustomers = from customer in customers
                     where customer.City == "Seoul"
                     select customer.name;
```
* 별칭을 사용하여 익명 형식의 **속성 이름 대/소문자를** 올바르게 표시합니다.(파스칼식 대/소문자 사용)
```csharp
var localDistributors =
    from customer in customers
    join distributor in distributors on customer.City equals distributor.City
    select new { Customer = customer, Distributor = distributor };
```
* 결과의 속성 이름이 모호하면 속성 이름을 바꿉니다.
* 예를 들어 쿼리에서 고객 이름과 배포자 ID를 반환하는 경우 결과에서 이러한 정보를 `Name` 및 `ID` 로 유지하는 대신 `Name`은 고객의 이름이고, `ID`는 배포자의 ID임을 **명확하게 나타내도록 이름을 바꿉니다.**
```csharp
var localDistributors2 =
    from customer in customers
    join distributor in distributors on customer.City equals distributor.City
    select new { CustomerName = customer.Name, DistributorID = distributor.ID };
```
* 쿼리 변수 및 범위 변수의 선언에서 **암시적 형식** 을 사용합니다.
```csharp
var seoulCustomers = from customer in customers
                     where customer.City == "Seoul"
                     select customer.Name;
```
* 내부 컬렉션에 액세스 하려면 **join** 절 대신 여러 `from` 절을 사용합니다.
```csharp
// Use a compound from to access the inner sequence within each elemnet.
var scoreQuery = from student in students
                 from score in student.Scores
                 where score > 90
                 select new { Last = student.Lastname, score };
```