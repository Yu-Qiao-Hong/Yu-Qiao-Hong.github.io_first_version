---
layout: post
title: "NLog使用範例(1)"
author: "Iverson Hong"
modified: 2016-04-29
tags: [開發工具, NLog]
---

## 在程式中使用NLog ##

[延續前一篇](http://iverson127.github.io/NLog_Config/)的設定，開始在程式中使用NLog

步驟1: 在欲加入的class中定義Logger物件

~~~csharp
private static Logger logger = LogManager.GetCurrentClassLogger();
~~~

步驟2: 呼叫使用
~~~csharp
private void button1_Click(object sender, EventArgs e)
{
    logger.Fatal("This is fatal msg");
    logger.Error("This is error msg");
    logger.Warn("This is warn msg");
    logger.Info("This is info msg");
    logger.Debug("This is debug msg");
    logger.Trace("This is trace msg");
}
~~~

產生的log檔如下：

    2016-04-29 21:09:50.7971 | FATAL | WindowsFormsApplication6.Form1.button1_Click | This is fatal msg  
    
    2016-04-29 21:09:50.7971 | ERROR | WindowsFormsApplication6.Form1.button1_Click | This is error msg  
    
    2016-04-29 21:09:50.7971 | WARN | WindowsFormsApplication6.Form1.button1_Click | This is warn msg  
    
    2016-04-29 21:09:50.7971 | INFO | WindowsFormsApplication6.Form1.button1_Click | This is info msg  
    
    2016-04-29 21:09:50.7971 | DEBUG | WindowsFormsApplication6.Form1.button1_Click | This is debug msg  
    
    2016-04-29 21:09:50.7971 | TRACE | WindowsFormsApplication6.Form1.button1_Click | This is trace msg  

----------

在程式不改的情況下，假設想要修改log印出來的等級，可直接修改NLog.Config檔

e.g. minlevel從"Trace"改為"**Error**"

~~~xml
  <rules>
    <logger name="*" minlevel="Error" writeTo="file" />
  </rules>
~~~

再執行一次，log檔內容為：

    2016-04-29 21:10:48.7971 | FATAL | WindowsFormsApplication6.Form1.button1_Click | This is fatal msg  
    
    2016-04-29 21:10:48.7971 | ERROR | WindowsFormsApplication6.Form1.button1_Click | This is error msg  
    
----------

[[NLog系列文章]](http://iverson127.github.io/tags/#NLog)

[[開發工具系列文章]](http://iverson127.github.io/tags/#開發工具)