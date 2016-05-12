---
layout: post
title: "SQLite安裝流程"
author: "Iverson Hong"
modified: 2016-05-12
tags: [開發工具, SQLite]
---

在開發軟體時，常有機會使用到資料庫儲存資料，但又不是開發幾千幾萬人用的系統，沒有開服務架Server的必要性,這時可用輕量級的資料庫"**SQLite**"。

下面為在Visual Studio下使用SQLite的紀錄：

## 安裝SQLite ##

1. 開啟Visual Studio(範例為VS2013)
2. 選擇TOOLS -> NuGet Package Manager
3. 選擇Package Manager Console

![](..\images\postImage\SQLite_Install\001.png)

![](..\images\postImage\SQLite_Install\002.png)

在Package Manager Console中輸入: "**Install-Package System.Data.SQLite**"

則會根據現在開啟的專案幫你安裝SQLite的相關套件：

![](..\images\postImage\SQLite_Install\003.png)

安裝完後會看到專案目錄有以下檔案

![](..\images\postImage\SQLite_Install\004.png)

----------

## Reference ##

- [https://www.nuget.org/packages/System.Data.SQLite](https://www.nuget.org/packages/System.Data.SQLite)

----------

[[SQLite系列文章]](http://iverson127.github.io/tags/#SQLite)
[[開發工具系列文章]](http://iverson127.github.io/tags/#開發工具)