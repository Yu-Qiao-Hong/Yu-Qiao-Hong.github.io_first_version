---
layout: post
title: "C# Anonymous Type"
author: "Iverson Hong"
modified: 2016-6-30
tags: [C#]
---

## Anonymous Type ##

有時候在寫程式時，需要一個暫時的類別來儲存資料，但又不想特定去制定一個類別，這時候就可以使用Anonymous Type。

### 直接使用 ###

~~~csharp
var a = new { Name = "Iverson Hong", Age = 18 };
Console.WriteLine(a.Name); // Iverson Hong
Console.WriteLine(a.Age); // 18
~~~

### 當作參數使用 ###

clinet端呼叫:

~~~csharp
testMethod(new { Name = "Iverson Hong", Age = 18 });
~~~

Anonymous Type繼承自"**System.Object**"，故method參數以object為其類別。可使用反射來取得property數值:

~~~csharp
void testMethod(object a)
{
    PropertyInfo[] propertyInfo = a.GetType().GetProperties();

    for(int i = 0; i < propertyInfo.Length; i++)
    {
        Console.WriteLine(propertyInfo[i].GetValue(a));
    }
}
~~~

這邊需要注意的是Anonymous Typen所有的的property都是**read only**，假設改寫上述method並重新assign值則會出現exception

~~~csharp
static void testMethod(object a)
{
    dynamic b = a;
    b.Age = 20;
    PropertyInfo[] propertyInfo = a.GetType().GetProperties();

    for (int i = 0; i < propertyInfo.Length; i++)
    {
        Console.WriteLine(propertyInfo[i].GetValue(a));

    }
}
~~~

執行到

~~~csharp
b.Age = 20;
~~~

則跳出exception視窗

![](..\images\postImage\CSharp_Anonymous_Type\001.png)

----------

## Reference ##

 - [http://huan-lin.blogspot.com/2009/01/anonymous-types.html](http://huan-lin.blogspot.com/2009/01/anonymous-types.html)
 - [http://www.c-sharpcorner.com/UploadFile/ff2f08/anonymous-types-in-C-Sharp/](http://www.c-sharpcorner.com/UploadFile/ff2f08/anonymous-types-in-C-Sharp/)

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)