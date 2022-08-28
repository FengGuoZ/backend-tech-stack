## MySQL基础

> 2022年2月~2022年3月

### 第1章 数据库导论

#### 1.1 数据存储的方式🌼

- **内存中**（数组、集合、链表等）：具有易失性
- **文件中**：数据项多了后，不好管理，查找、分类困难
- **数据库软件**
  - 实现数据持久化
  - 用完整的系统统一管理，查询方便



#### 1.2 DB、DBMS、SQL🌼

- **DB** database：存储数据的仓库
- **DBMS** Database Management System：DBMS创建和管理的DB
  - MySQL、Redis、Oracle、MangoDB
- **SQL** Structure Query Language：操作DB的接口语言
  - **通用性**，SQL非某个DBMS供应商专用的语言，几乎所有DBMS软件都支持SQL
  - 简单易学
  - 支持非常复杂、高级的操作

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220123214612048.png" alt="image-20220123214612048" style="zoom:50%;" />



#### 1.3 win下使用MySQL

- 启动指令：`net start 服务名`

  - 服务名可自定义

- 停止指令：`net stop 服务名`

  ```sh
  net start MySQL
  net stop MySQL
  ```

  <img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220125000818278.png" alt="image-20220125000818278" style="zoom:50%;" />

- 登录指令：`mysql -h主机名 -P端口号 -u用户名 -p密码`

  ```shell
  mysql -hlocalhost -P3306 -uroot -p
  ```

- 精简登录指令（默认本机服务、3306端口）

  ```sh
  mysql -uroot -p
  ```

- 退出指令：`exit`

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220124172459560.png" alt="image-20220124172459560" style="zoom: 67%;" />



#### 1.4 MySQL基础指令

```mysql
show databases;	# 显示当前所有DB
use dbName;
show tables [from dbName];
desc tableName;
select --vesion();
```



#### 1.5 常用库函数

##### 1.5.1 字符函数

- **length()**：获取参数值的字节个数

```mysql
SELECT LENGTH('john');

SELECT 
  LENGTH('张三丰hahaha') ;		#一个汉字占用3B（客户端为UTF-8字符集）
```

- **concat()**：拼接字符串

```mysql
SELECT 
  CONCAT(`last_name`, '_', `first_name`) AS 姓名 
FROM
  `employees` ;
```

- **upper()**、 **lower()**： 字符串大小写转化

```mysql
SELECT UPPER('john');
SELECT LOWER('ZWD');
```

- **substr()**：获取子串，MySQL从1开始计数

```mysql
SELECT SUBSTR('zhuwandong', 7) AS opt;
SELECT SUBSTR('zhuwandong', 1,3) AS opt;	#mysql函数支持重载

#案例:姓名中首字符大写，其他字符小写然后用_拼接，显示出来
SELECT 
  CONCAT(
    SUBSTR(`last_name`, 1, 1),
    '_',
    SUBSTR(`last_name`, 2)
  ) AS OPT 
FROM
  `employees` ;
```

- **repalce()**：指定**字符替换**（0-n个）

```mysql
SELECT 
  REPLACE(
    '张无忌爱上了周芷若周芷若周芷若周芷若',
    '周芷若',
    '赵敏'
  ) AS res ;
```



##### 1.5.2 数学函数

- **round()**：四舍五入取整

  ```mysql
  SELECT ROUND(-1.55) AS res; # -2
  ```

- **ceil()**：向上取整

  ```mysql
  SELECT CEIL(1.2) AS res; # 2
  ```

- **floor()**：向下取整

  ```mysql
  SELECT FLOOR(-9.7) AS res;	# -10
  ```

- **truncate()**：截断

  ```mysql
  SELECT TRUNCATE(12.26342, 3); # 12.263
  ```

- **mod()**：取余 等价于%

  ```mysql
  SELECT MOD(10,-3) AS res; # 1
  ```



##### 1.5.3 日期函数

- **now()**：返回当前系统日期+时间

  ```mysql
  select now();
  ```

- **curdate()**：返回当前系统日期

  ```mysql
  SELECT CURDATE()
  ```

- **curtime()**：返回当前时间

  ```mysql
  SELECT CURTIME(); 
  ```

- **datadiff()**：求**日期的天数差**

  ```mysql
  SELECT 
    MAX(`hiredate`),
    MIN(`hiredate`),
    DATEDIFF(MAX(`hiredate`), MIN(`hiredate`)) AS 最大入职天数差 
  FROM
    `employees` ;
  ```



