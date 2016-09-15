---
layout: post
title: "C# 減少建構子初始化時的重覆邏輯"
author: "Iverson Hong"
modified: 2016-09-16
tags: [C#, Effective C#]
---

在C#中若要建立多個建構子，除了可以用multiple overloads之外還可使用default parameter(C# 4.0後提供)

以下使用overload方式產生3種不同的建構子：

1. 無參數

2. 帶入1個參數

3. 帶入2個參數

~~~csharp
class MyClass
{
    private int count;
    private string address;

    public MyClass() :
        this(0, "")
    {
    }

    public MyClass(string address) :
        this(0, address)
    {
    }

    public MyClass(int count, string address)
    {
        this.count = count;
        this.address = address;
    }
}
~~~

----------

在C# 4.0 後可使用default parameter的方式來取代overload：

~~~csharp
class MyClass1
{
    private int count;
    private string address;

    public MyClass1() :
        this(0, string.Empty)
    {
    }

    public MyClass1(int count = 0, string address = "")
    {
        this.count = count;
        this.address = address;
    }
}
~~~

以上述程式為例，使用default parameter雖有較高的彈性(有4個建構子：沒參數的、帶入name參數、帶入address參數、帶入兩個參數)，但在使用時須注意所有的組合以及預設值皆需合理且不會有非預期的邏輯產生，也因此與用戶端產生較高的耦合性。

----------

若不使用上述兩種方式，而只是把公用的程式抽出來：

~~~csharp
class MyClass2
{
    private int count;
    private string address;

    public MyClass2() 
    {
        SetParameters(0, "");
    }

    public MyClass2(string address)
    {
        SetParameters(0, address);
    }

    public MyClass2(int count, string address)
    {
        SetParameters(count, address);
    }

    private void SetParameters(int count, string address)
    {
        this.count = count;
        this.address = address;
    }
}
~~~

這種方式看起來好像沒什麼差別，但透過IL得知3個建構子內部都會先對成員變數進行初始化後再呼叫SetParameters()，這是很沒效率的做法；另外兩種方式則是透過this把成員變數初始化的動作分派到其他的建構子中進行，此種方法可確保成員變數初始化只會做一次。

除了效能的考量外，若是成員變數為"**readonly**"時，也可在建構子來設定readonly初始值。

----------

[C# 成員變數(Member Variables)建立初始值的時間點](http://iverson127.github.io/CSharp_Effective_Item12/)

[C# 在初始化時使用靜態成員以及靜態建構子](http://iverson127.github.io/CSharp_Effective_Item13/)

總結這篇與上述兩篇，整理產生class instance時的順序：

1. 將static成員變數空間設為0

2. 初始化static變數

3. 執行base static class的建構子

4. 執行static class建構子

5. 將class成員變數空間設為0

6. 初始化成員變數

7. 執行base class建構子

8. 執行class建構子

產生相同class的instance時，就直接從步驟5開始，因static只會做一次。總結來說若無邏輯可直接對成員變數進行初始化，若有較複雜的邏輯可把初始化的行為寫在建構子內。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)
[[Effective C#系列文章]](http://iverson127.github.io/tags/#Effective C#)