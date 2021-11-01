<h2 align="center">Exercise 10: 視觀表、索引</h2>

- 10.1 使用 EMP 資料表中的員工編號 (empno)、姓名 (ename)及部門編號 (deptno)來建立一個 EMP_VU view，並將姓名欄位改成 EMPLOYEE。
```mysql
> create view EMP_VU as select ename EMPLOYEE, ename, deptno from emp;
```

- 10.2 顯示 EMP_VU view 中的資料內容。
```mysql
> select * from EMP_VU;
```

- 10.3 使用 EMP_VU view 來顯示所有員工之姓名及部門編號。
```mysql
# EMP_VU 的基底為 emp，改成 ename 一樣能執行
> select EMPLOYEE, deptno from EMP_VU;

# 注意：但若在 create 時宣告新欄名，ename 就無法查詢
```

- 10.4 新建一個名為 DEPT20 的 view，包含在部門 20 的所有員工之編號、姓名及部門編號。將 view 中的資料項目命名為 EMPLOYEE_ID, EMPLOYEE, DEPARTMENT_ID。
```mysql
# check option: 不允許使用者透過 DEPT20 更改員工的部門編號
> create view DEPT20 as select empno EMPLOYEE_ID, ename EMPLOYEE, deptno DEPARTMENT_ID from emp where deptno=20 with check option;
```

- 10.5 顯示 DEPT20 view 的欄位定義 (資料結構) 及其所有資料內容。
```mysql
> desc DEPT20;
> select * from DEPT20;
```

- 10.6 試著利用 DEPT20 view 將 Smith 轉調到部門 30。
```mysql
> update DEPT20 set DEPARTMENT_ID=30 where EMPLOYEE="Smith";
# ERROR 1369 (HY000): CHECK OPTION failed 'mydb.dept20';
```

- 10.7 新建一個名為 SALARY_VU 的 view，包含員工之姓名、部門名稱、薪資和薪資等級。將 view 中的資料項目分別命名為 Employee, Department, Salary, Grade。
```mysql
> create view SALARY_VU as select a.ename EMPLOYEE, a.deptno DEPARTMENT, a.sal SALARY, b.grade \
  # where 寫法亦可，只是將 between 條件由數字改設為欄位
  1. from emp a, salgrade b where a.sal between b.losal and b.hisal;
  2. from emp a join salgrade b on a.sal between b.losal and b.hisal;
```
