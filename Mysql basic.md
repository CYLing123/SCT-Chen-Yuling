# Mysql基础

## 第一章 课程内容&数据库相关概念

![image-20231123171712675](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231123171712675.png)

![image-20231123171852924](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231123171852924.png)

#### 一、理解数据库、数据管理系统和sql

数据库(DB)：存储数据的仓库，数据是有组织的进行存储

数据库管理系统(DBMS)：操纵和管理数据库的大型软件

SQL：操作关系型数据库的编程语言，定义了一套操作关系型数据库的统一标准

#### 二、启动和停止mysql

1、方式一：win+R：services.msc进入系统中，找到MySQL80，右键-停止启动

2、方式二：命令行直接输入：启动net start mysql80，停止net stop MySQL80![image-20231123192416704](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231123192416704.png)

#### 三、客户端链接

1、方式一：MySQL提供的客户端命令工具行![image-20231123193114463](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231123193114463.png)

2、方式二：系统自带的命令工具执行指令。注意：使用这种方式时，需要配置PATH环境变量

`MySQL [-h 127.0.0.1] [-p 3306] -u root -p`

![image-20231123200948356](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231123200948356.png)

#### 四、MySQL数据库模型

1、关系型数据库：建立再关系模型基础上，由**多张相互连接的二维表**组成的数据库

## 第二章 SQL

#### 一、SQL的通用语法

1、sql语句可以单行或多行书写，以分号结尾。

2、sql语句可以使用空格/缩进来增强语句的可读性

3、MySQL数据库的SQL语句不区分大小写，关键字建议使用大写

4、注释：

​	单行注释：--注释内容 或 #注释内容（MySQL特有）

​	多行注释：**/*注释内容*/**

#### 二、SQL语句分类

1、DDL定义语言

2、DML操作语言（增删改）

3、DQL查询语言

4、DCL控制语言（创建用户、控制数据库的访问权限）

#### 三、DDL

##### 1、DDL-数据库操作

###### 1.1查询-数据库

​	查询所有数据库:

```
SHOW DATABASES;
```

​	查询当前数据库：

```python
SELECT DATABASE();
```

​	创建：

```
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET字符集] [COLLATE 排列规则]；
```

​	删除：

```
DROP DATABASE[IF EXISTS]数据库名;
```

​	使用：

```
USE 数据库名；
```

##### 2、表操作

###### 2.1DDL-表操作-查询

查询当前数据库所有表

```
SHOW TABLES;
```

查询表结构

```
DESC表名;
```

查询指定表的建表语句

```
SHOW CREATE TABLE表名；
```

###### 2.2DDL-表操作-创建

```python
CREATE TABLE表名(
	字段1 字段1类型[COMMENT 字段1注释]，
	字段2 字段2类型[COMMENT 字段2注释]，
	字段3 字段3类型[COMMENT 字段3注释]，
	......
	字段n 字段n类型[COMMENT 字段n注释]
)[COMMENT 表注释];
#注意：[...]为可选参数，最后一个字段后面没有逗号。
```

###### 2.3DDL-表操作-数据类型

![image-20231124104638687](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231124104638687.png)

![image-20231124104616567](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231124104616567.png)

char(10)-----------存储空间是10，剩余会用空格补全--------性能好-------性别 username char(1)

varchar（10）-----存储空间是字符数-------新能稍微较差一些------------用户名 username varchar(50)

![image-20231124105940799](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231124105940799.png)

###### 2.4根据需求创建表（设计合理的数据类型、长度）

![image-20231124110159563](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231124110159563.png)

创建表结构

```python
create table emp(
	id int comment '编号',
	workno varchar(10) comment '工号',
	name varchar(10) comment '姓名',
	gender char(1) comment '性别',
	age tinyint unsigned comment '年龄',
	idcard char(18) comment '身份证号',
	entrydate date comment '入职时间'
) comment '员工表';
```

###### 2.5DDL-表操作-修改

添加字段

```python
ALTER TABLE 表名 ADD 字段名 类型（长度）[COMMENT 注释] [约束]；
```

修改字段名和字段类型

```python
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT注释] [约束];
```

###### 2.6DDL-表操作-修改

删除字段

```python
ALTER TABLE 表名 DROP 字段名；
```

修改表名

```
ALTER TABLE 表名 RENAME TO 新表名;
```

删除表

```
DROP TABLE[IF EXISTS] 表名；
```

删除指定表

```python
TRUNCATE TABLE 表名;
#注意：再删除表时，表中的全部数据也会被删除。
```

##### 3、小结

