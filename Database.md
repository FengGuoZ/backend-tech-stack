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

**事务指一组sql语句组成的执行单元**，这组语句要么都执行，要么都不执行

事务让一组sql语句构成逻辑上的整体

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



**事务的ACID属性**

- **原子性 Atomicity**
- **一致性 Consitency**
- **隔离性 Isolation**
- **持久性 Durability**



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

**mysql命令行**

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

#### 01 数据库三大范式🌼🌼

- **第一范式1NF**（first normal form）：**确保数据库表字段的原子性**
  - 要求**字段不可再分**
  - 字段 `localInfo:广东省10086'` ，依照第一范式必须拆分成 `localPos:广东省` +  `localTel:10086`两个字段

- **第二范式2NF**
  - 满足1NF
  - **表必须有1个主键**
  - 非主键列必须**完全依赖于主键**，不能只依赖于主键的一部分

- **第三范式3NF**
  - **满足1NF 2NF**
  - 非主键列**必须直接依赖于主键**，**不能存在传递依赖**
    - 即A依赖于B，B依赖于主键

| 范式 |                说明                 |
| :--: | :---------------------------------: |
|  1   |          保证字段的原子性           |
|  2   | 1+必须有主键+其余字段完全依赖于主键 |
|  3   |   2+必须直接依赖，不可以传递依赖    |



#### 02  E-R图

**实体-联系图**

- 是用于**标识实体类型、属性和联系的工具**
- 是对现实世界中事物关系的**抽象模型**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/4717673e36966e0e4b33fccfd753f6ea.png" alt="4717673e36966e0e4b33fccfd753f6ea" style="zoom: 80%;" />



#### 03 MySql表间关系

**1-n关系**，2张表可表示

- 国际分类下可以有很多文章

**n-n关系**，3张表可表示

- 学生可以选多门课，一门课下有多个学生
- **第3张表专门用于存储课与学生的对应关系**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220828143050078.png" alt="image-20220828143050078" style="zoom:80%;" />

#### 04 关系型数据库🌼🌼

**关系型数据库指建立在关系模型基础上的数据库**

- 关系模型指**数据以二维表格的形式存储**
  - redis采用的kv模型为非关系模型
- 关系型数据库中，**数据被存在在表格中**，每1行对应1条记录
- 大部分关系型数据库**使用SQL操作数据**、并支持事物的ACID特性
- 关系型数据库举例：MySQL、Oracle、SQL_Server、SQLite（微信使用）...



#### 05 MyISAM vs InnoDB

|             属性              |            MyISAM            |             InnoDB              |
| :---------------------------: | :--------------------------: | :-----------------------------: |
| **行级锁(row-level locking)** | 仅支持表级锁（并发性能极差） | 支持行级锁+表级锁（默认行级锁） |
|           **事务**            |            不支持            |              支持               |
|           **外键**            |            不支持            |              支持               |
|  **数据库异常崩溃后的恢复**   |            不支持            |   支持（依赖于redo log实现）    |

MySQL默认存储引擎为InnoDB

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220308015254622.png" alt="image-20220308015254622" style="zoom:80%;" />



#### 06 事务ACID属性&实现原理

- **原子性 Atomicity**：事务是不可分割的执行单位
  - 原子性保证事务中的所有语句，**要么一起执行，要么全不执行**
  - InnoDB使用**undo log(回滚日志)**保证事务的原子性
- **一致性 Consitency**：事务是**数据库从一致性状态A转化为一致性状态B**
  - 举例：转账前后，无论操作是否成功，**双方总金额不变**
  - 实现AID属性，可保障一致性
- **隔离性 Isolation**：并发访问数据库时，**A事务执行过程中不受其它事务影响**
  - 隔离级别包括`Read Uncommitted | Read Committed | Repeatable Read(Default in InnoDB) | SERIALIZABLE`
  - InnoDB使用**锁机制 | MVCC**保证事务的隔离性
- **持久性 Durability**：事务提交后，**改变是永久性的**，无论数据库是否发生故障
  - InnoDB使用**redo log(重做日志)**保证事务的持久性



#### 07 隔离级别与锁的对应关系

|     隔离级别     |                    加锁逻辑                    |
| :--------------: | :--------------------------------------------: |
| Read Uncommitted |                读操作时不加读锁                |
|  Read Committed  |      读操作时加读锁，语句执行完后立即释放      |
| Repeatable Read  |        读操作时加读锁，事务提交后才释放        |
|   SERIALIZABLE   | 读操作时对范围内所有键加读锁，事务提交后才释放 |



