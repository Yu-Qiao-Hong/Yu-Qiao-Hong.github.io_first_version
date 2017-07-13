---
layout: post
title: "C# 使用as, is operator進行類型轉換"
author: "Iverson Hong"
modified: 2016-07-10
tags: [C#, Effective C#]
---

寫程式時有時必須轉換類型進行操作，以下為C#類型轉換相關介紹：

## as operator ##

1. 轉換時不會產生exception，若無法轉換則回傳null，使用後應判斷是否等於null的來檢查是否成功轉換。

2. 只能轉換成reference type class，原因為若轉換失敗回傳null，但value type不能為null，故compiler不會過。

範例1:

~~~csharp
private void button1_Click(object sender, EventArgs e)
{
    object s1 = "Iverson Hong";
    string s2 = s1 as string;
    if (s2 != null)
    {
        Console.WriteLine(s2);
    }
    else
    {
        Console.WriteLine("Incompatible cast");
    }

    StringBuilder s3 = s1 as StringBuilder;
    if (s3 != null)
    {
        Console.WriteLine(s3);
    }
    else
    {
        Console.WriteLine("Incompatible cast");
    }
}
~~~

結果：

    Iverson Hong
    Incompatible cast


範例2:

~~~csharp
class MyClass : MyBase, MyInterface
{
    public void MyInterfaceMethod()
    {
        Console.WriteLine("This is MyClass:MyInterfaceMethod()");
    }
}

class MyBase
{
    public virtual void MyBaseMethod()
    {
        Console.WriteLine("This is MyBase:MyBaseMethod()");
    }
}

interface MyInterface
{
    void MyInterfaceMethod();
}
~~~

~~~csharp
private void button2_Click(object sender, EventArgs e)
{
    MyClass myClass = new MyClass();
    MyInterface myInterface = myClass as MyInterface;
    if (myInterface != null)
    {
        myInterface.MyInterfaceMethod();
    }
    else
    {
        Console.WriteLine("Incompatible cast");
    }

    MyBase myBase = myClass as MyBase;
    if (myBase != null)
    {
        myBase.MyBaseMethod();
    }
    else
    {
        Console.WriteLine("Incompatible cast");
    }
}
~~~

結果：

    This is MyClass:MyInterfaceMethod()
    This is MyBase:MyBaseMethod()
    
----------

## is operator ##

1. 不進行轉換，只是判斷是否可轉型，回傳bool，不會產生exception

2. 常與強制轉型一起使用

範例1:

~~~csharp
private void button3_Click(object sender, EventArgs e)
{
    int myInt = 0;
    bool compatible = myInt is int;
    System.Console.WriteLine("myInt is int = " + compatible);

    compatible = myInt is long;
    System.Console.WriteLine("myInt is long = " + compatible);

    compatible = myInt is float;
    System.Console.WriteLine("myInt is float = " + compatible);

    compatible = myInt is object;
    System.Console.WriteLine("myInt is object = " + compatible);
}
~~~

結果：

    myInt is int = True
    myInt is long = False
    myInt is float = False
    myInt is object = True

範例2:

~~~csharp
private void button4_Click(object sender, EventArgs e)
{
    object s1 = "Iverson Hong";
    if (s1 is string)
    {
        string s2 = (string)s1;
        Console.WriteLine(s2);
    }
    else
    {
        Console.WriteLine("Incompatible cast");
    }

    if (s1 is StringBuilder)
    {
        StringBuilder s3 = (StringBuilder)s1;
        Console.WriteLine(s3);
    }
    else
    {
        Console.WriteLine("Incompatible cast");
    }
}
~~~

結果：

    Iverson Hong
    Incompatible cast

範例3:

~~~csharp
private void button5_Click(object sender, EventArgs e)
{
    MyClass myClass = new MyClass();
    if (myClass is MyInterface)
    {
        ((MyInterface)myClass).MyInterfaceMethod();
    }
    else
    {
        Console.WriteLine("Incompatible cast");
    }

    if (myClass is MyBase)
    {
        ((MyBase)myClass).MyBaseMethod();
    }
    else
    {
        Console.WriteLine("Incompatible cast");
    }
}
~~~

結果：

    This is MyClass:MyInterfaceMethod()
    This is MyBase:MyBaseMethod()

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Effective C#系列文章]](http://yu-qiao-hong.github.io/tags/#Effective C#)