DDL-数据库操作

```
SHOW DATABASES;
CREATE DATABASE 数据库名；
USE 数据库名；
SELECT DATABASE();
DROP DATABASE 数据库名;
```

DDL-表操作

```
SHOW TABLES；
CREATE TABLE 表名(字段 字段类型，字段 字段类型);
DESC 表名;
SHOW CREATE TABLE 表名;
ALTER TABLE 表名 ADD/MODIFY/CHANGE/DROP/RENAME TO....;
DROP TABLE 表名;
```

### 四、 DML

DDL是用来操作数据库中表结构和表中字段的，DML完成数据的增删改，DML（数据操作语言）

​	添加数据（INSERT）

​	修改数据（UPDATE）

​	删除数据（DELETE）

##### 1、DML-添加数据

1.给指定字段添加数据

```python
INSERT INTO 表名 (字段名1，字段2，...) VALUES(值1，值2...);
```

2.给全部字段添加数据

```python
INSERT INTO 表名 VALUES (值1，值2，...);
```

3.批量添加数据

```python
INSERT INTO 表名(字段名1，字段名2，...) VALUES(值1，值2，...),(值1，值2，...),(值1，值2，...);
INSERT INTO 表名VALUES(值1，值2，...),(值1，值2，...),(值1，值2，...);
```

注意：

​	1、插入数据时，指定的字段舒徐需要与值得顺序时一一对应的。

​	2、字符串和日期类型数据应该包含在引导中。

​	3、插入的数据大小，应该在字段的规定范围内。 

##### 2、DML-修改数据

```python
UPDATE 表名 SET 字段名1=值1,字段名2=值2,....[WHERE 条件];
```

注意：修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据。![image-20231126111955167](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231126111955167.png)

##### 3、DML-删除数据

```python
DELETE FROM 表名 [WHERE 条件]
```

注意：DELETE语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。

DELETE语句不能删除某一个字段的值（可以使用UPDATE）![image-20231126112132309](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231126112132309.png)

### 五、 DQL-查询语句

DQL数据查询语言，用来查询数据库中表的记录。

查询关键字：select

##### 1、DQL-语法

```python
SELECT 
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

基本查询

条件查询(WHERE)

聚合查询(count、max、min、avg、sum)

分组查询(GROUP BY)

排序查询(ORDER BY)

分页查询(LIMIT)

##### 2、DQL-基本查询

1.查询多个字段

```python
SELECT 字段1，字段2，字段3...FROM 表名;
```

```
SELECT * FROM 表名;
```

2.设置别名

```python
SELECT 字段1 [AS 别名1],字段2 [AS 别名2]...FROM 表名；
```

3.去除重复记录

```python
SELECT DISTINCT 字段名列表 FROM 表名;
```

![image-20231126121555052](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231126121555052.png)

##### 3、DQL-条件查询

 语法

```
SELECT 字段列表 FROM 表名 WHERE 条件列表;
```

条件

![image-20231126122313203](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231126122313203.png)

案例

​	<img src="C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231126124159655.png" alt="image-20231126124159655" style="zoom:150%;" />

<img src="C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231126124236358.png" alt="image-20231126124236358" style="zoom: 150%;" />

##### 4、DQL-聚合函数

介绍

​	将一列数据作为一个整体，进行纵向计算。

常见聚合函数

![image-20231126123924722](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231126123924722.png)

```python
SELECT 聚合函数 (字段列表) FROM 表名;
#注意：null值不参与聚合函数运算。
```

![image-20231127143511018](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231127143511018.png)

##### 5、分组查询

1.语法

```python
SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名[HAVING 分组后过滤条件];
```

2.where和having区别

​	执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。

​	判断条件不同：where不能对聚合函数进行判断，而having可以。

​	**注意：**执行顺序：where > 聚合函数 > having.

​				分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。

![image-20231127144654108](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231127144654108.png)

##### 6、排序操作

1、语法

```python
SELECT 	字段列表 FROM ORDER BY 字段1 排序方式1 , 字段2 排序方式2;
```

2、排序方式

​		ASC：升序（默认）

​		DESC：降序

​	**注意：**如果多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序。

##### 7、分页查询

​	1、语法

```python
select 字段名列表 FROM 表名 LIMIT 起始索引，查询记录数；
```

注意：起始索引从0开始，起始索引=（查询页码-1） * 每页显示记录数。

​			分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT。

​			如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10。

![image-20231127164613712](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231127164613712.png)

##### 8、DQL执行语句

<img src="C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231127165451516.png" alt="image-20231127165451516" style="zoom:150%;" />

###   六、DCL-数据库控制语言

​	DCL数据库控制语言，用来管理数据库用户、控制数据库的访问权限。

##### 1、DCL-用户管理

查询用户

```python
USE mysql;
SELECT * FROM user;
```

创建用户

```python
CREAT USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

