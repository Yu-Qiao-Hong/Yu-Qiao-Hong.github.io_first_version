---
layout: post
title: "C# Generic Delegate"
author: "Iverson Hong"
modified: 2016-06-19
tags: [C#]
---

除了正規的delegate之外，C#還提供了3種Generic Delegate

1. Action
2. Func
3. Predicate

## Action ##

Action為**void回傳值**，參數為optional(最多可帶16個)的delegate，其定義為：

~~~csharp
public delegate void Action();
public delegate void Action<in T>(T obj);
public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
...
public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, in T14, in T15, in T16>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13, T14 arg14, T15 arg15, T16 arg16);
~~~

### 一般用法 ###

~~~csharp
private void button1_Click(object sender, EventArgs e)
{
    Action a = Print;
    a += Print1;
    a();

    Action<string> b = Print2;
    b("Iverson Hong");
}

private void Print()
{
    Console.WriteLine("Hello world!");
}

private void Print1()
{
    Console.WriteLine("This is Iverson Hong");
}

private void Print2(string str)
{
    Console.WriteLine(str);
}
~~~

### Lamda用法 ###

使用lamda可以省去制定出method的麻煩

~~~csharp
private void button2_Click(object sender, EventArgs e)
{
    Action a = () =>
    {
        Console.WriteLine("Hello world!");
    };

    a += () =>
    {
        Console.WriteLine("This is Iverson Hong");
    };

    a();

    Action<string> b = (str) =>
    {
        Console.WriteLine(str);
    };

    b("Iverson Hong");
}
~~~

### 結果 ###

    Hello world!
    This is Iverson Hong
    Iverson Hong

----------

## Func ##

Func為**必須一個回傳值**(不能為void)，參數為optional(最多可帶16個)的delegate，其定義為：

~~~csharp
public delegate TResult Func<out TResult>()
public delegate TResult Func<in T, out TResult>(T arg);
public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);
...
public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, in T14, in T15, in T16, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13, T14 arg14, T15 arg15, T16 arg16);
~~~

### 一般用法 ###

~~~csharp
private void button3_Click(object sender, EventArgs e)
{
    Func<int, int, string> a = Add;
    
    Console.WriteLine(a(1, 2));
}

private string Add(int first, int second)
{
    return (first + second).ToString();
}
~~~

### Lamda用法 ###

~~~csharp
private void button4_Click(object sender, EventArgs e)
{
    Func<int, int, string> a = (first, second) =>
    {
        return (first + second).ToString();
    };

    Console.WriteLine(a(1, 2));
}
~~~

### 結果 ###

    3

----------

## Predicate ##

Predicate**必須為布林回傳值**，**只能有一個參數**的delegate，其定義為：

~~~csharp
public delegate bool Predicate<in T>(T obj);
~~~

### 一般用法 ###

~~~csharp
private void button5_Click(object sender, EventArgs e)
{
    Predicate<string> a = IsUpperCase;

    Console.WriteLine(a("Iverson"));
}

private bool IsUpperCase(string str)
{
    foreach (var chr in str)
    {
        if (!char.IsUpper(chr))
        {
            return false;
        }
    }

    return true;
}
~~~

### Lamda用法 ###

~~~csharp
private void button6_Click(object sender, EventArgs e)
{
    Predicate<string> a = (str) =>
    {
        foreach (var chr in str)
        {
            if (!char.IsUpper(chr))
            {
                return false;
            }
        }

        return true;
    };

    Console.WriteLine(a("Iverson"));
}
~~~

### 結果 ###

    False

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)