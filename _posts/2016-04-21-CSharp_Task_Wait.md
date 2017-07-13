---
layout: post
title: "C# 使用Task.Wait()來處理timeout機制"
author: "Iverson Hong"
modified: 2016-04-21
tags: [C#]
---

有時在執行一個function時，牽扯到通訊或是IO讀寫，內部可能因某些錯誤死在裡面，而造成外部呼叫的程式因收不到回傳值而卡死，這時會需要一個timeout的機制來避免。

C#有很多timeout的寫法，用Task.Wait()可輕鬆做到。

----------

要執行的function，假設執行須花費2秒:

~~~csharp
static void Print()
{
    for(int i = 0; i < 10; i++)
    {
        Console.WriteLine(i.ToString());
        Thread.Sleep(200);
    }
}
~~~

Client端程式認為超過3秒才算沒有回應

~~~csharp
Task t = Task.Factory.StartNew(() => Print());

var isFinish = t.Wait(3000);

Console.WriteLine(isFinish ? "Finsih" : "No Finish");
// Do something...
~~~

執行結果如下:

    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    Finsih

假現在認為超過0.5秒就算沒有回應，把wait時間調成500

~~~csharp
Task t = Task.Factory.StartNew(() => Print());

var isFinish = t.Wait(500);

Console.WriteLine(isFinish ? "Finsih" : "No Finish");
// Do something...
~~~

執行結果如下:

    0
    1
    2
    No Finish
    
----------

## Reference ##

 - [http://huan-lin.blogspot.com/2013/06/csharp-notes-multithreading-6-tpl.html](http://huan-lin.blogspot.com/2013/06/csharp-notes-multithreading-6-tpl.html)

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)