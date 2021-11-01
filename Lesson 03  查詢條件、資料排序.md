<h2 align="center">Lesson 03: 查詢條件、資料排序</h2>

### 3-1 條件查詢
```mysql
> select ename, job from emp where sal>=2000;
# 字串加上引號，可用 =、!= 比對
> select ename, job from emp where job="analyst";
# 日期加上引號，適用於比較運算
> select ename, job from emp where hiredate<"1981-10-4";
```

### 3-2 結合邏輯運算
```mysql
> select ename, job from emp where sal>=1000 and job="clerk";
> select ename, job from emp where hiredate<"1981-10-4" or job="analyst";
# 注意：NOT 碰到帶有 NULL 之資料欄位，易出狀況
> select ename, job from emp where not(sal>2000 or job="manager");　
> select ename, job from emp where job="manager" or job="salesman" and sal<1500;

# 順序：()>not>and>or
> select ename, job from emp where (job="manager" or job="salesman") and sal<1500;
```

### 3-3 其他特定運算
```mysql
> select ename, job from emp where hiredate between "1981-2-15" and "1981-6-15";
# between: 運用首字進行比對
> select ename, job from emp where ename between "A" and "E";

# in: 完整比對 (只含這兩個數字及字母)
> select ename, job from emp where sal in(1000, 2000);
> select ename, job from emp where ename in("A", "E");

# 姓名第二個字母為 C
> select ename, job from emp where ename like "_C%";
# 姓名包含 A, E，且 A 在 E 前面
> select ename, job from emp where ename like "%A%E%";
# (NULL=NULL): NULL
> select ename, job from emp where mgr is not null;
```

### 3-4 case 運算
1. case 提供多種判別，以 case 開頭、when...then 定義條件、end 結尾。

2. case 本身形同欄位，要加上逗號、可定義別名。
```mysql
> select ename, job,
  case job
  when "president" then sal*2
  when "manager" then sal*1.5
  else sal
  end sal
  from emp;

> select ename, job,
  case
  when sal between 0 and 1500 then "A"
  when sal between 1501 and 3000 then "B"
  else "C"
  end sal
  from emp;
```

### 3-5 資料排序
```mysql
# 依據首字母升冪排序
> select ename, job from emp where sal>2200 order by ename [asc];
# 不在 select 的欄位也可以用來排序
> select ename, job from emp where sal>2200 order by hiredate desc;

# 多階排序條件，數字 1 代表第一欄
> select ename, job from emp where sal>2200 order by sal desc, 1;
# 10 部門中最高薪資第 1~5 名
> select ename, job from emp where deptno=10 order by sal desc limit 5;
# 20 部門中最低薪資第 6~10 名
> select ename, job from emp where deptno=20 order by sal limit 5, 5;
```
