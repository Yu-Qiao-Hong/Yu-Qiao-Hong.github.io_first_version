---
layout: post
title: "C# 介紹各種相等(Equality)的用法"
author: "Iverson Hong"
modified: 2016-07-16
tags: [C#, Effective C#]
---

在C#中若要使用"**相等(Equality)**"，常見的有4種方式，以下一一介紹：

~~~csharp
public static bool ReferenceEquals(object objA, object objB);

public static bool Equals(object objA, object objB);

public virtual bool Equals(object obj);

public static bool operator ==(MyClass left, MyClass right);
~~~

## Object.ReferenceEquals() ##

此方法不需override，用來比較兩個object是否指向相同的reference位址，若是比較兩個value type物件，其結果必為**False**，原因為value type要轉成object(reference type)此動作為Boxing。

~~~csharp
private void button1_Click(bject sender, EventArgs e)
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

簡單來說，兩個值為reference type，則與Object.ReferenceEquals()相同，比較是否指向相同的reference位址；

若是兩個比較值為value type，則比較實際的內容值是否相等。

----------

## instance Object.Equals() ##

此為可override的method，預設行為跟Object.ReferenceEquals()一樣，但若為value type時則需要override Object.Equals()，比較是否同類型且內容相等(預設利用反射，效率不高)。

若要比較兩個reference type，是比較實際內容是否相等而不是比較是否指向相同reference，這時就有override的必要性了。

接著以自定義的class說明如何override Object.Equals()，在override Object.Equals()方法時，也必須實作IEquatable<T>，才不會出現不預期的錯誤。

~~~csharp
public interface IEquatable<T>
{
    bool Equals(T other);
}
~~~

舉例說明：

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

在override Equals()時，裡面不應該出現丟出任何exception，只有相等True，不相等則回傳False兩種。

第一個判斷檢查是否為null，再來判斷是否指向相同reference位址(相同位址內表內容一定一樣)。

接著利用反射判斷是否為相同型態(使用this,而不直接使用Foo，因可能為繼承Foo的class。利用反射而不用as或is operator，因在繼承時會有兩邊結果不相等的問題產生，下面例子說明)

最後比較實際內容。

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
if(baseObj.Equal(derivedObj))
    Console.WriteLine("Equals");
else
    Console.WriteLine("Not Equals");
    
// Comparison 2
if(derivedObj.Equal(baseObj))
    Console.WriteLine("Equals");
else
    Console.WriteLine("Not Equals");
~~~

上面兩個比較結果，要嘛都回傳Equals，要嘛都回傳Not Equals，不應該兩個結果不同，但上面寫法在第二個比較必定回傳Not Equals，原因為B為base class，不可轉換為其衍生類別C，所以回傳false。

結論為若是自定義的value type，則需override Equal();若是reference type， Object.Equals()不能滿足你的需求才須自己override。

----------

## operator ==() ##

在使用自定義value type時，需redefine operator ==()，其原因與instance Object.Equals()一樣為預設使用反射比較兩個實際內容造成效率不高；
但在自定義reference type時，應盡量減少redefine operator ==()而應override Equals()使得語意更加清楚。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)
[[Effective C#系列文章]](http://iverson127.github.io/tags/#Effective C#)