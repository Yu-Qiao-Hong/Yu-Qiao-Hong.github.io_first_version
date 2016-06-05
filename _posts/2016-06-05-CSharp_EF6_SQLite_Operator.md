---
layout: post
title: "C# 使用EF6操作SQLite"
author: "Iverson Hong"
modified: 2016-06-05
tags: [C#, SQLite, Entity Framework 6]
---

接續前一篇[C# 在VS2013環境安裝EF6來操作SQLite](http://iverson127.github.io/CSharp_EF6_SQLite_VS2013_Install/)

## Insert ##

在資料庫中insert兩筆資料:

    Name = "Iverson.Hong", No = 11
    Name = "Michael Jordan", No = 23

SaveChanges()會傳回底層資料庫處理的數量

~~~csharp
private void button1_Click(object sender, EventArgs e)
{
    using (testefEntities db = new testefEntities())
    {
        db.Table1.Add(new Table1 { Name = "Iverson.Hong", No = 11 });
        db.Table1.Add(new Table1 { Name = "Michael Jordan", No = 23 });

        try
        {
            int cnt = db.SaveChanges();
        }
        catch (Exception ex)
        {
            Debug.WriteLine(ex.Message);
        }
    }
}
~~~

資料內容:

![](..\images\postImage\CSharp_EF6_SQLite_Operator\001.png)

----------

## Delete ##

想要把名字符合Iverson.Hong的資料都刪除

~~~csharp
private void button2_Click(object sender, EventArgs e)
{
    using (testefEntities db = new testefEntities())
    {
        var entities = db.Table1.Where(x => x.Name == "Iverson.Hong");

        foreach (var entity in entities)
        {
            db.Table1.Remove(entity);
        }

        try
        {
            int cnt = db.SaveChanges();
        }
        catch (Exception ex)
        {
            Debug.WriteLine(ex.Message);
        }
    }
}
~~~

資料內容:

![](..\images\postImage\CSharp_EF6_SQLite_Operator\002.png)

----------

## Query ##

把資料庫的內容全部印出來

~~~csharp
private void button3_Click(object sender, EventArgs e)
{
    using (testefEntities db = new testefEntities())
    {
        var entities = db.Table1.ToList();

        foreach (var entity in entities)
        {
            Debug.WriteLine("Id = {0}, Name = {1}, No = {2}", entity.Id, entity.Name, entity.No);
        }
    }
}
~~~

輸出結果:

    Id = 2, Name = Michael Jordan, No = 23
    Id = 4, Name = Michael Jordan, No = 23
    Id = 6, Name = Michael Jordan, No = 23
    Id = 8, Name = Michael Jordan, No = 23

----------

若要查詢特定的條件，除了可以用"**Where()**"之外(上述Delete有使用)，還可以用"**Find()**"，這邊須注意的是Find()裡面的參數需帶入table的**primary key**

~~~csharp
private void button4_Click(object sender, EventArgs e)
{
    using (testefEntities db = new testefEntities())
    {
        var entity = db.Table1.Find(2);
        if (entity != null)
            Debug.WriteLine("Id = {0}, Name = {1}, No = {2}", entity.Id, entity.Name, entity.No);
    }
}
~~~

輸出結果:

    Id = 2, Name = Michael Jordan, No = 23

----------

## Update ##

先讀取再更新

~~~csharp
private void button5_Click(object sender, EventArgs e)
{
    using (testefEntities db = new testefEntities())
    {
        var entity = db.Table1.Find(2);
        if (entity != null)
        {
            entity.Name = entity.Name + "(Taiwan)";
            try
            {
                int cnt = db.SaveChanges();
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }

        // print
        var after = db.Table1.Find(2);
        if (entity != null)
            Debug.WriteLine("Id = {0}, Name = {1}, No = {2}", entity.Id, entity.Name, entity.No);
    }
}
~~~

輸出結果:

    Id = 2, Name = Michael Jordan(Taiwan), No = 23

----------

## Reference ##

- [https://msdn.microsoft.com/en-us/data/jj573936](https://msdn.microsoft.com/en-us/data/jj573936)

----------

[[C#系列文章]](http://iverson127.github.io/tags/#C#)
[[SQLite]](http://iverson127.github.io/tags/#SQLite)
[[Entity Framework 6]](http://iverson127.github.io/tags/#Entity Framework 6)
