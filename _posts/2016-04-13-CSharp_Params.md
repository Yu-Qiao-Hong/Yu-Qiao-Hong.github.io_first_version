---
layout: post
title: "C# Params"
author: "Iverson Hong"
modified: 2016-04-13
tags: [C#]
---
## 使用時機 ##

有一method需帶入int陣列，並會將其陣列結果印出:

~~~csharp
void PrintNum(int[] ary)
{
    foreach (int n in ary)
        Console.WriteLine(n.ToString());
}
~~~

Clint在呼叫時需寫:

~~~csharp
int[] myAry = {1, 2, 3, 4, 5};
PrintNum(myAry);
~~~

每次client要呼叫method之前，都先需定義一int陣列

~~~csharp
int[] myAry2 = {7, 8, 9};
PrintNum(myAry2);
~~~

每次都要重新定義好麻煩喔~~

沒關係!還有別的方法

----------

## Params ##

修改一下上面的method，再帶入參數前面加上"**params**"

~~~csharp
void PrintNum(params int[] ary)
{
    foreach (int n in ary)
        Console.WriteLine(n.ToString());
}
~~~

這個時候client要呼叫時可直接把int帶入

~~~csharp
PrintNum(7, 8, 9);
~~~

一樣可以正常印出，且原來的呼叫方式也不會有任何錯誤發生。

----------

## Params 與其他參數混著用 ##

若是要再多帶參數，必須把params放在最後面，這樣compiler才知道params陣列的數量到底為多少。

~~~csharp
void PrintNum(string name, params int[] ary)
{
    Console.WriteLine(name);
    foreach (int n in ary)
        Console.WriteLine(n.ToString());
}
~~~

~~~csharp
int[] myAry = { 1, 2, 3, 4, 5 };
PrintNum("Iverson", myAry);

PrintNum("Hong", 7, 8, 9);
~~~

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)