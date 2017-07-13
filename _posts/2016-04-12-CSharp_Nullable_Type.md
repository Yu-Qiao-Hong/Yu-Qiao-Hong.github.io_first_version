---
layout: post
title: "C# Nullable Type"
author: "Iverson Hong"
modified: 2016-04-12
tags: [C#]
---
## 使用時機 ##

操作資料庫或是定義一資料結構時，使用到C#的Value type，例如:

~~~csharp
class Test
{
    public int x;
    public int y;
}
~~~

若是從資料庫撈不到資料，可能此變數x就不去assign值，但是在讀取的時候還是讀取的到

~~~csharp
Test c = new Test();
Console.WriteLine(c.x.ToString());  // 0
~~~

明明是沒有assign值阿，並不是int的初始值0阿!!

啊!我把x初始化給一個-9999的值代表沒有被assign(此方法可能很常見XD)

但這方法很不好，很明顯你會在你後面程式加一段

~~~csharp
if(c.x == -9999) // not assignment
{
    //...
}
~~~

若好死不剛好就剛好出現-9999那就又頭大了

這個時候就是nullable type該出場的時候了。

----------

## Nullable Type ##

有兩種宣告方式，宣告完後初始值皆為null

第一種:

~~~csharp
Nullable<int> x;
~~~

第二種:

~~~csharp
int? x;
~~~

我個人是傾向使用第二種方式，簡單明瞭。

後面判斷時可用"**HasValue**" property得知是否有值

~~~csharp
Test c = new Test();
if (c.x.HasValue)
{
    Console.WriteLine(c.x.ToString());
}
else
{
    Console.WriteLine("not assign");
}
~~~

----------

## Nullable type assign to non-nullable type ##

若現在我要把x的值assign給y,

~~~csharp
y = x; // compiler error
~~~

因為y不是nullable type,所以在使用時須強制轉換

~~~csharp
y = (int) x;
~~~

這邊需要注意的是，若x為null的話會產生Exception。

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)