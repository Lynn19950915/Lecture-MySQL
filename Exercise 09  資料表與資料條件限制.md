<h2 align="center">Exercise 09: 資料表與資料條件限制</h2>

- 9.1 利用下列資料來新建 DEPARTMENT 資料表。
```mysql
> create table DEPARTMENT(id numeric(7) not null, name varchar(24) not null);
```

- 9.2 利用 DEPT 資料表中的資料，將資料新增至 DEPTARTMENT 資料表中 (只新增相對的資料欄)。
```mysql
# insert+select，無需 as+子查詢、set 或 values 等
> insert into department select deptno, dname from dept;
```

- 9.3 利用下列資料來新建 EMPLOYEE 資料表。
```mysql
> create table EMPLOYEE (id numeric(7) not null, last_name varchar(24) not null, first_name varchar(24), dept_id numeric(7));
```

- 9.4 將 EMPLOYEE 資料表中 last_name 欄位的資料型態更改為 varchar(40)。
```mysql
# 修改資料型態不可用 alter/change
> alter table EMPLOYEE modify [column] last_name varchar(40);
```

- 9.5 使用 EMP 資料表的結構中之 EMPNO, ENAME, DEPTNO 之定義來新建 EMPLOYEE2 資料表，並將欄位名稱設定為 ID, LAST_NAME 及 DEPT_ID。
```mysql
# 新增方式如同 view，as 相較 like 彈性：指定欄位、別名，惟無 primary key 且會複製資料
> create table EMPLOYEE2 as (select empno ID, ename LAST_NAME, deptno DEPT_ID from emp);
# 保留表格結構，但刪除複製的資料：truncate table EMPLOYEE2;
```

- 9.6 刪除整個 EMPLOYEE 資料表。
```mysql
> drop table EMPLOYEE;
```

- 9.7 將 EMPLOYEE2 資料表改名為 EMPLOYEE。
```mysql
> alter table EMPLOYEE2 rename [to] EMPLOYEE;
```

- 9.8 將 EMPLOYEE 資料表中的 LAST_NAME 欄位刪除。
```mysql
# 別名雖寫在子查詢，但 table 已以別名儲存。這點與 view 不同 (比較 10.3)
> alter table EMPLOYEE drop [column] LAST_NAME;
```

- 9.9 修改 EMPLOYEE 資料表，新增一個欄位 SALARY 資料型態為 NUMERIC, precision 7。
```mysql
> alter table EMPLOYEE add [column] SALARY numeric(7);
```

- 9.10 修改 EMPLOYEE 資料表，使用 ID 欄位新增一個 Primary key 的限制條件 (constraint)，並為它命名。
```mysql
# constraint 及命名可省略，但 PK, UNI 後必宣告變數
> alter table EMPLOYEE add [constraint NAME] primary key(ID);
```

- 9.11 在 EMPLOYEE 資料表新增一個外來鍵，以確保員工不會被分派到一個不存在的部門。
```mysql
# 相依性或空值設定：on [delete/update] [cascade/set null]
> alter table EMPLOYEE add [constraint NAME] foreign key(DEPT_ID) references dept(deptno);
```
