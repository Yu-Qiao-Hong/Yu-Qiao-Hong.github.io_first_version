---
layout: post
title: "C# Facade Design Pattern"
author: "Iverson Hong"
modified: 2016-04-20
tags: [C#, Design Pattern]
---

## 麥噹噹套餐 ##

現在來用程式寫出一家速食店麥噹噹的餐點，它有漢堡薯條還有飲料可供選擇，他們的類別如下:

~~~csharp
class Hamburger
{
    public void SelectMeat(int type)
    {
        if (type == 0)
            Console.WriteLine("Beef hamburger");
        else if (type == 1)
            Console.WriteLine("Pork hamburger");
        else
            Console.WriteLine("Fish hamburger");
    }
}
~~~

~~~csharp
class FrenchFries
{
    public void SetSize(int size)
    {
        if(size == 0)
            Console.WriteLine("Small french fries");
        else if (size == 1)
            Console.WriteLine("Medium french fries");
        else
            Console.WriteLine("Big french fries");
    }
}
~~~

~~~csharp
class Drink
{
    public void Select(int type, int size)
    {
        if (size == 0)
            Console.Write("Small ");
        else if (size == 1)
            Console.Write("Medium ");
        else
            Console.Write("Big ");

        if (type == 0)
            Console.WriteLine("Coke");
        else if (type == 1)
            Console.WriteLine("Fanta");
        else
            Console.WriteLine("Sprite");
    }
}
~~~

----------

## 海陸套餐 ##

麥噹噹現在推出最新的套餐，它是由一個牛肉+一個魚肉漢堡搭配小薯以及中杯可樂組合而成。

現在顧客要點這個套餐了:

~~~csharp
Drink drink = new Drink(); ;
FrenchFries frenchFries = new FrenchFries();
Hamburger hamburger = new Hamburger();
 
hamburger.SelectMeat(1);
hamburger.SelectMeat(2);
drink.Select(0, 1);
frenchFries.SetSize(0);
~~~

結果為:

    Pork hamburger
    Fish hamburger
    Medium Coke
    Small french fries

可以發現到client與另外三個類別有著高度耦合的關係，假設我改變飲料種類或大小，我的client端程式也要跟著改變。很明顯這已經違反了物件導向程式迪米特法則(Law of Demeter，LoD)的概念了。

這時候就可以使用Facade design pattern 來解決這個問題。

----------

## Facade Pattern ##

簡單來講就是多包一層，不讓client去了解裡面做了什麼事，client只是方便使用就好，不需了解細部內容。藉此降低client與其他類別之間的耦合性。

### 建立一個Facade class ###

~~~csharp
class Facade
{
    Drink drink;
    FrenchFries frenchFries;
    Hamburger hamburger;

    public Facade()
    {
        drink = new Drink(); ;
        frenchFries = new FrenchFries();
        hamburger = new Hamburger();
    }

    public void BuyPackage()
    {
        hamburger.SelectMeat(1);
        hamburger.SelectMeat(2);
        drink.Select(0, 1);
        frenchFries.SetSize(0);
    }
}
~~~

### 更改原來的client程式 ###

~~~csharp
Facade facade = new Facade();
facade.BuyPackage();
~~~

其結果與上面一致，成功切開client與另外三個類別的直接關係。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#) [[Design Pattern系列文章]](http://iverson127.github.io/tags/#Design Pattern)