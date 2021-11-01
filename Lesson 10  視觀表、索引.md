<h2 align="center">Lesson 10: 視觀表、索引</h2>

### 10-1 新增、查詢 view
```mysql
# 遵照基底表格之欄位名稱
> create view emp10 as select * from emp where deptno=10;
# 效力：宣告表頭>欄位別名>欄位名稱
> create view emp20(employee_no, employee, annual_sal) as select empno, ename NAME, sal*12 from emp where deptno=10;

# join 及彙總函數不得用於創建，只可查詢
> create view deptsum as select dname, max(sal)-min(sal), avg(sal) from emp join dept using(deptno) group by dname;
> select count(*), sum(annual_sal) SUM from emp20;
```

### 10-2 view DML
```mysql
# annual_sal 欄值為 null
> insert into emp20(empno, NAME) values(8000, "Anita");
# Error: 計算／彙總欄位，該欄無法新增
> insert into emp20(empno, NAME, sal*12) values(8000, "Anita", 26000);

> update emp10 set salary=2000 where empno=7369;
# view 為虛擬表，使用別名或原欄位名均可
> update emp20 set employee="Irene" where empno=7566;
# Error: 計算／彙總欄位，該欄無法更動
> update emp20 set sal*12=60000 where NAME="jones";

> delete from emp20 where employee_no=8000;
# Error: 違反外來鍵 (king 為自我連結之 mgr FK 內容)
> delete from emp20 where ename="king";
# Error: 計算／彙總欄位，整表無法刪除
> delete from deptsum where dname="accounting";
```

### 10-3 view DDL
```mysql
> alter view emp20 \
  as select empno "EMP#", ename EMPLOYEE, sal from emp where deptno=20;
  # 自動檢查更新條件
  as select empno, ename, sal from emp where deptno=20 with check option;
# Error
> update emp20 set deptno=10 where "EMP#"=7499;

> drop view deptsum;
```

### 10-4 索引
```mysql
> show index from emp;

# (唯一)複合索引鍵
> create [unique] index NONUNI on emp(deptno, job);
# 索引為一物件，亦可視為表格的某一特徵：欄位刪除將出現索引報錯
> [alter table emp] drop index NONUNI [on emp];
```
