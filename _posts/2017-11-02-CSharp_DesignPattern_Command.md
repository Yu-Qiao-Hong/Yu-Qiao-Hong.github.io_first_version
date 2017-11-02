---
layout: post
title: "C# Command Design Pattern"
author: "Iverson Hong"
modified: 2016-11-02
tags: [C#, Design Pattern]
---

## 機器人 ##

建立一個機器人類別，裡面有基本的動作行為：

~~~csharp
class Robot
{
    public void GoAhead()
    {
        Console.WriteLine("Go Ahead");
    }

    public void TurnLeft()
    {
        Console.WriteLine("Turn Left");
    }

    public bool Stop()
    {
        Console.WriteLine("Stop");
        return true;
    }

    public bool GoPosition(int x, int y)
    {
        Console.WriteLine("Go x = {0}, y = {1}", x, y);
        return true;
    }
}
~~~

現在Client端希望讓機器人移動，往前走->左轉->走到位置(2, 4)->停止

~~~csharp
Robot robot = new Robot();
robot.GoAhead();
robot.TurnLeft();
robot.GoPosition(2, 4);
robot.Stop();
~~~

上述的寫法每個動作在Client呼叫後都直接執行，無法延後執行，且Client端要知道機器人對應動作的method名稱才可使用

----------

## Command Pattern ##

> Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

將請求封裝為物件，可以使用queue將請求儲存並延後執行。

### UML###

![](..\images\postImage\CSharp_DesignPattern_Command\CommandPattern.png)

此例子的UML：

![](..\images\postImage\CSharp_DesignPattern_Command\CommandPattern_example.png)

### Abstract Command Class ###

抽象命令類別，用來給機器人的動作行為繼承。

~~~csharp
abstract class Command
{
    protected Robot _receiver;

    public Command(Robot robot)
    {
        _receiver = robot;
    }

    abstract public void Excute();
}
~~~

### 將每個動作繼承抽象命令類別 ###

把機器人的動作都封裝成命令。

~~~csharp
class GoAheadCommand : Command
{
    public GoAheadCommand(Robot robot) : base(robot)
    {
    }

    public override void Excute()
    {
        _receiver.GoAhead();
    }
}

class TurnLeftCommand : Command
{
    public TurnLeftCommand(Robot robot) : base(robot)
    {
    }

    public override void Excute()
    {
        _receiver.TurnLeft();
    }
}

class StopCommand : Command
{
    public StopCommand(Robot robot) : base(robot)
    {
    }

    public override void Excute()
    {
        _receiver.Stop();
    }
}

class GoPositionCommand : Command
{
    private int _x;
    private int _y;

    public GoPositionCommand(Robot robot, int x, int y) : base(robot)
    {
        _x = x;
        _y = y;
    }

    public override void Excute()
    {
        _receiver.GoPosition(_x, _y);
    }
}
~~~

### Invoker ###

調用命令者，統一執行命令。

~~~csharp
class ControlCenter
{
    private List<Command> _commandList = new List<Command>();

    public void AddCommand(Command command)
    {
        _commandList.Add(command);
    }

    public void Run()
    {
        foreach (Command command in _commandList)
        {
            command.Excute();
        }
    }
}
~~~

### Client端 ###

~~~csharp
ControlCenter controlCentor = new ControlCenter();
controlCentor.AddCommand(new GoAheadCommand(robot));
controlCentor.AddCommand(new TurnLeftCommand(robot));
controlCentor.AddCommand(new GoPositionCommand(robot, 2, 4));
controlCentor.AddCommand(new StopCommand(robot));
controlCentor.Run();
~~~

----------

### 優點 ###

- 將請求者與接收者解耦，兩者透過Command溝通。

- 將請求封裝成物件，因此可將請求排隊與執行。

### 缺點 ###

- 若系統中有過多的Command，則會對每一個Command設計成Concrete Command Class，因而產生大量的類別，使用上並不便利。

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Design Pattern系列文章]](http://yu-qiao-hong.github.io/tags/#Design Pattern)