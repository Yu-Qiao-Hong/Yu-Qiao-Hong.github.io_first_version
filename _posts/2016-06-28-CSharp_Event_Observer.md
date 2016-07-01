---
layout: post
title: "C# Observer Design Pattern以Event來實現"
author: "Iverson Hong"
modified: 2016-06-28
tags: [C#]
---

定義兩個class：一個為球，一個為人，人觀察著球，當球打擊出去時，人會依據自己的能力做出相對應的動作。

### 事件參數 ###

定義事件所需參數，可繼承原生EventArgs

~~~csharp
class BallEventArgs : EventArgs
{
    public int Speed { get; set; }
    public int Height { get; set; }
}
~~~

### Ball class ###

使用delegate，參數為(object sender, BallEventArgs e)。定義打擊method並帶入EventArgs

~~~csharp
delegate void ballInPlayHandler(object sender, BallEventArgs e); // declare delegate
class Ball
{
    //public event Action<object, BallEventArgs> ballEvent; 
    public event ballInPlayHandler ballEvent; // declare event
    public void BallInPlay()
    {
        if (ballEvent != null)
            ballEvent(this, new BallEventArgs { Speed = 100, Height = 30 }); // rise event
    }
}
~~~

### Person class ###

由建構子帶入Person的自身能力以及Ball實體，並使用lamda expression寫出對應動作

~~~csharp
class Person
{
    public Person(string name, int maxSpeed, int maxHight, Ball ball)
    {
        ball.ballEvent += (object sender, BallEventArgs e) => //lamda
        {
            if (maxSpeed > e.Speed)
                Console.WriteLine("{0}: The speed is {1}, is too slow to me", name, e.Speed);
            else
                Console.WriteLine("{0}: The speed is {1}, is too fast to me", name, e.Speed);

            if (maxHight > e.Height)
                Console.WriteLine("{0}: The height is {1}, is too low to me", name, e.Height);
            else
                Console.WriteLine("{0}: The height is {1}, is too height to me", name, e.Height);
        };
    }
}
~~~

### Client端程式 ###

~~~csharp
static void Main(string[] args)
{
    var Ball = new Ball();
    var Person1 = new Person("Iverson", 110, 20, Ball);
    var Person2 = new Person("Hong", 50, 50, Ball);

    Ball.BallInPlay();
}
~~~

### 結果 ###

    Iverson: The speed is 100, is too slow to me
    Iverson: The height is 30, is too height to me
    Hong: The speed is 100, is too fast to me
    Hong: The height is 30, is too low to me
    
在Ball class中可以用**Action**替換掉delegate，使用上更為簡潔方便

~~~csharp
//delegate void ballInPlayHandler(object sender, BallEventArgs e); // declare delegate
class Ball
{
    public event Action<object, BallEventArgs> ballEvent; 
    //public event ballInPlayHandler ballEvent; // declare event
~~~

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)