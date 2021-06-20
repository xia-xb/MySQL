# MySQL

# 终端操作mysql

## mysql登入客户端操作

==登入之前需要保证本地mysql服务是开启状态==

- 连接mysql服务端指令

  ```sql
  mysql -uroot -p
  ```

- 显示当前时间

  ```sql
  select now()
  ```

- 退出连接

  ```sql
  exit/quit/contrl+d
  ```

  

## mysql数据库操作

- 查看所有数据库

  ```sql
  show databases;
  ```

- 创建数据库

  ```sql
  create database 数据库名 charset=utf8;
  ```

- 使用数据库

  ```sql
  use 数据库名
  ```

- 查看当前使用的数据库

  ```sql
  select database();
  ```

- 删除数据库

  ```sql
  drop database 数据库名
  ```

## mysql表操作

- 查看所有当前库中所有表

  ```sql
  show tables;
  ```

- 创建表

  ```sql
  create table 表名(字段名称 数据类型 可选的约束条件,column1 datatype contrai,...);
  ```

- 修改表字段类型

  ```sql
  alter table 表名 modify 列名 类型 约束;
  ```

- 删除表

  ```sql
  drop table 表名;
  ```

- 查看表结构

  ```sql
  desc 表名;
  ```

## mysql-CRUD操作

- 查询指令

  ```sql
  --查询所有列
  select * from 表名;
  
  --查询指定列
  select 列1，列2，... from 表名;
  ```

- 增加数据

  ```sql
  --全列插入:值的顺序必须和字段顺序完全一致
  insert into 表名 values(...);
  --部分列插入：值的顺序和给出的列的顺序对应
  insert into 表名(列1....) values(值1);
  --全列多行插入
  insert into 表名 valuse(...),(...),(...);
  --部分列多行插入
  insert into 表名(列1....) values(值1...),(值1...);
  ```

- 修改数据

  ```sql
  update 表名 set 列1=值1,列2=值2... where 条件;
  例：
  update student set age=18, gender='女' where id=6;
  ```

- 删除数据

  ```sql
  delete from 表名 where 条件;
  例：
  delete from students where id=5;
  ```

## 备份和恢复

- 备份导出

  ```sql
  #备份导出
  mysqldump -u用户名 -p密码 数据库名字 表名字 > data.sql
  #恢复导入
  cd 到数据文件路径下
  mysql -u用户名 -p密码
  use 数据库
  source data.sql
  ```

  