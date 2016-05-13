---
layout: post
title: "SQLite使用範例(1)"
author: "Iverson Hong"
modified: 2016-05-13
tags: [開發工具, SQLite]
---

[延續前一篇](http://iverson127.github.io/SQLite_Install/)的設定，開始在程式中使用SQLite：

建立一張table，裡面紀錄了公司職員的基本資料，例如工號、姓名、年齡與住址。

可用C#程式對此資料表新增刪除修改以及查詢，以下為操作SQLite常用到的方法

## 建立資料庫、資料庫連線 ##

~~~csharp
private SQLiteConnection OpenConn(string database)
{
    string connStr = string.Format("Data Source=" + database);
    SQLiteConnection sqlConn = new SQLiteConnection();
    sqlConn.ConnectionString = connStr;
    if (sqlConn.State == ConnectionState.Open)
        sqlConn.Close();

    sqlConn.Open();
    return sqlConn;
}
~~~

## Create、Insert、Update table##

這些屬於只對資料庫下指令，而沒有從資料庫取資料的

~~~csharp
private void RunSQL(string database, string sqlStr)
{
    SQLiteConnection sqlConn = OpenConn(database);
    SQLiteCommand sqlCmd = new SQLiteCommand(sqlStr, sqlConn);
    try
    {
        sqlCmd.ExecuteNonQuery();
    }
    catch (Exception ex)
    {
        Debug.WriteLine(ex.Message);
    }

    if (sqlConn.State == ConnectionState.Open)
        sqlConn.Close();
}
~~~

## Select ##

~~~csharp
public SQLiteDataReader SelectSQL(string database, string sqlStr)
{
    SQLiteDataReader reader = null;
    SQLiteConnection sqlConn = OpenConn(database);
    SQLiteCommand sqlCmd = new SQLiteCommand(sqlStr, sqlConn);
    try
    {
        reader = sqlCmd.ExecuteReader();
    }
    catch (Exception ex)
    {
        Debug.WriteLine(ex.Message);
        return reader;
    }

    return reader;
}
~~~

## Client 程式碼 ##

~~~csharp
private void button1_Click(object sender, EventArgs e)
{
    // 資料庫名稱
    string database = "test123.db";

    // 建立Table Information
    string createTableStr = "CREATE TABLE Information (" +
                            "Id INTEGER NOT NULL DEFAULT 0 PRIMARY KEY AUTOINCREMENT," +
                            "Name TEXT NOT NULL," +
                            "Age INTEGER," +
                            "Address TEXT);";

    RunSQL(database, createTableStr);

    //寫入資料到Information表中
    string insertstring = "INSERT INTO Information (Name, Age )" +
                            "values (\"Iverson Hong\", 30)";

    RunSQL(database, insertstring);

    insertstring = "INSERT INTO Information (Name, Age )" +
                            "values (\"Michael Jordan\", 50)";

    RunSQL(database, insertstring);

    // 讀取資料
    SQLiteDataReader reader = SelectSQL(database, "SELECT * FROM Information");
    while (reader.Read())
    {
        Console.WriteLine("Id: " + reader["Id"] + "\tName: " + reader["Name"] + "\tAge: " + reader["Age"] + "\tAddress: " + reader["Address"]);
    }
}
~~~

結果:
    Id: 1	Name: Iverson Hong	Age: 30	Address: 
    Id: 2	Name: Michael Jordan	Age: 50	Address:
    
----------

## Reference ##

- [http://blog.tigrangasparian.com/2012/02/09/getting-started-with-sqlite-in-c-part-one/](http://blog.tigrangasparian.com/2012/02/09/getting-started-with-sqlite-in-c-part-one/)

----------

[[SQLite系列文章]](http://iverson127.github.io/tags/#SQLite)
[[開發工具系列文章]](http://iverson127.github.io/tags/#開發工具)