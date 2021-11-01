<h2 align="center">Lesson 11: 權限管理</h2>

### 11-1 帳號與密碼
```mysql
> create user lynn.915@localhost;
# 設定登入密碼
> create user lynn.916@localhost identified by "samuel0916";
# 紀錄於系統資料庫 mysql (密碼經加密處理)
> select host, user, password from [mysql.]user;

# set, drop 均對系統設定
> set password=password("samuel0915");
> set password for lynn.915=password("samuel0915");
> drop user lynn.915@localhost;

# update, delete 則針對 user 資料表修改
> update mysql.user set password=password("samuel0915") where user=user()/lynn.916;
# 同步更新權限狀態
  flush privileges;
> delete from mysql.user where user="lynn.915";
  flush privileges;
```

### 11-2 帳號授權
1. 權限授權

| 層級 | 語法 | 紀錄存放 |
| :---: | :--- | :---: |
| 全域授權 | grant/revoke all on *.* | `mysql.users` |
| 資料庫授權 | grant/revoke all on database.* | `mysql.db` |
| 資料表授權 | `grant/revoke all on database.table` | mysql.tables_priv |
| 欄位授權 | `grant/revoke all on database.table(column)` | `mysql.columns_priv` |

2. 結合使用者帳號

| 層級 | 語法 | 權限 |
| :---: | :--- | :---: |
| 全域授權 | grant all on *.* to lynn.915@"%" identified by "samuel0915" | 全 IP 登入 |
| 資料庫授權 | grant all on `db.*` to lynn.915@localhost identified by "samuel0915" | 本地登入 |
| | grant `select, create, alter, drop` on db.* to lynn.915@localhost | 本地登入+DDL 權限 |
| 資料表授權 | grant all on db.emp to `lynn.915@192.168.1.100` identified by "samuel0915" | 限定 IP 登入 |
| | grant `select, insert, update, delete` on db.emp to lynn.915@localhost<br>[with grant option] | 本地登入+DML 權限<br>`# 再授權的權限` |

### 11-3 遠端連線
```mysql
> mysql --user=(帳號) --password=(密碼) --host=(IP位址) --port=(連接埠)
# 簡寫版本：password 無空白間隔，port 大寫 P 
> mysql -u (帳號) -p(密碼) -h (IP位址) -P (連接埠)
```
