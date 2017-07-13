---
layout: post
title: "C# Null Object Design Pattern"
author: "Iverson Hong"
modified: 2016-04-18
tags: [C#, Design Pattern]
---

### 建立一個interface或abstract class ###

~~~csharp
interface ICar
{
    void Run();
}
~~~

### 接著定幾個class繼承上述的介面: ###

~~~csharp
class FastCar : ICar
{
    public void Run()
    {
        Console.WriteLine("Run fast");
    }
}
~~~

~~~csharp
class SlowCar : ICar
{
    public void Run()
    {
        Console.WriteLine("Run slow");
    }
}
~~~

### 寫個factory用來產生實體: ###

~~~csharp
ICar CarFactory(int i)
{
    if (i == 0)
        return new FastCar();
    else if (i == 1)
        return new SlowCar();
    else
        return null;
}
~~~

### client端程式: ###

~~~csharp
ICar MyCar = CarFactory(0);
MyCar.Run(); // Run fast
MyCar = CarFactory(1);
MyCar.Run(); // Run slow
MyCar = CarFactory(2);
MyCar.Run();
~~~

執行到最後一個MyCar.Run()時，發生**Exception:System.NullReferenceException**

原因為最後一次的CarFactory並沒有產生實體，所以在呼叫Run()時會出現exception，那要怎麼解決呢?這時候就可以使用Null Object Pattern。

----------

## Null Object Pattern ##

簡單來講就是產生一個object，其內容不做任何事情，功能只是讓client端呼叫時不會發生錯誤。

### 建立一個null class ###

一樣繼承ICar interface， Run()裡面不做任何事。

~~~csharp
class NullCar: ICar
{
    public void Run()
    {
        // Do nothing
    }
}
~~~

### 更改原來的factory ###

將原來的return null改寫成**return new NullCar()**

新的factory變成:

~~~csharp
ICar CarFactory(int i)
{
    if (i == 0)
        return new FastCar();
    else if (i == 1)
        return new SlowCar();
    else
        return new NullCar();
}
~~~

相同的client端程式再呼叫一次則可正常運作，不會有任何錯誤出現。

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Design Pattern系列文章]](http://yu-qiao-hong.github.io/tags/#Design Pattern)