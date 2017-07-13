---
layout: post
title: "C# Random"
author: "Iverson Hong"
modified: 2016-04-25
tags: [C#]
---

寫程式常會遇到要產生亂數，以下為C# Random用法介紹:

----------

產生10個亂數:

~~~csharp
Random r = new Random();
for (int i = 0; i < 10; i++)
{
    Console.WriteLine(r.Next());
}
~~~

結果:

    1521347103
    44592919
    819220090
    370370703
    251115350
    393567729
    385552962
    1213683341
    926826767
    1802299373

----------

產生10個10~20的整數

~~~csharp
Random r = new Random();
for (int i = 0; i < 10; i++)
{
    Console.WriteLine(r.Next(10,20));
}
~~~

結果:

    12
    11
    11
    11
    16
    12
    11
    10
    12
    17

----------

產生0.0~1.0之間的亂數:

~~~csharp
Random r = new Random();
for (int i = 0; i < 10; i++)
{
    Console.WriteLine(r.NextDouble().ToString("F2"));
}
~~~

結果:

    0.86
    0.06
    0.55
    0.12
    0.75
    0.16
    0.63
    0.55
    0.08
    0.07

----------

在Random建構子不帶參數則使用**系統時間**來當預設種子，也就是在短時間產生的亂數值會有很高的機會重覆，要避免這情況的發生可在建構子帶入種子，下面是使用GUID的HashCode來當亂數種子的寫法:

~~~csharp
Random r = new Random(Guid.NewGuid().GetHashCode());
~~~

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)