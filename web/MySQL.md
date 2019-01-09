# MySQL基础

## 1. MySQL以及SQL概念

* **SQL通用语法**

  ```sql
  1. 一条sql语句是以;结尾，SQL语句不区分大小写
  2. SQL中的注释是--空格
  ```

* **SQL分类**

  ```sql
  DDL:对数据库和表进行增删改查：create alter drop show/desc
  DML:对数据库表中的数据进行增删改：insert update delete
  DQL:对数据库表中的数据进行查询：select
  DCL:用户管理以及授权
  ```

## 2. DDL——对数据库进行CRUD操作

* **创建数据以及删除数据**

  ```sql
  1.创建数据库
  	create database 数据库名称；
  	create database [if exists] 数据库名称 [character set utf8];
  2.删除数据
  	drop database [if exists] 数据库名称；
  3.其他操作：
  	使用/选择数据：use 数据库名称；
  	查看所有数据：show database;
  	修改数据字符集：alter database 数据名称 character set 新字符集
  ```

  

## 3. DDL——对数据库中的表进行CRUD操作

* **创建表和删除表**

  ```sql
  1. 创建表
  create table [if not exists] 表名称(
  	列名1 数据类型,
  	...
  	列名n 数据类型
  );
  	数据类型：int、double(总位数，小数位数)、char(长度)/varchar(长度)、date/datetime/timestamp、blob(二进制数据)、text(大文本)
  	varchar会根据所给的长度(小于限制长度)而给出其空间，char则不会
  	datetime和timestamp的区别：如果存进去的数据为null或不存数据，datetime列的默认值是null，而timestamp列的默认值会取系统当前时间
  	
  2. 删除表
  	drop table [if exists] 表名称;
  3.修改表
  	alter table 表名 rename to/add/drop/change/modify
  ```

## 4. DDL——对数据库表中的数据进行增、删、改操作

* insert添加数据

  ```sql
  -- 向指定列添加数据
  insert into 表名 (列1,...列n) values (值1,...值n);
  -- 向所有列添加数据
  insert into 表名 values (值1,...值n);
  -- 添加多条数据
  insert into 表名 values (值1,...值n),(值1,...值n)...(值1,...值n);
  ```

  **注意：添加的数据要和列一一对应，还不能超出长度限制**

* delete删除数据：一般都会带条件删除

  ```sql
  delete from 表名 where 条件；
  -- 删除所有数据
  delete from 表名; -- 将数据一条一条的删除
  truncate table 表名; -- DDL,先将表删除，再创建一个结构一样的新表
  ```

* update修改数据：一般都会带条件修改

  ```sql
  update 表名 set 列1=值1,列2=值2,...[where 条件];
  ```

  **注意：如果不带where条件，那么就会将一列的所有数据都修改了**

# MySQL约束

## 1. 单表查询

* 基础查询

  ```sql
  -- 查询所有列
  语法：select * from 表名;
  -- 查询指定列，并去重复值
  语法：select 列1,列2... from 表名;
  	select 列1 from 表名；
  	select distinct 列1 from 表名;
  -- 取别名
  select 列1 as 别名 from 表名; -- as关键字可以省略
  ```

  **注意：ifnull(列名，0)：如果该列值为null,那么就使用0代替**

* 条件查询-基本条件

  ```sql
  1. 基础运算符： > >= < <= !=或者<>;
  2. 连接符：and or
  3. between 值1 and 值2：查询值在值1和值2值之间的数据
  4. in(值1,值2...)：查询值等于值1或者等于值2,...
  5. 判断是否是null: is null 或者 is not null;
  ```

* 条件查询-模糊查询

  ```sql
  语法： where 列名 like '带占位符的字符串';
  占位符：
  	_:一个字符的占位符
  	%:0个或者多个字符的占位符
  ```

* 排序查询

  ```sql
  语法：order by 列1 asc/desc,列2 asc/desc,...;
  说明：当列1值相同，就会按照列2的规则排序
  ```

  **注意：排序不是查询条件，不影响查询结果的数量**