##### 1.5.4 聚合函数

- 作统计用，又成分组函数

- `sum avg max min count`

```mysql
SELECT SUM(`salary`) FROM `employees`;
SELECT AVG(`salary`) FROM `employees`;
SELECT MAX(`salary`) FROM `employees`;
SELECT MIN(`salary`) FROM `employees`;
SELECT COUNT(`salary`) FROM `employees`;
```



#### 1.6 MySQL数据类型

**常见数据类型**

- 整型：tinyint、smallint、**int**(4B)、bigint
- 小数：float、**double**
- 字符型：char、**varchar**、text、blob
- 日期：**timestamp**



**int(11) Vs int(3)**⭐

- int(11)与int(3)都占用4B，**取值范围相同**
- **11和3决定的是显示长度**
- int(3)在数值不足3位时会补全为3位（25→025），数值超过3位时无任何影响



**varchar VS char**⭐

- varchar为可变长度字符串


- varchar使用额外1-2B来存储string长度，从而**实现了变长度存储的特性**


- varchar(255) 类型的'FengGuo' **占用8个Byte存储**，其中一个Byte值为7(len)


- 如果用char(255) 存储'FengGuo' 则会占用255Byte存储



### 第2章 DQL 数据查询

Data Query Language

**查询的结果是一个临时的虚拟表格**



#### 2.1 基础查询 select🌼

```mysql
select 查询列表 from 表名;

# 查询指定字段
SELECT 
  last_name AS 姓,
  first_name AS 名 
FROM
  employees;

# 查询所有字段
SELECT * FROM employees;	# *表示所有

# DISTINCT关键字去重
SELECT DISTINCT 
  `department_id` 
FROM
  `employees` ;

# CONCAT 拼接字段
SELECT DISTINCT
  CONCAT(`last_name`,' ',`first_name`) AS 姓名 
FROM
  `employees` ;
```



#### 3.2 条件查询 where🌼

```mysql
select
	查询列表
from
	表名
where	
	筛选条件;

# 表达式筛选
SELECT 
  * 
FROM
  employees 
WHERE salary >= 10000 
  AND salary < 20000 ;
  
# 模糊筛选
SELECT 
  * 
FROM
  employees 
WHERE `last_name` LIKE '%s%' ;

SELECT 
  * 
FROM
  `employees` 
WHERE `employee_id`BETWEEN 100 AND 120 ;			# [100, 200]

SELECT 
  `last_name`,
  `job_id` 
FROM
  `employees` 
WHERE `job_id` IN ('IT_PROG', 'AD_VP', 'AD_PRES') ;	# in列表

SELECT 
  `last_name`,
  `commission_pct` 
FROM
  `employees` 
WHERE `commission_pct` IS NULL ;					# NULL值判断
```

**模糊筛选关键字**

- **like**，与通配符搭配使用
  - `%`：任意多个符(0~n)
  - `_`：任意1个字符
- **between and**，双边闭区间[]
- **in**，判断某字段的值是否属于in列表中的某一项
- **is null**，用于判断NULL值
  - =或<>**不能用于**判断NULL值
  - 用`is null` 或 `is not null`



#### 3.3 排序查询 order by🌼

```mysql
select 查询列表
from 表名
【where 筛选条件】
order by 排序列表 【asc | desc】

SELECT 
  *,
  `salary` * 12 * (1+ IFNULL(`commission_pct`, 0)) AS "YearPay"
FROM
  `employees` 
ORDER BY "YearPay" DESC,
  `employee_id`  ASC;		# 整体按年薪降序排序 年薪相同时工号升序
```

- 默认**asc 升序**,**desc 降序**

- 稳定排序
- 支持多字段，**优先级从左到右**



#### 3.4 分组查询 group by🌼

```mysql
select 聚合函数, 查询列表							# 5
from 表											# 1
【where 筛选条件】								 # 2
group by 分组的列表								 # 3
【having 分组后筛选】								# 4
【order by子句】								  # 6

SELECT AVG(`salary`), `department_id` 			# 本身按照department_id分组 不会二义
FROM `employees`
WHERE `email` LIKE '%a%'
GROUP BY `department_id`;
```

**查询列表必须特殊**，要求是聚合函数或分组列表本身，普通字段会导致逻辑错误

