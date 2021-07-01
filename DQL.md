[toc]

# 基础查询

==查询前需要使用库==

==use myemployees;==

语法：

```sql
select 查询列表 from 表名;
```

特点：

- 查询列表可以是：表中的字段、常量值、表达式、函数
- 查询的结果是一个虚拟的表格

## 查询字段

1. 查询表中的单个字段

   ```sql
   select last_name from employees;
   ```

2. 查询表中的多个字段

   ```sql
   select last_name,salary,email from employees
   ```

3. 查询表中的所有字段

   ```sql
   select * from employees;
   ```

## 查询常量值

```sql
select 100;
select 'john';
```

## 查询表达式

```sql
select 100*98;
```

## 查询函数

```sql
select version();
```

## 起别名

- 便于理解
- 如果要查询的字段有重名的情况，使用别名可以区分开来

==使用as和空格==

==别名有空格，#等特殊字符时，使用""==

```sql
select 100%98 as 结果;
select last_name as 姓,first_name as 名 from employees;
select last_name 姓,first_name 名 from employees;
```

## 去重

==distinct==

```sql
#查询员工表中涉及到的所有的部门的编号
select distinct department_id from employees;
```

## +的作用

==只有一个功能：运算符==

```sql
select 100+90;两个操作数都为数值型，则做加法运算
select '123'+90;一个为字符型，试图将字符型数值转换为数值型
select 'john'+90;转换成功，继续做加法运算，转换失败，将字符型数值转换成0
select null+10;只要其中一个为null，则结果为null
```



```sql
#查询员工名和姓连接成一个字段，并显示为姓名
select last_name+first_name as 姓名 from employees;
#有问题
```

## concat实现连接

```sql
select concat('a','b','c') as 结果;
select concat(last_name,first_name) as 姓名 from employees;
```

## ifnull函数

判断某字段或表达式是否为null，如果为null返回指定的值，否则返回原来的值

```sql
select ifnull(commission_pct,0) from employees;
```

## isnull函数

判断某字段或表达式是否为null，如果是，返回1，否则返回0

# 条件查询

语法

```sql
select 查询列表 from 表名 where 筛选条件;
```

顺序

1. from 表名
2. where 筛选条件
3. select 查询列表

分类：

- 按条件表达式筛选（条件运算符：>  <  =  !=  <>  <=  >=）
- 按逻辑表达式筛选（逻辑运算符：&&  ||  !  and  or  not）
- 模糊查询（like      between and      in      is null）

## 按条件表达式筛选

```sql
#查询工资>12000的员工信息
select * from employees where salary>12000;
#查询编号不等于90号的员工和部门编号
select last_name,department_id from employees where department_id!=90;
select last_name,department_id from employees where department_id<>90;
```

## 按逻辑表达式筛选

==用于连接条件表达式==

```sql
#查询工资在10000到20000之间的员工名、工资以及奖金
select last_name,salary,commision_pct from emplyees where salary>=10000 and salary<=20000;
#查询部门标号不是在90到110之间，或者工资高于15000的员工信息
select * from employees where not(department_id>=90 and department_id<=110) or salary>=15000;
```

## 模糊查询

like  between and   in   is null | is not null

### like

特点

- ==通常与通配符搭配使用，可以判断字符型或数值型==
  - %任意多个字符，包含0个字符
  - _任意单个字符

==当like中出现_等特殊字符时，可以用转义字符，且转义字符可以自定义，用escape指明转义字符==

```sql
#查询员工名中包含字符a的员工信息
select * from employees where last_name like '%a%';
#%标识通配符
#
select last_name,salary from employees where last_name like '__n_l%';
#查询员工名中第二个字符为_的员工名
select last_name from employees where last_name like '_\_%'; 
select last_name from employees where last_name like '_$_%' escape '$';
```

### between and

注意事项

- 使用between and可以提高语句的简洁度 
- 包含临界值
- 两个临界值不能调换顺序

```sql
#查询员工编号在100到120之间的员工信息
select * from employees where employee_id>=100 and employee_id<=120;
select * from employees where employee_id between 100 and 120;
```

### in

用于判断某字段的值是否属于in列表中的某一项

特点：

- 提高语句简洁度
- in列表的值类型必须一致或兼容
- ==不支持通配符==

```sql
#查询员工的工种编号是IT_PROG,AD_VP,AD_PRES中的一个员工名和工种编号
select last_name,job_id from employees where job_id='IT_PROG' job_id='AD_VP' or job_id='AD_PRES';
select last_name,job_id from employees where job_id in ('IT_PROG','AD_VP' ,'AD_PRES');
```

### is null

===或<>不能用于判断null==

==is null和is not null可以用于判断null==

```sql
#查询没有奖金的员工名和奖金率
select last_name,commission_pct from employees where commission_pct is null;
#查询有奖金的员工名和奖金率
select last_name,commission_pct from employees where commission_pct is not null;
```



### 安全等于

<=>

==可读性较差==

```sql
#判断null
select last_name,commission_pct from employees where commission_pct <=> null;
#查询工资为12000的员工信息
select last_name,salary from employees where salary <=>12000;
```

# 排序查询

语法

```sql
select 查询列表 from 表 where 筛选条件 order by 排序列表[asc|desc]

#案例 查询员工信息，要求工资从高到低排序
select * from employees order by salary desc;
select * from employees order by salary asc;
```

特点：

- asc代表升序，desc代表降序，如果不写，==默认是升序==
- order by子句中可以支持单个字段、多个字段、表达式、函数、别名
- order by子句一般是放在查询语句的最后面，但limit子句除外

```sql
#案例 查询部门编号>=90的员工信息，按入职时间的先后进行排序
select * from employees where department_id>=90 order by hiredate asc;
#案例 按表达式排序
#按年薪的高低显示员工的信息和年薪（按表达式排序）
select * ,salary*12*(1+ifnull(commission_pct,0)) as 年薪 from employees order bysalary*12*(1+ifnull(commission_pct,0)) desc;
#案例 按年薪的高低显示员工的信息和年薪（按别名排序）
select * ,salary*12*(1+ifnull(commission_pct,0)) as 年薪 from employees order 年薪 desc;
#案例 按姓名的长度显示员工的姓名和工资（按函数排序）
select length(last_name) as 字节长度,last_name,salart from employees order by length(last_name) desc;
#案例 查询员工信息，要求先按工资排序，再按员工编号排序（按多个字段排序）
select * from empolyees order by salary asc,employee_id desc;
```

测试题

```sql
#查询员工的姓名和部门号和年薪，按年薪降序，按姓名升序
SELECT 
last_name AS 姓名,
department_id AS 部门号,
salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
FROM employees
ORDER BY
年薪 DESC,
姓名 ASC;
#选择工资不在8000到17000的员工的姓名和工资，按工资降序
SELECT
last_name AS 姓名,
salary AS 工资
FROM
employees
WHERE 
salary NOT BETWEEN 8000 AND 17000
ORDER BY
salary DESC;
#查询邮箱中包含e的员工信息，先按有限的字节数降序，再按部门号升序
SELECT
*
FROM
employees
WHERE
email LIKE '%e%'
ORDER BY
LENGTH(email) DESC,
department_id ASC;
```