#### 08 mysql读写锁

**读锁(共享锁)**

- A上读锁后，B可以上读锁
- A上读锁后，B不可以上写锁

**写锁(独占锁/排他锁)**

- A上写锁后，B不可以上读/写锁

**读\写锁均属于悲观锁**

```mysql
lock table `user` read;		# 给user表上读锁 上锁后【所有用户】只能读user 不能写user
unlock tables;				# 释放锁

lock table `user` write;	# 给user表上写锁 上锁后【该用户】可读写user【其余用户】不可读写user
unlock tables;				# 释放锁
```

上读锁后，写请求被拒绝

![image-20220606000211071](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220606000211071.png)





#### 09 MySQL索引

- **索引(index)是一种辅助查询的数据结构**

- **索引常用数据结构**

  - B树

  - B+树

  - 哈希表：存在哈希碰撞问题，**且无法支持排序、分组**

- MyISAM、InnoDB引擎**均使用B+树作为索引结构**



#### 10 索引分类

- 主键索引primary，建立primary key时自动创建
- 唯一索引unique，建立unique key时自动创建
- 普通索引，手动添加
- 前缀索引，适用于字符串类型
- 全文索引

```mysql
create table class (
	id int(11) primary key auto_increment,	# 主键 自动创建索引
    name varchar(255),						
    email varchar(255) unique,				# 唯一键 自动创建索引
    score tinyint(4)
);

show index from `class`;								# 显示class表中的索引信息
create index index_username on `class`(`name`);			# 给class表的name字段添加名为index_username的普通索引
```

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220828220016416.png" alt="image-20220828220016416"  />



#### 11 索引的优缺点

**优点**

- 索引可以**极大地加快检索速度**，**尤其是group by 分组和order by排序时**

**缺点**

- **创建和维护索引有时间开销**，对数据进行**增删改时，执行速度变慢**
- 索引是磁盘上9的一种数据结构，**占用存储空间**



#### 12 intersect交集运算

**用于求解结果集的交集**，默认去重

- 结果集之间的列数据类型必须兼容
- 列数、顺序必须相同

```mysql
SELECT
    id
FROM
    a 
INTERSECT
SELECT
    id
FROM
    b;
```

![image-20220913212034890](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220913212034890.png)





#### 13 聚簇索引和辅助索引

**聚簇索引指数据与索引(index)存储在同一位置**

- 查找时，找到聚簇索引等价于找到行数据
- **一般情况下将主键索引设置为聚簇索引**
  - 没有主键时，InnoDB会使用第1个唯一且非空索引作为聚簇索引
  - 否则，InnoDB生成一个名为GEN_CLUST_INDEX的隐藏聚簇索引


**辅助索引(非聚簇索引)指数据与索引分开存储**

- 查找时，找到辅助索引后得到对应主键值，然后回表二次查询得到行数据
- **非聚簇索引比聚簇索引多了一次查询操作**，性能较低

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220919191050411.png" alt="image-20220919191050411"  />



#### 14 key Vs index

**MySQL中的key包含constraint和index两重含义**

-  **primary key**
  - 唯一约束和非空约束
  - 同时在此key上建立了index

- **unique key**
  - 唯一约束
  - 同时在此key上建立了index

**index是纯粹的索引，一种辅助查询的数据结构**



#### 15 delete truncate drop🌼🌼

**delete**：**用于删除指定数据or清空表格**

- 语法：**`delete from 表名 【where 筛选条件】`**
- 清空表格是，不会将自增长列重置
- **可以回滚**

**truncate**：**用于清空表格**

- 语法：**`truncate table 名称`**
- **清空表数据**，**自增长列重置**
- **不可回滚**

**drop**：**删除操作**，属于DDL语言

- 语法：**`drop table|database 名称`**
- **将表数据+表结构一并删除**
- **不可回滚**

|              |                语法                 |              特点              |  rollback  |
| :----------: | :---------------------------------: | :----------------------------: | :--------: |
|  **delete**  | delete from 表名 【where 筛选条件】 | 清空表格是，不会将自增长列重置 |  支持回滚  |
| **truncate** |        truncate table 表名;         |    清空表数据，自增长列重置    | 不支持回滚 |
|   **drop**   |          drop table 表名;           |    将表数据+表结构一并删除     | 不支持回滚 |



#### 16 B树 & B+树

B树/B+树都属于**多路平衡查找树**，Innodb/MyISAM存储引擎均采用B+树建立索引