|   关键字   |      作用      |            数据源            |        位置         |
| :--------: | :------------: | :--------------------------: | :-----------------: |
|   where    |   分组前筛选   |            原始表            |   group by 子句前   |
| **having** | **分组后筛选** | 数据源**分组后生成的临时表** | 位置group by 子句后 |



#### 3.5 连接查询 join🌼

- 同时查询**多个表中的关联数据**，又称多表查询

- 没有连接条件时**连接查询的结果为笛卡尔乘积**，共m*n条记录

![image-20220827233345774](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220827233345774.png)



##### 3.5.1 内连接 inner join

```mysql
select 查询列表
from 表1 AS 别名 
inner join 表2 AS 别名
on 连接条件;

# 案例1.查询员工名、部门名(可调换位置)
SELECT `last_name`, `department_name`
FROM `employees` AS e 
INNER JOIN `departments` AS d
ON e.`department_id`=d.`department_id`;
```

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220203023746334.png" alt="image-20220203023746334" style="zoom: 50%;" />

##### 3.5.2 外连接 left/right join

```mysql
# 女生的男友名 没有男友的显示为NULL
# 左外连接的结果集为全部女生
SELECT `name`, `boyName`
FROM `beauty`
LEFT JOIN `boys`
ON `beauty`.`boyfriend_id`=`boys`.`id`
```

外连接的**结果为主表中的所有记录**

- 如果从表中有和它匹配的，则显示匹配的值

- 如果从表中没有和它匹配的，则显示null
- `left join`左侧的是主表
- `right join`右侧的是主表

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220828001423480.png" alt="image-20220828001423480" style="zoom:50%;" />



##### 3.5.3 交叉连接

交叉连接的结果为**笛卡尔乘积**

```mysql
SELECT `name`, `boyName`
FROM `beauty`
CROSS JOIN `boys`;
```



#### 3.6 子查询

出现在其它语句中的**select子句称为子查询**

```mysql
# 案例1:谁的工资比Abel高?
SELECT `last_name`, `salary`
FROM `employees`
WHERE `salary`>(
	SELECT `salary`
	FROM `employees`
	WHERE `last_name`='Abel' 	# 子查询作为一个标量用
);
```



#### 3.7 分页查询 limit offset

```mysql
select 查询列表			  7
from 表1					1
【连接类型 join 表2】		 2
on 连接条件				  3
where 筛选条件			  4
group by 分组字段		  5
having 分组后筛选		 6
order by 排序字段】		 8
limit 【offset,】 size;  9

# 前端请求 显示的页数page，每页的条目数size
select 查询列表
from 表
limit (page-1)*size, size;

# 案例1:查询前五条员工信息
SELECT *
FROM `employees`
LIMIT 0, 5;
```

- offset从0开始计数
- limit位于语句最后



#### 3.8 联合查询 union

用于**将不同表中相同列中的数据查询出来**

```mysql
查询语句1
union
查询语句2
union...

#案例：查询中国用户中男性的信息以及外国用户中年男性的用户信息
SELECT id,cname FROM t_ca WHERE csex='男'
UNION ALL
SELECT t_id,tname FROM t_ua WHERE tGender='male';
```

- `union`关键字**默认去重**，如果使用`union all` 可以**包含重复项**

- `union`语句查询的字段类型必须一致

- **应用场景**

  - 要查询的结果**来自于多个表**

  - 表之间**没有直接的连接关系**

  - 查询的信息严格一致





### 第3章 DML 数据操控

**Data Manipulation Language**

- insert
- update
- delete



#### 3.1 插入语句 insert

```mysql
insert into 表名(字段名,...)
values(值1,...);

#6.支持多行同时插入
INSERT INTO `beauty`
VALUES	(23, '唐艺昕1', '女', '1990-4-23', '18999999999', NULL, 2),
		(24, '唐艺昕2', '女', '1990-4-23', '18999999999', NULL, 2),
		(25, '唐艺昕3', '女', '1990-4-23', '18999999999', NULL, 2);
```



#### 3.2 修改语句 update

```mysql
update 表名
set 列1=新值1, 列2=新值2, ...
where 筛选条件;

#1.修改单表的记录
#案例1:修改beauty表中姓唐的女神的电话为13899888899
UPDATE `beauty`
SET `phone`='13899888899'
WHERE `name` LIKE '唐%';
```



#### 3.3 删除语句 delete truncate

