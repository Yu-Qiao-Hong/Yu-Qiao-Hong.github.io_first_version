---
layout: post
title: "C# Indexer"
author: "Iverson Hong"
modified: 2016-04-16
tags: [C#]
---
## 使用時機 ##

定義一個"basketballPlayer"class，裡面描述前10名我所喜愛的球員

~~~csharp
class basketballPlayer
{
	public static int SIZE = 10;
	private string[] favoritePlayers;

	public basketballPlayer()
	{
		favoritePlayers = new string[SIZE];
	}
}
~~~

接著我想設定喜愛球員的排名，並把名單顯示出來

我當然可以寫個SetPlayer(int No, string Name)以及ShowList() method，但我現在想直接利用new出來的class直接以處理陣列的方式做設定。

~~~csharp
basketballPlayer playerList = new basketballPlayer();

playerList[1] = "Allen Iverson";
playerList[2] = "Michael Jordan";
playerList[5] = "Steve Nash";

for (int i = 0; i < basketballPlayer.SIZE; i++)
{
	if(playerList[i] != null)
		Console.WriteLine(i.ToString() + ": " + playerList[i]);
}
~~~

理所當然編譯不過，因為playerList是一個class不是一個array,這時候就可以使用indexer了。

----------

## Indexer ##

在"basketballPlayer"以下method，讓上面的程式碼可以正常的運作

~~~csharp
public string this[int i]
{
	get
	{
		if (i >= 0 && i < SIZE)
			return favoritePlayers[i];
		else
			return "";
	}
	set 
	{
		if (i >= 0 && i < SIZE)
			favoritePlayers[i] = value;
	}
}
~~~

這樣是不是跟使用array一樣方便了呢?

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)