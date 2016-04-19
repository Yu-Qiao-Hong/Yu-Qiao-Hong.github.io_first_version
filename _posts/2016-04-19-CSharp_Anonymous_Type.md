---
layout: post
title: "C# Anonymous Type"
author: "Iverson Hong"
modified: 2016-04-19
tags: [C#]
---

## Anonymous Type ##

有時候在寫程式時，需要一個暫時的類別來儲存資料，但又不想特定去制定一個類別，這時候就可以使用Anonymous Type。

~~~csharp
var a = new { Name = "Iverson Hong", Age = 18 };
Console.WriteLine(a.Name); // Iverson Hong
Console.WriteLine(a.Age); // 18
~~~

### 使用Anonymous Type當作參數使用 ###

使用反射來取得數值:

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

clinet端呼叫:

~~~csharp
testMethod(new { Name = "Iverson Hong", Age = 18 });
~~~

----------

## Reference ##

 - [http://huan-lin.blogspot.com/2009/01/anonymous-types.html](http://huan-lin.blogspot.com/2009/01/anonymous-types.html)

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)