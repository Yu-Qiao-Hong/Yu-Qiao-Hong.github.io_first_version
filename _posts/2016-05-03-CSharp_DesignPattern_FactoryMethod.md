---
layout: post
title: "C# Factory Method Design Pattern"
author: "Iverson Hong"
modified: 2016-05-03
tags: [C#, Design Pattern]
---

[接續前一篇](http://iverson127.github.io/CSharp_DesignPattern_SimpleFactory/)，現在狗工廠及貓工廠不僅只生產玩具，也開始賣相關的食品，參考上一篇的方法再增加一個食品類別。

## Factory Method Pattern ##

因此我們把簡單工廠內做的事情再抽出來，讓用戶端來決定現在要由哪個工廠製造玩具。

原來的玩具介面以及玩具類別**不**需要修改:

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

### 把原來的Factory抽出來 ###

工廠介面回傳的是玩具的介面

~~~csharp
interface IFactory
{
    IToy CreateToy();
}
~~~

新增狗跟貓的工廠類別，並讓他們繼承IFactory。狗工廠內產生玩具狗類別的實體；貓工廠內產生玩具貓類別的實體：

~~~csharp
class DogProductFactory : IFactory
{
    public IToy CreateToy()
    {
        return new DogToy();
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
}
~~~

### client端程式 ###

~~~csharp
IFactory fctory;
IToy toy;

fctory = new DogProductFactory();
toy = fctory.CreateToy();
toy.MakeToy();

fctory = new CatProductFactory();
toy = fctory.CreateToy();
toy.MakeToy();
~~~

其結果與上篇一樣

    Make dog toy...
    Make cat toy...

讓用戶端來決定實體化哪一個工廠，也就是原先簡單工廠的內部邏輯判斷搬到用戶端進行，這樣修改之後可發現要增加玩具車時，只要增加玩具車類別，以及對應的車工廠，原本已寫好的程式碼無須再更改，這樣就符合開放-封閉原則的精神了。

總結來說，工廠方法是簡單工廠的進一步抽象，有著簡單工廠的的優點(用戶端與玩具類別無相依性)，也保持程式的擴展性。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#) [[Design Pattern系列文章]](http://iverson127.github.io/tags/#Design Pattern)