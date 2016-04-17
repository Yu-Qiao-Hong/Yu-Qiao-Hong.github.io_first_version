---
layout: post
title: "C# 取得執行檔路徑"
author: "Iverson Hong"
modified: 2016-04-17
tags: [C#]
---

在寫程式的時候常常需要抓取現在執行檔的路徑，以下做一些整理:

假設現有專案執行檔的絕對路徑為:

C:\WindowsForms\bin\Debug\WindowsForms.exe

可以以下方法取得路徑

~~~csharp
string str = System.Environment.CurrentDirectory;
str = System.IO.Directory.GetCurrentDirectory();
str = System.AppDomain.CurrentDomain.BaseDirectory;
str = System.AppDomain.CurrentDomain.SetupInformation.ApplicationBase;
str = System.Diagnostics.Process.GetCurrentProcess().MainModule.FileName;

// only use in windows form
str = System.Windows.Forms.Application.StartupPath;
str = System.Windows.Forms.Application.ExecutablePath;
str = this.GetType().Assembly.Location;
~~~

直接在visual studio中執行，可得到以下結果:
    
    C:\WindowsForms\bin\Debug
    C:\WindowsForms\bin\Debug
    C:\WindowsForms\bin\Debug\
    C:\WindowsForms\bin\Debug\
    C:\WindowsForms\bin\Debug\WindowsForms.vshost.exe
    
    C:\WindowsForms\bin\Debug
    C:\WindowsForms\bin\Debug\WindowsForms.EXE
    C:\WindowsForms\bin\Debug\WindowsForms.exe

相同的程式若使用cmd在D:下執行，結果為:

    D:\
    D:\
    C:\WindowsForms\bin\Debug\
    C:\WindowsForms\bin\Debug\
    C:\WindowsForms\bin\Debug\WindowsForms.exe
    
    C:\WindowsForms\bin\Debug
    C:\WindowsForms\bin\Debug\WindowsForms.exe
    C:\WindowsForms\bin\Debug\WindowsForms.exe
    
兩者比較可以得知，有些是抓取當下執行的位置；有些則是取得執行的絕對路徑，在使用時必須特別注意。

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)