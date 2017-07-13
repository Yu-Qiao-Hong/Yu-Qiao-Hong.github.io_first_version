---
layout: post
title: "C# 取得是否有使用者權限"
author: "Iverson Hong"
modified: 2016-05-19
tags: [C#]
---

取得執行程式時是否有Admin權限:

## 程式碼: ##

~~~csharp
private bool CheckIsAdmin()
{
    WindowsIdentity identity = WindowsIdentity.GetCurrent();
    WindowsPrincipal principal = new WindowsPrincipal(identity);
    bool isAdmin = principal.IsInRole(WindowsBuiltInRole.Administrator);
    return isAdmin;
}
~~~

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)