```mysql
delete from 表名 where 筛选条件;

#方式一: delete
#1.单表的删除
#案例1:删除手机号以9结尾的女神信息
DELETE FROM `beauty`
WHERE `phone` LIKE '%9';

#方式二：truncate (清空数据)
TRUNCATE TABLE `my_employees`;
```



**`delete Vs truncate`**⭐

- delete **可以加where条件**，truncate不能加
- 在清空表数据时，truncate **效率高一丢丢**
- 删除的表中**有自增长列时**
  - delete删除后，再插入数据，**自增长列的值从断点开始**
  - truncate删除后，再插入数据，**自增长列的值从1开始**（清空得更彻底）
- **truncate 删除不能回滚**，delete 删除可以回滚.



### 第4章 DDL 数据定义语言

**Data Definition Language**

- create
- alter
- drop



#### 4.1 库的管理 create|alter|drop database

```mysql
create database 库名;

# 创建库
CREATE DATABASE IF NOT EXISTS books;	# 如果库不存在，则创建(if exists提供容错处理)

# 修改库属性
ALTER DATABASE books CHARACTER SET UTF8;

# 删除库
DROP DATABASE IF EXISTS books;
```

- `IF NOT EXISTS` **用于容错**，避免因库已存在而报错



#### 4.2 表的管理 create|alter|drop table

```mysql
# crate table
CREATE TABLE IF NOT EXISTS Book(
	id INT,
	bName VARCHAR(20),
	price DOUBLE,
	authorID INT, 
	publishDate DATETIME
);
DESC book;	# 显示表结构

# alter table
# alter table可搭配add|drop|change|modify column使用
ALTER TABLE 'author' ADD COLUMN 'annual' DOUBLE;

# drop table
DROP TABLE IF EXISTS 指定表名;
```



#### 4.3 约束 constraint

用于**限制字段属性，保障数据一致性**

|  约束名称  |     关键字      |                             作用                             |     应用举例     |
| :--------: | :-------------: | :----------------------------------------------------------: | :--------------: |
|    主键    | **PRIMARY KEY** |              保证该字段具备**唯一性，并且非空**              |   员工号、学号   |
|   唯一键   |   **UNIQUE**    |              保证该字段具备**唯一性，可以为空**              |     电话号码     |
|  非空约束  |  **NOT NULL**   |                     保障该字段的值不为空                     |  重要字段如姓名  |
| 默认值约束 |   **DEFALUT**   |                      保证该字段有默认值                      |    汉族、中国    |
|    外键    | **FOREIGN KEY** | 用于限制两个表的关系，用于保证从表该字段的值必须来自主表关联字段 | 学生表的专业编号 |
|  检查约束  |    **CHECK**    |                        mysql中不支持                         |        \         |

```mysql
# 创建表时添加约束
CREATE TABLE major(
    # 约束语法：字段名 类型 约束
	id INT PRIMARY KEY, 			#主键（列级）
	majorName VARCHAR(20) NOT NULL 	#非空约束（列级）
);

CREATE TABLE stuinfo(				# stuinfo依赖于major
	id INT PRIMARY KEY, 			#主键（列级）
	stuName VARCHAR(20) NOT NULL, 	#非空约束（列级）
	gender CHAR(1) ,
	seat INT UNIQUE,				#唯一约束（列级）
	age INT DEFAULT 18, 			#默认值约束（列级）
	majorID INT,
    
    # 外键约束语法：CONSTRAINT 约束名 FOREIGN KEY(从表字段名) REFERENCES 主表名(主表主键)
    CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorID) REFERENCES `major`(id)
);
```



**主键 VS 唯一键**

|        | 是否唯一 | 是否非空 | 同类键个数 |
| :----: | :------: | :------: | :--------: |
|  主键  |    √     |    √     |   <=1个    |
| 唯一键 |    √     |    ×     |    n个     |



#### 4.4 自增长列 AUTO_INCREMENT

可以不用手动插入值，**系统提供默认的自增长序列**

- 一般**与PRIMARY KEY连用**
- 默认初值为1，step为1

```mysql
# 创建表时设置标识列
CREATE TABLE `tab_identity`(
	id INT PRIMARY KEY AUTO_INCREMENT,	#设置自增长属性
	`name` VARCHAR(20)
);
```



```mysql
create table stu_info(
	id INT primary key auto_increment,
    name varchar(20),
    age int(3),
    money int(10) default 0
);
```





