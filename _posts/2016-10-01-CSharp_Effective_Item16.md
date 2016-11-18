---
layout: post
title: "C# 避免產生非必要的實體化"
author: "Iverson Hong"
modified: 2016-10-01
tags: [C#, Effective C#]
---

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

雖然GC可以自動回收資源，
但若在程式中產生過多的reference type實體(占用heap空間)，
配置以及銷毀這些物件還是需要時間來處理。
因此必須有效的創建這些實體，避免非必要的產生。

以OnPaint()為例：

~~~csharp
protected override void OnPaint(PaintEventArgs e)
{
    // Bad. Created the same font every paint event.
    using (Font MyFont = new Font("Arial", 10.0f))
    {
        e.Graphics.DrawString(DateTime.Now.ToString(),
        MyFont, Brushes.Black, new PointF(0, 0));
    }
    base.OnPaint(e);
}
~~~

OnPaint()會頻繁的被呼叫使用，每一次呼叫都會創建一個Font實體，結束時會通知GC進行資源回收，此種寫法很明顯沒有效率。可以將創建Font的動作改放到member variable：

~~~csharp
private readonly Font myFont = new Font("Arial", 10.0f);

protected override void OnPaint(PaintEventArgs e)
{
    e.Graphics.DrawString(DateTime.Now.ToString(),
    myFont, Brushes.Black, new PointF(0, 0));
    base.OnPaint(e);
}
~~~

這樣被呼叫時不會一直創建新的Font實體，但須注意的是，搬到member variable的class若有繼承IDisposable記得必須實作。也並非要把所有的區域變數都改成member variable，是要頻繁調用的function這樣才有改才有意義

----------

以上述為例，想使用一個黑色筆刷在專案內的多個視窗中使用，雖然已經抽到member variable，但在各個視窗類別中還是有許多相同的黑色筆刷物件，這時可以使用singleton來確保只會創建一份實體：

~~~csharp
private static Brush blackBrush;

public static Brush Black
{
    get
    {
        if (blackBrush == null)
            blackBrush = new SolidBrush(Color.Black);
        return blackBrush;
    }
}
~~~

----------

除了上述抽到member variable以及singleton外，還需盡量減少使用不可更改的類別(代表每次assignment都會產生新的實體)，System.String就是不可更改的類別

~~~csharp
string msg = "Hello, ";
msg += thisUser.Name;
msg += ". Today is ";
msg += System.DateTime.Now.ToString();
~~~

看起來沒什麼問題，其實效率非常低，等同於以下寫法：

~~~csharp
string msg = "Hello, ";
// Not legal, for illustration only:
string tmp1 = new String(msg + thisUser.Name);
msg = tmp1; // "Hello " is garbage.
string tmp2 = new String(msg + ". Today is ");
msg = tmp2; // "Hello <user>" is garbage.
string tmp3 = new String(msg + DateTime.Now.ToString());
msg = tmp3; // "Hello <user>. Today is " is garbage.
~~~

因不可更改所以每次的+=就會創建一個新的string實體返回，tmp1、tmp2、tmp3和最一開始的msg這4個字串都變成垃圾等待回收。應改寫成以下：

~~~csharp
string msg = string.Format("Hello, {0}. Today is {1}", thisUser.Name, DateTime.Now.ToString());
~~~

若較複雜的字串處理，建議可以使用StringBuilder，StringBuilder為可變字串型態，在產生最終的string之前，可以對其內容進行修改而不會在過程中創建新的實體出來：

~~~csharp
StringBuilder msg = new StringBuilder("Hello, ");
msg.Append(thisUser.Name);
msg.Append(". Today is ");
msg.Append(DateTime.Now.ToString());
string finalMsg = msg.ToString();
~~~

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)
[[Effective C#系列文章]](http://iverson127.github.io/tags/#Effective C#)