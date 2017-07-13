---
layout: post
title: "C# TreeView and TreeNode"
author: "Iverson Hong"
modified: 2016-04-26
tags: [C#]
---

在windows form上加一個TreeView控件:

## TreeView ##

直觀寫法:

~~~csharp
treeView1.Nodes.Add("root 0");
treeView1.Nodes.Add("root 1");
treeView1.Nodes.Add("root 2");

treeView1.Nodes[1].Nodes.Add("1-0");
treeView1.Nodes[1].Nodes.Add("1-1");
treeView1.Nodes[1].Nodes.Add("1-2");
~~~

![](..\images\postImage\CSharp_TreeView_TreeNode\001.png)

----------

## 使用TreeNode ##

~~~csharp
TreeNode treeNode = new TreeNode("Parent1");
treeView1.Nodes.Add(treeNode);

treeNode = new TreeNode("Parent2");
treeView1.Nodes.Add(treeNode);

TreeNode node1 = new TreeNode("Child3-1");
TreeNode node2 = new TreeNode("Child3-2");
TreeNode[] array = new TreeNode[] { node1, node2 };

treeNode = new TreeNode("Parent3", array);
treeView1.Nodes.Add(treeNode);
~~~

![](..\images\postImage\CSharp_TreeView_TreeNode\002.png)

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)