---
layout: post
title: "C# 介紹各種相等(Equality)的用法"
author: "Iverson Hong"
modified: 2016-09-20
tags: [C#, Effective C#]
---

在C#中若要使用"**相等(Equality)**"，常見的有4種方式，以下一一介紹：

~~~csharp
public static bool ReferenceEquals(object objA, object objB);

public static bool Equals(object objA, object objB);

public virtual bool Equals(object obj);

public static bool operator ==(MyClass left, MyClass right);
~~~

## static Object.ReferenceEquals() ##

此方法不需override，用來比較兩個object是否指向相同的reference位址，若是比較兩個value type物件，其結果必為**False**，原因為value type要轉成object(reference type)此動作為Boxing。

~~~csharp
static void Main(string[] args)
{
    int i = 5;
    int j = 5;

    if (Object.ReferenceEquals(i, j))
        Console.WriteLine("Never happens.");
    else
        Console.WriteLine("Always happens.");

    if (Object.ReferenceEquals(i, i))
        Console.WriteLine("Never happens.");
    else
        Console.WriteLine("Always happens.");
}
~~~

結果:

    Always happens.
    Always happens.

----------

## static Object.Equals() ##

此方法不需override，在runtime時比較兩個值是否相等，其實際運作為

~~~csharp
public static new bool Equals(object left, object right)
{
    // Check object identity
    if (Object.ReferenceEquals(left, right) )
        return true;

    // both null references handled above
    if (Object.ReferenceEquals(left, null) ||
    Object.ReferenceEquals(right, null))
        return false;

    return left.Equals(right);
}
~~~

簡單來說，兩個值為reference type，則與Object.ReferenceEquals()相同，比較是否指向相同的reference位址；若是兩個比較值為value type，則比較實際的內容值是否相等。

----------

## instance Object.Equals() ##

1. 此為可override的method。
 
2. reference type預設行為跟Object.ReferenceEquals()一樣，若此行為不是想要的則可override。

3. 若為boxing，例如object a = 1, b = 1。其內型態為int(value type)，此為多型，實際執行Int32.Equals(Object)。
 
3. value type則override比較是否為同類型且內容相等(預設利用反射，效率不高)。

接著以程式範例說明自定義的class如何override Object.Equals()，需記得實作IEquatable<T>介面中的Equals，才不會出現不預期的錯誤。

~~~csharp
public interface IEquatable<T>
{
    bool Equals(T other);
}
~~~

~~~csharp
public class Foo : IEquatable<Foo>
{
    public override bool Equals(object right)
    {
        // check null:
        // this pointer is never null in C# methods.
        if (object.ReferenceEquals(right, null))
            return false;

        if (object.ReferenceEquals(this, right))
            return true;

        // Discussed below.
        if (this.GetType() != right.GetType())
            return false;

        // Compare this type's contents here:
        return this.Equals(right as Foo);
    }

    #region IEquatable<Foo> Members
    public bool Equals(Foo other)
    {
        // elided.
        return true;
    }
    #endregion
}
~~~

在override Equals()時，裡面不應該出現丟出任何exception，只有相等True，不相等則回傳False兩種結果。第一個判斷檢查是否為null，再來判斷是否指向相同reference位址(相同位址內表內容一定一樣)。接著利用反射判斷是否為相同型態(使用this,而不直接使用Foo，因可能為繼承Foo的class，在繼承時會有兩邊結果不相等的問題產生，下面例子說明)，最後比較實際內容。

~~~csharp
class B : IEquatable<B>
{
    public override bool Equals(object right)
    {
        // ...
     
        B rightAsB = right as B;
        if(rightAsB == null)
            return false;
         
        // ...
    }
}

class C : B, IEquatable<C>
{
    public override bool Equals(object right)
    {
        // ...
     
        C rightAsC = right as C;
        if(rightAsC == null)
            return false;
         
        // ...
    }
}


// Client

B baseObj = new B();
C derivedObj = new C();

// Comparison 1
if(baseObj.Equals(derivedObj))
    Console.WriteLine("Equals");
else
    Console.WriteLine("Not Equals");
    
// Comparison 2
if(derivedObj.Equals(baseObj))
    Console.WriteLine("Equals");
else
    Console.WriteLine("Not Equals");
~~~

上面兩個比較結果，要嘛都回傳Equals，要嘛都回傳Not Equals，不應該兩個結果不同，但上面寫法在第二個比較必定回傳Not Equals，原因為B為base class，不可轉換為其衍生類別C，所以回傳false。

結論為若是自定義的value type，則需override Equal();若是reference type， Object.Equals()不能滿足你的需求才須自己override。

----------

## operator == () ##

只要是value type，皆需redefine operator == ()，其原因與instance Object.Equals()一樣為預設使用反射比較兩個實際內容造成效率不高；但在自定義reference type時，應盡量減少redefine operator ==()而應override Equals()使得語意更加清楚。

> (Notice that I didn’t say that you should write operator==() whenever you override instance Equals(). I said to write operator==() when you create value types. You should rarely override operator==() when you create reference types. The .NET Framework classes expect operator==() to follow reference semantics for all reference types.)

總結來說：

1. 正常情況下reference type比記憶體位置，string則為例外，它是比實際內容。

2. value type需override比較實際內容。

----------

## 總結 ##

介紹4種相等的方法：

- static Object.ReferenceEquals()與static Object.Equals()都提供了正確的判斷且與型態無關。

- value type：總是override instance Object.Equals()與operator == ()比較實際內容，以便得到較好的效能。

- reference type：當你不想只是比記憶體位置時才去override instance Object.Equals()。

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Effective C#系列文章]](http://yu-qiao-hong.github.io/tags/#Effective C#)