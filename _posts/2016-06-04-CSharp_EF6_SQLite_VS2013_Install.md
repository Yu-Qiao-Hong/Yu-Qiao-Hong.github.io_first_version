---
layout: post
title: "C# 在VS2013環境安裝EF6來操作SQLite"
author: "Iverson Hong"
modified: 2016-06-04
tags: [C#, SQLite, Entity Framework 6, 開發工具]
---

因工作使用到資料庫，預期資料庫table數量會越來越龐大，複雜性也會越來越高，不想再使用一般串SQL字串的方式來操作資料庫，C#有提供可直接把資料庫的內容轉成程式物件，在操作上無需使用SQL語法:

## 開發環境 ##

- Windows 10 X64
- Visual Studio Professional 2013

----------

## 安裝步驟 ##

1. 下載[Entity Framework 6 Tools for Visual Studio 2012 & 2013](https://www.microsoft.com/en-us/download/details.aspx?id=40762)

2. 到SQLite官網下載[Setups for 32-bit Windows (.NET Framework 4.5.1)](https://system.data.sqlite.org/downloads/1.0.101.0/sqlite-netFx451-setup-bundle-x86-2013-1.0.101.0.exe)

選擇完整安裝

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\001.png)

記得勾選

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\002.png)

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\003.png)

這邊會有點久，耐心等待一下

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\004.png)

總算結束

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\005.png)

----------

## 設定環境 ##

#### 1. 建立一SQLite資料庫 ####

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\010.png)

#### 2. 建立一.NET4.0專案 ####

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\006.png)

#### 3. 安裝SQLite ####

([安裝流程](http://iverson127.github.io/SQLite_Install/))

#### 4. 在專案新增項目「ADO.NET 實體資料模型」 ####

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\007.png)

#### 5. 點選「來自資料庫的 EF Designer」 ####

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\008.png)

#### 6. 選擇要連接的資料庫，點選新增連接 ####

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\009.png)

#### 7. 選擇你要連接的資料庫類型 ####

可按變更修改，在此範例我們使用SQLite Database

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\011.png)


![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\012.png)

#### 8. 選擇資料庫檔案來源 ####

可測試連接是否正常

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\013.png)

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\014.png)

#### 9. 連結資料庫的訊息 ####

可看到連結此資料庫的訊息字串，也可更改此連接設定的名稱

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\015.png)

#### 10. 選擇你要此資料庫的哪些物件要在程式中處理 ####

選擇完後按完成

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\016.png)

#### 11. 完成 ####

可在畫面中看到資料庫的模型(.edmx)被帶入專案裡了，其table與先前建立的一致

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\017.png)

![](..\images\postImage\CSharp_EF6_SQLite_VS2013_Install\018.png)

----------

## Reference ##

- [https://system.data.sqlite.org/index.html/doc/trunk/www/downloads.wiki](https://system.data.sqlite.org/index.html/doc/trunk/www/downloads.wiki)

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)

[[SQLite]](http://iverson127.github.io/tags/#SQLite)

[[Entity Framework 6]](http://iverson127.github.io/tags/#Entity Framework 6)

[[開發工具]](http://iverson127.github.io/tags/#開發工具)
