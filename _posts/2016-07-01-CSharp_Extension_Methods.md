---
layout: post
title: "C# 擴充方法(Extension Method)"
author: "Iverson Hong"
modified: 2016-07-01
tags: [C#]
---

假設有一Player class，property為Name及Height

~~~csharp
public class Player
{
    public string Name { get; set; }
    public int Height { get; set; }
}
~~~

現在想要比較兩個Player實體哪個比較高，並把資訊印出來：

~~~csharp
static void Main(string[] args)
{
    var p1 = new Player { Name = "Iverson Hong", Height = 171 };
    var p2 = new Player { Name = "Michael Jordan", Height = 198 };
    p1.Print(p2);
}
~~~

寫法很簡單，只要在Player class內加入"Print"method

~~~csharp
public class Player
{
    public string Name { get; set; }
    public int Height { get; set; }

    public void Print(Player p2)
    {
        Console.WriteLine("{0} is {1} cm.", Name, Height);
        Console.WriteLine("{0} is {1} cm.", p2.Name, p2.Height);
        if (Height > p2.Height)
        {
            Console.WriteLine("{0} is taller than {1}.", Name, p2.Name);
        }
        else if (Height == p2.Height)
        {
            Console.WriteLine("{0} is the same height as {1}.", Name, p2.Name);
        }
        else
        {
            Console.WriteLine("{0} is taller than {1}.", p2.Name, Name);
        }
    }
}
~~~

結果：

    Iverson Hong is 171 cm.
    Michael Jordan is 198 cm.
    Michael Jordan is taller than Iverson Hong.

以上寫法的前提是"**Player class是自己寫的"**，要是Player class為DLL，就不能這樣寫了，這時候就可以使用C#擴充方法(Extension Method)。

## Extension Method ##

擴充方法可在不改變原有的class內容下，對class method進行擴充。其步驟如下：

1. 定義一public static class

2. 在其內定義public static method

3. method第一個參數需加上**this**關鍵字並接著欲擴充的類別名稱

~~~csharp
public static class Extension
{
    public static void Print(this Player p, Player p2)
    {
        Console.WriteLine("{0} is {1} cm.", p.Name, p.Height);
        Console.WriteLine("{0} is {1} cm.", p2.Name, p2.Height);
        if (p.Height > p2.Height)
        {
            Console.WriteLine("{0} is taller than {1}.", p.Name, p2.Name);
        }
        else if (p.Height == p2.Height)
        {
            Console.WriteLine("{0} is the same height as {1}.", p.Name, p2.Name);
        }
        else
        {
            Console.WriteLine("{0} is taller than {1}.", p2.Name, p.Name);
        }
    }
}
~~~

其結果一致。若是原來Player class內已有個相同的method，並**不會**發生錯誤，compiler會執行原來在class內的method。

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)