修改用户密码

```python
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
```

删除用户

```python
DROP USER '用户名'@'主机名';
```

##### 2、权限控制

mysql中定义了很多种权限，但是常用的就以下几种：

<img src="C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231128115019692.png" alt="image-20231128115019692" style="zoom:150%;" />

##### 3、DCL-权限控制

​	查询权限

```python
SHOW GRANTS FOR '用户名'@'主机名';
```

​	授予权限

```python
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名'；
```

​	撤销权限

```python
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

注意：多个权限之间，使用逗号分隔

​			授权时，数据库名和表名可以使用*进行配通，代表所有。

## 第三章 函数

函数 是指一段可以直接被另一段程序调用的程序或代码。

##### 1、字符串函数

![image-20231129165630816](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231129165630816.png)

![image-20231129171445267](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231129171445267.png)

##### 2、数值函数

![image-20231129171944932](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231129171944932.png)

##### 3、日期函数

![image-20231129172942114](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231129172942114.png)

##### 4、流程函数

![image-20231129193507422](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231129193507422.png)

##  第四章 约束

概述：约束时作用于表中字段的规则，用于限制存储在表中的数据。

准备：创建emp和dept

```python
 create table dept(
     id int auto_increment comment 'ID' primary key,
     name varchar(50) not null comment'部门名称'
    )comment '部门表';
 
INSERT INTO dept(id,name) VALUES (1,'研发部'),(2,'市场部'),(3,'财务部'),(4,'销售部'),(5,'总经办');
```

```python
CREATE TABLE emp(  
    id INT AUTO_INCREMENT COMMENT 'ID' PRIMARY KEY,  
    name VARCHAR(50) NOT NULL COMMENT '姓名',  
    age INT COMMENT '年龄',  
    job VARCHAR(20) COMMENT '职位',  
    salary INT COMMENT '薪资',  
    entrydate DATE COMMENT '入职时间',  
    managerid INT COMMENT '直属领导',  
    dept_id INT COMMENT '部门ID'  
) COMMENT '员工表';

insert into emp (id,name,age,job,salary,entrydate,managerid,dept_id) values
     (1,'金庸',66,'总裁',20000,'2000-01-01',null,5),(2,'张无忌',20,'项目经理',125000,'2005-12-05',1,1),
     (3,'杨逍',33,'开发',8400,'2000-11-03',2,1),(4,'韦一笑',48,'开发',11000,'2002-02-05',2,1),
     (5,'常遇春',43,'开发',10500,'2004-09-07',3,1),(6,'小昭',19,'程序员鼓励师',66000,'2004-10-12',2,1);
