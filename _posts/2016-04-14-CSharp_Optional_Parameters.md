---
layout: post
title: "C# Optional parameters"
author: "Iverson Hong"
modified: 2016-04-14
tags: [C#]
---
## 使用時機 ##

有一method帶入5位數字，加總起來代表五位數:

~~~csharp
void test(int a, int b, int c, int d, int e)
{
	int total = a * 10000 + b * 1000 + c * 100 + d * 10 + e;
	Console.WriteLine(total);
}
~~~

Clint在呼叫時需寫:

~~~csharp
test(1, 2, 3, 4, 5) // 12345
~~~

假設想要顯示出1000，則需呼叫

~~~csharp
test(0, 1, 0, 0, 0) // 1000
~~~

要輸入一堆0好麻煩喔~~

一不小心又會輸入錯位數!

沒關係!還有別的方法

----------

## Optional parameters ##

修改一下上面的method，把每個參數加上default value 0

~~~csharp
void test(int a = 0, int b = 0, int c = 0, int d = 0, int e = 0)
{
	int total = a * 10000 + b * 1000 + c * 100 + d * 10 + e;
	Console.WriteLine(total);
}
~~~

這個時候client要呼叫時可直接指定要對哪個參數給值

~~~csharp
test(b : 1) // 1000
~~~

輸出結果一樣，在method有許多參數時此方法非常好用。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)