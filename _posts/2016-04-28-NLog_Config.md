---
layout: post
title: "NLog配置檔設定"
author: "Iverson Hong"
modified: 2016-04-28
tags: [開發工具, NLog]
---

## NLog.Config ##

可在專案目錄找到NLog.Config，此檔案用來設定與log相關的資訊，例如log檔案名稱，輸出格式，或者不同level的處理方式。

一般來說，只要設定兩個地方就好，分別是「**targets**」跟「**rules**」

----------

## targets ##

在target中可定義log檔案的名稱，輸出格式，等細部設定，其中可看到很多以$開頭後面加大括號{}的變數，每個變數都有其定義，詳情可參考[Layout-renderers](https://github.com/NLog/NLog/wiki/Layout-renderers),以下是我常使用的設定:

~~~xml
  <targets>
    <target name="file" xsi:type="File"
          layout="${longdate} | ${level:uppercase=true} | ${callsite:methodName=true} | ${message} ${onexception:${newline}${exception:format=tostring}} ${newline}"
          fileName="${basedir}/Logs/logfile.txt"
          archiveFileName="${basedir}/Logs/archives/log.{#}.txt"
          archiveEvery="Day"
          archiveNumbering="Rolling"
          maxArchiveFiles="7"
          concurrentWrites="true"
          keepFileOpen="false"
          encoding="UTF-8" />
  </targets>
~~~

----------

## rules ##

rule則是定義出不同level的log需要套用到哪個對應的target格式

在NLog中總共定義了6種level,以下是官網的描述:

| **Level** | **Example** |
|:------|:------|
| Fatal | Highest level: important stuff down |
|----
| Error | For example application crashes / exceptions. |
|----
| Warn | Incorrect behavior but the application can continue |
|----
| Info | Normal behavior like mail sent, user updated profile etc. |
|----
| Debug| Executed queries, user authenticated, session expired |
|----
| Trace| Begin method X, end method X etc |
|----
{: rules="groups"}

最簡單的設定方式就是全部都記錄:

~~~xml
  <rules>
    <logger name="*" minlevel="Trace" writeTo="file" />
  </rules>
~~~

----------

## Reference ##

- [https://github.com/NLog/NLog/wiki/Layout-renderers](https://github.com/NLog/NLog/wiki/Layout-renderers)
- [https://github.com/nlog/NLog/wiki/File-target](https://github.com/nlog/NLog/wiki/File-target)
- [https://github.com/NLog/NLog/wiki/Log-levels](https://github.com/NLog/NLog/wiki/Log-levels)

----------

[[NLog系列文章]](http://iverson127.github.io/tags/#NLog)
[[開發工具系列文章]](http://iverson127.github.io/tags/#開發工具)