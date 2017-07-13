---
layout: post
title: "C# Simple Factory Design Pattern"
author: "Iverson Hong"
modified: 2016-05-09
tags: [C#, Design Pattern]
---

## 玩具工廠 ##

玩具工廠製造玩具狗其class如下：

~~~csharp
class DogToy
{
    public void MakeToy()
    {
        Console.WriteLine("Make dog toy...");
    }
}
~~~

也製造玩具貓：

~~~csharp
class CatToy
{
    public void MakeToy()
    {
        Console.WriteLine("Make cat toy...");
    }
}
~~~

開始製造：

~~~csharp
DogToy dog = new DogToy();
dog.MakeToy();

CatToy cat = new CatToy();
cat.MakeToy();
~~~

結果如下：

    Make dog toy...
    Make cat toy...
    
假設之後玩具工廠要製造玩具車、玩具飛機...，用戶端必須知道每一個玩具類別並實體化後才可使用，也就是client端與每個玩具類別之間有著高度耦合，可透過簡單工廠設計模式來解決。

----------

## Simple Factory Pattern ##

簡單來講就是用一個單獨的類別來產生出不同實體的過程。

### UML架構 ###

![](..\images\postImage\CSharp_DesignPattern_SimpleFactory\SimpleFactory.png)

### 抽出共用的介面 ###

~~~csharp
interface IToy
{
    void MakeToy();
}
~~~

修改原來的玩具狗跟玩具貓類別，讓他們繼承IToy

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

### 工廠類別 ###

用來生成玩具的實體：

~~~csharp
class ToyFactory
{
    public static IToy CreateToy(string toyType)
    {
        if (toyType == "Dog")
        {
            return new DogToy();
        }
        else if (toyType == "Cat")
        {
            return new CatToy();
        }

        return null;
    }
}
~~~

### 更改原來的client程式 ###

~~~csharp
IToy toy;

toy = ToyFactory.CreateToy("Dog");
toy.MakeToy();

toy = ToyFactory.CreateToy("Cat");
toy.MakeToy();
~~~

用戶端只要告訴玩具工廠他要製造什麼玩具就好，玩具實體的產生由工廠負責，降低了用戶端與玩具類別之間的耦合。

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Design Pattern系列文章]](http://yu-qiao-hong.github.io/tags/#Design Pattern)