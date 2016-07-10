---
layout: post
title: "C# "
author: "Iverson Hong"
modified: 2016-07-10
tags: [C#, Effective C#]
---

寫程式時有時必須轉換類型進行操作，以下介紹C#類型轉換相關用法：

## as operator ##

1. 轉換時不會產生exception，若無法轉換則回傳null，因此使用後應判斷是否等於null的來檢查是否成功轉換。

2. 只能轉換為reference type class，原因為若轉換失敗回傳null，但value type不能為null，故compiler不會過。

範例1:

~~~csharp
object s1 = "Iverson Hong";
string s2 = s1 as string;
if(s2 != null)
    Console.WriteLine(s2);
else
    Console.WriteLine("Incompatible cast");

StringBuilder s3 = s1 as StringBuilder;
if (s3 != null)
    Console.WriteLine(s3);
else
    Console.WriteLine("Incompatible cast"); 
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
private void button1_Click(object sender, EventArgs e)
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
int myInt = 0;
bool compatible = myInt is int;
System.Console.WriteLine("myInt is int = " + compatible);

compatible = myInt is long;
System.Console.WriteLine("myInt is long = " + compatible);

compatible = myInt is float;
System.Console.WriteLine("myInt is float = " + compatible);

compatible = myInt is object;
System.Console.WriteLine("myInt is object = " + compatible);
~~~

結果：

    myInt is int = True
    myInt is long = False
    myInt is float = False
    myInt is object = True

範例2:

~~~csharp
object s1 = "Iverson Hong";
if(s1 is string)
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
~~~

結果：

    Iverson Hong
    Incompatible cast

範例3:

~~~csharp
private void button2_Click(object sender, EventArgs e)
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

## 總結 ##

as, is operator是在程式runtime time時執行，不會產生exception(強制傳型失敗則會丟出InvalidCastException)。

- 由object轉成reference type，用as operator。

- 由object轉成value type，用is operator判斷再用強制轉換。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)
[[Effective C#系列文章]](http://iverson127.github.io/tags/#Effective C#)