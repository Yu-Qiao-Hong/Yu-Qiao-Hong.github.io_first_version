---
layout: post
title: "C# Builder Design Pattern"
author: "Iverson Hong"
modified: 2016-04-23
tags: [C#, Design Pattern]
---

## 生產線 ##

某工廠有兩條生產線生產兩樣產品，此兩樣產品生產需經三個步驟(PartA, PartB, PartC)才可完成，定義出其介面:

~~~csharp
interface IBuilder 
{
    void BuildPratA();
    void BuildPratB();
    void BuildPratC();
}
~~~

第一樣產品由ABC三步驟組成

~~~csharp
class ConcreteBuilder1 : IBuilder
{
    public void BuildPratA()
    {
        Console.WriteLine("A");
    }

    public void BuildPratB()
    {
        Console.WriteLine("B");
    }

    public void BuildPratC()
    {
        Console.WriteLine("C");
    }
}
~~~

第二樣產品由XYZ三步驟組成:

~~~csharp
class ConcreteBuilder2 : IBuilder
{
    public void BuildPratA()
    {
        Console.WriteLine("X");
    }

    public void BuildPratB()
    {
        Console.WriteLine("Y");
    }

    public void BuildPratC()
    {
        Console.WriteLine("Z");
    }
}
~~~

----------

## 開始運作 ##

已經定義出產品第一樣產品是由ABC三步驟完成；第二樣產品由XYZ三步驟完成，現在開始讓他們運作:

~~~csharp
IBuilder product1 = new ConcreteBuilder1();
product1.BuildPratA();
product1.BuildPratC();

IBuilder product2 = new ConcreteBuilder2();
product2.BuildPratA();
product2.BuildPratB();
product2.BuildPratC();
~~~

結果為:

    A
    C
    X
    Y
    Z

哎呀!第一樣產品忘記呼叫PartB，而且client端在使用時不需要知道他是由三個步驟完成的，他只要指定現在要讓哪條生產線運作就好，以避免client與運作細節過度耦合。

這時候就可以使用Builder design pattern 來解決這個問題。

----------

## Builder Pattern ##

> The intent of the Builder design pattern is to separate the construction of a complex object from its representation. By doing so the same construction process can create different representations.

簡單來講就是用一個相同的建構過程產生不同的結果。

### 建立一個Director class ###

指揮者，用來控制建構過程，也用來分離client與生產過程的關聯。

~~~csharp
class Director
{
    IBuilder _builder;
    public Director(IBuilder builder)
    {
        _builder = builder;
    }

    public void Run()
    {
        _builder.BuildPratA();
        _builder.BuildPratB();
        _builder.BuildPratC();
    }
}
~~~

### 更改原來的client程式 ###

~~~csharp
Director director = new Director(new ConcreteBuilder1());
director.Run();

director = new Director(new ConcreteBuilder2());
director.Run();
~~~

執行結果:

    A
    B
    C
    X
    Y
    Z
    
----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#) [[Design Pattern系列文章]](http://iverson127.github.io/tags/#Design Pattern)