**B树性质**

- 非叶子节点也可以保存数据/数据指针
- 叶子节点之间不相连

**B+树性质**

- B+树是多叉树，非叶子节点仅保存了n个关键字，不保存数据/数据指针
- 叶子节点保存了全部关键字信息，及其对应的数据/数据指针
- B+树的**叶子节点由一条链表相连**，**能支持高效的顺序检索**

![image-20220718170452180](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220718170452180.png)



#### 17 B+树 vs B树

B+树比B树更适合做数据库库索引，Innodb/MyISAM存储引擎均采用B+树建立索引

- **B+树磁盘IO代价更低**
  - B树非叶子节点也存储数据，查找时B+树单次IO读取的关键字到内存的数量大于B树
  - B+树查询的IO次数少于B树
- **B+树查询效率更稳定**
  - B+树只有叶子节点存储数据，任意关键字查询的路径长度相同
- **B+树便于范围查询**
  - B+树叶子节点之间由链表连接，范围查找时，找到下界后顺序遍历即可



**磁盘IO补充**

> 一次内存访问时间约50ns
>
> 一次磁盘访问时间约10ms
>
> B+树节点存储在页单位中，页由n个block组成
>
> 一次磁盘IO读取1页





## Redis基础

### 1 Redis简介

#### 1.1 技术发展

**web1.0** 访问量有限，单体服务器扛全部需求

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220702150334737.png" alt="image-20220702150334737" style="zoom:50%;" />

**web2.0** 移动互联网时代，客户端QPS激增，产生高并发需求

- 分布式服务器 + 负载均衡，由Nginx反向代理，分配访问请求

- Redis扛读QPS

- 专用的文件、视频服务器加速访问

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220702151546385.png" alt="image-20220702151546385" style="zoom:50%;" />



#### 1.2 NoSQL & Redis

**Not only SQL**，泛指非关系型数据库，**基于kv模型存储数据**

- SQL数据库侧重于业务逻辑，根据业务需求定制表格结构

- 缓存型数据库侧重于效率，随存随用，满足高并发需求



 **Remote Dictionary Server 远程字典服务**

- **开源标准C编写的kv数据库，单线程+IO复用技术实现**，提供多种语言API
- 内存数据库，**数据存储在内存中**，读写速度极快，QPS>100k，**广泛应用于缓存**
- 支持持久化，RDB| AOF
- 支持主从同步master-slaver



#### 1.3 安装与启动

