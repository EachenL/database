## 第二章 关系数据库

### 2.1 关系数据结构及形式化定义

### 2.2 关系操作

### 2.3 关系的完整性

### 2.4 关系代数

### 2.5 关系演算

## 第三章 关系数据库标准语言SQL

### 3.1 SQL概述

### 3.2 学生-课程数据库

###3.3 数据定义 

#### 3.3.1 模式的定义与删除

#####1. 定义模式

`create schema <模式名>  authorization <用户名> `

`[<表定义子句> | <视图定义子句> | <授权定义子句>]`

#####2. 删除模式

`drop schema <模式名> <cascade | restrict>`

#### 3.3.2 基本表的定义与删除

#####1. 定义基本表`表定义子句`

` create table <表名> `  

`( <列名> <数据类型> [列级完整性约束条件][, ...] `

` [, <表级完整性约束条件>]);`

#####2. 删除基本表

`drop table <表名> [restrict | cascade];`

#####3. 修改基本表

`alter table <表名> ` 

` [add [完整性约束] <新列名> <数据类型>]` 

 `[drop <完整性约束名>] `

 `[alter column <列名> <数据类型>]; `

#####4. 模式与表

#####5. 表的数据类型

`char(n)` `varchar(n)` `int` `smallint` `numeric(p,d)` `real` `double precision` `float` `date` `time` 

#### 3.3.3 索引的建立与删除

#####1. 建立索引

`create [unique] [cluster] index <索引名> on <表名> `

`(<列名> [<次序>][,<列名> [<次序>]]...);`

#####2. 删除索引

`drop index <索引名>;`

### 3.4 数据查询

`select [<聚集函数> ([all | distinct] <目标列表达式> [,<目标列表达式>]...)]`

`from <表名或视图名> [,<表名或视图名>]...`

`[where <条件表达式>]`

`[group by <列名1> [having <条件表达式>]]`

`[order by <列名2> [asc | desc]];`

#### 3.4.1 单表查询

#####1. 查询表中若干列

1. 查询指定列

   `select sno, sname from student;`

2. 查询全部列

   `select * from student;`

3. 查询经过计算的值

   `select sname, 2004-sage from student;`

##### 2. 选择表中的若干元组

1. 消除取值重复的行

   `select distinct sno from sc`

2. 查询满足条件的元组

   | 查询条件 | 谓词                                       |
   | :--- | :--------------------------------------- |
   | 比较   | =, >, <, >=, <=, != or <>, !>, !<; not+上述运算符 |
   | 确定范围 | between and, not between and             |
   | 确定集合 | in, not in                               |
   | 字符匹配 | like, not like                           |
   | 空值   | is null, is not null                     |
   | 逻辑运算 | and, or, not                             |

   <条件表达式> = <列名> <查询谓词> <条件>

   (1) 比较 `sdept = 'cs'` `sage < 20` 

   (2) 范围`sage between 20 and 23` 

   (3) 集合`sdept not in ('cs','ma','is')`

   (4) 字符 `sno like '1501'` `sname like '刘%'` `sname like '欧阳__'` `sname like '__阳%'` 
   `cname like 'db\_design' escape '\'` 

   (5) 空值`grade is null` 

   (6) 逻辑`sdept = 'cs' or sdept = 'ma' or sdept = 'is'`

##### 3. order by子句

​	`order by sdept, sage desc;`sdept 为升序 缺省

##### 4. 聚集函数

​	`count` `sum` `avg` `max` `min` 

​	select <聚集函数> ( [d | a] <列名>| * ) 

##### 5. group by子句

​	`group by cno;`

#### 3.4.2 连接查询

##### 1. 等值与非等值连接查询

​	`where studen.sno = sc.sno;`

##### 2. 自身连接

​	`select first.cno, second.cpno `

​	`from course first, course second `

​	`where first.cpno = second.cno;`

##### 3. 外连接

​	`from student left out join sc on (student.sno = sc.sno);`或

​	`from student left out join sc using (sno)`;

##### 4. 符合条件连接

​	`where student.sno = sc.sno and sc.cno = '2' and sc.grade > 90;`

​	`where student.sno = sc.sno and sc.cno = course.cno;`

#### 3.4.3 嵌套查询

##### 1. 带有in谓词的子查询

​	`where sdept in(select sdept from student where sname = '刘晨');`

##### 2. 带有比较运算符的子查询

​	`where sdept = ();`

​	`where () = sdept;`

 	`where grade >= (select avg(grade) from sc y where y.sno = x.sno);`

#####3. 带有any(some)或all谓词的子查询

​	<比较运算符>< any | all >

##### 4. 带有exists谓词的子查询

​	`where not exists (select* from sc where sno = student.sno and cno = '1');`

#### 3.4.4 集合查询

​	`union` `intersect` `except` 并 / 交 / 差

#### 3.4.5 select语句的一般格式

​	**select**一般格式:

​		`select [all | distinct] <目标列表达式> [别名] [ , <> [] ]...`

