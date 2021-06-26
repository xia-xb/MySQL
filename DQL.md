[toc]

# 基础查询

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

