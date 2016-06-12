---
layout: post
title: "C# Abstract Factory Design Pattern"
author: "Iverson Hong"
modified: 2016-05-09
tags: [C#, Design Pattern]
---

[接續前一篇](http://iverson127.github.io/CSharp_DesignPattern_FactoryMethod/)，現在狗工廠及貓工廠不僅只生產玩具，也開始賣相關的食品，參考上一篇的方法再增加一個食品介面與對應食品類別。

## Abstract Factory Pattern ##

其實就是工廠方法的再延伸，工廠方法內只會有一項產品(玩具)；而抽象工廠則是許多產品(除了玩具之外，現在還有食品)，而用戶端使用時，先建立具體的工廠(狗工廠或貓工廠)，利用此工廠來建立特定的產品物件(玩具或食品)。

### UML架構 ###

![](..\images\postImage\CSharp_DesignPattern_AbstractFactory\AbstractFactory.png)

原來的玩具介面以其衍生類別**不**需要修改:

~~~csharp
interface IToy
{
    void MakeToy();
}
~~~

~~~csharp
class DogToy : IToy
{
    public void MakeToy()
    {
        Console.WriteLine("Make dog toy...");
    }
}
~~~

~~~csharp
class CatToy : IToy
{
    public void MakeToy()
    {
        Console.WriteLine("Make cat toy...");
    }
}
~~~

再建立食品介面及其衍生類別：

~~~csharp
interface IFood
{
    void MakeFood();
}
~~~

~~~csharp
class DogFood : IFood
{
    public void MakeFood()
    {
        Console.WriteLine("Make dog food...");
    }
}
~~~

~~~csharp
class CatFood : IFood
{
    public void MakeFood()
    {
        Console.WriteLine("Make cat food...");
    }
}
~~~

### Factory介面再增加一個建立食品的抽象方法 ###

工廠介面提供一個回傳玩具介面方法之外，再增加一個回傳食品介面的方法

~~~csharp
interface IFactory
{
    IToy CreateToy();
    
    IFood CreateFood(); 
}
~~~

新增狗跟貓工廠類別，並讓他們繼承IFactory。狗工廠內產生玩具狗類別的實體以及狗食物的實體；貓工廠內產生玩具貓類別的實體及貓食物的實體：

~~~csharp
class DogProductFactory : IFactory
{
    public IToy CreateToy()
    {
        return new DogToy();
    }
    
    public IFood CreateFood()
    {
        return new DogFood();
    }
}
~~~

~~~csharp
class CatProductFactory : IFactory
{
    public IToy CreateToy()
    {
        return new CatToy();
    }
    
    public IFood CreateFood()
    {
        return new CatFood();
    }
}
~~~

### client端程式 ###

~~~csharp
IFactory fctory;
IToy toy;
IFood food;

fctory = new DogProductFactory();
toy = fctory.CreateToy();
toy.MakeToy();
food = fctory.CreateFood();
food.MakeFood();

fctory = new CatProductFactory();
toy = fctory.CreateToy();
toy.MakeToy();
food = fctory.CreateFood();
food.MakeFood();
~~~

結果：

    Make dog toy...
    Make dog food...
    Make cat toy...
    Make cat food...

使用抽象工廠模式好處在於在用戶端切換具體工廠，就可以使用不同產品(切換到狗工廠，可以生產狗玩具跟狗食品)，且用戶端是使用抽象的介面來操作實體，產品具體的類別名稱(DogToy、DogFood)在用戶端程式中也不會出現。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)[[Design Pattern系列文章]](http://iverson127.github.io/tags/#Design Pattern)