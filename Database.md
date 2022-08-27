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

**将多条查询语句的结果合并成一个结果**

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

- **应用场景**

  - 要查询的结果**来自于多个表**

  - 表之间**没有直接的连接关系**

  - 查询的信息严格一致