* 聚合函数/统计函数

  `count(列名或者*);max(列名);min(列名);sum(列名);avg(列名)`

* 分组查询

  ```sql
  语法： group by 列名 [having 条件]
  where和having的区别：
  	1. 分组前过滤使用where,分组后过滤使用having;
  	2. where条件不能使用聚合函数,having条件可以使用聚合函数;
  	3. where条件是对原始表进行过滤，having是对结果进行过滤;
  ```

* 分页查询-limit

  ```sql
  语法：limit 起始索引,每页显示条数;
  说明：起始索引从0开始
  -- 起始索引=（页数-1）*每页显示条数
  ```

* **总结：**

```sql
书写顺序：
	select * from 表名 [where 条件] [group by 列名] [having 条件] [order by 列名 asc/desc] [limit 起始索引,每页显示条数];
mysql关键字的解析顺序：
	select * from ... where ... group by ... 结果集(修改*)...having...order by ...limit;
```



## 2. 约束

* 单表约束

  ```sql
  1. 主键约束：primary key
  2. 非空约束：not null
  3. 唯一约束：unique
  
  创建表的完整语法：
  	create table 表名(
      	列1 数据类型 约束1 约束2...
          ....
          列n 数据类型 约束1 约束2...
      );
  ```

  ```sql
  例如：实际开发中创建表：
  	create table stu(
      	id int primary key AUTO_INCREMENT, -- AUTO_INCREMENT(自动增长)
      	name varchar(20) not null unique,
          age int,
          sex varchar(5),
          address varchar(100),
          math int,
          english int
      );
  创建表完成后添加约束：
  -- 添加非空约束
  Alter table stu modify name varchar(20) not null;
  -- 删除name的非空约束
  Alter table stu modify name varchar(20);
  
  -- 添加唯一约束
  Alter table stu modify name varchar(20) unique;
  -- 删除唯一约束
  Alter table stu DROP index name;
  
  ```

  **注意：一个表只能有一个主键，主键是用来作为一条数据的唯一标识；主键字段默认唯一且非空，可以一个字段为主键也可多个字段关联为一个主键；如果主键是int类型，可以使用自动增长**

* 多表约束

  ```sql
  1. 外键约束：当一张表的某个字段的值都来自于另一张表的主键，那么就需要给前一个表的这个字段添加外键约束；外键表/从表，主键表/主表
  
  2. 创建表时添加外键
  create table 表名(
  	....,
      constraint 外键别名 foreign key (外键列) references 主键名称(主键)；
  ）;
  
  3. 删除外键
      Alter table 表名 drop foreign key 外键名称；
      
  4. 创建表之后，添加外键
      Alter table 表名 add constraint 外键名称 foreign key (外键字段名称) references 主表名称
  ```

## 3. 多表关系

```sql
一对一：在任意一方添加外键执行另一方的主键，并且要给外键字段添加unique约束
一对多：在多的一方添加外键指向一的一方主键
多对多：创建一张中间表，在中间表至少有两个字段作为外键分别指向所对双方的主键
```

# MySQL多表&事务

## 1. 多表查询

* 连接查询

  * 交叉连接：将两张表无条件连接起来，得到的结果是笛卡尔积；

  * **内连接：结果是两个表的交集部分；关联条件一般是主外键关联**

    ```sql
    语法：
    	显示内连接：select * from 表1 inner join 表2 on 关联条件； -- inner可以省略
    	显示内连接：select * from 表1, 表2,... where 关联条件及其他
    ```

  * **外连接**

    ```sql
    1. 左外连接：左表的全部数据以及和右表的交集数据
    	语法：select * from 表1 left outer join 表2 on 关联条件; -- Outer可以省略
    2. 右外连接：右表的全部数据以及和左表的交集数据
    	语法：select * from 表1 right outer join 表2 on 关联条件; -- Outer可以省略
    	
    -- 统计每个部门的总人数
    select d.id,count(e.dept_id) 总人数 from dept d left outer join emp e on d.id=dept_id group by d.id;
    ```

