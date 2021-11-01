<h2 align="center">Lesson 08: 資料維護與交易控制</h2>

### 8-1 DML 資料處理
```mysql
> insert into dept values(50, "shippment", "los angeles");
# 數量需與宣告欄位符合，未提及者均為 null (不可違反欄位預設)
> insert into dept(deptno, dname) values(60, "advertisement");
> insert into dept(deptno, dname, loc) values(70, "service", "san francisco");

> update dept set loc="Hawaii" where deptno=60;
> update dept set dname="ADVERTISEMENT", loc="HAWAII" where deptno>60;
# 無條件則全部刪除 (資料清空、結構保留，同 truncate table)
> delete from dept;
> delete from dept where deptno in(50, 60);
```

### 8-2 DML 結合子查詢
```mysql
# insert+select all: 即複製表格
> insert into bonus select * from emp;
> insert into bonus select * from emp where empno=7844;
> insert into bonus(empno, ename, deptno) select empno, ename, deptno from emp where empno=7900;

# 取值子查詢：注意兩表不得相同 (參考相關子查詢)
> update emp set deptno=(select deptno from dept where loc="dallas") where empno=7499;
> update emp set sal=sal+400 where deptno=(select deptno from dept where dname="accounting");
> delete from emp where deptno in (select deptno from dept where dname like "%e%");
```

### 8-3 常見 DML 錯誤

| 錯誤類型 | 範例 | 備註 |
| :---: | :--- | :--- |
| NOT NULL | insert into dept(dname, loc) values("marketing", "florida") | |
| 主鍵重複 (duplicate PRI key) | insert into dept(deptno, dname) values(40, "marketing") | 已存在 deptno=40 之資料 |
| `未定義的外來鍵` | update emp set deptno=75 where deptno=30 | deptno 為 FK (取自資料表 dept) |
| `使用中的外來鍵被刪除` | delete dept where deptno=30 | |
| 自我連結 | delete emp where empno=7839 | a.mgr=b.empno，僅`未出現於 mgr 者才可直接刪除` |

### 8-4 交易控制：innodb 支援<br>
(alter table dept engine=innodb)
1. 單次交易
```mysql
> start transaction;
> (dml_statement)
# rollback：回復上一步
> rollback;
```

2. 連續交易
```mysql
# 安全交易開始
> set autocommit=0;

# 建立記憶儲存點 (類似復原電腦)
> (dml_statement1); savepoint S1;
> (dml_statement2); savepoint S2;
> (dml_statement3); savepoint S3;

# 可往前執行但不得向後復原 (例：S2 再復原回 S3)
> rollback to S3;
> rollback to S2;
> rollback to S1;

# 安全交易結束
> set autocommit=1
```
