---
layout: post
title: "C# yield"
author: "Iverson Hong"
modified: 2016-09-17
tags: [C#]
---

yield 翻譯成中文有退讓、讓位的意思。以程式的觀點來看，它就是先把結果丟出去給外部處理，以實際範例說明：

有一內容為1-10的int集合，使用一般以及yield的方式取得其平方集合。

~~~csharp
static void Main(string[] args)
{
    List<int> numberList = new List<int>();
    for (int i = 1; i < 11; i++)
        numberList.Add(i);

    IEnumerable<int> commonList = CommonReturn(numberList);
    foreach (int num in commonList)
        Console.WriteLine(num);

    IEnumerable<int> yieldList = YieldReturn(numberList);
    foreach (int num in yieldList)
        Console.WriteLine(num);
}
~~~

~~~csharp
private static IEnumerable<int> CommonReturn(List<int> numberList)
{
    List<int> returnList = new List<int>();
    foreach (var num in numberList)
    {
        returnList.Add(num * num);
    }

    return returnList;
}
~~~

~~~csharp
private static IEnumerable<int> YieldReturn(List<int> numberList)
{
    foreach (var num in numberList)
    {
        yield return num * num;
    }
}
~~~

兩種方式結果都一樣，但yield的流程不同於一般流程，以圖示來解釋

![](..\images\postImage\CSharp_Yield\001.png)

步驟2每一次執行foreach都會從YieldReturn()內取出一個平方值再印出其內容。因此執行順序為1->2->3->4->2->3->4...直到numberList取完。

yield的回傳值必須為IEnumerable、IEnumerator、IEnumerable<T>或IEnumerator<T>。若自訂的class繼承IEnumerable卻不使用yield的話，必須自己實作IEnumerator。

~~~csharp
public interface IEnumerable
{
    IEnumerator GetEnumerator();
}
~~~

~~~csharp
public interface IEnumerator
{
    object Current { get; }
    bool MoveNext();
    void Reset();
}
~~~

![](..\images\postImage\CSharp_Yield\002.png)

----------

## Reference ##

 - [https://dotblogs.com.tw/hatelove/archive/2012/05/10/introducing-foreach-ienumerable-ienumerator-yield-iterator.aspx](https://dotblogs.com.tw/hatelove/archive/2012/05/10/introducing-foreach-ienumerable-ienumerator-yield-iterator.aspx "https://dotblogs.com.tw/hatelove/archive/2012/05/10/introducing-foreach-ienumerable-ienumerator-yield-iterator.aspx")
 - 
----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)