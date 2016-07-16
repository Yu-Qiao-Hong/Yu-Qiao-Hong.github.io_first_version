---
layout: post
title: "C# 介紹各種"相等(Equality)"的用法"
author: "Iverson Hong"
modified: 2016-07-16
tags: [C#, Effective C#]
---

在C#中若要使用"**相等(Equality)**"，常見的有4種方式，以下一一介紹：

~~~csharp
public static bool ReferenceEquals(object objA, object objB);

public static bool Equals(object objA, object objB);

public virtual bool Equals(object obj);

public static bool operator ==(MyClass left, MyClass right);
~~~

## Object.ReferenceEquals() ##

此方法不需override，用來比較兩個object是否指向相同的reference位址，若是比較兩個value type物件，其結果必為**False**，原因為value type要轉成object(reference type)此動作為Boxing。

~~~csharp
private void button1_Click(bject sender, EventArgs e)
{
    int i = 5;
    int j = 5;
    if (Object.ReferenceEquals(i, j))
        Console.WriteLine("Never happens.");
    else
        Console.WriteLine("Always happens.");
    if (Object.ReferenceEquals(i, i))
        Console.WriteLine("Never happens.");
    else
        Console.WriteLine("Always happens.");
}
~~~

結果:

    Always happens.
    Always happens.

----------

## static Object.Equals() ##

此方法不需override，在runtime時比較兩個值是否相等，其實際運作為

~~~csharp
public static new bool Equals(object left, object right)
{
    // Check object identity
    if (Object.ReferenceEquals(left, right) )
        return true;

    // both null references handled above
    if (Object.ReferenceEquals(left, null) ||
    Object.ReferenceEquals(right, null))
        return false;

    return left.Equals(right);
}
~~~

簡單來說，兩個值為reference type，則與Object.ReferenceEquals()相同，比較是否指向相同的reference位址；

若是兩個比較值為value type，則比較實際的內容值是否相等。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)
[[Effective C#系列文章]](http://iverson127.github.io/tags/#Effective C#)