### 第5章 TCL 事务控制

**Transaction Control Language**



#### 5.1 事务

事务指一组sql语句组成执行单元

InnoDB支持事务，MyISAM不支持事务

```mysql
# 转账事务演示

# 1.开启事务
SET autocommit=0;

# 2.业务语句
UPDATE `account`
SET `balance`=500
WHERE `username`='张无忌';
UPDATE `account`
SET `balance`=1500
WHERE `username`='赵敏';

# 3.提交or回滚事务
COMMIT;
# ROLLBACK;

# 4.autocommit自动重置为1
```



**事务的ACID属性**（⭐面试题）

- **原子性 Atomicity**：事务是不可分割的工作单位
- **一致性 Consitency**：事务是**数据库从一致性状态A转化为一致性状态B**
  - 转账事务余额总量不变
- **隔离性 Isolation**：一个事务在执行时，**不被其它事务所干扰**
- **持久性 Durability**：事务提交后，**改变是永久性的**，不可撤销



#### 5.2 隔离级别

**事务并发问题**：并行事务同时访问数据库中相同数据时产生并发问题

- **脏读**：一个事务读取了其它事务更新但未提交的数据
- **不可重复读**：一个事务两次读取的值不一致
- **幻读**：一个事务读取了其它事务插入但未提交的数据，两次读到的记录数不一致



**数据库事务隔离技术**：用于解决事务并发问题，**使事务之间不会相互影响**

- **隔离级别**：指事务与其它事务之间的隔离程度

- **分为4级**，隔离级别越高，**数据一致性越好**，**但并发能力越弱**）（内生矛盾）
- 以最高级**serializable(串行化)**为例，该事务操作表A时，其余事务访问表A时，**会被阻塞**（类似于锁机制）

|                                  | 脏读 | 不可重复读 | 幻读  |
| :------------------------------: | :--: | :--------: | :---: |
| **read uncommitted**（读未提交） |  √   |     √      |   √   |
|  **read committed**（读已提交）  |  ×   |     √      |   √   |
| **repeatable read**（可重复读）  |  ×   |     ×      |   √   |
|     **serializable**(串行化)     |  ×   |     ×      | **×** |

**数据库默认隔离级别**

- **Mysql：repeatable read**
- Oracle：read committed



**事务隔离相关指令**

```mysql
# 查看当前隔离级别
SELECT @@tx_isolation;

# 设置 当前连接|数据库 隔离级别
SET SESSION|GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;
```



### Linux下使用MySQL

mysql命令行

```bash
sudo service mysql start	# 启动mysql服务
sudo mysql -uroot -p		# 登录mysql客户端 需要输入密码

status						# 查看mysql基础配置 可通过vim /etc/my.cnf修改
show databases;				# 查看当前数据库
use DBname;					# 使用name库
show tables;				# 查看当前库中所有表
desc tableName;				# 展示表结构

exit;						# 退出mysql客户端
sudo service mysql stop		# 关闭mysql服务
```

![image-20220828135642817](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220828135642817.png)

![image-20220828141823369](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220828141823369.png)



## MySQL面试八股

#### 01 MySql表间关系

**1-n关系**，2张表可表示

- 国际分类下可以有很多文章

**n-n关系**，3张表可表示

- 学生可以选多门课，一门课下有多个学生
- **第3张表专门用于存储课与学生的对应关系**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220828143050078.png" alt="image-20220828143050078" style="zoom:80%;" />

#### 02 key Vs index

**key包含constraint和index两重含义**

-  **primary key**
  - 唯一约束和非空约束
  - 同时在此key上建立了index

- **unique key**
  - 唯一约束
  - 同时在此key上建立了index



**index是纯粹的索引，一种辅助查询的数据结构**

```mysql
show index from `class`;								# 显示class表中的索引信息
create index index_username on `class`(`name`);			# 给class表的name字段添加名为index_username的索引(普通索引)
```

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220828144856077.png" alt="image-20220828144856077"  />



#### 03 mysql读写锁

```mysql
lock table `user` read;		# 给user表上读锁 上锁后【所有用户】只能读user 不能写user
unlock tables;				# 释放锁

lock table `user` write;	# 给user表上写锁 上锁后【该用户】可读写user【其余用户】不可读写user
unlock tables;				# 释放锁
```

上读锁后，写请求被拒绝

![image-20220606000211071](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220606000211071.png)

