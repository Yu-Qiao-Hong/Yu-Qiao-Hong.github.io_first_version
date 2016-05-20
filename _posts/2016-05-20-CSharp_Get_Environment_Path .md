---
layout: post
title: "C# 取得系統環境路徑"
author: "Iverson Hong"
modified: 2016-05-20
tags: [C#]
---

在C++程式中可用SHGetFolderPath()取得系統環境路徑，換到C#，除了可用import dll方式之外，還可用C#提供的方法。

## 程式碼: ##

~~~csharp
string str = Environment.GetFolderPath(Environment.SpecialFolder.Desktop); // C:\Users\user\Desktop
str = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments); // C:\Users\user\Documents
str = Environment.GetFolderPath(Environment.SpecialFolder.ProgramFiles); // C:\Program Files (x86)
str = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData); // C:\Users\user\AppData\Roaming
~~~

----------

## Reference ##

- [https://msdn.microsoft.com/zh-tw/library/system.environment.specialfolder(v=vs.100).aspx](https://msdn.microsoft.com/zh-tw/library/system.environment.specialfolder(v=vs.100).aspx)
- [https://msdn.microsoft.com/zh-tw/library/bb762494.aspx](https://msdn.microsoft.com/zh-tw/library/bb762494.aspx)
- 
----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)