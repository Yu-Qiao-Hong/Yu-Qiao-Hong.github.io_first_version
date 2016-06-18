---
layout: post
title: "C# Generic Delegate"
author: "Iverson Hong"
modified: 2016-06-14
tags: [C#, Design Pattern]
---

除了正規的delegate之外，C#還提供了3種Generic Delegat

1. Action
2. Func
3. Predicate

## Action ##

## Func ##

## Predicate ##

宣告一Dictionary並在main thread之外建立另外三個thread：

1. 一個增加Dictionary item
2. 一個印出現在Dictionary內有多少數量
3. 一個刪除Dictionary內第一個item

## 沒有thread-safe的範例 ##

~~~csharp
static void Main(string[] args)
{
    Dictionary<string, string> dict = new Dictionary<string, string>();
    bool running = true;
            
    Task.Factory.StartNew(() =>
    {
        while (running)
        {
            dict.Add(Guid.NewGuid().ToString(), "test");
        }
    });

    Task.Factory.StartNew(() =>
    {
        while (running)
        {
            Console.WriteLine("Dictionary Count={0}", dict.Count);
            Thread.Sleep(1000);
        }
    });

    Task.Factory.StartNew(() =>
    {
        while (running)
        {
            if (dict.Count > 0)
            {
                string key = dict.Keys.First();
                dict.Remove(key);
            }
        }
    });

    Console.ReadLine();
    running = false;
}
~~~

執行起來什麼亂七八糟的Exception都出現了，因為每個thread都對Dictionary容器操作，這就是典型的沒處理thread-safe，一般來說，我們習慣使用"**lock**"來處理，在Add()或Remove()時限制其他thread進行操作。

----------

## 使用lock ##

~~~csharp
static void Main(string[] args)
{
    Dictionary<string, string> dict = new Dictionary<string, string>();
    bool running = true;
            
    Task.Factory.StartNew(() =>
    {
        lock (dict)
        {
            while (running)
            {
                dict.Add(Guid.NewGuid().ToString(), "test");
            }
        }
    });

    Task.Factory.StartNew(() =>
    {
        while (running)
        {
            Console.WriteLine("Dictionary Count={0}", dict.Count);
            Thread.Sleep(1000);
        }
    });

    Task.Factory.StartNew(() =>
    {
        while (running)
        {
            lock (dict)
            {
                if (dict.Count > 0)
                {
                    string key = dict.Keys.First();
                    dict.Remove(key);
                }
            }
        }
    });

    Console.ReadLine();
    running = false;
}
~~~

## 結果 ##

    Dictionary Count=41
    Dictionary Count=839797
    Dictionary Count=1675168
    Dictionary Count=2521714
    Dictionary Count=3320396
    Dictionary Count=4220085

使用lock解決了同時存取操作的問題，但缺點也就是要是在每一個的操作加上lock判斷，若其中一個操作漏掉lock，就不能保證有thread-safe了，因此.NET 4.0 提供了"ConcurrentDictionary"類別，就是Dictionary thread-safe版本。操作大致上與Dictionary相同。

----------

## 使用ConcurrentDictionary ##

~~~csharp
static void Main(string[] args)
{
    ConcurrentDictionary<string, string> dict = new ConcurrentDictionary<string, string>();
    bool running = true;
            
    Task.Factory.StartNew(() =>
    {
        while (running)
        {
            dict.TryAdd(Guid.NewGuid().ToString(), "test");
        }
    });

    Task.Factory.StartNew(() =>
    {
        while (running)
        {
            Console.WriteLine("Dictionary Count={0}", dict.Count);
            Thread.Sleep(1000);
        }
    });

    Task.Factory.StartNew(() =>
    {
        while (running)
        {
            if (dict.Count > 0)
            {
                string key = dict.Keys.First();
                string dafault;
                dict.TryRemove(key, out dafault);
            }
        }
    });

    Console.ReadLine();
    running = false;
}
~~~

## 結果 ##

    Dictionary Count=0
    Dictionary Count=5643
    Dictionary Count=9400
    Dictionary Count=11969
    Dictionary Count=13998
    Dictionary Count=15994
    
----------

## Reference ##

- [http://blog.darkthread.net/post-2014-04-15-concurrentdictionary.aspx](http://blog.darkthread.net/post-2014-04-15-concurrentdictionary.aspx)
- [http://dotnetpattern.com/csharp-concurrentdictionary](http://dotnetpattern.com/csharp-concurrentdictionary)

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)[[Design Pattern系列文章]](http://iverson127.github.io/tags/#Design Pattern)