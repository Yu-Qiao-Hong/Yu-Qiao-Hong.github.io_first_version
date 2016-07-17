---
layout: post
title: "C# 為class提供有意義的ToString() method"
author: "Iverson Hong"
modified: 2016-07-14
tags: [C#, Effective C#]
---

C#所有的class都繼承Object class，其內有一 virtual ToString() method。若不override，則會印出此class的名稱，其資訊對使用者來說並不重要。

~~~csharp
class Test
{
}

private void button1_Click(object sender, EventArgs e)
{
    Test test = new Test();
    Console.WriteLine(test.ToString());
}

private void button2_Click(object sender, EventArgs e)
{
    List<int> list = new List<int>();
    Console.WriteLine(list.ToString());
}
~~~

分別執行結果為：

    ConsoleApplication1.Test
    System.Collections.Generic.List`1[System.Int32]

可override ToString()使得class更有意義

~~~csharp
class Test
{
    public override string ToString()
    {
        return "This is override ToString()";
    }
}

private void button1_Click(object sender, EventArgs e)
{
    Test test = new Test();
    Console.WriteLine(test.ToString());
}
~~~

結果：

    This is override ToString()
    
除此之外在C#中許多操作也會隱性的呼叫ToString()，如Console.WriteLine

~~~csharp
private void button2_Click(object sender, EventArgs e)
{
    Test test = new Test();
    Console.WriteLine(test);
}
~~~

結果跟上一個例子一樣。

----------

## Reference ##

- [https://dotblogs.com.tw/larrynung/2009/10/13/11033](https://dotblogs.com.tw/larrynung/2009/10/13/11033)

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)
[[Effective C#系列文章]](http://iverson127.github.io/tags/#Effective C#)