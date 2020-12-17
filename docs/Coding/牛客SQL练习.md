# SQL练习

> 鉴于同学被字节狂问SQL题，因此也激发了我的危机感。 作为非科班的， 写SQL还是比较慌的， 因此做下专题训练。

## 理论知识:

SQL语句执行顺序

### 1.[sql执行顺序 ](https://www.cnblogs.com/yyjie/p/7788428.html)

```sql
(1) from 
(3) join 
(2) on 
(4) where 
(5) group by(开始使用select中的别名，后面的语句中都可以使用)
(6) avg,sum.... 
(7) having 
(8) select 
(9) distinct 
(10) order by 
```

2.[sql语句执行顺序](https://blog.csdn.net/u013887008/article/details/93377939)

```sql
(8) SELECT (9)DISTINCT<select_list>
(1) FROM <left_table>
(3) <join_type> JOIN <right_table>
(2)         ON <join_condition>
(4) WHERE <where_condition>
(5) GROUP BY <group_by_list>
(6) WITH {CUBE|ROLLUP}
(7) HAVING <having_condition>
(10) ORDER BY <order_by_list>
(11) LIMIT <limit_number>
```

### 用`group by`需要注意的:

- 在select**指定的字段**
  - 要么就是**包含在Group By语句**的后面，为作为分组的依据的字段；
  - 要么就要**被包含在聚合函数**中, e..g: `sum, avg, count`。



### SQL查询语句中的 limit 与 offset 的区别：

- `limit y` 分句表示: 读取 y 条数据
- `limit x, y` 分句表示: 跳过 x 条数据，读取 y 条数据
- `limit y offset x` 分句表示: 跳过 x 条数据，读取 y 条数据

#### 分页操作

语法：limit开始索引，每页查询的记录数
注：索引从0开始
`公式：开始索引=（当前页码-1）*每页查询的记录数`即 `index = (nowPageNum - 1) * pageSize`

```sql
SELECT * FROM table 
WHERE 查询条件 
ORDER BY 排序条件 
LIMIT ((页码-1)*页大小),页大小;
-- LIMIT (pageNum-1)*pageSize, pageSize
-- 第一个参数是偏移量， 第二个是所取数据数
```



### 引号区别
> 在标准 SQL 中，字符串使用的是**单引号**。
> 如果字符串本身也包括单引号，则使用两个单引号（注意，不是双引号，字符串中的双引号不需要另外转义）。
> 但在其它的数据库中可能存在对 SQL 的扩展，比如在 MySQL 中允许使用单引号和双引号两种。

MySQL 参考手册：
> 字符串指用单引号`'`或双引号`"`引起来的字符序列。例如：
> 'a string'
> "another string"
> 如果SQL服务器模式启用了NSI_QUOTES，可以只用单引号引用字符串。用双引号引用的字符串被解释为一个识别符。

```sql
使用双字符:
插入时          库中
'aa''b''cc'     aa'b'cc
"aa"b""cc"      aa"b"cc

使用转义字符(\):
插入时          库中
'aa\'b\'cc'     aa'b'cc
"aa\"b\"cc"     aa"b"cc

在单引号包裹的字符串中使用双引号、在双引号包裹的字符串中使用单引号 不需要使用双引号或转义字符。
插入时          库中
"aa'b'cc"       aa'b'cc
'aa"b"cc'       aa"b"cc
```

反引号（`）

```sql
保留字不能用于表名，比如desc，此时需要加入反引号来区别，但使用表名时可忽略反引号。
create table desc报错
create table `desc`成功
create table `test`成功
drop table test成功
 
保留字不能用于字段名，比如desc，此时也需要加入反引号，并且insert等使用时也要加上反引号。
create table `test`（`desc` varchar(255)）成功
insert into test(desc) values('fxf')失败
insert into test(`desc`) values('fxf')成功
```

[+号](https://blog.csdn.net/ZHUYUEHE/article/details/50377636?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-9.pc_relevant_is_cache&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-9.pc_relevant_is_cache)

<u>字符串数据</u>是用单引号包在外面的，而<u>+号</u>只是用来连接这些字符串的. 数据库里的字段是整型的时候不要加单引号，是字符串的时候要加，其它类型根据实际情况来,双引号就是用来拼接字符串的，单引号是sql文的固有写法，因为你要动态的来拼接，涉及到变量，所以要用“+”来组合各个字符串片段。最终结果无非就是得出能在数据库查询分析器中执行的sql文。

```sql
        String sql = "insert into student values ( " + student.getId() + " ,' "
                + student.getUsername() + " ',  " + student.getAge() + " ,' "
                + student.getClassnumber()+" ')";
```

因为id和age是int型的所以不用加单引号，你的Username在数据库中定义的是一个varchar型的,而对字符型进行条件查询的时候是要加 ' '号的：`select  count(*)  from  student  where username= 'aaa ' `
因此在后台写查询字符串的时候就必须这样写: `string  sql  =  "select  count(*)  from  student  where username= ' "+userName+ " ' " `，这样映射成的查询语句就是: `select  count(*)  from student where student= 'aaa ' ` 了. 

---



题目

## 1[ 查找最晚入职员工的所有信息](https://www.nowcoder.com/practice/218ae58dfdcd4af195fff264e062138f?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)(入门题)

```sql
select * from employees order by hire_date limit 1;
```

## 2[查找入职员工时间排名倒数第三的员工所有信息](https://www.nowcoder.com/practice/ec1ca44c62c14ceb990c3c40def1ec6c?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select * from employees 
order by hire_date desc 	-- 递减排序
limit 2,1 ;					-- offset 2， 取1
```

### SQL查询语句中的 limit 与 offset 的区别：

- `limit y` 分句表示: 读取 y 条数据
- `limit x, y` 分句表示: 跳过 x 条数据，读取 y 条数据
- `limit y offset x` 分句表示: 跳过 x 条数据，读取 y 条数据

## 3[ 查找各个部门当前领导当前薪水详情以及其对应部门编号dept_no](https://www.nowcoder.com/practice/c63c5b54d86e4c6d880e4834bfd70c3b?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select s.*, d.dept_no 
from salaries as s
    join dept_manager as d
    on s.emp_no = d.emp_no
where d.to_date='9999-01-01' and s.to_date='9999-01-01'
order by s.emp_no;
```



## 4[ 查找所有已经分配部门的员工的last_name和first_name](https://www.nowcoder.com/practice/6d35b1cd593545ab985a68cd86f28671?tpId=82&rp=1&ru=%2Fta%2Fsql&qru=%2Fta%2Fsql%2Fquestion-ranking)

```sql
select e.last_name, e.first_name, d.dept_no 
from employees e 
inner join dept_emp d 
on e.emp_no = d.emp_no;
```

## 5[查找所有员工的last_name和first_name以及对应部门编号dept_no](https://www.nowcoder.com/practice/dbfafafb2ee2482aa390645abd4463bf?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 暂时没有分配具体部门的员工==> employees有信息, 而dept_emp表中可能还没有信息；两表联合查询时以employees为准， 匹配不到dept_emp的数据用null填充--->所以用外部联结的左联结

```sql
select e.last_name, e.first_name, d.dept_no 
from employees e
left join dept_emp d
on e.emp_no = d.emp_no

```

### join

#### 内联结(Inner join)

联接仅返回<u>两个联接表中都具有匹配项的行</u>。例如，您可以将employees和*departments*表联接在一起，以创建一个显示每个雇员的部门名称的结果集。在内部联接中，<u>没有部门信息的雇员不包括在结果集中，没有雇员的部门也不会包括在结果集中</u>。

#### 外联结(Outer join)

外联接是内部联接的扩展。 即使外联接在联接表中没有相关行，外联接也会返回这些行。 外联接共有三种类型：左联接（[left join](https://www.nhooo.com/sql/sql-left-join-operation.html)），右联接（[right join](https://www.nhooo.com/sql/sql-right-join-operation.html)）和完全联接（[full join](https://www.nhooo.com/sql/sql-full-join-operation.html)）。

left join(左联接) 返回包括**左表中的所有记录**和右表中<u>联结字段相等</u>（有匹配项）的记录 ，否则用NULL
right join(右联接) 返回包括**右表中的所有记录**和左表中<u>联结字段相等</u>的记录
inner join(等值连接) 只返回两个表中<u>联结字段相等</u>的行

![](https://images0.cnblogs.com/i/407365/201405/241947220904425.jpg)

总结: 

- inner join是两集合取交集
- **FULL [OUTER] JOIN**: 两集合取并集
- left [outer] join: 产生表A的完全集, B中有匹配则有值, 没匹配则为null
  - left join是以A表的记录为基础的,A可以看成左表,B可以看成右表,left join是以左表为准的.换句话说,左表(A)的记录将会全部表示出来,而右表(B)只会显示符合搜索条件的记录(例子中为: A.aID = B.bID).
    B表记录不足的地方均为NULL填充.

Q: 最上层的两张图分别是全A和全B，那么left join和right join的作用是什么呢?

A: 联表查询, 拓展字段



## 6[ 查找所有员工入职时候的薪水情况](https://www.nowcoder.com/practice/23142e7a23e4480781a3b978b5e0f33a?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 两表并列查找，题目重点在于: *有多条薪水信息中找出入职时候的薪水情况*

```sql
select e.emp_no, s.salary 
from employees e, salaries s
where e.emp_no = s.emp_no and e.hire_date = s.from_date
order by e.emp_no desc
```

> 联表查询

```sql
select e.emp_no,s.salary 
from employees e
left join salaries s
where e.emp_no= s.emp_no and e.hire_date = s.from_date 
order by e.emp_no desc
```



## 7[查找薪水涨幅超过15次的员工号emp_no以及其对应的涨幅次数t](https://www.nowcoder.com/practice/6d4a4cff1d58495182f536c548fee1ae?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> - 将select出来的数据重命名
> - having用法

```sql
select s.emp_no, count(s.emp_no) as t
from salaries s
group by s.emp_no
having count(salary) > 15 
-- 由于吧count(s.emp_no)替换成t了, 因此这边可以写成 having t > 15, 见执行顺序avg,count等聚合函数优先于having
```

### Question:

Q: `select count(s.emp_no) as t`的执行顺序在`having t > 15`之前吗?

A: No是聚合函数count优先于having

### **SQL 别名: AS**

- SQL 别名用于为 表 或 表中的列 提供临时名称。 
- SQL 别名通常用于使 表名 或 列名 更具可读性。 
- SQL 一个别名只存在于查询期间。 

别名使用 AS 关键字赋予。 

#### 什么情况下需要给表起别名？

1.表名比较长
2.当需要在多个表中进行查询并把查询内容同时输出的时候
3.当需要进行表连接的时候（其实和2一个意思，一般情况下多个表进行连接主要目的就是为了从多个表中查询所需要的内容）



### having

在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与<u>聚合函数</u>一起使用。

HAVING 子句可以让我们筛选分组后的各组数据。

### 聚合函数

> 聚合函数对一组值执行计算并返回单一的值

#### 聚合函数有什么特点？

1. 除了 COUNT 以外，聚合函数忽略空值。
2. 聚合函数经常与 SELECT 语句的 **GROUP BY** 子句一同使用。
3. 所有聚合函数都具有确定性。任何时候用一组给定的输入值调用它们时，都返回相同的值。
4. 标量函数：只能对单个的数字或值进行计算。主要包括字符函数、日期/时间函数、数值函数和转换函数这四类。

## 8[ 找出所有员工当前具体的薪水salary情况](https://www.nowcoder.com/practice/ae51e6d057c94f6d891735a48d1c2397?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select distinct salary 
from salaries
where to_date = '9999-01-01'
order by salary desc

-- 或者使用group by
select salary 
    from salaries 
    where to_date = '9999-01-01'
    group by salary
order by salary desc 
```

说明：
对于distinct与group by的使用：
1.当对系统的性能高并且数据量大时使用group by
2.当对系统的性能不高时或者使用数据量少时两者借口
3.尽量使用group by



## 9[获取所有部门当前manager的当前薪水情况](https://www.nowcoder.com/practice/4c8b4a10ca5b44189e411107e1d8bec1?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 用where连接并列查询的两表
select d.dept_no, s.emp_no, s.salary 
from dept_manager as d, salaries as s
where d.emp_no = s.emp_no and d.to_date='9999-01-01' and s.to_date='9999-01-01';

-- 用inner join合并两表
select d.dept_no, s.emp_no, s.salary
from dept_manager as d
inner join salaries as s
on d.to_date = '9999-01-01' and s.to_date = '9999-01-01' and d.emp_no = s.emp_no;
```



## 10[ 获取所有非manager的员工emp_no](https://www.nowcoder.com/practice/32c53d06443346f4a2f2ca733c19660c?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 把在dept_manager中的都筛选掉, 之前join的练习: A - A∩B

```sql
-- LEFT JOIN左连接 + IS NULL
select e.emp_no
from employees as e
left join dept_manager d
on e.emp_no = d.emp_no 
where d.emp_no is null
-- where d.emp_no isnull 中 isnull是个关键字, 正确用法是
-- ISNULL ( check_expression , replacement_value )将被检查是否为 NULL的表达式替换为replacement_value

-- NOT IN+子查询
select emp_no
from employees where emp_no 
not in (select emp_no from dept_manager)
```

使用见: [#join](#join)

- 只有left join的效果

![left_join](SQL练习/left_join.jpg)

- 加上is null的效果 ==> 找出B表中emp_no不匹配的(他们填充的数据都是null)

![isnull](SQL练习/isnull.jpg)





## 11[ 获取所有员工当前的manager](https://www.nowcoder.com/practice/e50d92b8673a440ebdf3a517b5b37d62?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- my
select de.emp_no, dm.emp_no as manager_no
from dept_emp as de
left join  dept_manager dm
on de.dept_no = dm.dept_no
where dm.to_date ='9999-01-01' and dm.emp_no != de.emp_no

-- 题解: INNER JOIN+不等于; 不等于可以用<>或者!=表示
SELECT de.emp_no, dm.emp_no AS manager_no 
FROM dept_emp AS de INNER JOIN dept_manager AS dm
ON de.dept_no = dm.dept_no 
WHERE dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01' AND de.emp_no <> dm.emp_no
```



## 12[ 获取所有部门中当前员工薪水最高的相关信息](https://www.nowcoder.com/practice/4a052e3e1df5435880d4353eb18a91c6?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select de.dept_no, de.emp_no, max(s.salary) 
from dept_emp as de 
inner join salaries as s
on s.emp_no = de.emp_no and de.to_date = '9999-01-01' and s.to_date = '9999-01-01'
group by de.dept_no
```

使用GROUP BY子句时，SELECT子句中只能有聚合键、聚合函数、常数。



## 13[ 从titls表获取按照title进行分组](https://www.nowcoder.com/practice/72ca694734294dc78f513e147da7821e?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select title, count(title) as t
from titles
group by title
having t >= 2

```



## 14 [ 从titles表获取按照title进行分组，注意对于重复的emp_no进行忽略](https://www.nowcoder.com/practice/c59b452f420c47f48d9c86d69efdff20?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 忽略重复的emp_no， 上题的count(title) 其实可以写成count(emp_no) , 即有一条包含title的条目就++，而emp_no是其主键, 因此可以用emp_no的数目来代替title的数目。因此这题要求的不重复emp_no直接加个distinct即可

```sql
select title, count(distinct emp_no) as t
from titles
group by title
having t >= 2
```



## 15[ 查找employees表所有emp_no为奇数](https://www.nowcoder.com/practice/a32669eb1d1740e785f105fa22741d5c?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)



```sql
select emp_no, birth_date, first_name, last_name, gender, hire_date
from employees
where emp_no % 2 == 1 and last_name != 'Mary' 
-- 补充：emp_no % 2=1也可以改成MOD(emp_no, 2)=1，但是某些sql版本可能不支持后者(比如题库就不支持)
order by hire_date desc
```



## 16[ 统计出当前各个title类型对应的员工当前薪水对应的平均工资](https://www.nowcoder.com/practice/c8652e9e5a354b879e2a244200f1eaae?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 通过t.title来进行分组

```sql
select t.title, avg(s.salary)
from titles as t
inner join salaries as s
on t.emp_no = s.emp_no and t.to_date ='9999-01-01' and s.to_date = '9999-01-01'
--  on t.emp_no=s.emp_no where t.to_date='9999-01-01' and s.to_date='9999-01-01' 也行, 表示在on执行后生成的虚拟表上再执行where
group by t.title
```

注意：AVG(*)是自动命名为avg的，所以不用重命名





## 17 [ 获取当前薪水第二多的员工的emp_no以及其对应的薪水](https://www.nowcoder.com/practice/8d2c290cc4e24403b98ca82ce45d04db?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 考验limit用法

```sql
select emp_no, salary 
from salaries
order by salary desc
limit 1, 1
```

## 18[ 查找当前薪水排名第二多的员工编号emp_no](https://www.nowcoder.com/practice/c1472daba75d4635b7f8540b837cc719?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 用MAX函数，先查出最大salary，再利用<得到不含最大salary的子表，在子表上再求最大值
select e.emp_no, max(s.salary), e.last_name, e.first_name
from employees as e
inner join salaries as s
on e.emp_no = s.emp_no
where to_date = '9999-01-01'
and salary < ( select max(salary) from salaries as s where s.to_date = '9999-01-01') 	
```



## 19[查找所有员工的last_name和first_name以及对应的dept_name](https://www.nowcoder.com/practice/5a7975fabe1146329cee4f670c27ad55?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 1. 列出`employees`表里所有员工last_name, first_name,
> 2. 根据`employees`中emp_no对应`dept_emp`中的dept_no,没有分配的员工找不到对应-->采用`LEFT JOIN`
> 3. 再根据dept_no对应`departments`表中的dept_name,没有分配的员工找不到对应-->采用`LEFT JOIN`

```sql
select e.last_name, e.first_name, dm.dept_name
from employees as e
left join dept_emp as de		-- 这边使用left join因为要针对没有分配部门的员工
on e.emp_no = de.emp_no
left join departments as dm
on de.dept_no = dm.dept_no
```



## 20[查找员工编号emp_now为10001其自入职以来的薪水salary涨幅值growth](https://www.nowcoder.com/practice/c727647886004942a89848e2b5130dc2?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 最大值-最小值

```sql
-- 题解, sum的结果默认为growth
select (max(salary)-min(salary)) as growth
from salaries
where emp_no='10001';
```



## 21[ 查找所有员工自入职以来的薪水涨幅情况](https://www.nowcoder.com/practice/fc7344ece7294b9e98401826b94c6ea5?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 这题比较有难度

```sql
select la.emp_no, (now.salary - la.salary) as growth
from (select e.emp_no, s.salary
     from employees as e
     left join salaries as s
     on e.emp_no = s.emp_no and e.hire_date = s.from_date ) as la    -- 入职时的工资表
inner join
    (select e.emp_no, s.salary
    from employees as e
    left join salaries as s
    on e.emp_no = s.emp_no 
    where s.to_date = '9999-01-01') as now      -- 现在的工资表
on la.emp_no = now.emp_no
order by growth asc			-- order by 默认asc

```





## 22[统计各个部门的工资记录数](https://www.nowcoder.com/practice/6a62b6c0a7324350a6d9959fa7c21db3?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)



```sql
-- my
select d.dept_no, d.dept_name, count(d.emp_no) as `sum`
from  
    (select * from dept_emp as de
    inner join departments as dm
    on de.dept_no = dm.dept_no) as d
inner join salaries as s
on d.emp_no = s.emp_no	
group by d.dept_no		-- 根据题目要求（统计各个部门的工资记录数）确定group by对象

-- 题解
select dm.dept_no, dm.dept_name, count(*)
from  departments as dm
inner join 
    (select * from dept_emp as de
     --    (dept_emp as de		也可以
    inner join salaries as s
    on de.emp_no = s.emp_no) as d
on dm.dept_no = d.dept_no
group by d.dept_no

```







## 23[对所有员工的当前薪水按照salary进行按照1-N的排名](https://www.nowcoder.com/practice/b9068bfe5df74276bd015b9729eec4bf?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> SQL窗口函数（OLAP函数）中用于排序的专用窗口函数用法: RANK函数的使用 ->mysql不支持

```sql
select emp_no, salary, dense_rank() over (order by salary desc) as rank
from salaries
where to_date = '9999-01-01'
```

下面介绍三种用于进行排序的专用窗口函数：

1、RANK()

  在计算排序时，若存在相同位次，会跳过之后的位次。

  例如，有3条排在第1位时，排序为：1，1，1，4······

2、DENSE_RANK()

  这就是题目中所用到的函数，在计算排序时，若存在相同位次，不会跳过之后的位次。

  例如，有3条排在第1位时，排序为：1，1，1，2······

3、ROW_NUMBER()

  这个函数赋予唯一的连续位次。

  例如，有3条排在第1位时，排序为：1，2，3，4······

窗口函数用法：

```
<开窗函数> over ([partition by <列清单>] order by <排序用列清单>)
```

**开窗函数大体可以分为以下两种：**

1.能够作为开窗函数的聚合函数（sum，avg，count，max，min）
2.rank，dense_rank。row_number等专用开窗函数。

### 1.4 开窗函数和聚合函数的区别

（1）SQL 标准允许将所有聚合函数用作开窗函数，用OVER 关键字区分开窗函数和聚合函数。
（2）聚合函数每组只返回一个值，开窗函数每组可返回多个值。

## 24[ 获取所有非manager员工当前的薪水情况](https://www.nowcoder.com/practice/8fe212a6c71b42de9c15c56ce354bebe?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 方法1：多表联查+NOT IN
SELECT de.dept_no, de.emp_no, s.salary
    FROM dept_emp AS de, employees AS e, salaries AS s
WHERE de.emp_no=e.emp_no
    AND de.emp_no=s.emp_no
    AND s.to_date='9999-01-01'
    AND e.emp_no NOT IN (SELECT emp_no
FROM dept_manager
	WHERE to_date='9999-01-01')
```



## 25[ 获取员工其当前的薪水比其manager当前薪水还高的相](https://www.nowcoder.com/practice/f858d74a030e48da8e0f69e21be63bef?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select de.emp_no, dm.emp_no as manager_no, s1.salary as emp_salary, s2.salary as manager_salary
from dept_emp as de, dept_manager as dm, salaries s1, salaries s2 
where 
    de.dept_no = dm.dept_no     -- 找到部门的boss
and de.emp_no = s1.emp_no
and dm.emp_no = s2.emp_no
and s1.salary > s2.salary
and s2.to_date='9999-01-01'
and s1.to_date='9999-01-01';


-- 依次构造两张表, 再链表查询
SELECT a.emp_no, b.manager_no, a.emp_salary, b.manager_salary
FROM (
    SELECT de.dept_no, de.emp_no, s.salary AS emp_salary
    FROM dept_emp AS de, salaries AS s
    WHERE de.emp_no=s.emp_no
    AND de.to_date='9999-01-01'
    AND s.to_date='9999-01-01') AS a,
    (
    SELECT dm.dept_no, dm.emp_no AS manager_no, s.salary AS manager_salary
    FROM dept_manager AS dm, salaries AS s
    WHERE dm.emp_no=s.emp_no
    AND dm.to_date='9999-01-01'
    AND s.to_date='9999-01-01') AS b
WHERE 
    a.dept_no=b.dept_no
    AND a.emp_salary>b.manager_salary;
```



## 26[ 汇总各个部门当前员工的title类型的分配数目](https://www.nowcoder.com/practice/4bcb6a7d3e39423291d2f7bdbbff87f8?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
use niuke;
select dp.dept_no, dp.dept_name, t.title, count(t.title) 
from departments  as dp, dept_emp as de, titles  as t
where 
    dp.dept_no = de.dept_no
and de.emp_no = t.emp_no
and de.to_date = '9999-01-01'
and t.to_date = '9999-01-01'
group by dp.dept_no,t.title		-- 题目的难点在于理解group by的条件

-- 题解
SELECT
    de.dept_no AS dept_no,
    d.dept_name AS dept_name,
    t.title AS title,
    COUNT(*) AS `count`			-- 分好组后计算每个组内有多少行
FROM
    (SELECT * FROM dept_emp AS de1 WHERE de1.to_date='9999-01-01') AS de,
    (SELECT * FROM titles AS t1 WHERE t1.to_date='9999-01-01') AS t,
    departments AS d
WHERE d.dept_no = de.dept_no
AND de.emp_no = t.emp_no
GROUP BY d.dept_no, t.title
```

对于group by多个关键字的使用， 见[B站视频](https://www.bilibili.com/video/BV1r54y1e77F?from=search&seid=16839861860769481900)

- group by和distinct可以实现相同效果， 在redshift中group by快于distinct

![groupby](SQL练习/groupby.jpg)

## [27 给出每个员工每年薪水涨幅超过5000的员工编号emp_no](https://www.nowcoder.com/practice/eb9b13e5257744db8265aa73de04fd44?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 两个salary子查询相减: 表内根据某一列进行差值比较, 就需要分别获得两行数据, 因此可以获得两次表, on找到对应数据
SELECT s1.emp_no, s2.from_date, (s2.salary - s1.salary) AS salary_growth
    FROM salaries AS s1
    JOIN salaries AS s2
ON s1.emp_no = s2.emp_no AND s1.to_date = s2.from_date
    WHERE s2.salary - s1.salary > 5000
    ORDER BY salary_growth DESC;
```

注意: 这边只能是(inner) JOIN, 如果LEFT JOIN, RIGHT JOIN会报错;  原数据7条, `join`以后变成49条(把s2的每一条都对应给了s1的每一条, 7*7)==>和`SELECT * FROM salaries AS s1, salaries AS s2;`效果一样，但left join、right join效果不一样

```sql
-- 其他题
SELECT s1.emp_no, s2.from_date, (s2.salary - s1.salary) AS salary_growth
FROM salaries AS s1, salaries AS s2
WHERE s1.emp_no=s2.emp_no
AND (STRFTIME('%Y', s2.from_date) - STRFTIME('%Y', s1.from_date) = 1
OR STRFTIME('%Y', s2.to_date) - STRFTIME('%Y', s1.to_date) = 1)
AND salary_growth>5000
ORDER BY salary_growth DESC;
```



## [28 查找描述信息中包括robot的电影对应的分类名称以及电影数目](https://www.nowcoder.com/practice/3a303a39cc40489b99a7e1867e6507c5?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 这题题意其实是有一点绕的： 查找描述信息中**包含robot的电影**对应的<u>分类名称</u>以及<u>电影数目</u>，注意需要该分类包含电影总数量>=5部

包含robot的数据, 通过like选出: `select * from film where film.description like '%robot%';`

记录:

- category: 16条
- film: 10条
- film_category: 10条

```sql
select * from film f,category c,film_category fc			-- 1600条数据
where f.description like '%robot%' 							-- 160条数据
and f.film_id = fc.film_id									-- 16条数据

-- 题解:
select c.name AS `分类名称category.name`, COUNT(fc.film_id) AS `电影数目count(film.film_id)`
from film f,category c,film_category fc
where f.description like '%robot%'
and f.film_id=fc.film_id
and fc.category_id=c.category_id
and c.category_id in (select category_id
                      from film_category
                      group by category_id
                       having count(film_id)>=5) -- 需要该分类包含电影总数量(count(film_category.category_id))>=5部
```

▲注: 这题无论怎么写在本地的MYSQL上都跑不出来, 但是OJ上能过. 具体报错为: `In aggregated query without GROUP BY, expression #1 of SELECT list contains nonaggregated column 'niuke.c.name'; this is incompatible with sql_mode=only_full_group_by`, 这个是由于sql_mode设置不当引起的，修改下sql_mode即可. 做法为: https://cloud.tencent.com/developer/article/1404739





## [29 使用join查询方式找出没有分类的电影id以及名称](https://www.nowcoder.com/practice/a158fa6e79274ac497832697b4b83658?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
 select f.film_id as '电影id', f.title as '名称'
 from film f
     left join film_category as fc
     on f.film_id = fc.film_id
 where fc.category_id is null	-- 注意什么时候用on 什么时候用where, 这边是固定用法
```





## [30 使用子查询的方式找出属于Action分类的所有电影对应的title,description](https://www.nowcoder.com/practice/2f2e556d335d469f96b91b212c4c203e?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 要求子查询， 就需要先根据类别为Action将子表给筛选出来

```sql
-- 联子表， 用inner join一样的
select f.title, f.description
    from film as f, (SELECT fc.film_id
    from film_category fc, category c
	where fc.category_id = c.category_id
	and c.name = 'Action') as ff
where f.film_id = ff.film_id;

-- 题解
select title,description
from film f
where f.film_id in (select fc.film_id
                    from category c join film_category fc on 					                       c.category_id=fc.category_id
                    where name='Action')
```



### inner join 和 where比较(实际上是cross join**笛卡尔积**)

```sql
A: select a.x, b.x 
    from table1 a,table2 b 
    where a.id=b.id
B: select * from table1 a 
	cross join table2 b 
	where a.id=b.id (注：cross join后加条件只能用where,不能用on)
C: select * from table1 a 
    inner join table2 b 
    on a.id=b.id
```

一般不建议使用方法A和B，因为如果有WHERE子句的话，往往会先生成两个表<u>行数乘积的行的数据表</u>然后才根据WHERE条件从中选择。因此，如果两个需要求交际的表太大，将会非常非常慢，不建议使用。

### 连接查询与子查询

初步实践证明：连接查询的性能优于子查询，所以能用连接查询的地方尽量少用子查询

**连接查询**

连接查询是将两个或多个的表按某个条件连接起来，从中选取需要的数据，连接查询是同时查询两个或两个以上的表的使用的。当不同的表中存在相同意义的字段时，可以通过该字段来连接这几个表。



## [32 将employees表的所有员工的last_name和first_name拼接起来作为Name，中间以一个空格区分](https://www.nowcoder.com/practice/6744b90bbdde40209f8ecaac0b0516fe?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- sqlite中无concat函数
select (last_name||' '||first_name) as Name from employees	


-- mysql
CREATE TABLE if NOT EXISTS `employees`(
`last_name` VARCHAR(60),
`first_name` VARCHAR(60)
);

INSERT INTO `employees` VALUES('mr', 'li');
select concat(last_name, ' ', first_name) as 'Name' from employees	-- sqlite

```



---

# 练习CRUD

## [33 创建一个actor表，包含如下列信息](https://www.nowcoder.com/practice/ac233de508ef4849b0eeb4f38dcf09cf?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 发现图中有的含义列不需要用comment表示出
create table if not exists `actor`(
    `actor_id` smallint(5) not null ,
    `first_name` varchar(45) not null ,
    `last_name` varchar(45) not null ,
    `last_update` date not null,
    primary key(actor_id)
);

-- 题解
CREATE TABLE IF NOT EXISTS actor (
actor_id smallint(5) NOT NULL PRIMARY KEY, -- 设置主键
first_name varchar(45) NOT NULL,
last_name varchar(45) NOT NULL,
last_update timestamp NOT NULL DEFAULT (datetime('now','localtime'))) -- 获取默认系统时间

```



## [34 批量插入数据](https://www.nowcoder.com/practice/51c12cea6a97468da149c04b7ecf362e?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```SQL
insert into `actor`values
    (1, 'PENELOPE', 'GUINESS', '2006-02-15 12:34:33'),
    (2, 'NICK', 'WAHLBERG', '2006-02-15 12:34:33');
```



## [35 批量插入数据,如果数据已经存在，请忽略，不使用replace操作](https://www.nowcoder.com/practice/153c8a8e7805400ba8e384e03acc6b3e?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- sqlite
insert or ignore into actor
values(3,'ED','CHASE','2006-02-15 12:34:33');

-- mysql
insert into actor
values(3,'ED','CHASE','2006-02-15 12:34:33');
```

## [36 创建一个actor_name表，将actor表中的所有first_name以及last_name导入改表](https://www.nowcoder.com/practice/881385f388cf4fe98b2ed9f8897846df?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
CREATE TABLE IF NOT EXISTS `actor_name`(
`first_name` varchar(45) not null,
`last_name` VARCHAR(45) not null
);

INSERT INTO `actor_name` SELECT first_name, last_name from actor;
```



## [37 对first_name创建唯一索引uniq_idx_firstname，对last_name创建普通索引idx_lastname](https://www.nowcoder.com/practice/e1824daa0c49404aa602cf0cb34bdd75?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- sqlite在已有表上创建索引方式
	-- 1.创建单列普通索引
CREATE INDEX index_name ON table_name (column_name);
	-- 2.创建唯一索引
CREATE UNIQUE INDEX index_name on
table_name (column_name);
-- 注意, 这边索引是不能用``或者''框起来的
CREATE UNIQUE INDEX uniq_idx_firstname on actor (first_name);
CREATE INDEX idx_lastname ON actor (last_name);
```



## [38 针对actor表创建视图actor_name_view](https://www.nowcoder.com/practice/b9db784b5e3d488cbd30bd78fdb2a862?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
CREATE VIEW actor_name_view AS
SELECT first_name first_name_v ,last_name last_name_v
FROM  actor;
```



## [39 针对上面的salaries表emp_no字段创建索引idx_emp_no，查询emp_no为10005,](https://www.nowcoder.com/practice/f9fa9dc1a1fc4130b08e26c22c7a1e5f?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 针对salaries表emp_no字段创建了索引idx_emp_no。请强制使用索引查询emp_no为10005
>
> ```sql
> -- sqlite使用索引查询的语法为
> SELECT|DELETE|UPDATE column1, column2...
> INDEXED BY (index_name)
> table_name
> WHERE (CONDITION);
> ```
>
> 它可以与 DELETE、UPDATE 或 SELECT 语句一起使用。
> "INDEXED BY index-name" 子句规定必须用命名的索引来查找前面表中值，如果索引名 index-name 不存在或不能用于查询，SQLite 语句的查询失败。

```sql
SELECT * FROM salaries INDEXED BY idx_emp_no WHERE emp_no = 10005
```



## [40 在last_update后面新增加一列名字为create_date](https://www.nowcoder.com/practice/119f04716d284cb7a19fba65dd876b03?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 向表中添加列 
alter table table_name add col_name char
-- 答案

ALTER TABLE actor ADD create_date datetime NOT NULL DEFAULT('0000-00-00 00:00:00');
```



## [41 构造一个触发器audit_log，在向employees表中插入一条数据的时候，触发插入相关的数据到audit中](https://www.nowcoder.com/practice/7e920bb2e1e74c4e83750f5c16033e2e?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 在MySQL中，创建触发器语法如下：
CREATE TRIGGER trigger_name
trigger_time trigger_event ON tbl_name
FOR EACH ROW
trigger_stmt
```

- trigger_name：标识触发器名称，用户自行指定；
- trigger_time：标识触发时机，取值为 BEFORE 或 AFTER；
- trigger_event：标识触发事件，取值为 INSERT、UPDATE 或 DELETE；
- tbl_name：标识建立触发器的表名，即在哪张表上建立触发器；
- trigger_stmt：触发器程序体，可以是一句SQL语句，或者用 BEGIN 和 END 包含的多条语句，每条语句结束要分号结尾。

```sql
create trigger audit_log 
after insert on employees_test
for each row
begin 
    insert into audit values(new.id,new.name);
    -- new为插入到employees_test的数据
end
```

**【NEW 与 OLD 详解】**
MySQL 中定义了 NEW 和 OLD，用来表示触发器的所在表中，触发了触发器的那一行数据。
具体地：

1. 在 INSERT 型触发器中，NEW 用来表示将要（BEFORE）或已经（AFTER）插入的新数据；
2. 在 UPDATE 型触发器中，OLD 用来表示将要或已经被修改的原数据，NEW 用来表示将要或已经修改为的新数据；
3. 在 DELETE 型触发器中，OLD 用来表示将要或已经被删除的原数据；
   使用方法： NEW.columnName （columnName 为相应数据表某一列名）

## [42 删除emp_no重复的记录，只保留最小的id对应的记录。](https://www.nowcoder.com/practice/3d92551a6f6d4f1ebde272d20872cf05?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
delete from titles_test where id not in (
    select min(id)
    from titles_test
    group by emp_no
);
```



## [43 将所有to_date为9999-01-01的全部更新为NULL,且 from_date更新为2001-01-01](https://www.nowcoder.com/practice/859f28f43496404886a77600ea68ef59?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
update titles_test set to_date = null, from_date = '2001-01-01' where to_date ='9999-01-01';
```



## [44 将id=5以及emp_no=10001的行数据替换成id=5以及emp_no=10005,其他数据保持不变，使用replace实现。](https://www.nowcoder.com/practice/2bec4d94f525458ca3d0ebf3bc8cd240?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 考察replace函数: 其中包含三个参数，第一个参数为该字段的名称，第二参数为该字段的需要被修改值，第三个参数为该字段修改后的值

update titles_test set emp_no = replace(emp_no, 10001, 10005) where id = 5;
```



## [45 将titles_test表名修改为titles_2017](https://www.nowcoder.com/practice/5277d7f92aa746ab8aa42886e5d570d4?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
-- 因为在 MySQL里面修改表名和表里的字段都是用的 ALTER TABLE + table_name + 后面的修改部分
alter table titles_test rename to titles_2017;
```

结合[40 在last_update后面新增加一列名字为create_date](#40 在last_update后面新增加一列名字为create_date)一起看

- **ALTER TABLE 表名 ADD 列名/索引/主键/外键等；**
- **ALTER TABLE 表名 DROP 列名/索引/主键/外键等；**
- **ALTER TABLE 表名 ALTER 仅用来改变某列的默认值；**
- **ALTER TABLE 表名 RENAME 列名/索引名 TO 新的列名/新索引名；**
- **ALTER TABLE 表名 RENAME TO/AS 新表名;**
- 
- **ALTER TABLE 表名 MODIFY 列的定义但不改变列名；**
- **ALTER TABLE 表名 CHANGE 列名和定义都可以改变。**



## [46 在audit表上创建外键约束，其emp_no对应employees_test表的主键id](https://www.nowcoder.com/practice/aeaa116185f24f209ca4fa40e226de48?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
DROP TABLE audit; 
CREATE TABLE audit( 
    emp_no INT NOT NULL, 
    create_date datetime NOT NULL, 
    FOREIGN KEY(emp_no) REFERENCES employees_test(id) 
)
```



## [48 将所有获取奖金的员工当前的薪水增加10%](https://www.nowcoder.com/practice/d3b058dcc94147e09352eb76f93b3274?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
update salaries set salary = 1.1*salary 
where salaries.to_date = '9999-01-01'
and emp_no in (
    select emp_no 
    from emp_bonus
)
```



## [50 将employees表中的所有员工的last_name和first_name通过(')连接起来。](https://www.nowcoder.com/practice/810bf4ee3ac64949b08983aa66ec7bee?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select (last_name || "'" || first_name ) as name from employees;
```





## [51 查找字符串'10,A,B'](https://www.nowcoder.com/practice/e3870bd5d6744109a902db43c105bd50?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

把串 "10,A,B" 中的 逗号用空串替代， 变成了 "10AB", 然后原来串的长度 - 替换之后的串的长度 就是 被替换的 逗号的个数

```sql
select ( length('10,A,B') - length(replace('10,A,B', ',', '')) ) as cnt;
```





## [52 获取Employees中的first_name，查询按照first_name最后两个字母，按照升序进行排列](https://www.nowcoder.com/practice/74d90728827e44e2864cce8b26882105?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 用mysql的话有函数` right `函数。就是取右边第几位的意思，同样还有一个 left 函数。`select * from salaries order by right(emp_no,2) `但是本题数据库是SQlite 只能用substr(emp_no,-2)

```sql
select e.first_name
from employees as e
order by substr(e.first_name, -2) asc;
-- substr(e.first_name, -2, 2) 从最后第二位开始取2位
```



## [53 按照dept_no进行汇总，属于同一个部门的emp_no按照逗号进行连接，结果给出dept_no以及连接出的结果employees](https://www.nowcoder.com/practice/6e86365af15e49d8abe2c3d4b5126e87?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> 聚合函数`group_concat（X，Y）`，其中X是要连接的字段，Y是连接时用的符号，默认为逗号，可省略。此函数必须与GROUP BY配合使用。
>
> 此题以dept_no作为分组，将每个分组中不同的emp_no用逗号连接起来（即可省略Y）。

```sql
select dept_no, group_concat(emp_no) as employees
from dept_emp
group by dept_no
```



## [54 查找排除当前最大、最小salary之后的员工的平均工资avg_salary](https://www.nowcoder.com/practice/95078e5e1fba4438b85d9f11240bc591?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select avg(s.salary) as avg_salary
from salaries as s
where to_date= '9999-01-01'
and s.salary not in ( select min(salary) from salaries where to_date= '9999-01-01' )
and s.salary not in ( select max(salary) from salaries where to_date= '9999-01-01' );
```



## [55 分页查询employees表，每5行一页，返回第2页的数据](https://www.nowcoder.com/practice/f24966e0cb8a49c192b5e65339bc8c03?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

> `limit offset, size`
> size是每页几条数据pageCnt，分页时offset输出页数（pageNum-1）*pageCnt

```sql
select *
from employees
limit (2-1)*5, 5;
```

## [56 获取所有员工的emp_no](https://www.nowcoder.com/practice/e2dab5477fdd4ec0ba84031f8e817b8a?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)



## [57 使用含有关键字exists查找未分配具体部门的员工的所有信息。](https://www.nowcoder.com/practice/c39cbfbd111a4d92b221acec1c7c1484?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)



## [59 获取有奖金的员工相关信息。](https://www.nowcoder.com/practice/5cdbf1dcbe8d4c689020b6b2743820bf?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)



## [60 统计salary的累计和running_total](https://www.nowcoder.com/practice/58824cd644ea47d7b2b670c506a159a6?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)





## [66 牛客每个人最近的登录日期(一)](https://www.nowcoder.com/practice/ca274ebe6eac40ab9c33ced3f2223bb2?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select max(date)
from login
group by user_id
```

## [67 牛客每个人最近的登录日期(二)](https://www.nowcoder.com/practice/7cc3c814329546e89e71bb45c805c9ad?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

```sql
select user.name,client.name,max(login.date)
from login
left join user on login.user_id = user.id
left join client on login.client_id = client.id
group by user_id
order by user.name;
```

用`group by`需要注意的:

- 在select**指定的字段**
  - 要么就要**包含在Group By语句**的后面，作为分组的依据；
  - 要么就要**被包含在聚合函数**中。