* **子查询：一个查询中嵌套另一个查询：**

  ```sql
  1. where子句子查询：将一个查询的结果作为另一个查询的条件
  	单行单列：作为条件运算符(> >= < <= !=/<>)后面的值;
  	多行单列：作为in的括号中的值;
  2. from子句子查询：将子查询结果作为一张表和其他表连接查询
  	多行/单行多列：作为虚拟表使用
  3. select子句子查询：将子查询结果作为另一个查询的结果
  
  案例：
  -- 查询最高工资和最低工资的员工信息
  select max(salary) from emp;
  select min(salary) from emp;
  select * from emp where salary=(select max(salary) from emp) or salary=(select min(salary) from emp);
  
  select * from emp where salary in((select max(salary) from emp),(select min(salary) from emp));
  
  select max(salary),min(salary) from emp;
  select 
  e1.*
  from
  emp e1,
  (select max(salary) big,min(salary) small from emp) e2
  where
  e1.salary in (e2.big,e2.small);
  
  -- 查询每个员工编号、姓名及其所在部门名称
  select emp.id,emp.name,dept.name from emp,dept where emp.dept_id=dept.id;
  select id,name,dept_id,(select name from dept where id=dept_id) 部门名称 from emp;
  
  -- 查询最高工资和最低工资的员工信息及部门信息
  select 
  e1.*,(select name from dept where id=dept_id) 部门名称
  from
  emp e1,
  (select max(salary) big,min(salary) small from emp) e2
  where
  e1.salary in (e2.big,e2.small);
  
  ```



## 2. 事务以及事务隔离级别

* 事务：一组sql操作**要么同时成功要么同时失败**，那么就必须使用事务管理

  ```sql
  事务给管理思路：
  	1. 开启事务：start transaction;
  	2. 如果一组sql执行成功，那么就提交事务：commit
  	3. 如果一组sql执行失败，那么就回滚事务：rollback
  ```

* 事务的4大特征

  ```sql
  原子性：一组sql操作要么同时成功要么同时失败，不可分割
  持久性：事务一旦提交或回滚，那么数据就永久生效，持久化到硬盘中
  隔离性：事务之间本不应该相互影响，应该相互独立
  一致性：事务执行前后数据总量不会发生变化
  ```

* 事务的隔离级别

  ```sql
  1. read uncommitted: 读未提交
  	产生的问题：脏读、不可重复读、幻读
  2. read committed:读已提交(Oracle默认)
  	产生的问题：不可重复读、幻读
  3. repeatable read：可重复读(MySQL默认)
  	产生的问题：幻读
  4. serializable:串行化
  	可以解决所有的问题
  数据库查询隔离级别：
  	select @@tx_isolation;
  数据库设置隔离级别：
  	set global transaction isolation level 级别字符串;
  ```

  **注意：隔离级别从小到大安全性越来越高，但是效率越来越低**

## 3. DCL——用户管理及授权

* 修改root用户密码

  ```sql
  1. 需要管理员运行cmd --> net stop mysql 停止mysql服务
  2. 使用无验证方式启动mysql服务： mysql --skip-grant-tables
  3. 打开新的cmd窗口，直接输入mysql命令，敲回车，就可以登录成功
  4. use mysql;
  5. update user set password = password('你的新密码') where user='root';
  6. 关闭两个窗口
  7. 打开任务管理器，手动结束mysqld.exe的进程
  8. 启动mysql服务
  9. 使用新密码登录
  ```

* 用户管理及授权

  ```sql
  1.查询用户
  	use mysql;
  	select * from user;
  2.创建用户
  	语法：create user '用户名'@'主机名' identified by '密码';
  3.删除用户
  	语法：drop user '用户名'@'主机名';
  4.查看权限
  	语法：show grants for '用户名'@'主机名';
  5.授予权限
  	语法：grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
  6.撤销权限
  	语法：revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
  	
  ```

  

  