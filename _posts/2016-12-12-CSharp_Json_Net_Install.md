---
layout: post
title: "C# 在Visual Studio 環境安裝Json.NET套件"
author: "Iverson Hong"
modified: 2016-12-12
tags: [JSON, 開發工具]
---

在專案開發初期，使用了C#提供的XML序列化(XmlSerializer)作為存檔讀檔資料的基礎，隨著專案慢慢長大，當初制定的資料結構也隨著時間改變。出現新版本讀取舊版本會有反序列化失敗的問題，因XML序列化/反序列化牽扯到反射機制，會記錄許多編譯時的資訊如namespace, assembly...,也因為利用反射機制，因此在效能上也不是算是很好。

在網頁端程式檔案傳輸格式已由XML漸漸改成使用JSON傳輸，JSON相對於XML有著許多優點，格式簡單、易於閱讀修改、佔用空間更小。在Visual Studio有一功能強大操作JSON套件"Json.NET"。

## 安裝Json.NET ##

1. 開啟Visual Studio(範例為VS2015)
2. 選擇TOOLS -> NuGet Package Manager
3. 選擇Package Manager Console

在Package Manager Console中輸入: "**Install-Package Newtonsoft.Json -Version 9.0.1**"

則會根據現在開啟的專案幫你安裝相對應的Json.NET版本：

![](..\images\postImage\CSharp_Json_Net_Install\001.png)

安裝完後會看到專案目錄有以下檔案

![](..\images\postImage\CSharp_Json_Net_Install\002.png)

----------

## Reference ##

- [https://www.nuget.org/packages/newtonsoft.json/](https://www.nuget.org/packages/newtonsoft.json/)

----------

[[JSON系列文章]](http://yu-qiao-hong.github.io/tags/#JSON)

[[開發工具系列文章]](http://yu-qiao-hong.github.io/tags/#開發工具)