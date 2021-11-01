<h2 align="center">Lesson 09: 資料表與資料條件限制</h2>

### 9-1 新建資料表
```mysql
> create table dept(deptno smallint(4), dname varchar(14));
# 若不存在則新建
> create table [if not exists] dept(deptno smallint(4), dname varchar(14));
> create table [if not exists] dept(deptno smallint(4), dname varchar(14)) engine=innodb;
```

| | 篩選欄位 | 指定別名 | 結構複製 | 資料複製 |
| :---: | :---: | :---: | :---: | :---: |
| as | Ｏ | Ｏ | Ｏ(not null) | Ｏ |
| like | Ｘ | Ｘ | Ｏ(PRI+not null) | Ｘ |

```mysql
# 僅複製表格結構 (及主鍵)，無資料
> create table EMP10 like emp;
> create table EMP10 as select * from emp;
> create table EMP10 as select empno "EMP#", ename EMPLOYEE from emp where deptno=10;
```

### 9-2 欄位設定
1. 欄位檢查條件
```mysql
# default 預設填入 None
> create table dept(deptno smallint(4) not null, dname varchar(14) default "None");
# auto_increment 自動編號：可指定起始值、常搭配 PRI 或 UNI 使用
> create table dept(deptno smallint(4) auto_increment, dname varchar(14));
> create table dept(deptno smallint(4) auto_increment, dname varchar(14)) auto_increment=100;
```

2. KEY constraint

| | 多組 |  NULL | 範例 |
| :---: | :---: | :---: | :--- |
| 主鍵 (Primary Key) | Ｘ | Ｘ | create table dept(deptno smallint(4) primary key, dname varchar(14)) |
| 唯一鍵 (Unique) | Ｏ | Ｏ | create table dept(deptno smallint(4) primary key, dname varchar(14) unique) |

```mysql
# 也可事後一併宣告，如複合鍵 (兩個以上欄位)
> create table dept(deptno samllint(4), dname varchar(14) \
  [constraint PRI] primary key(deptno, dname));
  [constraint UNI] unique(deptno, dname));
  [constraint FRK] foreign key(deptno) references emp(deptno));
  
  # 刪除或修改時相依影響／設為空值
  [constraint FRK] foreign key(deptno) references emp(deptno) on delete/update cascade);
  [constraint FRK] foreign key(deptno) references emp(deptno) on delete/update set null);
```

### 9-3 資料表維護
1.　欄位相關
```mysql
> alter table EMP10 add [column] mgr smallint(6);
# 可新增多欄及指定位置 (column 可省略)
> alter table EMP10 add [column] hiredate date first/after ename;
> alter table EMP10 add [column](mgr smallint(6), hiredate date);
> alter table EMP10 drop [column] empno, mgr;
```

| | 功用 | 範例 |
| :---: | :---: | :--- |
| alter | 預設值設定 | alter table EMP10 alter [column] comm `drop/set default 0` |
| modify/change | 欄位定義 (型態或排序) | alter table EMP10 modify [column] comm int(11) not null after sal<br>alter table EMP10 change [column] comm COMMISSION int(11) |

2.　其他維護

| alter table EMP10 add | 名稱 | 刪除語法 |
| :--- | :---: | :---: |
| [constraint PRI] primary key(empno) | 主鍵 | |
| [constraint UNI] unique(ename, job) | 唯一鍵 | `drop index+名稱` |
| [constraint FRK] foreign key(mgr) references emp(mgr) | `外來鍵` | drop foreign key+名稱 |

```mysql
> alter table EMP10 rename [to] EMPLOYEE10;
> alter table EMP10 engine=myisam;
 
# 刪除資料但保留表格結構
> truncate table dept;
> drop table dept;
```
