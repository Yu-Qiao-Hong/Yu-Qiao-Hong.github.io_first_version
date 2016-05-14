---
layout: post
title: "NLog安裝流程"
author: "Iverson Hong"
modified: 2016-04-27
tags: [開發工具, NLog]
---

當開發的程式架構越來越大，越來越不好debug時，這時常常會需要使用到Log檔來記錄最後程式死在哪裡?

後來搜尋發現有一套非常強大的log套件，專門給.NET使用，以下做些紀錄：

## 安裝NLog ##

1. 開啟Visual Studio(範例為VS2013)
2. 選擇TOOLS -> NuGet Package Manager
3. 選擇Package Manager Console

![](..\images\postImage\NLog_Install\001.png)

![](..\images\postImage\NLog_Install\002.png)

在Package Manager Console中輸入: "**Install-Package NLog.Config**"

則會根據現在開啟的專案幫你安裝相對應的NLog版本：

![](..\images\postImage\NLog_Install\003.png)

安裝完後會看到專案目錄有以下檔案

![](..\images\postImage\NLog_Install\004.png)

![](..\images\postImage\NLog_Install\005.png)

----------

## Reference ##

- [http://nlog-project.org/download/](http://nlog-project.org/download/)

----------

[[NLog系列文章]](http://iverson127.github.io/tags/#NLog)
[[開發工具系列文章]](http://iverson127.github.io/tags/#開發工具)