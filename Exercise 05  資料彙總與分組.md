<h2 align="center">Exercise 05: 資料彙總與分組</h2>

- 5.1 顯示所有員工的最高、最低、總和及平均薪資，依序將表頭命名為 Maximum, Minimum, Sum 和 Average，請將結果顯示為四捨五入的整數。
```mysql
> select max(sal) Maximum, min(sal) Minimum, sum(sal) Sum, round(avg(sal)) Average from emp;
```

- 5.2 顯示每種職稱的最低、最高、總和及平均薪水。
```mysql
> select job, max(sal), min(sal), sum(sal), round(avg(sal), 2) AVG from emp group by job;
```

- 5.3 顯示每種職稱的人數。
```mysql
# 查看不重複職稱要用 distinct (非函數，加上文字即可)
> select [distinct] job, count(job) from emp group by job;
```

- 5.4 顯示資料項命名為 Number of Managers 來表示擔任主管的人數。
```mysql
# mgr 本身已經是欄位，只要查看不重複人數即可
> select count(distinct mgr) "Number of Managers" from emp;
```

- 5.5 顯示資料項命名為 DIFFERENCE 的資料來表示公司中最高和最低薪水間的差額。
```mysql
> select max(sal)-min(sal) DIFFERENCE from emp;
```

- 5.6 顯示每位主管的員工編號及該主管下屬員工最低的薪資，排除沒有主管和下屬員工最低薪資少於 1000 的主管，並以下屬員工最低薪資做降冪排列。
```mysql
# 以 mgr 分組彙總及篩選條件：having
> select mgr, min(sal) from emp group by mgr \
  having mgr is not null and min(sal)>=1000 order by 2 desc;
```

- 5.7 顯示在 1980~1983 各年進公司的員工數量，並給該資料項一個合適的名稱。
```mysql
> select year(hiredate) HIREYEAR, \
  1. count(*) from emp group by HIREYEAR;
  # 組內逐位不重複列出：group_concat 搭配分隔符 |
  2. group_concat([disinct] ename separator "|") from emp group by HIREYEAR;
```
