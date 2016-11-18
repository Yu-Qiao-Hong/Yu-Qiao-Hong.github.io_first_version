---
layout: post
title: "C# 字串轉列舉"
author: "Iverson Hong"
modified: 2016-07-24
tags: [C#]
---

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

列舉轉字串很簡單：

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

~~~csharp
enum Days
{
    Sun,
    Mon,
    tue,
    Wed, 
    thu, 
    Fri, 
    Sat
};

private void button1_Click(object sender, EventArgs e)
{
    string str = Days.Fri.ToString();
    Console.WriteLine(str); // Fri
}
~~~

字串轉列舉：

~~~csharp
private void button2_Click(object sender, EventArgs e)
{
    string str = "Sun";
    Days day = (Days)Enum.Parse(typeof(Days), str); 
}
~~~

若輸入的字串沒辦法轉為列舉型態則會丟出Exception：

~~~csharp
private void button3_Click(object sender, EventArgs e)
{
    string str = "Dec";
    Days day = (Days)Enum.Parse(typeof(Days), str); 
}
~~~

![](..\images\postImage\CSharp_String_To_Enum\001.png)

    
安全的作法為：

~~~csharp
private void button4_Click(object sender, EventArgs e)
{
    Days day;
    bool success = Enum.TryParse("Dec", true, out day);
    if (success)
    {
        Console.WriteLine("Parse failure");
    }
    else
    {
        Console.WriteLine("Parse Success");
    }
}
~~~


需注意的是，使用TryParse()後要判斷是否轉換成功，因enum定義出來的型態為value type，也就是會有初始值(為enum所定義的第一個值)，所以不判斷直接取值可能造成後續的錯誤。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)