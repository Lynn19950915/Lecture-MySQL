<h2 align="center">Exercise 06: 多資料表查詢</h2>

- 6.1 顯示所有員工之姓名、所屬部門編號、部門名稱及部門所在地點。
```mysql
> select a.ename, a.deptno, b.dname, b.loc \
  from emp a join dept b using (deptno);
```

- 6.2 顯示所有有賺取佣金的員工之姓名、佣金金額、部門名稱及部門所在地點。
```mysql
> select a.ename, a.comm, b.dname, b.loc \
  1. from emp a join dept b using (deptno) where comm>0;
  # join 也可以寫在 where 條件中
  2. from emp a, dept b where a.deptno=b.deptno and comm>0;
```

- 6.3 顯示姓名中包含有 A 的員工之姓名及部門名稱。
```mysql
# % 在比對上代表 0 至多個字元
> select a.ename, b.dname \
  1. from emp a join dept b using (deptno) where ename like "%A%";
  2. from emp a, dept b where a.deptno=b.deptno and ename like "%A%";
```

- 6.4 顯示所有在 DALLAS 工作的員工之姓名、職稱、部門編號及部門名稱。
```mysql
> select a.ename, a.job, b.deptno, b.dname \
  1. from emp a join dept b using (deptno) where loc="DALLAS";
  2. from emp a, dept b where a.deptno=b.deptno and loc="DALLAS";
```

- 6.5 顯示出表頭名為：Employee, Emp#, Manager, Mgr#，分別表示所有員工之姓名、員工編號、主管姓名、主管之員工編號。
```mysql
# 主管姓名就是將 mgr 丟回 emp 再查一次，同個表可以自身 join
> select a.ename Employee, a.empno "Emp#", b.ename Manager, b.empno "Mgr#" \
  1. from emp a join emp b on (a.mgr=b.empno);
  2. from emp a, emp b where (a.mgr=b.empno);
```

- 6.6 顯示出 SALGRADE 資料表的結構，並建立一查詢顯示所有員工之姓名、職稱、部門名稱、薪資及薪資等級。
```mysql
> desc salgrade;
# 與各表列出順序無關，重點是 join 條件宣告 (可搭配 between)
> select a.ename, a.job, b.dname, a.sal, c.grade \
  from emp a join dept b on (a.deptno=b.deptno) \
  join salgrade c on (a.sal between c.losal and c.hisal);
```

- 6.7 顯示出表頭名為：Employee, Emp Hiredate, Manager, Mgr Hiredate 的資料項，顯示所有比自己主管還早進公司之員工姓名、進公司日期和主管姓名及進公司日期。
```mysql
> select a.ename Employee, a.hiredate "Emp Hiredate", b.ename Manager, b.hiredate "Mgr Hiredate" \
  1. from emp a join emp b on (a.mgr=b.empno) where a.hiredate<b.hiredate;
  2. from emp a, emp b where a.mgr=b.empno and a.hiredate<b.hiredate;
```

- 6.8 顯示出表頭名為：dname, loc, Number of People, Salary 的資料來顯示所有部門之名稱、所在地點、員工數量及員工平均薪資 (四捨五入取到小數第二位)。
```mysql
# 所有部門都要顯示，以 dept 為主 [left] join
> select a.dname, a.loc, count([distinct] b.empno) "Number of People", round(avg(b.sal), 2) Salary \
  # group by dname 也可以 (任何表的欄位均可分組)
  from dept a left join emp b using(deptno) group by deptno;
```
