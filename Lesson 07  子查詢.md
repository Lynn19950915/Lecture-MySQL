<h2 align="center">Lesson 07: 子查詢</h2>

### 7-1 自主子查詢
```mysql
# 多條件子查詢+彙總函數
> select empno, ename, deptno from emp where job= \
  (select job from emp where empno=7499) and \
  sal<=(select avg(sal) from emp);

> select empno, ename, deptno from emp where empno \
  1. in (select [distinct] mgr from emp);
  2. not in (select [distinct] mgr from emp where mgr is not null);
  # 相當於 <max(sal)、>max(sal)
  sal **<any/>all** (select sal from emp where job="clerk");
  # 相當於 >min(sal)、<min(sal)
  sal **>any/<all** (select sal from emp where job="clerk");
```

### 7-2 相關子查詢：無法獨立執行
```mysql
# 個別 custid 有相應之之 max(orderdate)
> select custid, ordid, orderdate from ord A where \
  orderdate=(select max(orderdate) from ord B where A.custid=B.custid);
# 比較：select custid, ordid, max(orderdate) from ord A group by custid;

# 個別 deptno 有相應之 max(sal) (使用等號而非 in)
> select ename, sal, deptno from emp A where \
  sal=(select max(sal) from ord B where A.deptno=B.deptno);
# 比較：select ename, max(sal), deptno from emp A group by deptno;
```

- 存在性測試
```mysql
# 逐一判別 (不) 曾下過訂單者
> select custid, name from customer A where \
  [not] exists(select * from ord B where A.custid=B.custid);
```
