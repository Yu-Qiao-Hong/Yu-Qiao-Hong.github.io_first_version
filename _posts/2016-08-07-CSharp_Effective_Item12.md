---
layout: post
title: "C# 成員變數(Member Variables)建立初始值的時間點"
author: "Iverson Hong"
modified: 2016-08-07
tags: [C#, Effective C#]
---

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

在Effective C#中建議在宣告成員變數時就直接給予初始值，
此作法可避免在程式中使用到未初始化的變數。
在宣告時給予初始值，
其執行順序比執行此件類別建構子還之前：
　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　
　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

~~~csharp
class Test
{
    int myIntrger = 5;

    public Test()
    {
        Console.WriteLine(myIntrger.ToString());
    }
}

static void Main(string[] args)
{
    Test t = new Test(); // 5
}
~~~

----------

但也不是在所有情況下都適合在宣告時直接給予初始值，有3種情況應**避免**使用：

## 將變數值設為0或null ##

0及null皆為C#的預設初始值，不須額外設置影響效率(此為書中觀點，我覺得寫了有較佳的可讀性)

~~~csharp
class Test
{
    int myIntrger1;
    int myIntrger2 = 0;
    
    object myObj1;
    object myObj2 = null;
}
~~~

可從IL看出myIntrger2與myObj2配置0及null。

![](..\images\postImage\CSharp_Effective_Item12\001.png)

----------

## 多個建構子 ##

多個建構子代表了建構子本身會帶有不同的參數，若沒處理好則會造成效能問題。

~~~csharp
class Test
{
    List<int> myList = new List<int>();

    public Test()
    {
           
    }

    public Test(int size)
    {
        myList = new List<int>(size);
    }
}
~~~

上述程式若呼叫Test(int size)建構子，則myList建立兩次實體，第一次產生的實體馬上變成記憶體中的垃圾，應改寫成如下寫法較好：

~~~csharp
class Test
{
    List<int> myList;

    public Test()
    {
        myList = new List<int>();
    }

    public Test(int size)
    {
        myList = new List<int>(size);
    }
}
~~~

----------

## 會有Exception的產生 ##

在宣告成員變數時無法使用try catch來處理exception，若沒處理則會在初始化的過程中直接crash。較好的處理方式為將此初始化的動作移到建構子內進行。

~~~csharp
class Test
{
    TestA a = new TestA();
    public Test()
    {
    
    }
}

class TestA
{
    public TestA()
    {
        throw new Exception();
    }
}
~~~

![](..\images\postImage\CSharp_Effective_Item12\002.png)

將初始化動作放到建構子內使用try catch：

~~~csharp
class Test
{
    TestA a;
    public Test()
    {
        try
        {
            a = new TestA();
        }
        catch 
        {
        }
    }
}
~~~

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Effective C#系列文章]](http://yu-qiao-hong.github.io/tags/#Effective C#)