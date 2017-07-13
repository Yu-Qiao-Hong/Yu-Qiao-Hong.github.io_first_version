---
layout: post
title: "C# 使用C++ DLL"
author: "Iverson Hong"
modified: 2016-05-21
tags: [C#]
---

在寫C#時，可能遇到舊的專案已C++來開發，而C++專案內使用了許多windows相關的DLL，要如何在C#中也能像C++一樣使用這些DLL呢?以SHGetFolderPath()為例

其C++中定義為:

~~~cpp
HRESULT SHGetFolderPath(
  _In_  HWND   hwndOwner,
  _In_  int    nFolder,
  _In_  HANDLE hToken,
  _In_  DWORD  dwFlags,
  _Out_ LPTSTR pszPath
);
~~~

可以看到許多的型態是C#中沒有支援的，因此必須轉換過後才能使用，以下為C#與C++資料型態對應表:

![](..\images\postImage\CSharp_Import_Cpp_DLL\001.png)

![](..\images\postImage\CSharp_Import_Cpp_DLL\002.png)

![](..\images\postImage\CSharp_Import_Cpp_DLL\003.png)

![](..\images\postImage\CSharp_Import_Cpp_DLL\004.png)

![](..\images\postImage\CSharp_Import_Cpp_DLL\005.png)

C#中使用前須先定義如下:

~~~csharp
using System.Runtime.InteropServices;

[DllImport("shell32.dll")]
static extern int SHGetFolderPath(UIntPtr hwndOwner,
    int nFolder,
    UIntPtr hToken,
    uint dwFlags,
    StringBuilder pszPath);
~~~

呼叫方式跟一般一樣:

~~~csharp
private void button1_Click(object sender, EventArgs e)
{
    StringBuilder sb = new StringBuilder();
    int retVal = SHGetFolderPath(UIntPtr.Zero, 0x001a, UIntPtr.Zero, 0, sb);
    Debug.WriteLine(sb);
}
~~~

結果
    
    C:\Users\user\AppData\Roaming

----------

## Reference ##

- [https://msdn.microsoft.com/zh-tw/library/windows/desktop/bb762181(v=vs.85).aspx](https://msdn.microsoft.com/zh-tw/library/windows/desktop/bb762181(v=vs.85).aspx)
- [http://www.kobashicomputing.com/calling-native-windows-functions-from-your-managed-code](http://www.kobashicomputing.com/calling-native-windows-functions-from-your-managed-code)
- [http://www.kobashicomputing.com/mapping-data-types-c-c-net](http://www.kobashicomputing.com/mapping-data-types-c-c-net)
- 

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)