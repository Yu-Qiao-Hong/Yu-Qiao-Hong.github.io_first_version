---
layout: post
title: "C# Template Class"
author: "Iverson Hong"
modified: 2016-06-10
tags: [C#]
---

假設class內有一"Data" property，其資料型態為int

~~~csharp
class TestInt
{
    public int Data { get; set; }
}
~~~

用戶端呼叫時：

~~~csharp
var testInt = new TestInt { Data = 3 };
Console.WriteLine(testInt.Data); // 3
~~~

若支援更多的資料型態，例如string, bool，則須定義出下列class

~~~csharp
class TestString
{
    public string Data { get; set; }
}

class TestBool
{
    public bool Data { get; set; }
}
~~~

呼叫端也要跟著更改：

~~~csharp
var testString = new TestString { Data = "Iverson.Hong" };
Console.WriteLine(testString.Data); // Iverson.Hong

var testBool = new TestBool { Data = true };
Console.WriteLine(testBool.Data); // True
~~~

----------

可以發現只是"Data"資料型態不同，卻因三個不同資料型態寫出三個class，這時可用到泛型(template)來改善。

~~~csharp
class Tset<T>
{
    public T Data { get; set; }
}
~~~

用戶端寫法：

~~~csharp
var testInt = new Tset<int> { Data = 3 };
Console.WriteLine(testInt.Data); // 3

var testString = new Tset<string> { Data = "Iverson.Hong" };
Console.WriteLine(testString.Data); // Iverson.Hong

var testBool = new Tset<bool> { Data = true };
Console.WriteLine(testBool.Data); // True
~~~

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)