- [Install Redis on Linux](https://redis.io/docs/getting-started/installation/install-redis-on-linux/)

- 安装目录 -- /usr/bin

```bash
$ redis-server	# 启动服务器
$ shutdown		# 关闭服务器
$ redis-cli		# 启动客户端
$ exit			# 退出客户端
```

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220702155534155.png" alt="image-20220702155534155" style="zoom: 67%;" />

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220705161023423.png" alt="image-20220705161023423" style="zoom:67%;" />



### 2 数据类型

#### 2.1 redis key

**key默认string类型**

key操作

```bash
keys *		# 显示所有key
exists key
type key	# 显示key对应value的数据类型
del key
expire key	# 设置key过期时间，-1永不过期
```



#### 2.2 string字符串

- redis基本数据类型

- 可以包含任何二进制数据

- 最大512MB



**string基本操作**

```bash
set k1 v1
mset k1 v1 k2 v2
get k1
mget k1 k2
setnx k1 vx
```



**string底层为动态字符数组**，扩容方式

- 1M以下cap倍增

- 1M以上每次扩1M

- 最大512M

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220702164539102.png" alt="image-20220702164539102" style="zoom:50%;" />



#### 2.3 list列表

- key对应一个链表，链表中有多个element

- element被pop空时，key自动消亡

- **底层为双向链表**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220702164920430.png" alt="image-20220702164920430" style="zoom:50%;" />



**list基本操作**

```bash
lpush key element [element ...]	# 链表左插元素 第一次插入即创建list
lpop key count					# 链表左弹count个元素
lrange key start stop			# 显示链表左侧[start, stop]范围内元素
```

![image-20220829100730763](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220829100730763.png)





#### 2.4 set集合

- key对应一个集合，集合中包含多个member，且自动去重

- **底层为hash表**，增删改查O(1)



**set基本操作**

```bash
sadd key member [member ...]	# 向set中插入元素 第一次插入即创建set
smembers key 					# 显示set当前所有元素
sismember key member			# 判断member是否在set中
spop key						# 随机弹出一个set元素
```

![image-20220829101513998](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220829101513998.png)



#### 2.5 hash哈希类型

- key对应一个field-value类型的map，map中包含多对元素

- **底层为hashtable**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220702171457757.png" alt="image-20220702171457757" style="zoom: 67%;" />

```bash
hset key field value [field value ...]	# 向hash中插入field-value对 第一次插入即创建hash
hgetall key								# 展示hash中所有field-value对
hget key field							# 查询hash中field字段对应value
```

![image-20220829102927157](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220829102927157.png)



#### 2.6 zset有序表

- key对应一个有序表，有序表中含多对member-score
- score用于zset对key下所有member排序，**默认升序**
- **底层为hash表 + 跳表**
  - hash表用于存储member-score对
  - 跳表用于对score排序

![image-20220829103532342](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220829103532342.png)

```bash
zadd key score member [score member ...]	# 向zset中插入sore-member对 第一次插入即创建zset
zrange key min max							# 将排名[min, max]的member取出 [0, -1]表示所有
zrangebyscore key min max					# 将得分[min, max]区间的member取出
zrank key member							# 显示menber对应排名 排名从0开始计数
zincrby key increment member				# member得分加increment
```

![image-20220829105306256](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220829105306256.png)





### 3 redis的发布与订阅

- 客户端A发布channel1
- 客户端B订阅channel1后，可以收到客户端A在channel中发送的所有消息

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220829110026069.png" alt="image-20220829110026069" style="zoom:50%;" />

![image-20220829110330469](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220829110330469.png)







## Redis面试八股

#### 01 RDB持久化

redis每隔一段时间，**将内存中的数据集快照**写入磁盘中的**dump.rdb文件中**

- redis重启时，将磁盘中的dump.rdb内容恢复

- redis fork子进程完成RDB操作，主进程不参与磁盘IO，确保高性能

- RDB优点是大数据集恢复时，速度更快

- RDB缺点是会丢失最后1次持久化后的内容，无法保证数据完整性

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220705105634179.png" alt="image-20220705105634179" style="zoom:50%;" />

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220705105800212.png" alt="image-20220705105800212" style="zoom:50%;" />

#### 02 AOF持久化

**append only file**

- 以日志的形式记录写操作记录，存储在磁盘appendonly.aof文件中

- 只可以追加，不允许修改

- redis重启时按顺序执行AOF文件，以恢复之前的状态

- AOF文件损坏时，可用redis-check-aof--fix appendonly.aof进行修复

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220705152216280.png" alt="image-20220705152216280" style="zoom:50%;" />



**AOF可以设置同步频率**

- appendfsync always -- 每次写操作都会触发同步，牺牲性能保证数据完整性
- appendfsync everysec -- 秒级同步
- appendfsync no -- redis不主动同步



**Rewrite压缩**

redis提供重写机制，**可将AOF文件压缩为最小指令集**，目的是避免AOF文件过大



#### 03 RDB Vs AOF

- RDB是redis数据快照，侧重于效率，不保证数据完整性
- AOF记录写操作，能保证数据完整性，同时可读性较好，可以处理误操作
- AOF文件体积大于RDB文件，可以通过重写节省空间。



#### 04 缓存穿透

 **key对应的数据不存在** → 缓存查不到 → 直接访问数据库

- 黑客有意地**高并发访问不存在的key**
- 可以穿透redis，把MySQL打挂

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220702211549036.png" alt="image-20220702211549036" style="zoom: 67%;" />



**解决方案**

- **请求参数校验** -- 过滤非法请求
- **对空值缓存** -- 可临时缓解MySQL压力
- **布隆过滤器判空** -- 过滤明显不存在的数据
- 设置白名单 -- 拦截不明来源的请求



#### 05 缓存击穿

**热key突然出现或过期**，导致瞬时访问量过大，把DB打挂

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220703133500764.png" alt="image-20220703133500764" style="zoom: 67%;" />

**解决方案**

- 实时延长热key过期时间

- 预热数据，在热点事件带来高并发访问前，加载数据至redis备用



#### 06 缓存雪崩

key过期时间设置不合理，导致**大量key在同一时间过期**，瞬时高并发把DB打卦

缓存雪崩对系统危害性极大

![image-20220829111510592](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220829111510592.png)

**解决方案**

- 构建多级缓存架构，nginx + redis + 其它缓存

- 分散key过期时间，在基础过期时间上增加一个随机值，避免集中过期

