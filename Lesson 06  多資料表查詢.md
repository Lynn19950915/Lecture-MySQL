<h2 align="center">Lesson 06: 多資料表查詢</h2>

### 6-1 交叉連結
A, B 表連結，產生 A+B 個欄位以及 A*B 筆紀錄。
```mysql
> select * from emp A cross join dept B;
> select ename, dname from emp A cross join dept B;
# ambiguous: 欄位同名時，宜在表格或欄位加上別稱
> select deptno from emp A cross join dept B;
```

### 6-2 內部連結
1. 雙資料表連結
```mysql
# 需加上條件 on/using，否則預設交叉連結
> select ename, empno, dname, loc from emp join dept;
> select ename, empno, dname, loc from emp join dept using(deptno);
> select ename, empno, dname, loc from emp A join dept B on(A.deptno=B.deptno);
# where: 要加上表格名稱
> select ename, empno, dname, loc from emp A, dept B where A.deptno=B.deptno;

# 不相等的連結條件
> select ename, empno, sal, grade from emp A join salgrade B on(A.sal between B.losal and B.hisal);
> select ename, empno, sal, grade from emp A, salgrade B where A.sal between B.losal and B.hisal;
```

2. 多資料表連結
```mysql
> select name, custid, ordid, itemid from customer join ord using(custid) join item using(ordid);
# join 順序不影響
> select name, A.custid, B.ordid, itemid \
  from customer A join ord B on(A.custid=B.custid) join item C on(B.ordid=C.ordid);
# 傳統寫法：where 並列多個條件
> select name, A.custid, B.ordid, itemid \
  from customer A, ord B, item C where A.custid=B.custid and B.ordid=C.ordid;
```

3. 進階內部連結
```mysql
> select ename, dname, sal from emp join dept using(deptno) where empno=7844;
> select ename, dname, sal from emp, dept where emp.deptno=dept.deptno and empno=7844;

# 搭配 group by(+having) 分組
> select dname, sum(sal) from emp join dept using(deptno) group by dname;
> select dname, sum(sal) from emp join dept using(deptno) group by dname having sum(sal)>10000;
```

### 6-3 外部連結
```mysql
# 以 dept 為主，列出所有配對項
> select dept.deptno, dname, empno, ename from dept left outer join emp using(deptno);
# emp 無 deptno=40 資料，因此不會顯示出
> select dept.deptno, dname, empno, ename from emp left outer join dept using(deptno);
```

### 6-4 自我連結：屬於內部連結
```mysql
# 自我連結無 on 寫法
> select a.empno, a.ename, b.empno, b.ename from emp a join emp b on(a.mgr=b.empno);
# 會少一筆資料：mgr=null (老闆)
> select a.empno, a.ename, b.empno, b.ename from emp a, emp b where a.mgr=b.empno;
```
