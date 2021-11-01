<h2 align="center">Exercise 02: 簡單查詢</h2>

- 2.1 建立一個查詢來顯示部門 (dept) 資料表中的所有資料。
```mysql
> select * from dept;
```

- 2.2 建立一個查詢來顯示每一位員工的姓名 (name)、職稱 (job)、進公司日期 (hire date)及員工編號 (employee number)，並且將員工編號顯示在最前面。
```mysql
> select empno "employee number", ename name, job, hiredate "hire date" from emp;
```

- 2.3 建立一個查詢來顯示所有員工所擔任的職稱有哪些。(重複的資料只顯示一次)
```mysql
> select distinct job from emp;
```

- 2.4 承 2.2，並將資料表頭重新命名為：Emp#, Employee, Job, Hire Date。
```mysql
> select empno "Emp#", ename Employee, Job, hiredate "Hire Date" from emp;
```

- 2.5 建立一個查詢將姓名 (name) 及職稱 (job) 串接為一個資料項，並將表頭重新命名為 Employee and Title。
```mysql
# 資料中間利用一個空白及逗號做區隔
> select concat(ename, " ,", job) "Employee and Title" from emp;
```