```

目的：保证数据库中数据的正确、有效性和完整性。

分类：![image-20231204094436162](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231204094436162.png)

![image-20231204101440376](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231204101440376.png)

外键约束：外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性

![image-20231205111209742](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231205111209742.png)

语法

​	添加外键

```python
CREATE TABLE表名(
	字段名 数据类型,
	....
	[CONSTRAINT] [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表(主表列明)
	);
```

```python
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键名称) REFERENCES 主表(主表列名);
```

删除\更新行为

![image-20231206103230960](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206103230960.png)

```python
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE CASCADE ON DELETE CASCADE;
```

## 第五章 多表查询

一对多，多对多，一对一

##### 1、介绍

![image-20231206120231966](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206120231966.png)

![image-20231206190019109](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206190019109.png)

一对一

![image-20231206195331828](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206195331828.png)

##### 2、多表查询概述

概述：指从多张表中查数据

笛卡尔积：笛卡尔乘积是指数学中，两个集合A集合和B集合的所有组合情况。（在多表查询时，需要消除无效的笛卡尔积）

![image-20231206200649411](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206200649411.png)

##### 3、内连接

![image-20231206200758990](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206200758990.png)

内连接查询语法：

​	隐式内连接

```python
SELECT 字段列表 FROM 表1，表2 WHERE 条件...;
```

​	![image-20231206201700368](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206201700368.png)

​	显示内连接

```python
SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件..；
```

![image-20231206201921175](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206201921175.png)

##### 4、外连接

![image-20231206202326190](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206202326190.png)

左外连接:相当于查询表1（左表）的所有数据包含表1和表2交集部分的数据

```python
SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件...;
```

![image-20231206202649111](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206202649111.png)

右外连接：相当于查询表2（右表）的所有数据包含表1和表2交集部分的数据

```
SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件...;
```

![image-20231206202904685](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206202904685.png)

##### 5、自连接

语法结构:自连接查询，可以时内连接查询，也可以时外连接查询。

```python
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件...;
```

![image-20231206203925529](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206203925529.png)

##### 6、联合查询-union,union all

对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。

```python
SELECT 字段列表名 FROM 表A....
UNION[ALL]
SELECT 字段列表 FROM 表B....;
```

对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致。

union all 会将全部的数据直接合并在一起，union会对合并之后的数据去重。

## 第七章 子查询

概念：SQL语句中嵌套SELECT语句，称为嵌套查询，又称子查询。

```python
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```

子查询外部的语句可以是INSERT / UPDATE / DELETE / SELECT 中的任何一个。

根据子查询结果不同，分为：

​	标量子查询（子查询结果为单个值）

​	列子查询（子查询结果为一列）

​	行子查询（子查询结果为一行）

​	表子查询（子查询结果为多行多列）

根据子查询位置，分为：WHERE之后、FROM之后、SELECT之后。

##### 1、标量子查询

​	子查询返回的结果是单个值（数字、字符串、日期等），最简单的形式，这种子查询成为标量子查询。

​	常用的操作符：= <> > >= < <=

![image-20231206214736507](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206214736507.png)

##### 2、列子查询

子查询返回的结果是一列（可以是多行），这种子查询称为列子查询。

常用的操作符：IN、NOT IN、ANY、SOME、ALL

![image-20231206222551825](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206222551825.png)

![image-20231206223159232](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206223159232.png)

![image-20231206223544089](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206223544089.png)

![image-20231206224609009](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231206224609009.png)

##### 3、行子查询

行子查询返回的结果是一行（可以是多列），这种子查询称为行子查询。

常用的操作符：= 、<>、IN、NOT IN

```python
select salary , managerid from emp where name = '张无忌';
select * from emp  where salary = 125000 and managerid = 1;
select * from emp where (salary , managerid)=(125000,1);
select * from emp where (salary , managerid) = (select salary , managerid from emp where name = '张无忌');
```

 7.3表子查询

子查询返回的是多行多列，这种子查询称为表子查询

常用的操作符：IN

```python
# 查询与‘杨逍’，‘韦一笑’的职位和薪资相同的员工信息。

#a.查询‘杨逍’，‘韦一笑’的职位和薪资
select job ,salary from emp where name = '杨逍' or name = '韦一笑';

#b.查询与‘杨逍’，‘韦一笑’的职位和薪资相同的员工信息。
select * from emp where (job,salary) in (select job,salary from emp where name = '杨逍' or name = '韦一笑');
```

​	

```python
# 2.查询入职日期是“2005-01-01”之后的员工信息，及其部门信息
#a.入职日期是“2005-01-01”之后的员工信息
SELECT * FROM emp WHERE entrydate > '2005-01-01';

#b.查询这部分员工，对应的部门信息；
select e.*, d. * from (SELECT * FROM emp WHERE entrydate > '2005-01-01') e left join  dept d on e.dept_id=d.id;
```

##### 4、案例

```python
#1.查询员工的姓名，年龄，职位，部门信息（隐式内链接）
#表：emp,dept
#连接条件：emp.dept_id=dept.id 
select e.name , e.age , e.job , d.name from emp e , dept d where e.dept_id=d.id ;

#2.查询年龄小于30岁的员工的姓名，年龄，职位，部门信息（显式内链接）
#表：emp,dept
#连接条件：emp.dept_id=dept.id 
select e.name , e.age , e.job , d.name from emp e join dept d on e.dept_id=d.id where e.age < 30 ;

#3.查询拥有员工的部门ID、部门名称——内连接
#表：emp,dept
#连接条件：emp.dept_id=dept.id 
#查询结果去重 distinct
select distinct d.id , d.name  from emp e , dept d where e.dept_id=d.id ;

#4.查询所有年龄大于40岁的员工，及其归属的部门名称；如果员工没有分配部门，也需要展示出来
#表：emp,dept
#连接条件：emp.dept_id=dept.id 
#外连接
select e.* , e.job from emp e left join dept d on  e.dept_id=d.id where age > 40  ;

#5.查询员工所有等级
#表：emp,salgrade
#连接条件：emp.salary >= salgrade.losal and emp.salary <= salgrade.hisal
select e.*,s.grade from emp e , salgrade s where e.salary >= s.losal and e.salary <= s.hisal;
select e.*,s.grade from emp e , salgrade s where e.salary between s.losal and e.salary;

