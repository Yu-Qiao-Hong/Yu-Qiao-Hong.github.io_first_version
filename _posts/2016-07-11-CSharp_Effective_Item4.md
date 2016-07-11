---
layout: post
title: "C# 使用Conditional Attribute取代#if #endif"
author: "Iverson Hong"
modified: 2016-07-11
tags: [C#, Effective C#]
---

寫程式有時需要在同一份function中有因不同條件而執行部分程式，例如想在debug時印些log訊息，但在release版本卻不希望印出，常見的做法為使用#if #endif把特定的程式內容包起來。

## #if #endif ##

使用#if #endif有許多缺點:當這種用法一多時，程式的閱讀性大大降低，且容易產生bug

### 易有BUG ###

~~~csharp
public void Func()
{
    string msg = null;

    #if DEBUG
    msg = GetDiagnostics();
    #endif
    
    Console.WriteLine(msg);
}
~~~

上面程式在DEGUG中執行不會有問題，但在RELEASE執行時則印出空白行。

### 效能問題 ###

~~~csharp
public void Func1()
{
    #if DEBUG
    Console.WriteLine("123");
    Console.WriteLine("456");
    Console.WriteLine("789");
    #endif
}

private void button1_Click(object sender, EventArgs e)
{
    Func1();
}
~~~

上面的程式在DEBUG環境時印出:

    123
    456
    789

在RELEASE環境則什麼都不會印出。

利用反組譯觀看程式碼有甚麼差異：

DEBUG:

~~~csharp
public void Func1()
{
    Console.WriteLine("123");
    Console.WriteLine("456");
    Console.WriteLine("789");
}

private void button1_Click(object sender, EventArgs e)
{
    this.Func1();
}
~~~

RELEASE:

~~~csharp
public void Func1()
{
}

private void button1_Click(object sender, EventArgs e)
{
    this.Func1();
}
~~~

可以發現不論是在DEBUG還是RELEASE環境下用戶端都會呼叫Func1，但在RELEASE環境中，Func1其實裡面完全沒有內容了，但用戶端還是會花時間呼叫，這個動作是多餘的。

----------

## Conditional Attribute ##

C#有提供另一種較有效率的方式，Conditional Attribute，其用法是在method宣告前加上[Conditional("XXX")]，除此之外，他還有一些限制

1. 必須用於method前

2. Method必須回傳void

3. 可帶參數，但不建議，有機會產生bug

以Conditional Attribute方式改寫上面程式：

~~~csharp
[Conditional("DEBUG")]
public void Func2()
{
    Console.WriteLine("123");
    Console.WriteLine("456");
    Console.WriteLine("789");
}

private void button1_Click(object sender, EventArgs e)
{
    Func2();
}
~~~

執行結果一樣，利用反組譯觀看程式碼有甚麼差異：

DEBUG:

~~~csharp
[Conditional("DEBUG")]
public void Func2()
{
    Console.WriteLine("123");
    Console.WriteLine("456");
    Console.WriteLine("789");
}

private void button1_Click(object sender, EventArgs e)
{
    this.Func2();
}
~~~

RELEASE:

~~~csharp
[Conditional("DEBUG")]
public void Func2()
{
    Console.WriteLine("123");
    Console.WriteLine("456");
    Console.WriteLine("789");
}

private void button1_Click(object sender, EventArgs e)
{
}
~~~

可發現不論是在DEBUG還是RELEASE環境下，都產生一樣的Func2，但在RELEASE環境中用乎端不會呼叫Func2，因此更有效率。

----------

也可以使用多個Conditional Attributes,例如想在DEBUG或者(**OR**)TRACE的環境下執行，可寫：

~~~csharp
[Conditional("DEBUG"), Conditional("TRACE")]
public void Func3()
{
    Console.WriteLine("123");
    Console.WriteLine("456");
    Console.WriteLine("789");
}
~~~

若想用兩個條件都符合才執行的話(**AND**)，可寫：
~~~csharp
#if(DEBUG && TRACE)
#define BOTH
#endif

[Conditional("BOTH")]
public void Func4()
{
    Console.WriteLine("123");
    Console.WriteLine("456");
    Console.WriteLine("789");
}
~~~

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)
[[Effective C#系列文章]](http://iverson127.github.io/tags/#Effective C#)