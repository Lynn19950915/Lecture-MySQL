<h2 align="center">Exercise 07: 子查詢</h2>

- 7.1 顯示和 Blake 同部門的所有員工之姓名和進公司日期。
```mysql
> select ename, hiredate from emp where deptno= \
  (select deptno from emp where ename="Blake");
```

- 7.2 顯示所有在 Blake 之後進公司的員工之姓名和進公司日期。
```mysql
> select ename, hiredate from emp where hiredate> \
  (select hiredate from emp where ename="Blake");
```

- 7.3 顯示薪資比公司平均薪資高的所有員工之編號、姓名和薪資，並依薪資由高到低排列。
```mysql
> select empno, ename, sal from emp where sal> \
  (select avg(sal) from emp) order by sal desc;
```

- 7.4 顯示和姓名中包含 T 的人在相同部門工作的所有員工之編號和姓名。
```mysql
# 找出符合條件的部門，但要扣掉部門中本身包含 T 的人
> select empno, ename from emp where deptno in \
  (select deptno from emp where ename like "%T%") [and not ename like "%T%"];
```

- 7.5 顯示在 Dallas 工作的所有員工之姓名、部門編號和職稱。
```mysql
# 子查詢可以跨表運用，以 deptno 做為串聯
> select ename, deptno, job from emp where deptno= \
  (select deptno from dept where loc="Dallas");

# join 寫法
> select a.ename, a.deptno, a.job \
  from emp a, dept b using (deptno) where b.loc="Dallas";
```

- 7.6 顯示直屬於 King 的員工之姓名和薪資。
```mysql
> select ename, sal from emp where mgr= \
  (select empno from emp where ename="King");

# join 寫法
> select a.ename, a.sal \
  from emp a join emp b on (a.mgr=b.empno) where b.ename="King";
```

- 7.7 顯示銷售部門 Sales 所有員工之部門編號、姓名和職稱。
```mysql
> select deptno, ename, job from emp where deptno= \
  (select deptno from dept where dname="Sales");

# join 寫法
> select a.deptno, a.ename, a.job \
  from emp a join dept b using (deptno) where b.dname="Sales";
```

- 7.8 顯示薪資比公司平均薪資還要高，且和名字中有 T 的人在相同部門上班的所有員工之編號、姓名和薪資。
```mysql
> select empno, ename, sal from emp where sal> \
  (select avg(sal) from emp) and deptno in \
  (select deptno from emp where ename like "%T%") [and not ename like "%T%"];
```

- 7.9 顯示和有賺取佣金的員工之部門編號和薪資都相同的員工之姓名、部門編號和薪資。
```mysql
> select ename, deptno, sal `from emp a where (deptno, sal) in \
  (select deptno, sal from emp b where comm>0 \
  # 關聯子查詢排除自己：!= 排除
  and a.ename != b.ename);
```

- 7.10 顯示和在 Dallas 工作的員工之薪資和佣金都相同的員工之姓名、部門編號和薪資。
```mysql
> select ename, deptno, sal from emp a where (sal, ifnull(comm, 0)) in \
  (select sal, ifnull(comm, 0) from emp b where a.ename != b.ename) and deptno in \
  (select deptno from dept where loc="Dallas");

# join 寫法
> select ename, deptno, sal from emp a where (sal, ifnull(comm, 0)) in \
  # 將地點、姓名寫入 sal, comm 判定，且 b, c 表均不可寫在子查詢外
  (select sal, ifnull(comm, 0) from emp b join dept c using (deptno) \
  where c.loc="Dallas" and a.ename != b.ename);
```

- 7.11 顯示薪資和佣金都和 Scott 相同的所有員工之姓名、進公司日期和薪資 (不要在結果中顯示 Scott 之資料)。
```mysql
# 可用 in 合併條件相同的子查詢 (同時回傳 Scott 的薪資及佣金)
> select ename, hiredate, sal from emp where (sal, ifnull(comm, 0)) in \
  (select sal, ifnull(comm, 0) from emp where ename="Scott") \
  # 注意：此處條件是寫在子查詢外 (比較 7.9)
  and ename != "Scott";
```

- 7.12 顯示薪資比所有職稱是 Clerks 還高的員工之姓名、進公司日期和薪資，並將結果依薪資由高至低顯示。
```mysql
> select ename, hiredate, sal from emp \
  # all 在此功能等同 min/max 函數
  1. where sal>all (select sal from emp where job="Clerk") order by sal desc;
  2. where sal>(select max(sal) from emp where job="Clerk") order by sal desc;
```
