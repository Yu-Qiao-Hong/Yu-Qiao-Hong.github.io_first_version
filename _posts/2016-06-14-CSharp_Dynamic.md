---
layout: post
title: "C# Dynamic"
author: "Iverson Hong"
modified: 2016-06-14
tags: [C#]
---

C# 4.0提供了dynamic寫法，讓C#有了弱類型語言的特性，在編譯時期不檢查其用法正確性，而是在Runtime時期才檢查，這也意味著編寫程式時不能使用"intellisense"功能。

## 範例1 ##

~~~csharp
dynamic str = "Iverson.Hong";
Console.WriteLine(str); // Iverson.Hong

int len = str.Length;
Console.WriteLine(len); // 12

len = str.length; // Runtime exception
Console.WriteLine(len);

string upperStr = str.ToUpper();
Console.WriteLine(upperStr); // IVERSON.HONG

upperStr = str.toUpper(); // Runtime exception
Console.WriteLine(upperStr);
~~~

在編譯時期完全沒有錯誤，但一執行則出現exception

![](..\images\postImage\CSharp_Dynamic\001.png)

使用dynamic物件時應增加判斷或以try catch來處理才是正確的寫法，

~~~csharp
dynamic str = "Iverson.Hong";
try
{
    Console.WriteLine(str); // Iverson.Hong
}
catch(Exception e)
{
    Console.WriteLine(e.Message);
}

try
{
    int len = str.Length;
    Console.WriteLine(len); // 12
}
catch (Exception e)
{
    Console.WriteLine(e.Message);
}

try
{
    int len = str.length; // Runtime exception
    Console.WriteLine(len);
}
catch (Exception e)
{
    Console.WriteLine(e.Message);
}

try
{
    string upperStr = str.ToUpper();
    Console.WriteLine(upperStr); // IVERSON.HONG
}
catch (Exception e)
{
    Console.WriteLine(e.Message);
}

try
{
    string upperStr = str.toUpper(); // Runtime exception
    Console.WriteLine(upperStr);
}
catch (Exception e)
{
    Console.WriteLine(e.Message);
}
~~~

## 範例1結果 ##

    Iverson.Hong
    12
    'string' 不包含 'length' 的定義
    IVERSON.HONG
    'string' 不包含 'toUpper' 的定義

----------

## 範例2 ##

範例2的用法比較接近實際的用法，Template class時在編譯時期無法操作實際帶入類別的Method或Property，這時可利用dynamic來操作。

~~~csharp
class A
{
    public string Name { get; set; }
    public int Age { get; set; }
}

class B
{
    public string Name { get; set; }
    public string Gender { get; set; }
}

~~~

~~~csharp
class Test<T> where T : class
{
    T _t;

    public Test(T t)
    {
        _t = t;
    }

    public void Print()
    {
        dynamic d = _t;

        PropertyInfo property = typeof(T).GetProperty("Name");
        if (property != null &&
            property.PropertyType.Name == "String")
        {
            Console.WriteLine(d.Name);
        }

        if (typeof(T).Name == "A")
        {
            if (typeof(T).GetProperty("Age") != null &&
                typeof(T).GetProperty("Age").PropertyType.Name == "Int32")
                Console.WriteLine(d.Age.ToString());
        }
        else if (typeof(T).Name == "B")
        {
            if (typeof(T).GetProperty("Gender") != null &&
                typeof(T).GetProperty("Gender").PropertyType.Name == "String")
                Console.WriteLine(d.Gender);
        }
    }
}
~~~

Client端程式：

~~~csharp
A a = new A { Name = "Iverson", Age = 30};
B b = new B { Name = "Hong", Gender = "Male"};

Test<A> test1 = new Test<A>(a);
test1.Print();

Test<B> test2 = new Test<B>(b);
test2.Print();
~~~

## 範例2結果 ##

    Iverson
    30
    Hong
    Male

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)[[Design Pattern系列文章]](http://iverson127.github.io/tags/#Design Pattern)