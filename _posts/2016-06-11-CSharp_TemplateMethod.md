---
layout: post
title: "C# Template Method"
author: "Iverson Hong"
modified: 2016-06-11
tags: [C#]
---

接續前一篇[Template Class](http://iverson127.github.io/CSharp_TemplateClass/)，其泛型由class帶入，在使用method時也可以使用泛型

底下為泛型method範例：

~~~csharp
static void Swap<T>(ref T t1, ref T t2)
{
    T temp = t1;
    t1 = t2;
    t2 = temp;
}

static void Print<T>(T t1, T t2)
{
    Console.WriteLine("t1 = {0}, t2 = {1}", t1, t2);
}
~~~

用戶端呼叫時：

~~~csharp
static void Main(string[] args)
{
    int a = 1;
    int b = 2;

    Print(a, b);
    Swap(ref a, ref b);
    Print(a, b);

    string x = "Iverson";
    string y = "Hong";

    Print(x, y);
    Swap(ref x, ref y);
    Print(x, y);
}
~~~

結果：

    t1 = 1, t2 = 2
    t1 = 2, t2 = 1
    t1 = Iverson, t2 = Hong
    t1 = Hong, t2 = Iverson
    
----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)