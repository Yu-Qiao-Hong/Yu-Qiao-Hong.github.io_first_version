---
layout: post
title: "AutoIt教學範例(1)"
author: "Iverson Hong"
modified: 2016-04-12
tags: [AutoIt, Automated Testing]
---

## AU3Info tools ##

開啟AutoIt編輯軟體，點選**Tools->AU3Info**，如下圖

![](http://i.imgur.com/RaopiLD.png)

![](http://i.imgur.com/aG1pxSs.png)

此Tools功能類似Spy++，可抓取Window GUI基本資料，利用此工具可更精準地抓到我們想要控制或是讀取的Window控件。

----------

## 範例 ##

### 情境: ###

1. 開啟一個記事本
2. 在記事本內輸入文字
3. 從記事本內讀取文字
4. 比對讀取結果並跳出訊息視窗告知使用者比對結果
5. 關閉記事本

### AutoIt 程式碼 ###

    $iPID = Run('notepad')
    WinWaitActive('未命名 - 記事本', '')
    ControlSend('未命名 - 記事本', '', '[CLASS:Edit; INSTANCE:1]', 'Hello World, this is test')
    
    $text = ControlGetText('未命名 - 記事本', '', '[CLASS:Edit; INSTANCE:1]')
    If $text = 'Hello World, this is test' Then
    	MsgBox(0,'debug','The same')
    Else
    	MsgBox(0,'debug','Not the same')
    EndIf
    
    ProcessClose($iPID)

----------

## Reference ##

[https://www.autoitscript.com/autoit3/docs/intro/au3spy.htm](https://www.autoitscript.com/autoit3/docs/intro/au3spy.htm)

----------

[[AutoIt系列文章]](http://iverson127.github.io/tags/#AutoIt)