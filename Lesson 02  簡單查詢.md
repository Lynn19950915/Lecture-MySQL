<h2 align="center">Lesson 02: 簡單查詢</h2>

### 2-1 基本資料查詢
```mysql
# 常數：表頭、內容都是 10
> select 10;

> select * from emp;
> select empno, ename, sal from emp;
# 表頭大小依輸入決定
> select EMPNO, ename, sal from emp;

> select empno, ename, sal+1000 from emp;
# 可相加，但若出現空值則顯示 NULL
> select empno, ename, sal+comm from emp;
```

### 2-2 欄位別名
```mysql
> select empno EMP_NUMBER from emp;
> select empno as EMP_NUMBER from emp;
> select empno "員工編號" from emp;
> select empno "emp number#" from emp;
```

### 2-3 設定條件
```mysql
> select job from emp;
# Distinct: 只出現不重複值
> select distinct job from emp;
(=select job from emp group by job;)

# 顯示第 1~5 筆
> select job from emp limit 5;
# 顯示第 6~10 筆 (略過五筆)
> select job from emp limit 5, 5;
```
