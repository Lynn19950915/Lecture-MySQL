<h2 align="center">Exercise 03: 查詢條件、資料排序</h2>

- 3.1 顯示出所有薪資超過 2850 元的員工之姓名和薪資。
```mysql
> select ename, sal from emp where sal>2850;
```

- 3.2 顯示員工編號為 7566 之員工姓名和他所屬的部門編號。
```mysql
> select ename, deptno from emp where empno=7566;
```

- 3.3 顯示薪資不介於 1500~2850 元之間的所有員工之姓名和薪資。
```mysql
# not 會將設定條件反轉，順序要對調
> select ename, sal from emp \
  1. where sal not between 1500 and 2850;
  2. where not sal between 1500 and 2850;
```

- 3.4 顯示於 1981 年 2 月 20 日至 1981 年 5 月 1 日間進入公司的員工之姓名、職稱與進公司日期，並依進公司日期由小到大排序。
```mysql
> select ename, job, hiredate from emp \
  where hiredate between "1981-02-20" and "1981-05-01" order by hiredate;
```

- 3.5 顯示部門 10 和 30 之所有員工姓名及其所屬的部門編號，並依名字之英文字母順序排序。
```mysql
> select ename, deptno from emp \
  where deptno in(10, 30) order by ename;
```

- 3.6 顯示薪資超過 1500，且在 10 或 30 號部門工作之員工姓名和薪資，並分別把表頭命名為 Employee 和 Monthly Salary。
```mysql
> select ename Employee, sal "Monthly Salary" from emp \
  1. where sal>1500 and deptno in(10, 30);
  2. where sal>1500 and (deptno=10 or deptno=30);
```

- 3.7 顯示於 1982 年進公司的所有員工之姓名、職稱和進公司日期。
```mysql
> select ename, job, hiredate from emp \
  1. where hiredate like "1982%";
  2. where hiredate between "1982-1-1" and "1982-12-31";
```

- 3.8 顯示沒有主管的員工之姓名和職稱。
```mysql
> select ename, job from emp where mgr is null;
```

- 3.9 顯示所有有賺取佣金的員工之姓名、薪資和佣金，並以薪資和佣金作降冪排列。
```mysql
> select ename, sal, comm from emp \
  1. where comm>0 order by sal desc, comm desc;
  # 無法排除 comm=0 者
  2. where comm is not null order by sal desc, comm desc;
```

- 3.10 顯示所有名字裡第三個英文字母為 A 的員工之姓名與職稱。
```mysql
> select ename, job from emp where ename like "__a%";
```

- 3.11 顯示名字裡有兩個 L，且在 30 號部門工作或經理編號為 7782 之員工姓名、經理員工編號及所屬的部門編號。
```mysql
> select ename, mgr, deptno from emp \
  1. where ename like "%l%l%" and deptno=30 or mgr=7782;
  2. where ename like "%l%l%" and (deptno=30 or mgr=7782);
```

- 3.12 顯示職稱為 Clerk 或 Analyst 且薪水不等於 1000、3000、5000 的員工之姓名、職稱和薪資。
```mysql
> select ename, job, sal from emp \
  where job in("clerk", "analyst") and sal not in(1000, 3000, 5000);
```

- 3.13 顯示佣金比薪水的 1.1 倍還多的員工之姓名、薪資和佣金。
```mysql
> select ename, sal, comm from emp where comm>sal*1.1;
# 若問低於 1.1 倍則需考慮 NULL：where ifnull(comm, 0)<sal*1.1
```
