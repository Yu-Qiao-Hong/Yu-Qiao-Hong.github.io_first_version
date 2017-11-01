---
layout: post
title: "C# Builder Design Pattern"
author: "Iverson Hong"
modified: 2016-11-02
tags: [C#, Design Pattern]
---

## 生產線 ##

某工廠有兩條生產線生產兩樣產品，此兩樣產品生產需經三個步驟(Part1, Part2, Part3)才可完成，定義出其介面:

~~~csharp
interface IBuilder 
{
    void BuildPrat1();
    void BuildPrat2();
    void BuildPrat3();
}
~~~

產品A由ABC三步驟組成

~~~csharp
class ProductA : IBuilder
{
    public void BuildPrat1()
    {
        Console.WriteLine("A");
    }

    public void BuildPrat2()
    {
        Console.WriteLine("B");
    }

    public void BuildPrat3()
    {
        Console.WriteLine("C");
    }
}
~~~

產品B由XYZ三步驟組成:

~~~csharp
class ProductB : IBuilder
{
    public void BuildPrat1()
    {
        Console.WriteLine("X");
    }

    public void BuildPrat2()
    {
        Console.WriteLine("Y");
    }

    public void BuildPrat3()
    {
        Console.WriteLine("Z");
    }
}
~~~

----------

## 開始運作 ##

已經定義出產品第一樣產品是由ABC三步驟完成；第二樣產品由XYZ三步驟完成，現在開始讓他們運作:

~~~csharp
IBuilder productA = new ProductA();
productA.BuildPrat1();
productA.BuildPrat3();

IBuilder productB = new ProductB();
productB.BuildPrat1();
productB.BuildPrat2();
productB.BuildPrat3();
~~~

結果為:

    A
    C
    X
    Y
    Z

哎呀!第一樣產品忘記呼叫Part2	，而且client端在使用時不需要知道他是由三個步驟完成的，他只要指定現在要讓哪條生產線運作就好，以避免client與運作細節過度耦合。

這時候就可以使用Builder design pattern 來解決這個問題。

----------

## Builder Pattern ##

> The intent of the Builder design pattern is to separate the construction of a complex object from its representation. By doing so the same construction process can create different representations.

簡單來講就是用一個相同的建構過程產生不同的結果。

### 建立一個Director class ###

指揮者，用來控制建構過程，也用來分離Client與生產過程的關聯。Client只需知道產品類別(Concrete Builder)，無須知道生產過程的細節。

~~~csharp
class Director
{
    private IBuilder _builder;
    public Director(IBuilder builder)
    {
        _builder = builder;
    }

    public void Run()
    {
        _builder.BuildPrat1();
        _builder.BuildPrat2();
        _builder.BuildPrat3();
    }
}
~~~

### 更改原來的client程式 ###

~~~csharp
Director director = new Director(new ProductA());
director.Run();

director = new Director(new ProductB());
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

### 優點 ###

- Client 不需要知道產品生產過程的細節，將產品本身與生產過程解藕，使得具有相同生產過程可以建造出不同的產品。

- 每個產過程的細節相對獨立，與其他Concrete Builder無關，因此可以方便的替換或新增Concrete Builder。Client使用不同Concrete Builder即可產生不同的產品。

- 可將生產細部內容寫在各自Concrete Builder，生產步驟交由Director來負責，使得生產過程更將清晰。

- 新增Concrete Builder不需動到原有Concrete Builder，符合"**Open Closed Principle**"。

### 缺點 ###

- Builder模式適用於類似的生產流程，因此產品之間的差異性不大，若差異大則使用範圍易受限。

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Design Pattern系列文章]](http://yu-qiao-hong.github.io/tags/#Design Pattern)