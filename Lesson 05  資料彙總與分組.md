<h2 align="center">Lesson 05: 資料彙總與分組</h2>

### 5-1 群組 (彙總) 函數
```mysql
> select count(*) from emp where deptno=10;
# 該欄位不為空值之資料筆數
> select count(job) from emp where deptno=20;
# 非空值且不重複的資料筆數
> select count(distinct job) from emp where deptno=20;

# 均忽略 NULL value
> select max(sal), min(sal), sum(sal), avg(sal) from emp where deptno=30;
```

### 5-2 分組函數
1. 資料分組
```mysql
# 仍可彙總 (全體總和)，但 deptno 無意義
> select deptno, sum(sal) from emp;
# 以 deptno 為單位計算總和，故不必強調 distinct
> select [distinct] deptno, sum(sal) from emp group by deptno;

> select [distinct] comm, count(*) from emp group by comm;
# 除了 NULL value 外都被分組計數
> select [distinct] comm, count(comm) from emp group by comm;
```

2. 進階分組彙總
```mysql
# 階層分組：deptno(1) 相同者再依 job(2) 細分
> select deptno, job, sum(sal) from emp group by 1, 2;
# deptno 由小而大，其中 job 再由大而小進行排序
> select deptno, job, sum(sal) from emp group by 1, 2 order by 1, 2 desc;

> select deptno, job, sum(sal) from emp group by 1, 2 having sum(sal)>3000;
# 最接近但總和未達 3000 的小組
> select deptno, job, sum(sal) from emp group by 1, 2 having sum(sal)<3000 order by 3 desc limit 1;
```

### 5-3 其他補充
1. where vs having
```mysql
# where: (個別) 剔除 clerk 後依職稱分組篩選
> select job, sum(sal) from emp where job !="clerk"
group by job having sum(sal)>5000 order by 2;　

# having: 依職稱分組後，篩選掉 clerk (組別)
> select job, sum(sal) from emp
group by job having sum(sal)>5000 and job !="clerk" order by 2;
```

2. group_concat
```mysql
# 以 deptno 分組寫入 job 之值，預設逗號分隔
> select deptno, group_concat(job) from emp group by deptno;
# 組內不重複寫值 (自動排序)，加上 separator
> select deptno, group_concat(distinct job separator "|") from emp group by deptno;　
```