#6.查询”研发部“所有员工的信息及工资等级
#表：emp ,salgrade,dept
#连接条件： emp.salary between salgrade.losal and salgrade.hisal , emp.dept_id = dept.id
#查询条件：dept.name = '研发部'
SELECT
	e.*,
	s.grade 
FROM
	emp e,
	dept d,
	salgrade s 
WHERE
	e.dept_id = d.id 
	AND e.salary BETWEEN s.losal 
	AND s.hisal 
	AND d.NAME = '研发部';
    	#7.查询“研发部”员工的平均工资
	#表：emp,dept
	#连接条件：emp.dept_id = dept.id 
	select avg (e.salary) from emp e , dept d where e.dept_id = d.id and d.name = '研发部';
	
	#8.查询工资比“杨逍”搞得员工信息。
	#a.查询“杨逍”的薪资
	#b.查询比她工资高的员工数据
	select * from emp where salary > (select salary from emp where name = '杨逍');
	
	#9.查询比平均薪资高的员工信息
	#a.查询员工的平均薪资
	#b.查询比平均薪资高的员工信息
	select * from emp where salary > (select avg(salary) from emp);
	
	#10.查询低于本部门平均工资的员工信息
	#a.查询指定部门平均薪资
	#b.查询比本部门低的员工薪资
	select * from emp e2 where e2.salary < (select avg(e1.salary) from emp e1 where e1.dept_id = e2.dept_id);

    #11.查询所有的部门信息，并统计部门的员工人数
	select d.id,d.name,(select count(*) from emp e where e.dept_id = d.id ) '人数' from dept d;
	
	#12.查询所有学生的选修课情况，展示出学生名称，学号，课程名称
	#表：student，course，student_course
	#连接条件：student.id = student_course.studentid , course.id = student_course.courseid 
	
	select s.name ,s.no,c.name from  student s , student_course sc ,course c where s.id =sc.studentid and sc.courseid = c.id;
```

## 第八章 事务

事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提示提交或撤销操作作为一个整体一起向系统提交或插销操作请求，即这些操作要么同时成功，要么同时失败。

##### 1、事务操作语法

查看/设置事务提交方式

```python
SELECT @@autocommit;
SET @@autocommit=0;
```

提交事务

```python
COMMIT;
```

回滚事务

```python
ROLLBACK；
```

```python
create table account (
	id int auto_increment primary key comment '主键ID',
	name varchar(10) comment '姓名',
	money int comment '余额'
	)comment '账户表';
	insert into account (id,name,money) values (null,'张三',2000),(null,'李四',2000);
	
#恢复数据
update account set money = 2000 where name = '张三' or name = '李四';

select @@autocommit;   #设置为自动提交
set @@autocommit = 0; #设置为手动提交
	
#转账操作
#1.查询张三账号余额
select * from account where name = '张三';
#2.将张三账户余额-1000
update account set money = money - 1000 where name = '张三';
#3.将李四账户余额+1000
update account set money = money + 1000 where name = '李四';

#提交事务
commit;

#回滚事务
rollback;
```

方式二：

开启事务

```python
START TRANSACTION 或 BEGIN;
```

提交事务

```python
COMMIT;
```

回滚事务

```python
ROLLBACK;
```

```python
#方式二
#转账操作（张三给李四转账1000）
start transaction;

#转账操作
#1.查询张三账号余额
select * from account where name = '张三';
#2.将张三账户余额-1000
update account set money = money - 1000 where name = '张三';
#3.将李四账户余额+1000
update account set money = money + 1000 where name = '李四';

#提交事务
commit;

#回滚事务
rollback;
```

##### 2、事务四大特性

原子性(Atomicity)∶事务是不可分割的最小操作单元，要么全部成功，要么全部失败。

一致性(Consistency) :事务完成时，必须使所有的数据都保持一致状态。

隔离性（Isolation):数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。

持久性（Durability):事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

##### 3、并发事务问题

![image-20231209103000381](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231209103000381.png)

##### 4、事务隔离级别

![image-20231209103502997](C:\Users\cyl\AppData\Roaming\Typora\typora-user-images\image-20231209103502997.png)

查看事务隔离级别

```python
SELECT @@TRANSACTION_ISOLATION;
```

设置事务隔离级别

```python
SET [SESSION / GLOBAL ] TRANSACTION ISOLATION LEVEL {read uncommtted/ READ COMMITTED/REPEATABLE READ /SERIALLZABLE}
```
