<h2 align="center">Lesson 04: 資料型態、內建函數</h2>

### 4-1 日期、時間顯示
```mysql
> select empno, ename, hiredate from emp \
  # YYMMDD/YYYYMMDD 字串均可
  1. where hiredate="811203"/"81-12-03"/"81.12.03"/"81/12/03";
  2. where hiredate="19811203"/"1981-12-03"/"1981.12.03"/"1981/12/03";
```

### 4-2 字串函數
1. 基本顯示
```mysql
# 字母長度與字元個數
> select ename, length(ename), char_length(ename) from emp;
> select ename, lower/lcase(ename), upper/ucase(ename) from emp;
> select concat(ename, "+", empno) from emp;
```

2. 擷取與增補
```mysql
> select ename, left(ename, 3), right(ename, 3), reverse(ename) from emp;
# 以下兩行結果相同
> select ename, substring(ename, 1), substring(ename, 1, 3) from emp;
> select ename, substring(ename from 1), substring(ename from 1 for 3) from emp;

> select empno, ltrim(empno), rtrim(empno), trim(empno) from emp;
# 可用單符、多符或欄位做為變數填入
> select ename, lpad(ename, 8, "#"), lpad(ename, 8, "*@"), rpad(ename, 8, deptno) from emp;
```

3. 查找與取代
- instr: 查找字根在後
- locate: 查找字根在前
```mysql
> select ename, instr(ename, "A") from emp;
> select ename, locate("A", ename) from emp;
```

```mysql
# 從第 4 字元插入，覆蓋取代掉 N 個字元 (長度無限制)
> select ename, insert(ename, 4, 0/1/2, "<3") from emp;
# 將變數中的某字元全數取代 (長度無限制)
> select ename, replace(ename, "A", "*"/"**") from emp;
> select ename, repeat("*", length(ename)) from emp;
```

### 4-3 數值函數
```mysql
# 要宣告位數，預設為 0 (此時 truncate=floor)
> select round(1995.915, 2), truncate(1995.915, 2);
> select ceiling(1995.915), floor(1995.915);
# abs 取絕對值、sign 判斷正負 (-1/0/1)
> select mod(5, 2), power(5, 2), sqrt(25), abs(-11), sign(-11);
```

### 4-4 日期、時間函數
1. 查詢相關
```mysql
# 日期、時間、日期+時間
> select curdate(), curtime(), current_timestamp, utc_date(), utc_time(), utc_timestamp();
> select year/month/day/hour/minute/second(current_timestamp());
```
- 描述月份
```mysql
> select hiredate, extract(month from hiredate), monthname(hiredate) from emp;
```
- 描述週次／星期<br>
weekday 由 0 起算、dayofweek 由 1 起算
```mysql
> select hiredate, extract(week from hiredate), week(hiredate), weekofyear(hiredate) from emp;
# dayofweek 從週日起編 =1、weekday 從週一起編 =0
> select hiredate, dayname(hiredate), dayofweek(hiredate), weekday(hiredate) from emp;
```
- 描述日期
```mysql
> select hiredate, extract(day from hiredate), dayofmonth(hiredate), dayofyear(hiredate) from emp;
```

2. 計算相關
```mysql
# 由前減後
> select datediff(curdate(), "2019-9-15");
> select adddate(curdate(), interval 10 day), subdate(curdate(), interval 10 day);
# 只差在 curtime/timestamp 格式
> select addtime/timestamp(curtime(), "10:00"), subtime/timediff(curtime(), "10:00");
# makedate(year, dayofyear)
> select date(191004), makedate(2019, 300), maketime(23, 15, 20);
```

### 4-5 其他函數
1. 格式化輸出
```mysql
# Friday, the 4th of October, 2019
> select date_format(curdate(), "%W, the %D of %M, %Y");
# Fri, 4/10/19
> select date_format(curdate(), "%a, %e/%c/%y");
# 23:24:14, 11:26:56 PM (等同於 %r)
> select date_format(curtime(), "%H:%i:%S"), date_format(curtime(), "%h:%i:%S %p");
```

2. 其他通用
- ifnull: 判斷第一個值是否 NULL，若是則回傳第二個值。
- nullif: 判斷給的兩個值是否相等，若是則回傳 NULL。
```mysql
> select ename, if(ifnull(comm, 0)>0, "A", "B") from emp;
> select ename, nullif(sal, 3000) from emp;
```

```mysql
# 無號數之轉換：二進位轉十進位
> select convert(1-2, unsigned), convert(convert(1-2, unsigned), signed);
# 查看系統資訊
> select user(), version(), database(), connection_id(), charset("lynn.915");
```
