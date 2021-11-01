<h2 align="center">Exercise 04: 資料型態、內建函數</h2>

- 4.1 顯示系統目前的日期並將表頭命名為系統日期。
```mysql
> select curdate() "系統日期";
```

- 4.2 顯示所有員工之員工編號、姓名、薪資及將薪資增加 15% (且以整數表示)，並將表頭命名為 New Salary。
```mysql
> select empno, ename, sal, round(sal*1.15, 0) "New Salary" from emp;
```

- 4.3 承 4.2，增加一個資料項表頭命名為 Increase (將 New Salary 減掉 Salary 之值)。
```mysql
> select empno, ename, sal, round(sal*0.15, 0) Increase from emp;
```

- 4.4 顯示員工的姓名、進公司日期、檢討薪資的日期 (指在進公司工作六個月後的第一個星期一)，將該欄命名為 REVIEW，並自訂日期格式為：Sunday, the seventh of September (星期幾, 幾月幾日)。
```mysql
# weekday 由週一起算，以 (7-該數) 再對 7 取餘數，做為外層 interval 之日期
> select ename, hiredate, \
  date_format(adddate(adddate(hiredate, interval 6 month), interval mod(7-weekday(adddate(hiredate, interval 6 month)), 7) day), \
  "%W, the %D of %M") REVIEW from emp;
```

- 4.5 顯示每位員工的姓名，資料項 (MONTHS_WORKED)：計算到今天為止工作了幾個月 (將月數四捨五入到整數)。
```mysql
# round 預設 0 (四捨五入取整數)
> select ename, round(datediff(curdate(), hiredate)/30, 0) MONTHS_WORKED from emp;
```

- 4.6 顯示如下格式：<員工姓名>earns<薪水>but wants<3倍的薪水>，並將表頭顯示為 Dream Salaries。
```mysql
> select concat(ename, " earns ", sal, " but wants ", sal*3) "Dream Salaries" from emp;
```

- 4.7 顯示所有員工之姓名和薪資，設定薪資長度為 15 個字元並且在左邊加上＄符號，將表頭命名為 SALARY。
```mysql
> select ename, lpad(sal, 15, "$") SALARY from emp;
```

- 4.8 顯示員工之姓名、進公司日期、資料項 (DAY)：顯示員工被雇用的那天為星期幾，並以星期一作為一週的起始日，依星期排序。
```mysql
# 星期幾以 dayname 呈現 (英文)，若要以星期數排序需加寫 weekday
> select ename, hiredate, \
  1. dayname(hiredate) DAY, weekday(hiredate) from emp order by 4;
  2. date_format(hiredate, '%W') DAY, weekday(hiredate) from emp order by 4;
```

- 4.9 顯示員工的姓名和名為 COMM 的欄位：顯示佣金額，如果該員工沒有賺取佣金則顯示 No Commission。
```mysql
> select ename, ifnull(comm, "No Commission") COMM from emp;
```

- 4.10 顯示資料項命名為 EMPLOYEE_AND_THEIR_SALARIES 的資料來顯示所有員工之名字和薪資，且用星號來表示 (每一個星號代表 100 元)，並以薪資由高到低來顯示。
```mysql
# 用 concat 整合訊息為單一欄位、lpad 調整左右對齊
> select concat(lpad(ename, 6, " "), lpad(sal, 8, " "), " ", \
  1. repeat("＊", floor(sal/100))) "EMPLOYEE AND THEIR SALARIES" from emp order by sal desc;
  2. repeat("＊", truncate((sal/100), 0))) "EMPLOYEE AND THEIR SALARIES" from emp order by sal desc;
```