​		` from <表名或视图> [别名] [ , <>[] ]... ` 

​		`[where <条件表达式>] [group by <列名> [having <条件表达式> ] ] `

​		`[order by <列名> [ asc|distinct ] ]`

 1.  **目标列表达式**可选项:

     ​	(1) *

     ​	(2) <表名>.*

     ​	(3) <聚集函数>(  [all|distinct] <*|列名> )	

	2. **where**子句的**条件表达式**可选项:

    ​	(1) <属性列名> {<属性列名>, <常量>, [any|all]  (select语句)}

    ​	(2) <属性列名> [not] between {<属性列名>, <常量>, (select语句) } and {<属性列名>, <常量>, (select语句)}

    ​	(3) <属性列名> [not] in { ( <值1> [ , <值2> ] ... ), (select语句) }

    ​	(4) <属性列名> [not] like <匹配串>

    ​	(5) <属性列名> is [not] null

    ​	(6) [not] exists (select语句)

    ​	(7) <条件表达式>   {and, or}   <条件表达式>   [  {and, or}   <条件表达式>  ] ...

### 3.5 数据更新

#### 3.5.1 插入数据

##### 1. 插入元组

​	`insert `

​	`into <表名> [ ( <属性列1> [ ,<属性列2>] ... ) ] `

​	`values ( <常量1> [ , <常量2> ] ...);`

##### 2. 插入子查询结果

​	`insert `

​	`into <表名> [ ( <属性列1> [ ,<属性列2>] ... ) ] `

​	`子查询;`

#### 3.5.2 修改数据

​	`update <表名> `

​	`set <列名> = <表达式> [ , <列名> = <表达式>]... `

​	`[where <条件>];`

##### 1. 修改某一个元组的值

​	`update student set age = 22 where sno = '1501';`

##### 2. 修改多个元组的值

​	`update student set age = age + 1;`

##### 3. 带子查询的修改语句

​	`update sc set grade = 0 `

​	`where 'cs' = (select sdept from student where student.sno = sc.sno);`

#### 3.5.3 删除数据

​	`delete from <表名> [where <条件>];`

##### 1. 删除某一个元组的值

​	`delete from student where sno = '1501';`

##### 2. 删除多个元组的值

​	`delete from sc;`

##### 3. 带子查询的删除语句

​	`delete from sc where 'cs' = (select sdept from student where student.sno = sc.sno);`

### 3.6 视图

#### 3.6.1 定义视图

##### 1. 建立视图

​	`create view <视图名> [ ( <列名> [,<列名>]...) ] `

​	`as <子查询> `

​	`[with check optionn];`

##### 2. 删除视图

​	`drop view <视图名> [cascade];`

#### 3.6.2 查询视图

​	可视为表操作

#### 3.6.3 更新视图

​	`update修改` `delete删除 ` `insert插入`可视为对表操作

#### 3.6.4 视图的作用

 	1. 简化用户操作
	2. 可以从多角度看待统一数据
	3. 对重构数据库提供一定程度的逻辑独立性
	4. 对机密数据提供安全保护
	5. 更清晰的表达查询

## 第四章 数据库安全性

### 4.1 计算机安全性概述

### 4.2 数据库安全性控制

### 4.3 视图控制

### 4.4 审计

### 4.5 数据加密

### 4.6 统计数据库安全性

## 第五章 数据库完整性

### 5.1 实体完整性

#### 5.1.1 实体完整性定义

#### 5.1.2 实体完整新检查和违约处理

### 5.2 参照完整性

#### 5.2.1 参照完整性定义

#### 5.2.2 参照完整性检查和违约处理

### 5.3 用户定义完整性

#### 5.3.1 属性上的约束条件和定义

#### 5.3.2 属性上的约束条件检查和违约处理

#### 5.3.3 元组上的约束条件的定义

#### 5.3.4 元组上的约束条件检查和违约处理

### 5.4 完整性约束命名子句

### 5.5 域中的完整性限制

### 5.6 触发器

#### 5.6.1 定义触发器

#### 5.6.2 激活触发器

#### 5.6.3 删除触发器

## 第六章 关系数据理论

### 6.1 问题的提出

### 6.2 规范化

#### 6.2.1 函数依赖

#### 6.2.2 码

#### 6.2.3 范式

#### 6.2.4 2NF

#### 6.2.5 3NF

#### 6.2.6 BCNF

#### 6.2.7 多值依赖

#### 6.2.8 4NF

### 6.3 数据依赖的公理系统

### 6.4 模式的分解

#### 6.4.1 模式分解的3个定义

#### 6.4.2 分解的无损连接性和保持函数依赖性

#### 6.4.3 模式分解的算法

## 第七章 数据库设计

### 7.1 数据库设计概述

### 7.2 需求分析

### 7.3 概念结构设计

### 7.4 逻辑结构设计

### 7.5 数据库的物理设计

### 7.6 数据库的实施和维护

## 第八章 数据库编程

### 8.1 嵌入式SQL

### 8.2 存储过程

### 8.3 odbc编程





