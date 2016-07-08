---
layout: post
title: "NLog使用範例(2)"
author: "Iverson Hong"
modified: 2016-07-08
tags: [開發工具, NLog]
---

在C#中使用NLog時，若有需切換logger name或需要在印出log前增加判斷條件。

舉例來說，在同一個專案內有兩種模式(以A, B代表)，兩種模式不會同時存在，且log要分開來記錄，Nlog.config設定如下：

~~~xml
<targets>
    
  <target name="aa" xsi:type="File"
          layout="${longdate} | ${level:uppercase=true} | ${callsite:methodName=true} | ${message} ${onexception:${newline}${exception:format=tostring}} ${newline}"
          fileName="${basedir}/Logs/Alogfile.txt"
          concurrentWrites="true"
          keepFileOpen="false"
          encoding="UTF-8" />
    
   <target name="bb" xsi:type="File"
          layout="${longdate} | ${level:uppercase=true} | ${callsite:methodName=true} | ${message} ${onexception:${newline}${exception:format=tostring}} ${newline}"
          fileName="${basedir}/Logs/Blogfile.txt"
          concurrentWrites="true"
          keepFileOpen="false"
          encoding="UTF-8" />
    
</targets>
  
<rules>

    <logger name="A" minlevel="Debug" writeTo="aa" />
    <logger name="B" minlevel="Debug" writeTo="bb" />
    
</rules>
  
~~~

Test class在A, B兩種模式都會使用到，必須在每個都會用到的method前面加上判斷

~~~csharp
class Test
{
    Logger aLogger = LogManager.GetLogger("A");
    Logger bLogger = LogManager.GetLogger("B");

    string _mode;

    public Test(string mode)
    {
        _mode = mode;
    }

    public void MyFunc1()
    {
        if(_mode == "A")
            aLogger.Debug("This is mode A");
        else if(_mode == "B")
            bLogger.Debug("This is mode B");
        // do something 
    }

    public void MyFunc2()
    {
        if (_mode == "A")
            aLogger.Debug("This is mode A");
        else if (_mode == "B")
            bLogger.Debug("This is mode B");
        // do something 
    }
}
~~~

client端程式：

~~~csharp
static void Main(string[] args)
{
    Test t = new Test("A");
    t.MyFunc1();
    t.MyFunc2();
}
~~~

執行後產生Alogfile.txt檔案，內容為：

    2016-07-08 10:57:58.3181 | DEBUG | ConsoleApplication1.Test.MyFunc1 | This is mode A  
    
    2016-07-08 10:57:58.3371 | DEBUG | ConsoleApplication1.Test.MyFunc2 | This is mode A  

到目前為止都OK，但要是很多method都是共用的呢?或是現在又多加一個mode C進來，難道要在每一個method前面加判斷?那就把邏輯抽出來好了。

增加一個ChangeLogger()，把判斷條件寫在裡面，MyFunc1(), MyFunc2()也一併修改如下：

~~~csharp
public void MyFunc1()
{
    ChangeLogger();
    // do something 
}

public void MyFunc2()
{
    ChangeLogger();
    // do something 
}

private void ChangeLogger()
{
    if (_mode == "A")
        aLogger.Debug("This is mode A");
    else if (_mode == "B")
        bLogger.Debug("This is mode B");
}
~~~

再次執行後產生Alogfile.txt檔案，內容為：
    
    2016-07-08 10:58:58.3181 | DEBUG | ConsoleApplication1.Test.ChangeLogger | This is mode A  
    
    2016-07-08 10:58:58.3371 | DEBUG | ConsoleApplication1.Test.ChangeLogger | This is mode A  

仔細一看發現印出來的log都是由ChangeLogger()印出，因NLog會利用反射取得呼叫寫log的method名稱，因此都會得到ChangeLogger

----------

## 解法 ##

NLog有個wrapperType的用法，可以將NLog包一層，並告訴NLog這是包起來的class，因此在使用時不會取得包起來這層的反射資訊，而是取得呼叫這class的資訊，以下範例說明：

寫一個class把NLog包一層，並把想要的邏輯寫在裡面：

~~~csharp
public class MyLogger
{
    public static void Write(string mode, LogLevel level, string message)
    {
        string loggerName = string.Empty;

        if (mode == "A")        loggerName = "A";
        else if (mode == "B")   loggerName = "B";
                
        Logger logger = LogManager.GetLogger(loggerName);
        LogEventInfo le = new LogEventInfo(level, logger.Name, message);
        logger.Log(typeof(MyLogger), le);
    }
}
~~~

將原來的Test class改寫：

~~~csharp
class Test
{
    string _mode;

    public Test(string mode)
    {
        _mode = mode;
    }

    public void MyFunc1()
    {
        MyLogger.Write(_mode, LogLevel.Debug, "Good idea 1");
        // do something 
    }

    public void MyFunc2()
    {
        MyLogger.Write(_mode, LogLevel.Debug, "Good idea 2");
        // do something 
    }
}
~~~

再次執行後產生Alogfile.txt檔案，內容為：

    2016-07-08 10:59:58.3181 | DEBUG | ConsoleApplication1.Test.MyFunc1 | Good idea 1  
    
    2016-07-08 10:59:58.3371 | DEBUG | ConsoleApplication1.Test.MyFunc2 | Good idea 2  

使用這種方式包一層就可以在使用NLog前加上自己想要的判斷邏輯，且不會抓取到錯誤的反射資訊，大大增加了使用彈性。

----------

[[NLog系列文章]](http://iverson127.github.io/tags/#NLog)
[[開發工具系列文章]](http://iverson127.github.io/tags/#開發工具)