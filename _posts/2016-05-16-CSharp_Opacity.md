---
layout: post
title: "C# 視窗透明度設定"
author: "Iverson Hong"
modified: 2016-05-16
tags: [C#]
---

使用C# TrackBar加上Opacity設定來調整視窗的透明度。

#### 1. 建立一個windows form

#### 2. 在windows form上加一個TrackBar

#### 3. 設定屬性

![](..\images\postImage\CSharp_Opacity\001.png)

其中Minimum,Maximum為TrackBar的上下限，value為初始值，必須落在上下限內。

SmallChange為使用鍵盤方向鍵一次移動的量；LargeChange為用滑鼠或是鍵盤Page Up/Down的量。

#### 4. 選擇"**Scroll**"事件

~~~csharp
private void trackBar1_Scroll(object sender, EventArgs e)
{
    this.Opacity = (double)trackBar1.Value / trackBar1.Maximum;
}
~~~

Opacity為透明度，預設是1(不透明)，最低是0(全透明)，因此可藉由TrackBar來控制透明度，上述例子透明度範圍為0.2~1，因TrackBar Minimum設1

#### 6. 執行結果:

![](..\images\postImage\CSharp_Opacity\002.png)

----------

## Reference ##

- [https://msdn.microsoft.com/zh-tw/library/system.windows.forms.form.opacity(v=vs.110).aspx](https://msdn.microsoft.com/zh-tw/library/system.windows.forms.form.opacity(v=vs.110).aspx)

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)