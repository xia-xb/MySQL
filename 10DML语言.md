# DML语言

数据操作语言：

- 插入insert
- 修改update
- 删除delete

# 插入语句

## 语法

```sql
insert into 表名(列名,...) values（值1,...）;
```

## 方式一

经典的插入

注意事项

- 插入的值的类型要与列的类型一致或兼容

```sql
#插入的值的类型要与列的类型一致或兼容
insert into beauty(id,NAME,sex,bornadate,phone,photo,boyfirend_id)
values(13,'唐艺昕','女','1990-4-23','18988888888',NULL,2);

#不可以为null的列必须插入值，可以为null的列如何插入值
#方式一
insert into beauty(id,NAME,sex,bornadate,phone,photo,boyfirend_id)
values(13,'唐艺昕','女','1990-4-23','18988888888',NULL,2);
#方式二
insert into beauty(id,NAME,sex,bornadate,phone,boyfirend_id)
values(13,'唐艺昕','女','1990-4-23','18988888888',2);

#列的顺序可以调换，但是值需要对应
insert inro beauty(NAME,sex,id,phone)
values('蒋欣','女',16,'110');

#列数和值的个数必须一致
insert inro beauty(NAME,sex,id,phone,boyfriend_id)
values('关晓彤','女',17,'110');

#可以省略列名，默认所有列，而且列的顺序和表中列的顺序一致
insert into beauty
value(18,'张飞','男',null,'119',null,null);
```

## 方式二

```sql
insert into 表名
set 列名=值,列名=值,...;
```

```sql
insert into beauty
set id=19,NAME='刘涛',phone='999';
```

## 两种方式对比

- 方式一支持多行，方式二不支持
- 方式一支持子查询，方式二不支持

```sql
#多行
insert into beauty(id,NAME,sex,bornadate,phone,photo,boyfirend_id)
values(23,'唐艺昕1','女','1990-4-23','18988888888',NULL,2),
(24,'唐艺昕2','女','1990-4-23','18988888888',NULL,2),
(25,'唐艺昕3','女','1990-4-23','18988888888',NULL,2);

#子查询
insert into beauty(id,NAME,phone)
select 26,'宋茜','118';
```

# 修改语句

## 修改单表的记录

语法

```sql
update 表名
set 列=新值,列=新值,...
where 筛选条件;
```

```sql
#修改beauty表中姓唐的电话为138998889
update beauty
set phone='138998889'
where name like '唐%'；

#修改boys表中id为2的名称为张飞，魅力值10
update boys
set boyname='张飞',usercp=10
where id=2;
```



## 修改多表的记录

语法

```sql
#sq192语法
update 表1 别名，表2 别名
set 列=值,...
where 连接条件
and 筛选条件

#sq199语法
update 表1 别名
inner|left|right join 表2 别名
on 连接条件
set 列=值,...
where 筛选条件;
```

```sql
#修改张无忌的奴朋友的手机号为114
update boys as bo
inner join beauty b on bo.id=b.boyfriend_id
set b.phone='114'
where bo.name='张无忌';

#修改没有男朋友的女的男朋友编号都为2
update boys as bo
right join beauty b on bo.id=b.boyfriend_id
set b.boyfriend_id=2
where bo.id is null;
```

# 删除语句

## 方式一

delete

语法

```sql
delete from 表名 where 筛选条件;
```

### 单表的删除

```sql
#删除手机号最后一位9的信息
delete from beauty
where phone like '%9';
```



### 多表的删除

```sql
#sq192
delete 表1的别名，表2的别名
from 表1 别名，表2 别名,...
where 筛选条件
and 筛选条件;

#sq199
delete 表1的别名，表2的别名
from 表1 别名
inner|left|right|join 表2 别名
on 连接条件
where 筛选条件;
```



```sql
#删除张无忌的女朋友的信息
delete b
from beauty as b
inner join boys as bo on b.boysfriend_id=bo.id
where bo.boyname='张无忌'；

#删除黄晓明的信息以及他女朋友的信息
delete b,bo
from beauty as b
inner join boys as bo on b.boyfriend_id=bo.id
where bo.boyname='黄晓明'';
```



## 方式二

truncate

==清空数据==

```sql
truncate table 表名;
```



```sql
#将魅力值>100的男信息删除
truncate table boys;
```

## 区别

==面试题==

- delete 可以加where条件，truncate不能加
- truncate删除效率高一些
- 加入要删除的表中有自增长列，如果用delete删除后，再插入数据，子增加列的值从断点开始；而truncate删除后，再插入数据，自增长列的值从1开始
- truncate删除没有返回值，delete删除有返回值
- truncate删除不能回滚，delete删除可以回滚