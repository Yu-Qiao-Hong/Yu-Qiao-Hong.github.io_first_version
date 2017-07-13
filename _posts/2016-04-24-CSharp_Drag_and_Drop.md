---
layout: post
title: "C# Drag and Drop"
author: "Iverson Hong"
modified: 2016-04-24
tags: [C#]
---

使用滑鼠選擇檔案並拖曳至Form裡面放開，Form裡面顯示出你選擇了那些檔案路徑。

#### 1. 建立一個windows form

#### 2. 在windows form上加一個TextBox

#### 3. 在"**AllowDrop**"屬性選擇"**True**"

#### 4. 在"**Multiline**"屬性選擇"**True**"

![](..\images\postImage\CSharp_Drag_and_Drop\001.png)

#### 5. 在事件裡面選擇"**DragDrop**", "**DragEnter**"

![](..\images\postImage\CSharp_Drag_and_Drop\002.png)

~~~csharp
private void textBox1_DragEnter(object sender, DragEventArgs e)
{
    e.Effect = DragDropEffects.All;
}

private void textBox1_DragDrop(object sender, DragEventArgs e)
{
    var files = e.Data.GetData(DataFormats.FileDrop, false) as string[];
    foreach (var file in files)
    {
        textBox1.AppendText(file);
        textBox1.AppendText("\n");
    }
}
~~~

#### 6. 執行結果:

![](..\images\postImage\CSharp_Drag_and_Drop\003.png)

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)