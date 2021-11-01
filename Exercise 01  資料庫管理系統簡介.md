<h2 align="center">Exercise 01: 資料庫管理系統簡介</h2>

- 1.1 查看目前 Server 中現有的資料庫。
```mysql
> show databases;
```

- 1.2 建立一示範資料庫。
```mysql
> create database mydb;
```

- 1.3 執行 Script File 以建立示範資料庫中的資料表。
```mysql
# 選擇一個資料庫
> use mydb; (select database();)
> source C:\***\mysql_demobld.sql
```

- 1.4 查看示範資料庫中的資料表。
```mysql
> show tables;
> desc customer;
```
