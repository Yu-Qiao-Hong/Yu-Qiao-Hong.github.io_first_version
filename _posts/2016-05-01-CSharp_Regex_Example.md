---
layout: post
title: "C# 正規表示式使用範例"
author: "Iverson Hong"
modified: 2016-05-01
tags: [C#]
---

[接續上篇](http://iverson127.github.io/CSharp_Regex/)，以下為常見的正規表示式的範例：

### Unsigned Integer ###

~~~csharp
// 123
^\d*$ 
~~~

### Signed Integer ###

~~~csharp
// -123
^([+-]?)\d*$
~~~

### Float ###

~~~csharp
// 123.456
^([+-]?)\d*\.\d*$
~~~

### IP Address ###

~~~csharp
// 192.168.1.1
^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}$
~~~

### Taiwan ID ###

~~~csharp
// A123456789
^([A-Z]|[a-z])\d{9}$
~~~

### Cell phone number ###

~~~csharp
// 0912-345-678
^[0-9]{4}\-[0-9]{3}\-[0-9]{3}$
~~~

### Time ###

~~~csharp
// 23:12:17(HH:MM:SS)
^([0-1][0-9]|2[0-3])\:[0-5][0-9]\:[0-5][0-9]$
~~~

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)