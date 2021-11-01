<h2 align="center">Exercise 08: 資料維護與交易控制</h2>

- 8.1 將下列之資料新增至 MY_EMP 資料表中，不要列舉欄位。
```mysql
> insert into my_emp values(1, "Patel", "Ralph", "rpatel", 795);
```

- 8.2 使用列舉欄位方式，將下列之資料新增至 MY_EMP 資料表中。
```mysql
> desc my_emp;
> insert into my_emp(ID, LAST_NAME, FIRST_NAME, USERID, SALARY) values(2, "Dancs", "Betty", "bdancs", 860);
```

- 8.3 將下列之資料新增至 MY_EMP 資料表中。
```mysql
# 整份資料包在一個括號中，可一次新增多筆
> insert into my_emp values(3, "Biri", "Ben", "bbiri", 1100), (4, "Newman", "Chad", "cnewman", 750);
```

- 8.4 將員工編號為 3 的名字 (last name) 更改為 Drexler。
```mysql
> update my_emp set LAST_NAME="Drexler" where ID=3;
```

- 8.5 將薪資低於 900 元的所有員工薪資調整為 1000 元。
```mysql
> update my_emp set SALARY=1000 where SALARY<900;
```

- 8.6 確認你所做的資料更新已更改到資料庫中。
```mysql
# 顯示資料筆數、最後修改時間等資訊
> show table status [from mydb] like "my_emp";
```

- 8.7 刪除 Betty Dancs 的資料。
```mysql
> delete from my_emp where FIRST_NAME="Betty";
```

- 8.8 請依下列指令啟動一個資料庫交易。
```mysql
# 安全交易開始
> set autocommit=0;

# 將所有員工調薪 10%，設定一個儲存點 (注意：不能以數字開頭)
> update my_emp set salary=salary*1.1;
> savepoint T1;

# 發現資料誤刪，回復 (rollback) 至前一個儲存點
> delete from my_emp;
> select * from my_emp;
> rollback to savepoint T1;

# 安全交易結束
> set autocommit=1;
```
