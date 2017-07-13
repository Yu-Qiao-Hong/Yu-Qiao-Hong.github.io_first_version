---
layout: post
title: "C# 使用using和try/finally來釋放資源"
author: "Iverson Hong"
modified: 2016-09-25
tags: [C#, Effective C#]
---

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

在C++程式中可使用destructor來釋放資源，而C#中則透過GC(Garbage Collector)來自動回收資源，GC為CLR內的機制之一，凡是在CLR所運作中的程式都可稱為managed code，不是在CLR運作中的程式就稱為unmanaged code(stream、與資料庫的連結、COM物件...等)。也就是說若是使用了unmanaged code後並沒有釋放資源，則會等到程式結束時才釋放。

在C#中使用unmanaged code想要釋放資源必須要使用者明確的寫出釋放資源的方法，也就是使用Finalize()和Dispose()。呼叫Finalize()並不會馬上執行而是交給GC來處理，這邊不多做討論；而Dispose()則可是明確通知GC進行資源回收。可透過using statement以及try/finally來呼叫Dispose()。

以執行sql程式為例：

~~~csharp
public void ExecuteCommand(string connString, string commandString)
{
    SqlConnection myConnection = new SqlConnection(connString);
    SqlCommand mySqlCommand = new SqlCommand(commandString, myConnection);

    myConnection.Open();
    mySqlCommand.ExecuteNonQuery();
}
~~~

SqlConnection以及SqlCommand的實體會一直存在記憶體直到程式結束。因此可再修改為：

~~~csharp
public void ExecuteCommand(string connString, string commandString)
{
    SqlConnection myConnection = new SqlConnection(connString);
    SqlCommand mySqlCommand = new SqlCommand(commandString, myConnection);

    myConnection.Open();
    mySqlCommand.ExecuteNonQuery();

    mySqlCommand.Dispose();
    myConnection.Dispose();
}
~~~

還是有可能發生問題，若是在呼叫Dispose()之前發生exception，則永遠不會釋放記憶體，因此可使用using statement來確保Dispose()一定會被呼叫到：

~~~csharp
public void ExecuteCommand(string connString, string commandString)
{
    using (SqlConnection myConnection = new SqlConnection(connString))
    {
        using (SqlCommand mySqlCommand = new SqlCommand(commandString, myConnection))
        {
            myConnection.Open();
            mySqlCommand.ExecuteNonQuery();
        }
    }
}
~~~

也可用try/finally來改寫上述程式，其IL內容與使用using statement完全一樣：

~~~csharp
public void ExecuteCommand(string connString, string commandString)
{
    SqlConnection myConnection = null;
    SqlCommand mySqlCommand = null;
    try
    {
        myConnection = new SqlConnection(connString);
        try
        {
            mySqlCommand = new SqlCommand(commandString, myConnection);
            myConnection.Open();
            mySqlCommand.ExecuteNonQuery(); 
        }
        finally
        {
            if (mySqlCommand != null)
                mySqlCommand.Dispose();
        }
    }
    finally
    {
        if (myConnection != null)
            myConnection.Dispose();
    }
}
~~~

也就是一個using statement可以用一個try/finally來改寫。

----------

若using statement內的類別沒有支援IDisposable interface，在編譯時就會產生錯誤：

~~~csharp
using (string msg = "This is a message")
    Console.WriteLine(msg);
~~~

![](..\images\postImage\CSharp_Effective_Item15\001.png)

----------

在編譯時期就知道有支援IDisposable interface的型態才可使用using statement，在編譯時期無法確認則無法使用：

~~~csharp
using (object obj = Factory.CreateResource())
    Console.WriteLine(obj.ToString());
~~~

但可透過[as statement轉型](http://yu-qiao-hong.github.io/CSharp_Effective_Item3/)後使用

~~~csharp
object obj = Factory.CreateResource();
using (obj as IDisposable)
    Console.WriteLine(obj.ToString());
~~~

----------

## Reference ##

- [http://code2study.blogspot.tw/2011/09/c.html](http://code2study.blogspot.tw/2011/09/c.html "http://code2study.blogspot.tw/2011/09/c.html")

- [http://sedc.pixnet.net/blog/post/14184015-c%23---%E8%B3%87%E6%BA%90%E9%87%8B%E6%94%BE%E7%9A%84%E8%A7%80%E5%BF%B5%E6%95%B4%E7%90%86](http://sedc.pixnet.net/blog/post/14184015-c%23---%E8%B3%87%E6%BA%90%E9%87%8B%E6%94%BE%E7%9A%84%E8%A7%80%E5%BF%B5%E6%95%B4%E7%90%86 "http://sedc.pixnet.net/blog/post/14184015-c%23---%E8%B3%87%E6%BA%90%E9%87%8B%E6%94%BE%E7%9A%84%E8%A7%80%E5%BF%B5%E6%95%B4%E7%90%86")

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)
[[Effective C#系列文章]](http://yu-qiao-hong.github.io/tags/#Effective C#)