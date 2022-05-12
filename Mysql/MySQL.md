# 基础篇

## 通用语法及分类

- DDL: 数据定义语言，用来定义数据库对象（数据库、表、字段）
- DML: 数据操作语言，用来对数据库表中的数据进行增删改
- DQL: 数据查询语言，用来查询数据库中表的记录
- DCL: 数据控制语言，用来创建数据库用户、控制数据库的控制权限

### DDL（数据定义语言）

数据定义语言

#### 数据库操作

查询所有数据库：
`SHOW DATABASES;`
查询当前数据库：
`SELECT DATABASE();`
创建数据库：
`CREATE DATABASE [ IF NOT EXISTS ] 数据库名 [ DEFAULT CHARSET 字符集] [COLLATE 排序规则 ];`
删除数据库：
`DROP DATABASE [ IF EXISTS ] 数据库名;`
使用数据库：
`USE 数据库名;`

##### 注意事项

- UTF8字符集长度为3字节，有些符号占4字节，所以推荐用utf8mb4字符集

#### 表操作

查询当前数据库所有表：
`SHOW TABLES;`
查询表结构：
`DESC 表名;`
查询指定表的建表语句：
`SHOW CREATE TABLE 表名;`

创建表：
```mysql
CREATE TABLE 表名(
	字段1 字段1类型 [COMMENT 字段1注释],
	字段2 字段2类型 [COMMENT 字段2注释],
	字段3 字段3类型 [COMMENT 字段3注释],
	...
	字段n 字段n类型 [COMMENT 字段n注释]
)[ COMMENT 表注释 ];
```
**最后一个字段后面没有逗号**

添加字段：
`ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];`
例：`ALTER TABLE emp ADD nickname varchar(20) COMMENT '昵称';`

修改数据类型：
`ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);`
修改字段名和字段类型：
`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];`
例：将emp表的nickname字段修改为username，类型为varchar(30)
`ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT '昵称';`

删除字段：
`ALTER TABLE 表名 DROP 字段名;`

修改表名：
`ALTER TABLE 表名 RENAME TO 新表名`

删除表：
`DROP TABLE [IF EXISTS] 表名;`
删除表，并重新创建该表：
`TRUNCATE TABLE 表名;`

### DML（数据操作语言）

#### 添加数据

指定字段：
`INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);`
全部字段：
`INSERT INTO 表名 VALUES (值1, 值2, ...);`

批量添加数据：
`INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);`
`INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);`

##### 注意事项

- 字符串和日期类型数据应该包含在引号中
- 插入的数据大小应该在字段的规定范围内

#### 更新和删除数据

修改数据：
`UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ... [ WHERE 条件 ];`
例：
`UPDATE emp SET name = 'Jack' WHERE id = 1;`

删除数据：
`DELETE FROM 表名 [ WHERE 条件 ];`

### DQL（数据查询语言）

语法：
```mysql
SELECT
	字段列表
FROM
	表名字段
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后的条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

#### 基础查询

查询多个字段：
`SELECT 字段1, 字段2, 字段3, ... FROM 表名;`
`SELECT * FROM 表名;`

设置别名：
`SELECT 字段1 [ AS 别名1 ], 字段2 [ AS 别名2 ], 字段3 [ AS 别名3 ], ... FROM 表名;`
`SELECT 字段1 [ 别名1 ], 字段2 [ 别名2 ], 字段3 [ 别名3 ], ... FROM 表名;`

去除重复记录：
`SELECT DISTINCT 字段列表 FROM 表名;`

转义：
`SELECT * FROM 表名 WHERE name LIKE '/_张三' ESCAPE '/'`
/ 之后的\_不作为通配符

#### 条件查询

语法：
`SELECT 字段列表 FROM 表名 WHERE 条件列表;`

条件：

| 比较运算符          | 功能                                        |
| ------------------- | ------------------------------------------- |
| >                   | 大于                                        |
| >=                  | 大于等于                                    |
| <                   | 小于                                        |
| <=                  | 小于等于                                    |
| =                   | 等于                                        |
| <> 或 !=            | 不等于                                      |
| BETWEEN ... AND ... | 在某个范围内（含最小、最大值）              |
| IN(...)             | 在in之后的列表中的值，多选一                |
| LIKE 占位符         | 模糊匹配（\_匹配单个字符，%匹配任意个字符） |
| IS NULL             | 是NULL                                      |

| 逻辑运算符         | 功能                         |
| ------------------ | ---------------------------- |
| AND 或 &&          | 并且（多个条件同时成立）     |
| OR 或 &#124;&#124; | 或者（多个条件任意一个成立） |
| NOT 或 !           | 非，不是                     |

例子：
```mysql
-- 年龄等于30
select * from employee where age = 30;
-- 年龄小于30
select * from employee where age < 30;
-- 小于等于
select * from employee where age <= 30;
-- 没有身份证
select * from employee where idcard is null or idcard = '';
-- 有身份证
select * from employee where idcard;
select * from employee where idcard is not null;
-- 不等于
select * from employee where age != 30;
-- 年龄在20到30之间
select * from employee where age between 20 and 30;
select * from employee where age >= 20 and age <= 30;
-- 下面语句不报错，但查不到任何信息
select * from employee where age between 30 and 20;
-- 性别为女且年龄小于30
select * from employee where age < 30 and gender = '女';
-- 年龄等于25或30或35
select * from employee where age = 25 or age = 30 or age = 35;
select * from employee where age in (25, 30, 35);
-- 姓名为两个字
select * from employee where name like '__';
-- 身份证最后为X
select * from employee where idcard like '%X';
```

#### 聚合查询（聚合函数）

常见聚合函数：

| 函数  | 功能     |
| ----- | -------- |
| count | 统计数量 |
| max   | 最大值   |
| min   | 最小值   |
| avg   | 平均值   |
| sum   | 求和     |

语法：
`SELECT 聚合函数(字段列表) FROM 表名;`
例：
`SELECT count(id) from employee where workaddress = "广东省";`

#### 分组查询

语法：
`SELECT 字段列表 FROM 表名 [ WHERE 条件 ] GROUP BY 分组字段名 [ HAVING 分组后的过滤条件 ];`

where 和 having 的区别：

- 执行时机不同：where是分组之前进行过滤，不满足where条件不参与分组；having是分组后对结果进行过滤。
- 判断条件不同：where不能对聚合函数进行判断，而having可以。

例子：

```mysql
-- 根据性别分组，统计男性和女性数量（只显示分组数量，不显示哪个是男哪个是女）
select count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性数量
select gender, count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性的平均年龄
select gender, avg(age) from employee group by gender;
-- 年龄小于45，并根据工作地址分组
select workaddress, count(*) from employee where age < 45 group by workaddress;
-- 年龄小于45，并根据工作地址分组，获取员工数量大于等于3的工作地址
select workaddress, count(*) address_count from employee where age < 45 group by workaddress having address_count >= 3;
```

##### 注意事项

- 执行顺序：where > 聚合函数 > having
- 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义

#### 排序查询

语法：
`SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1, 字段2 排序方式2;`

排序方式：

- ASC: 升序（默认）
- DESC: 降序

例子：

```mysql
-- 根据年龄升序排序
SELECT * FROM employee ORDER BY age ASC;
SELECT * FROM employee ORDER BY age;
-- 两字段排序，根据年龄升序排序，入职时间降序排序
SELECT * FROM employee ORDER BY age ASC, entrydate DESC;
```

##### 注意事项

如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序

#### 分页查询

语法：
`SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;`

例子：

```mysql
-- 查询第一页数据，展示10条
SELECT * FROM employee LIMIT 0, 10;
-- 查询第二页
SELECT * FROM employee LIMIT 10, 10;
```

##### 注意事项

- 起始索引从0开始，起始索引 = （查询页码 - 1） * 每页显示记录数
- 分页查询是数据库的方言，不同数据库有不同实现，MySQL是LIMIT
- 如果查询的是第一页数据，起始索引可以省略，直接简写 LIMIT 10

#### DQL执行顺序

FROM -> WHERE -> GROUP BY -> SELECT -> ORDER BY -> LIMIT

### DCL

#### 管理用户

查询用户：

```mysql
USER mysql;
SELECT * FROM user;
```

创建用户:
`CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';`

修改用户密码：
`ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';`

删除用户：
`DROP USER '用户名'@'主机名';`

例子：

```mysql
-- 创建用户test，只能在当前主机localhost访问
create user 'test'@'localhost' identified by '123456';
-- 创建用户test，能在任意主机访问
create user 'test'@'%' identified by '123456';
create user 'test' identified by '123456';
-- 修改密码
alter user 'test'@'localhost' identified with mysql_native_password by '1234';
-- 删除用户
drop user 'test'@'localhost';
```

##### 注意事项

- 主机名可以使用 % 通配

#### 权限控制

常用权限：

| 权限                | 说明               |
| ------------------- | ------------------ |
| ALL, ALL PRIVILEGES | 所有权限           |
| SELECT              | 查询数据           |
| INSERT              | 插入数据           |
| UPDATE              | 修改数据           |
| DELETE              | 删除数据           |
| ALTER               | 修改表             |
| DROP                | 删除数据库/表/视图 |
| CREATE              | 创建数据库/表      |

更多权限请看[权限一览表](#权限一览表 "权限一览表")

查询权限：
`SHOW GRANTS FOR '用户名'@'主机名';`

授予权限：
`GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';`

撤销权限：
`REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';`

##### 注意事项

- 多个权限用逗号分隔
- 授权时，数据库名和表名可以用 * 进行通配，代表所有

## 函数

- 字符串函数
- 数值函数
- 日期函数
- 流程函数

### 字符串函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CONCAT(s1, s2, ..., sn)  | 字符串拼接，将s1, s2, ..., sn拼接成一个字符串  |
| LOWER(str)  | 将字符串全部转为小写  |
| UPPER(str)  | 将字符串全部转为大写  |
| LPAD(str, n, pad)  | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度  |
| RPAD(str, n, pad)  | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度  |
| TRIM(str)  | 去掉字符串头部和尾部的空格  |
| SUBSTRING(str, start, len)  | 返回从字符串str从start位置起的len个长度的字符串  |

使用示例：

```mysql
-- 拼接
SELECT CONCAT('Hello', 'World');
-- 小写
SELECT LOWER('Hello');
-- 大写
SELECT UPPER('Hello');
-- 左填充
SELECT LPAD('01', 5, '-');
-- 右填充
SELECT RPAD('01', 5, '-');
-- 去除空格
SELECT TRIM(' Hello World ');
-- 切片（起始索引为1）
SELECT SUBSTRING('Hello World', 1, 5);
```

### 数值函数

常见函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CEIL(x)  | 向上取整  |
| FLOOR(x)  | 向下取整  |
| MOD(x, y)  | 返回x/y的模  |
| RAND() | 返回0~1内的随机数 |
| ROUND(x, y) | 求参数x的四舍五入值，保留y位小数 |

### 日期函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| CURDATE()  | 返回当前日期  |
| CURTIME()  | 返回当前时间  |
| NOW()  | 返回当前日期和时间  |
| YEAR(date)  | 获取指定date的年份  |
| MONTH(date)  | 获取指定date的月份  |
| DAY(date)  | 获取指定date的日期  |
| DATE_ADD(date, INTERVAL expr type)  | 返回一个日期/时间值加上一个时间间隔expr后的时间值  |
| DATEDIFF(date1, date2)  | 返回起始时间date1和结束时间date2之间的天数  |

例子：

```mysql
-- DATE_ADD
SELECT DATE_ADD(NOW(), INTERVAL 70 YEAR);
```

### 流程函数

常用函数：

| 函数  | 功能  |
| ------------ | ------------ |
| IF(value, t, f)  | 如果value为true，则返回t，否则返回f  |
| IFNULL(value1, value2)  | 如果value1不为空，返回value1，否则返回value2  |
| CASE WHEN [ val1 ] THEN [ res1 ] ... ELSE [ default ] END  | 如果val1为true，返回res1，... 否则返回default默认值  |
| CASE [ expr ] WHEN [ val1 ] THEN [ res1 ] ... ELSE [ default ] END  | 如果expr的值等于val1，返回res1，... 否则返回default默认值  |

例子：

```mysql
select
	name,
	(case when age > 30 then '中年' else '青年' end)
from employee;
select
	name,
	(case workaddress when '北京市' then '一线城市' when '上海市' then '一线城市' else '二线城市' end) as '工作地址'
from employee;
```

## 约束

分类：

| 约束  | 描述  | 关键字  |
| ------------ | ------------ | ------------ |
| 非空约束  | 限制该字段的数据不能为null  | NOT NULL  |
| 唯一约束  | 保证该字段的所有数据都是唯一、不重复的  | UNIQUE  |
| 主键约束  | 主键是一行数据的唯一标识，要求非空且唯一  | PRIMARY KEY  |
| 默认约束  | 保存数据时，如果未指定该字段的值，则采用默认值  | DEFAULT  |
| 检查约束（8.0.1版本后）  | 保证字段值满足某一个条件  | CHECK  |
| 外键约束  | 用来让两张图的数据之间建立连接，保证数据的一致性和完整性  | FOREIGN KEY  |

约束是作用于表中字段上的，可以再创建表/修改表的时候添加约束。

### 常用约束

| 约束条件  | 关键字  |
| ------------ | ------------ |
| 主键  | PRIMARY KEY  |
| 自动增长  | AUTO_INCREMENT  |
| 不为空  | NOT NULL  |
| 唯一  | UNIQUE  |
| 逻辑条件  | CHECK  |
| 默认值  | DEFAULT  |

例子：

```mysql
create table user(
	id int primary key auto_increment,
	name varchar(10) not null unique,
	age int check(age > 0 and age < 120),
	status char(1) default '1',
	gender char(1)
);
```

### 外键约束

添加外键：

```mysql
CREATE TABLE 表名(
	字段名 字段类型,
	...
	[CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
);
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);

-- 例子
alter table emp add constraint fk_emp_dept_id foreign key(dept_id) references dept(id);
```

删除外键：
`ALTER TABLE 表名 DROP FOREIGN KEY 外键名;`

#### 删除/更新行为

| 行为  | 说明  |
| ------------ | ------------ |
| NO ACTION  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与RESTRICT一致）  |
| RESTRICT  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与NO ACTION一致）  |
| CASCADE  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录  |
| SET NULL  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（要求该外键允许为null）  |
| SET DEFAULT  | 父表有变更时，子表将外键设为一个默认值（Innodb不支持）  |

更改删除/更新行为：
`ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE 行为 ON DELETE 行为;`

## 多表查询

### 多表关系

- 一对多（多对一）
- 多对多
- 一对一

#### 一对多

案例：部门与员工
关系：一个部门对应多个员工，一个员工对应一个部门
实现：在多的一方建立外键，指向一的一方的主键

#### 多对多

案例：学生与课程
关系：一个学生可以选多门课程，一门课程也可以供多个学生选修
实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

#### 一对一

案例：用户与用户详情
关系：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率
实现：在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的（UNIQUE）

### 查询

合并查询（笛卡尔积，会展示所有组合结果）：
`select * from employee, dept;`

> 笛卡尔积：两个集合A集合和B集合的所有组合情况（在多表查询时，需要消除无效的笛卡尔积）

消除无效笛卡尔积：
`select * from employee, dept where employee.dept = dept.id;`

### 内连接查询

内连接查询的是两张表交集的部分

隐式内连接：
`SELECT 字段列表 FROM 表1, 表2 WHERE 条件 ...;`

显式内连接：
`SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ...;`

显式性能比隐式高

例子：

```mysql
-- 查询员工姓名，及关联的部门的名称
-- 隐式
select e.name, d.name from employee as e, dept as d where e.dept = d.id;
-- 显式
select e.name, d.name from employee as e inner join dept as d on e.dept = d.id;
```

### 外连接查询

左外连接：
查询左表所有数据，以及两张表交集部分数据
`SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ...;`
相当于查询表1的所有数据，包含表1和表2交集部分数据

右外连接：
查询右表所有数据，以及两张表交集部分数据
`SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ...;`

例子：

```mysql
-- 左
select e.*, d.name from employee as e left outer join dept as d on e.dept = d.id;
select d.name, e.* from dept d left outer join emp e on e.dept = d.id;  -- 这条语句与下面的语句效果一样
-- 右
select d.name, e.* from employee as e right outer join dept as d on e.dept = d.id;
```

左连接可以查询到没有dept的employee，右连接可以查询到没有employee的dept

### 自连接查询

当前表与自身的连接查询，自连接必须使用表别名

语法：
`SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ...;`

自连接查询，可以是内连接查询，也可以是外连接查询

例子：

```mysql
-- 查询员工及其所属领导的名字
select a.name, b.name from employee a, employee b where a.manager = b.id;
-- 没有领导的也查询出来
select a.name, b.name from employee a left join employee b on a.manager = b.id;
```

### 联合查询 union, union all

把多次查询的结果合并，形成一个新的查询集

语法：

```mysql
SELECT 字段列表 FROM 表A ...
UNION [ALL]
SELECT 字段列表 FROM 表B ...
```

#### 注意事项

- UNION ALL 会有重复结果，UNION 不会
- 联合查询比使用or效率高，不会使索引失效

### 子查询

SQL语句中嵌套SELECT语句，称谓嵌套查询，又称子查询。
`SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2);`
**子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个**

根据子查询结果可以分为：

- 标量子查询（子查询结果为单个值）
- 列子查询（子查询结果为一列）
- 行子查询（子查询结果为一行）
- 表子查询（子查询结果为多行多列）

根据子查询位置可分为：

- WHERE 之后
- FROM 之后
- SELECT 之后

#### 标量子查询

子查询返回的结果是单个值（数字、字符串、日期等）。
常用操作符：- < > > >= < <=

例子：

```mysql
-- 查询销售部所有员工
select id from dept where name = '销售部';
-- 根据销售部部门ID，查询员工信息
select * from employee where dept = 4;
-- 合并（子查询）
select * from employee where dept = (select id from dept where name = '销售部');

-- 查询xxx入职之后的员工信息
select * from employee where entrydate > (select entrydate from employee where name = 'xxx');
```

#### 列子查询

返回的结果是一列（可以是多行）。

常用操作符：

| 操作符  | 描述  |
| ------------ | ------------ |
| IN  | 在指定的集合范围内，多选一  |
| NOT IN  | 不在指定的集合范围内  |
| ANY  | 子查询返回列表中，有任意一个满足即可  |
| SOME  | 与ANY等同，使用SOME的地方都可以使用ANY  |
| ALL  | 子查询返回列表的所有值都必须满足  |

例子：

```mysql
-- 查询销售部和市场部的所有员工信息
select * from employee where dept in (select id from dept where name = '销售部' or name = '市场部');
-- 查询比财务部所有人工资都高的员工信息
select * from employee where salary > all(select salary from employee where dept = (select id from dept where name = '财务部'));
-- 查询比研发部任意一人工资高的员工信息
select * from employee where salary > any (select salary from employee where dept = (select id from dept where name = '研发部'));
```

#### 行子查询

返回的结果是一行（可以是多列）。
常用操作符：=, <, >, IN, NOT IN

例子：

```mysql
-- 查询与xxx的薪资及直属领导相同的员工信息
select * from employee where (salary, manager) = (12500, 1);
select * from employee where (salary, manager) = (select salary, manager from employee where name = 'xxx');
```

#### 表子查询

返回的结果是多行多列
常用操作符：IN

例子：

```mysql
-- 查询与xxx1，xxx2的职位和薪资相同的员工
select * from employee where (job, salary) in (select job, salary from employee where name = 'xxx1' or name = 'xxx2');
-- 查询入职日期是2006-01-01之后的员工，及其部门信息
select e.*, d.* from (select * from employee where entrydate > '2006-01-01') as e left join dept as d on e.dept = d.id;
```

## 事务

事务是一组操作的集合，事务会把所有操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

基本操作：

```mysql
-- 1. 查询张三账户余额
select * from account where name = '张三';
-- 2. 将张三账户余额-1000
update account set money = money - 1000 where name = '张三';
-- 此语句出错后张三钱减少但是李四钱没有增加
模拟sql语句错误
-- 3. 将李四账户余额+1000
update account set money = money + 1000 where name = '李四';

-- 查看事务提交方式
SELECT @@AUTOCOMMIT;
-- 设置事务提交方式，1为自动提交，0为手动提交，该设置只对当前会话有效
SET @@AUTOCOMMIT = 0;
-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK;

-- 设置手动提交后上面代码改为：
select * from account where name = '张三';
update account set money = money - 1000 where name = '张三';
update account set money = money + 1000 where name = '李四';
commit;
```

操作方式二：

开启事务：
`START TRANSACTION 或 BEGIN TRANSACTION;`
提交事务：
`COMMIT;`
回滚事务：
`ROLLBACK;`

操作实例：

```mysql
start transaction;
select * from account where name = '张三';
update account set money = money - 1000 where name = '张三';
update account set money = money + 1000 where name = '李四';
commit;
```

### 四大特性ACID

- 原子性(Atomicity)：事务是不可分割的最小操作但愿，要么全部成功，要么全部失败
- 一致性(Consistency)：事务完成时，必须使所有数据都保持一致状态
- 隔离性(Isolation)：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性(Durability)：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

### 并发事务

| 问题  | 描述  |
| ------------ | ------------ |
| 脏读  | 一个事务读到另一个事务还没提交的数据  |
| 不可重复读  | 一个事务先后读取同一条记录，但两次读取的数据不同  |
| 幻读  | 一个事务按照条件查询数据时，没有对应的数据行，但是再插入数据时，又发现这行数据已经存在  |

> 这三个问题的详细演示：https://www.bilibili.com/video/BV1Kr4y1i7ru?p=55cd 

并发事务隔离级别：

| 隔离级别  | 脏读  | 不可重复读  | 幻读  |
| ------------ | ------------ | ------------ | ------------ |
| Read uncommitted  | √  | √  | √  |
| Read committed  | ×  | √  | √  |
| Repeatable Read(默认)  | ×  | ×  | √  |
| Serializable  | ×  | ×  | ×  |

- √表示在当前隔离级别下该问题会出现
- Serializable 性能最低；Read uncommitted 性能最高，数据安全性最差

查看事务隔离级别：
`SELECT @@TRANSACTION_ISOLATION;`
设置事务隔离级别：
`SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE };`
SESSION 是会话级别，表示只针对当前会话有效，GLOBAL 表示对所有会话有效

# 进阶篇

## 存储引擎

MySQL体系结构：

![image-20220430223031264](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204302230408.png)

![image-20220430224329103](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204302243390.png)

==存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式==。存储引擎是基于表而不是基于库的，所以存储引擎也可以被称为表引擎（`一个数据库的多张表能使用不同的存储引擎`）。
默认存储引擎是InnoDB。

相关操作：

```mysql
-- 查询建表语句
show create table account;
-- 建表时指定存储引擎
CREATE TABLE 表名(
	...
) ENGINE=INNODB;  #不写的话默认也是INNODB  show create table 表名查看就会帮我们自动补上
-- 查看当前数据库支持的存储引擎
show engines;
```

![image-20220430225248987](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204302252079.png)

### InnoDB

InnoDB 是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB 是默认的 MySQL 引擎。

特点：

- DML（`数据的curd语句`） 操作遵循 ACID 模型，支持**事务**
- **行级锁**，提高并发访问性能
- 支持**外键**约束，保证数据的完整性和正确性

文件：

- xxx.ibd: xxx代表表名，InnoDB 引擎的每张表都会对应这样一个表空间文件，存储该表的表结构、数据和索引。(`8版本`)

![image-20220505201620684](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052016035.png)

==在8之前InnoDB存储表的结构和数据时，分别存储在frm文件和ibd文件。而在8的时候全都存到了ibd文件里==

- ![image-20220430231248287](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204302312376.png)

![image-20220430231521920](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204302315997.png)

参数：innodb_file_per_table，决定多张表共享一个表空间还是每张表对应一个表空间 ==默认是on代表每张表对应一个表空间==

知识点：

查看 Mysql 变量：
`show variables like 'innodb_file_per_table';`

![image-20220430225847593](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204302258659.png)

从idb文件提取表结构数据：
（在cmd运行）
`ibd2sdi xxx.ibd`

InnoDB 逻辑存储结构：

![image-20220430232245219](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204302322341.png)

### MyISAM

MyISAM 是 MySQL 早期的默认存储引擎。

特点：

- 不支持事务，不支持外键
- 支持表锁，不支持行锁
- 访问速度快

文件：

- xxx.sdi: 存储表结构信息
- xxx.MYD: 存储数据
- xxx.MYI: 存储索引

### Memory

Memory 引擎的表数据是存储在内存中的，受硬件问题、断电问题的影响，只能将这些表作为临时表或缓存使用。

特点：

- 存放在内存中，速度快
- hash索引（默认）

文件：

- xxx.sdi: 存储表结构信息

### 存储引擎特点

| 特点  | InnoDB  | MyISAM  | Memory  |
| ------------ | ------------ | ------------ | ------------ |
| 存储限制  | 64TB  | 有  | 有  |
| 事务安全  | `支持` | -  | -  |
| 锁机制  | `行锁` | 表锁  | 表锁  |
| B+tree索引  | 支持  | 支持  | 支持  |
| Hash索引  | -  | -  | 支持  |
| 全文索引  | 支持（5.6版本之后）  | 支持  | -  |
| 空间使用  | 高  | 低  | N/A  |
| 内存使用  | 高  | 低  | 中等  |
| 批量插入速度  | 低  | 高  | 高  |
| 支持外键  | `支持` | -  | -  |

### 存储引擎的选择

在选择存储引擎时，应该==根据应用系统的特点选择合适的存储引擎==。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。

- InnoDB: 如果应用对事物的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，`还包含很多的更新、删除操作`，则 InnoDB 是比较合适的选择
- MyISAM: 如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不高，那这个存储引擎是非常合适的。(日志)
- Memory: 将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。Memory 的缺陷是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性

电商中的日志和评论适合使用 MyISAM 引擎，缓存适合使用 Memory 引擎。==一般都不用下面这两个了。适合这些的应用都用nosql数据库代替了==

## 性能分析

### 查看执行频次

查看当前数据库的 INSERT, UPDATE, DELETE, SELECT 访问频次：
`SHOW GLOBAL STATUS LIKE 'Com_______';` 或者 `SHOW SESSION STATUS LIKE 'Com_______';`
例：`show global status like 'Com_______'`

![image-20220501123246787](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011232891.png)

我们执行SELECT * FROM `user` 后，就会发现Com_select的`Value` 145;   

### 慢查询日志

慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志。
MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件（/etc/my.cnf）中配置如下信息：

```mysql
在下面那幅图对应的位置配置就好了
# 开启慢查询日志开关
slow_query_log=1
# 设置慢查询日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
long_query_time=2
```

更改后记得重启MySQL服务，日志文件位置：`/www/server/data/mysql-slow.log`

![image-20220501131632164](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011316279.png)

查看慢查询日志开关状态：
`show variables like 'slow_query_log';`    ==里面记录了慢查询的sql语句及相关信息==

### profile

show profile 能在做SQL优化时帮我们了解时间都耗费在哪里。通过 have_profiling 参数，能看到当前 MySQL 是否支持 profile 操作：
`SELECT @@have_profiling;`

profiling 默认关闭，可以通过set语句在session/global级别开启 profiling：
`SET profiling = 1;`

查看所有语句的耗时：
`show profiles;`

![image-20220501145039624](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011450711.png)

查看指定query_id的SQL语句各个阶段的耗时：
`show profile for query query_id;`

查看指定query_id的SQL语句CPU的使用情况
`show profile cpu for query query_id;`



### explain

==EXPLAIN 或者 DESC== 命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中连接的顺序。
语法：

```mysql
# 直接在select语句之前加上关键字 
explain / desc  SELECT 字段列表 FROM 表名 HWERE 条件;
```

EXPLAIN 各字段含义：

- id：select 查询的序列号，表示查询中执行 select 子句或者操作表的顺序（id相同，执行顺序从上到下；id不同，值越大越先执行）
- select_type(==可不看==)：表示 SELECT 的类型，常见取值有 SIMPLE（简单表，即不使用表连接或者子查询）、PRIMARY（主查询，即外层的查询）、UNION（UNION中的第二个或者后面的查询语句）、SUBQUERY（SELECT/WHERE之后包含了子查询）等

==这个仅仅是说明当前select的查询类型罢了==

- type（`sql效率的重要指标`）：表示连接类型，性能由好到差的连接类型为 NULL（`不查询表才会显示这个类型`）、system、const(`根据Primary或者unique的字段查询`)、eq_ref、ref（`使用非唯一性索引查询`）、range、index、all（`全表扫描，性能十分低下`）

==优化sql语句的时候，尽量把type往前优化==

- possible_key：可能应用在这张表上的索引，一个或多个
- Key（）：实际使用的索引，如果为 NULL，则没有使用索引
- Key_len：表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好
- rows：MySQL认为必须要执行的行数，在InnoDB引擎的表中，是一个估计值，可能并不总是准确的
- filtered：表示返回结果的行数占需读取行数的百分比，filtered的值越大越好

## 索引

索引是帮助 MySQL **高效获取数据**的**数据结构（有序）**。这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查询算法，这种数据结构就是索引。

优缺点：

优点：

- 提高数据检索效率，降低数据库的IO成本（`io成本是指操作mysql的数据实际是在操作磁盘的数据`）
- 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗

缺点：

- 索引列也是要占用空间的
- 索引大大提高了查询效率，但降低了更新的速度，比如 INSERT、UPDATE、DELETE

### 索引结构

| 索引结构  | 描述  |
| ------------ | ------------ |
| B+Tree  | 最常见的索引类型，大部分引擎都支持B+树索引  |
| Hash  | 底层数据结构是用哈希表实现，只有精确匹配索引列的查询才有效，不支持范围查询  |
| R-Tree(空间索引)  | 空间索引是 MyISAM 引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少  |
| Full-Text(全文索引)  | 是一种通过建立倒排索引，快速匹配文档的方式，类似于 Lucene, Solr, ES  |

| 索引  | InnoDB  | MyISAM  | Memory  |
| ------------ | ------------ | ------------ | ------------ |
| B+Tree索引  | 支持  | 支持  | 支持  |
| Hash索引  | 不支持  | 不支持  | 支持  |
| R-Tree索引  | 不支持  | 支持  | 不支持  |
| Full-text  | 5.6版本后支持  | 支持  | 不支持 |

==我们通常所说的索引，如果没有特别指明，都是指的B+树结构组织的索引==

#### B-Tree（==多路==平衡查找树）

![image-20220501101212393](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011012610.png)

二叉树的缺点可以用红黑树来解决：

![image-20220501102602246](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011026328.png)

红黑树也存在大数据量情况下，层级较深，检索速度慢的问题。



为了解决上述问题，可以使用 B-Tree 结构。
B-Tree (多路平衡查找树) 以一棵最大度数（max-degree，指一个节点的子节点个数）为5（5阶）的 b-tree 为例（每个节点最多存储4个key，5个指针）

==树的度指的是一个节点的子节点个数==   

![image-20220501102618140](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011026238.png)

==分裂原则：当一个指针的数据排满以后，中间元素向上分裂==

红黑树在线演示过程：[红黑树动画在线演示 (phpisfuture.com)](http://rbtree.phpisfuture.com/)

> B-Tree 的数据插入过程动画参照：https://www.bilibili.com/video/BV1Kr4y1i7ru?p=68
> 演示地址：https://www.cs.usfca.edu/~galles/visualization/BTree.html

#### B+Tree

结构图：

![image-20220501102630900](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011026015.png)

> 演示地址：https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html

与 B-Tree 的区别：

- 所有的数据都会出现在叶子节点
- 叶子节点形成一个单向链表

MySQL 索引数据结构对经典的 B+Tree 进行了优化。在原 B+Tree 的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的 B+Tree，==提高区间访问的性能。==   `一页`的大小是有限制的

![image-20220505201951595](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052019841.png)

#### Hash

哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。
如果两个（或多个）键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。

![image-20220501103016636](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011030768.png)

特点：

- Hash索引只能用于对等比较（=、in），不支持范围查询（betwwn、>、<、...）【==因为hash存储位置是根据hash算出来的==】
- 无法利用索引完成排序操作 【==因为hash存储位置是根据hash算出来的==】
- 查询效率高，通常只需要一次检索就可以了，效率通常要高于 B+Tree 索引（==限定值通常是指不存在哈希碰撞的情况==）

存储引擎支持：

- Memory
- InnoDB: 具有自适应hash功能，hash索引是存储引擎根据 B+Tree 索引在指定条件下自动构建的

#### 面试题

1. 为什么 InnoDB 存储引擎选择使用 B+Tree 索引结构？

- 相对于二叉树，层级更少，搜索效率高
- 对于 B-Tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针也跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低
- 相对于 Hash 索引，B+Tree 支持范围匹配及排序操作

### 索引分类

| 分类  | 含义  | 特点  | 关键字  |
| ------------ | ------------ | ------------ | ------------ |
| 主键索引  | 针对于表中主键创建的索引  | 默认自动创建，只能有一个  | PRIMARY  |
| 唯一索引  | 避免同一个表中某数据列中的值重复  | 可以有多个  | UNIQUE  |
| 常规索引  | 快速定位特定数据  | 可以有多个  |   |
| 全文索引(==这个几乎不用，黑马也没讲==) | 全文索引查找的是文本中的关键词，而不是比较索引中的值  | 可以有多个  | FULLTEXT  |

在 InnoDB 存储引擎中，根据索引的存储形式，又可以分为以下两种：

| 分类  | 含义  | 特点  |
| ------------ | ------------ | ------------ |
| 聚集索引(Clustered Index)  | 将数据存储与索引放一块，索引结构的叶子节点保存了行数据  | 必须有，而且只有一个  |
| 二级索引(Secondary Index)  | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键  | 可以存在多个  |

演示图：

![大致原理](https://dhc.pythonanywhere.com/media/editor/原理图_20220318194454880073.png "大致原理")
![演示图](https://dhc.pythonanywhere.com/media/editor/演示图_20220319215403721066.png "演示图")

聚集索引选取规则：

- 如果存在主键，主键索引就是聚集索引
- 如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引
- 如果表没有主键或没有合适的唯一索引，则 InnoDB 会自动生成一个 rowid 作为隐藏的聚集索引

#### 思考题

1\. 以下 SQL 语句，哪个执行效率高？为什么？

```mysql
select * from user where id = 10;
select * from user where name = 'Arm';
-- 备注：id为主键，name字段创建的有索引
```

答：第一条语句，因为第二条需要回表查询，相当于两个步骤。

2\. InnoDB 主键索引的 B+Tree 高度为多少？

==一页默认大小是16k==

答：假设一行数据大小为1k，一页中可以存储16行这样的数据。InnoDB 的指针占用6个字节的空间，主键假设为bigint，占用字节数为8.
可得公式：`n * 8 + (n + 1) * 6 = 16 * 1024`，其中 8 表示 bigint 占用的字节数，n 表示当前节点存储的key的数量，(n + 1) 表示指针数量（比key多一个）。算出n约为1170。 

如果树的高度为2，那么他能存储的数据量大概为：`1171 * 16 = 18736`；
如果树的高度为3，那么他能存储的数据量大概为：`1171 * 1171 * 16 = 21939856`。

另外，如果有成千上万的数据，那么就要考虑分表，涉及运维篇知识。

### 语法

创建索引：
`CREATE [ UNIQUE | FULLTEXT ] INDEX index_name ON table_name (index_col_name, ...);`
如果不加 CREATE 后面不加索引类型参数，则创建的是常规索引

查看索引：
`SHOW INDEX FROM table_name;`

![image-20220501121313854](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011213021.png)

删除索引：
`DROP INDEX index_name ON table_name;`

案例：

```mysql
-- name字段为姓名字段，该字段的值可能会重复，为该字段创建索引
create index idx_user_name on tb_user(name);
-- phone手机号字段的值非空，且唯一，为该字段创建唯一索引
create unique index idx_user_phone on tb_user (phone);      #相当于在建表时给phone添加了一个unique条件
-- 为profession, age, status创建联合索引
create index idx_user_pro_age_stat on tb_user(profession, age, status);
-- 为email建立合适的索引来提升查询效率
create index idx_user_email on tb_user(email);

-- 删除索引
drop index idx_user_email on tb_user;
```

### 验证索引对效率的提升

==教程中的案例，有一张表只有一个主键索引，其他说明都没有。 根据主键查询某一条数据 耗时0.01s 根据其他字段(`比如sn`)查询`一条`数据用了20s。可见索引的强大之处。==

接着我们给sn添加常规索引，发现添加索引也十分耗时，因为mysql服务器要去根据这一千万条数据构建一个B+树。再次根据sn查询一条数据，发现耗时0.01s。



### 使用规则

#### 最左前缀法则

如果索引关联了多列（联合索引），要遵守最左前缀法则，最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。
如果跳跃某一列，==索引将部分失效（后面的字段索引失效）。==

> 案例：

```sql
#查看索引
show index from tb_user;
```

![image-20220501155801723](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011558007.png)

![image-20220501160032977](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011600166.png)

![image-20220501160159589](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011601817.png)

![image-20220501160258406](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011602542.png)

联合索引中，出现范围查询（<, >），==范围查询右侧的列索引失效==。可以用>=或者<=来规避索引失效问题。

![image-20220501160546155](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011605423.png)

#### 索引失效情况

1.在索引列上进行运算操作，==索引将失效==。如：`explain select * from tb_user where substring(phone, 10, 2) = '15';`

![image-20220501162012535](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011620694.png)

2.字符串类型字段使用时，不加引号，索引将失效。如：`explain select * from tb_user where phone = 17799990015;`，此处phone的值没有加引号

3.模糊查询中，如果仅仅是尾部模糊匹配，索引不会是失效；如果是头部模糊匹配，索引失效。如：`explain select * from tb_user where profession like '%工程';`，前后都有 % 也会失效。 

![image-20220501162258173](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011622474.png)

4.用 or 分割开的条件，如果 or 其中一个条件的列没有索引，那么涉及的索引都不会被用到。

![image-20220501163644895](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011636029.png)

==由于age没有索引，索引即使id有索引，索引也会失效，索引需要针对于age也要建立索引==

![image-20220501163811299](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011638437.png)

5.==如果 MySQL 评估使用索引比全表更慢，则不使用索引。==

全表数据：

![image-20220501164221896](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011642192.png)

![image-20220501163927317](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011639534.png)

==之所以第二条sql语句没有走索引，是因为>=此情况的条件是全表。直接走全表会比走索引更快==

![image-20220501164414246](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011644407.png)

==同理，因为满足条件的记录在全表中比较少，系统评估走索引比走全表搜索快，因此走索引。==

下面两图也是同理

![image-20220501164450745](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011644992.png)

![image-20220501164710935](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011647204.png)

#### SQL 提示

是优化数据库的一个重要手段，简单来说，就是==在SQL语句中加入一些人为的提示来达到优化操作的目的。==

例如：

使用索引：
`explain select * from tb_user use index(idx_user_pro) where profession="软件工程";`

不使用哪个索引：
`explain select * from tb_user ignore index(idx_user_pro) where profession="软件工程";`

必须使用哪个索引：
`explain select * from tb_user force index(idx_user_pro) where profession="软件工程";`

use 是建议，实际使用哪个索引 MySQL 还会自己权衡运行速度去更改，force就是无论如何都强制使用该索引。

![image-20220501170155406](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011701574.png)

![image-20220501170232755](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011702989.png)

#### 覆盖索引&回表查询

> 案例

![image-20220501171911258](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011719414.png)

![image-20220501171945461](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011719658.png)

![image-20220501172405454](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011724597.png)

![image-20220501172436639](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011724791.png)

![image-20220501172528810](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011725956.png)

尽量使用覆盖索引（查询使用了索引，并且需要返回的列，在该索引中已经全部能找到），减少 select *。

==explain 中 extra 字段含义：==

`using index condition`：查找使用了索引，但是需要回表查询数据

`using where; using index;`：查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询

如果在==聚集索引==中直接能找到对应的行，则直接返回行数据，只需要一次查询，哪怕是select \*；如果在辅助索引中找聚集索引，如`select id, name from xxx where name='xxx';`，也只需要通过辅助索引(name)查找到对应的id，返回name和name索引对应的id即可，只需要一次查询；如果是通过辅助索引查找其他字段，则需要回表查询，如`select id, name, gender from xxx where name='xxx';`

所以尽量不要用`select *`，容易出现回表查询，降低效率，除非有联合索引包含了所有字段



> 面试题：

一张表，有四个字段（id, username, password, status），由于数据量大，需要对以下SQL语句进行优化，该如何进行才是最优方案：
`select id, username, password from tb_user where username='itcast';`

==解：给username和password字段建立联合索引，则不需要回表查询，直接覆盖索引==



#### 前缀索引

==当字段类型为字符串（varchar, text等）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率，此时可以只降字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率。==

语法：`create index idx_xxxx on table_name(columnn(n));`

前缀长度：可以根据索引的选择性来决定，==而选择性是指不重复的索引值（基数）和数据表的记录总数的比值，索引选择性越高则查询效率越高==，唯一索引的选择性是1，这是最好的索引选择性，性能也是最好的。

求选择性公式：

```mysql
select count(distinct email) / count(*) from tb_user;
select count(distinct substring(email, 1, 5)) / count(*) from tb_user;
```

==show index 里面的sub_part可以看到接取的长度==

> 案例

![image-20220501183523472](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011835646.png)

![image-20220501183602852](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011836089.png)

![image-20220501183651665](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011836768.png)

![image-20220501183717772](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011837954.png)

![image-20220501183808587](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011838774.png)

==根据emial的前五个去进行匹配==

> 原理

![image-20220501183824808](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011838929.png)

==根据前缀去查找，如上图找到lvbu6，进行回表查询然后判断完整的emial是否满足条件，接着又会去lvbu6的右边匹配xiaoy是否满足，是的话进行回表查询将其也返回。反之结束查询==

#### 单列索引&联合索引

==单列索引:==即一个索引只包含单个列

==联合索引：==即一个索引包含了多个列

==**在业务场景中，如果存在多个查询条件，考虑针对于查询字段建立索引时，建议建立联合索引，而非单列索引。**==

> 案例：

![image-20220501193918588](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011939960.png)

![image-20220501194007383](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011940756.png)

==上图只查询phone的索引，因此会去进行连表查询，效率稍低。因此我们将这两个条件合起来建立一个联合索引，提高效率==

![image-20220501194316411](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011943760.png)

> 原理

![image-20220501194332652](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205011943998.png)

==我们建立上面的联合索引时，会先根据phone进行排序，再根据name进行排序。==

##### 注意事项

- ==**多条件联合查询时，MySQL优化器会评估哪个字段的索引效率更高，会选择该索引完成本次查询**==

### 设计原则

1. 针对于数据量较大，且查询比较频繁的表建立索引（一般100w以上采取考虑建立索引。几千几万条即使不建立索引，查询效率依旧很高。==不查询，只插入，不管数据有多少，都不用建索引，因为索引是用来检索数据的，和插入没有半毛钱关系，你建了影响增删改效率并且浪费磁盘空间==）
2. 针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高（==性别，状态表建了索引，效率也不太高==）
4. 如果是字符串类型的字段，字段长度较长，可以针对于字段的特点，建立前缀索引
5. 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，避免回表，提高查询效率
6. 要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价就越大，`会影响增删改的效率` 

 ==**索引结构=索引数据结构=b+树=索引**==

7. 如果索引列不能存储NULL值，请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询

## SQL 优化

### 插入数据

1. 采用批量插入（`如果一条一条的插入的话，每次都要和数据库建立连接，效率极低。一次插入的数据不建议超过1000条`）

例如：==insert into db_test values(1,’tom’),(2,’cat’),(3,’jerry’)；==

  2.手动提交事务（默认你每插入一条数据，事务就提交了。 那么多插入几条，就会频繁的事务开启，关闭。效率极低）

例如：

```sql
start transaction;
insert into db_test values(1,’tom’),(2,’cat’),(3,’jerry’);
insert into db_test values(4,’tom’),(5,’cat’),(6,’jerry’);
insert into db_test values(7,’tom’),(8,’cat’),(9,’jerry’);
commit
```

  3.主键顺序插入(`为什么主键顺序插入效率高 这取决于mysql的数据组织结构`)

```sql
主键乱序插入 : 8 1 9 21 88 2 4 15 89 5 7 3 
主键顺序插入 : 1 2 3 4 5 7 8 9 15 21 88 89
```

###  大批量插入：

如果一次性需要插入大批量数据(比如: 几百万的记录)，使用insert语句插入性能较低，此时可以使

用MySQL数据库提供的load指令进行插入。操作如下：`左边是数据的原始结构，符合我们的表` 不一定是要,其他符号也行， 在`terminated by ',' `将对应的符号作修改即可

![image-20220501212325355](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205012123502.png)

```mysql
# 客户端连接服务端时，加上参数 --local-infile（这一行在bash/cmd界面输入）
mysql --local-infile -u root -p
# 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;
select @@local_infile;
# 执行load指令将准备好的数据，加载到表结构中
load data local infile '/root/load_user_100w_sort.sql' into table tb_user fields terminated by ',' lines terminated by '\n' ;
# 说明
','说明每个字段映射用，来区分。 '\n'表示换行就插入下一条数据。
```

`load data local infile '/root/load_user_100w_sort.sql' into table tb_user fields terminated by ',' lines terminated by '\n' ;`  我们执行这个文件。文件里面有100w条记录。发现用这种方式11s就插入完毕了。

### 主键优化

数据组织方式：在==InnoDB存储引擎==中，表数据都是==根据主键顺序组织存放的==，这种存储方式的表称为索引组织表（Index organized table, IOT）

![image-20220501231304630](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205012313823.png)

行数据，都是存储在聚集索引的叶子节点上的。而我们之前也讲解过InnoDB的逻辑结构图：

![image-20220501231342800](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205012313969.png)

`页分裂`：页可以为空，也可以填充一半，也可以填充100%。每个页包含了N行数据(`如果一行数据过大，会行溢出`)，根据主键排列。

![image-20220501231634122](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205012316380.png)

![image-20220501231856205](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205012318433.png)

![image-20220501231703462](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205012317657.png)

![image-20220501231714843](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205012317210.png)

`页合并`：

当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许被其他记录声明使用。当页中删除的记录到达 MERGE_THRESHOLD（默认为页的50%），InnoDB会开始寻找最靠近的页（前后）看看是否可以将这两个页合并以优化空间使用。

`MERGE_THRESHOLD`：合并页的阈值，可以自己设置，在创建表或创建索引时指定

> 演示说明：

目前表中已有数据的索引结构(叶子节点)如下：

![image-20220502100600282](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021006537.png)

![image-20220502100614361](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021006500.png)

==**主键设计原则：**==

- 满足业务需求的情况下，尽量降低主键的长度(==主键越长，回表查询要匹配的时候就越久==)
- 插入数据时，尽量选择顺序插入，选择使用 AUTO_INCREMENT 自增主键(==否则页分裂，降低插入效率==)
- 尽量不要使用 UUID 做主键或者是其他的自然主键，如身份证号（==可能会导致页分裂==）
- 业务操作时，避免对主键的修改(==可能会重建B+树==)

### order by优化

MySQL的==排序==，有==两种方式==：

==Using filesort== : 

通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sortbuffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序。

==Using index== : 

通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高。

对于以上的两种排序方式，`Using index`的性能高，而`Using filesort`的性能低，我们在优化排序操作时，尽量要`优化为 Using index。`

> 案例：

![image-20220502102955934](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021029096.png)

![image-20220502103701278](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021037621.png)

![image-20220502103455499](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021034061.png)

![image-20220502103908501](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021039088.png)

![image-20220502105246088](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021052555.png)

![image-20220502105334095](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021053368.png)

![image-20220502105353701](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021053055.png)

![image-20220502105643536](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021056012.png)

> 图解：

![image-20220502105852970](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021058139.png)

总结：

- 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
- 尽量使用覆盖索引（==不要用*,会去缓冲区排序==）
- 一条sql多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）
- 如果不可避免出现filesort，大数据量排序时，可以适当增大排序缓冲区大小 sort_buffer_size（默认256k）

### group by优化

- 在分组操作时，可以通过索引来提高效率
- 分组操作时，索引的使用也是满足最左前缀法则的

> 案例：

![image-20220502111754690](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021117011.png)

![image-20220502111817828](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021118051.png)

出现了Using temporary 代表用了临时表 效率较低。紧接着我们创建一个联合索引

```sql
create index idx_user_pro_age_sta on tb_user(profession , age , status);
```

![image-20220502111924192](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021119470.png)

![image-20220502111942032](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021119325.png)

我们发现，如果==仅仅根据age分组==，就会出现 ==Using temporary== ；而如果是 ==根据profession,age两个字段同时分组==，则==不会出现 Using temporary==。原因是因为对于分组操作，在联合索引中，也是符合最左前缀法则的。

所以，在分组操作中，我们需要通过以下两点进行优化，以提升性能：

A. 在分组操作时，可以通过索引来提高效率。

B. 分组操作时，索引的使用也是满足最左前缀法则的。



### limit优化

常见的问题如`limit 2000000, 10`，此时需要 MySQL 排序前2000000条记录，但仅仅返回2000000 - 2000010的记录，其他记录丢弃，查询排序的代价非常大。

==**优化方案：**==一般分页查询时，通过创建覆盖索引能够比较好地提高性能，可以==通过覆盖索引加子查询形式进行优化==

例如：

```mysql
-- 此语句耗时很长
select * from tb_sku limit 9000000, 10;
-- 通过覆盖索引加快速度，直接通过主键索引进行排序及查询
select id from tb_sku order by id limit 9000000, 10;
-- 下面的语句是错误的，因为 MySQL 不支持 in 里面使用 limit
-- select * from tb_sku where id in (select id from tb_sku order by id limit 9000000, 10);
-- 通过连表查询即可实现第一句的效果，并且能达到第二句的速度
select * from tb_sku as s, (select id from tb_sku order by id limit 9000000, 10) as a where s.id = a.id;

#教程测的数据是 没优化前 19s 优化后10s
```

### count优化

在之前的测试中，我们发现，如果数据量很大，在执行count操作时，是非常耗时的。

- MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 count(*) 的时候会直接返回这个数，效率很高； 但是如果是带条件的count，MyISAM也慢。*
- *InnoDB 引擎就麻烦了，它执行 count(*) 的时候，需要把数据一行一行地从引擎里面读出来，然后累积计数。

如果说要大幅度提升InnoDB表的count效率，主要的优化思路：自己计数(可以借助于redis这样的数据库进行,但是如果是带条件的count又比较麻烦了)。



> count的几种用法：
>

用法：count（*）、count（主键）、count（字段）、count（数字）

各种用法的性能：

1. count(主键)：InnoDB 引擎会遍历整张表，把每一行的 主键id 值都取出来，返回给服务层。服务层拿到主键后，直接按行进行累加
2. count(字段)：没有not null 约束 : InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为null，不为null，计数累加。有not null 约束：InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加。
3. count(1)：InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字“1”进去，直接按行进行累加。
4. count(\*)：InnoDB引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加。

==**按效率排序：count(字段) < count(主键) < count(1) < count(\*)，所以尽量使用 count(\)**==



### update优化（避免行锁升级为表锁）

我们主要需要注意一下update语句执行时的注意事项。

==**InnoDB 的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁。**==

如以下两条语句：
`update student set no = '123' where id = 1;`，这句由于id有主键索引，所以只会锁这一行；
`update student set no = '123' where name = 'test';`，这句由于name没有索引，所以会把整张表都锁住进行数据更新，解决方法是给name字段添加索引

> 案例：id是主键 username是唯一索引 除此之外没有其他索引

一个mysql客户端：

![image-20220502131110120](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021311321.png)

另外一个客户端：

![image-20220502131139887](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021311975.png)

两者都提交，发现数据库记录都修改成功。

==**总结：因为id建立了索引，因此第一个客户端事务修改某条记录的时候上锁上的是行锁，第二个客户端的事务操作修改的是其他记录，其他记录并没有被上锁，因此能并发执行**==

> 再测试一组：

![image-20220502131800542](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021318648.png)

发现另外一个客户端执行update的时候阻塞了。这是因为上一个客户端的update操作把整个表给锁了 

![image-20220502131540158](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021315254.png)

第一个客户端提交后（`成功释放表锁`）：第二个客户端马上成功修改了

![image-20220502131557444](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021315723.png)

==**总结:因为password字段没有设置索引，因此我们进行事务操作的时候，加了一个表锁，导致第二个客户端修改时表锁还没释放，因此阻塞等待，第一个客户端提交事务成功释放表锁后，第二个客户端拿到锁可以马上修改数据；**==



## 视图

### 介绍

视图（View）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，并且是在使用视图时动态生成的。

通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上

### 语法

1). 创建

```sql
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] 
AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
```



2). 查询

```sql
查看创建视图语句：SHOW CREATE VIEW 视图名称; 
查看视图数据：SELECT * FROM 视图名称 ...... ;
```

3). 修改

```sql
方式一：CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] 
AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ] 
方式二：ALTER VIEW 视图名称[(列名列表)] 
AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
```

4). 删除

```sql
DROP VIEW [IF EXISTS] 视图名称 [,视图名称] ...
```

> 演示案例：

```sql
-- 创建视图
create or replace view stu_v_1 as select id,name from student where id <= 10 ;
-- 查询视图
show create view stu_v_1;
select * from stu_v_1;
select * from stu_v_1 where id < 3 ;
-- 修改视图
create or replace view stu_v_1 as select id,name,no from student where id <= 10 ;
alter view stu_v_1 as select id,name from student where id <= 10 ;
-- 删除视图
drop view if exists stu_v_1;
```

上述我们演示了，视图应该如何创建、查询、修改、删除，那么我们能不能通过视图来插入、更新数据

呢？ 接下来，做一个测试。

```sql
create or replace view stu_v_1 as select id,name from student where id <= 10 ;
select * from stu_v_1;
insert into stu_v_1 values( 6 ,'Tom');
insert into stu_v_1 values( 17 ,'Tom22');
紧接着查看视图：
```

![image-20220502143148129](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021431234.png)

执行上述的SQL，我们会发现，==id为 6 和 17 的数据都是可以成功插入的==。 但是我们执行查询视图，查询出
来的数据，却没有id为 17 的记录。

因为我们在创建视图的时候，指定的条件为 id<=10, id为 17 的数据，是不符合条件的，所以没有查
询出来，但是这条数据确实是已经成功的插入到了基表中。

==如果我们定义视图时，指定了条件，然后我们在插入、修改、删除数据时，是否可以做到必须满足条件才能操作，否则不能够操作呢？ 答案是可以的，这就需要借助于视图的检查选项了。==

### 检查选项

当使用WITH CHECK OPTION子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如 插
入，更新，删除，以使其符合视图的定义。==MySQL允许基于另一个视图创建视图==，它还会检查依赖视
图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项： CASCADED 和 LOCAL。如果`WITH  OPTION` 写成这样 就等价于`WITH CASCADED OPTION` 因为默认是`CASCADED` 



1). CASCADED ：级联

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 `cascaded`，但是v1视图
创建时未指定检查选项。 则在执行检查时，不仅会检查v2，还会级联检查v2的关联视图v1。

![image-20220502144119105](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021441214.png)

==因为视图2写了WITH CASCADED OPTION，所以相当于视图1也加上了WITH CASCADED OPTION。因此会去检查两个视图的条件==

==插入数据进v3的时候，不会去检查视图3的条件，但是会去检查v1和v2的条件。==

2). LOCAL：本地

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 local ，但是v1视图创
建时未指定检查选项。 则在执行检查时，知会检查v2，不会检查v2的关联视图v1。

![image-20220502144127715](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021441812.png)

==通过v1插入数据不会去判断条件。通过v3去插入数据只会检查v2的条件。==

###  视图的更新

要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。如果视图包含以下任何一项，则该视图不可更新：

A. 聚合函数或窗口函数（SUM()、 MIN()、 MAX()、 COUNT()等）

B. DISTINCT

C. GROUP BY

D. HAVING

E. UNION 或者 UNION ALL



###  视图作用

1). 简单

视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。

2). 安全

数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图可以保证用户只能查询和修改他们所能见到的数据



3). 数据独立

视图可帮助用户屏蔽真实表结构变化带来的影响。（`比如基表某个字段改了个名字,视图表的字段名字可以永不变`）



## 存储过程

### 介绍

存储过程是事先经过编译并存储在数据库中的一段 SQL 语句的集合，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

==存储过程思想上很简单，就是数据库 SQL 语言层面的代码封装与重用。==

特点:

- 封装，复用 -----------------------> 可以把某一业务SQL封装在存储过程中，需要用到的时候直接调用即可。
- 可以接收参数，也可以返回数据 --------> 再存储过程中，可以传递参数，也可以接收返回值。
- 减少网络交互，效率提升 -------------> 如果涉及到多条SQL，每执行一次都是一次网络传输。 而如果封装在存储过程中，我们只需要网络交互一次可能就可以了。

###  基本语法

1). 创建

```sql
DELIMITER $
CREATE PROCEDURE 存储过程名称 ([ 参数列表 ])  可以多参数
BEGIN
-- SQL语句
END $
DELIMITER ;
```

2). 调用

```sql
CALL 名称 ([ 参数 ]); 可以多参数
```

3). 查看

```sql
SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = 'xxx'; -- 查询指
定数据库的存储过程及状态信息
SHOW CREATE PROCEDURE 存储过程名称 ; -- 查询某个存储过程的定义
```

4). 删除

```sql
 DROP PROCEDURE [ IF EXISTS ] 存储过程名称 ；
```

注意:

==**不管在可视化工具还是命令行中，执行创建存储过程时，都通过关键字 delimiter 指定SQL语句的结束符。**==

> 案例：

![image-20220502194533271](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021945831.png)

![image-20220502194611673](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021946961.png)

![image-20220502194708058](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021947705.png)

### 变量

在MySQL中变量分为三种类型: 系统变量、用户定义变量、局部变量。



#### 系统变量

系统变量 是MySQL服务器提供，不是用户定义的，属于服务器层面。分为`全局变量（GLOBAL）`、`会话
变量（SESSION）`。

1). 查看系统变量

```sql
SHOW [ SESSION | GLOBAL ] VARIABLES ; -- 查看所有系统变量   如果不指定SESSION | GLOBAL 默认是Session
SHOW [ SESSION | GLOBAL ] VARIABLES LIKE '......'; -- 可以通过LIKE模糊匹配方式查找变量

SELECT @@[SESSION | GLOBAL].系统变量名; -- 查看指定变量的值
```

2). 设置系统变量

```sql
SET [ SESSION | GLOBAL ] 系统变量名 = 值 ;  如果不指定SESSION | GLOBAL 默认是Session
SET @@[SESSION | GLOBAL].系统变量名 = 值 ;
```

注意:

如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量。

A. 全局变量(GLOBAL): 全局变量针对于所有的会话。

B. 会话变量(SESSION): 会话变量针对于单个会话，在另外一个会话窗口(`另一个控制台`)就不生效了。

> 演示示例: 

```sql
1 mysql服务重新启动(`systemctl restart mysqld`)之后，所设置的全局参数会失效，要想不失效，可以在 /etc/my.cnf 中配置。
-- 查看系统变量
show session variables ;
show session variables like 'auto%';
show global variables like 'auto%';
select @@global.autocommit;
select @@session.autocommit;
-- 设置系统变量
set session autocommit = 1 ;
insert into course(id, name) VALUES ( 6 , 'ES');  发现插入进去了
set global autocommit = 0 ;
insert into course(id, name) VALUES ( 6 , 'ES');  发现没有插入进去 需要commit才能插入进去
select @@global.autocommit;
```



####  用户定义变量

用户定义变量 是用户根据需要自己定义的变量，==用户变量不用提前声明，在用的时候直接用 "@变量名" 使用就可以。其`作用域为当前连接`。==

1). 赋值

方式一:

```sql
SET @var_name = expr [, @var_name = expr] ... ;
SET @var_name := expr [, @var_name := expr] ... ;
```

赋值时，可以使用 = ，也可以使用 := 。

方式二:

```sql
SELECT @var_name := expr [, @var_name := expr] ... ;
SELECT 字段名 INTO @var_name FROM 表名;
```

2). 使用

```sql
SELECT @var_name ;
```

![image-20220502165127756](../AppData/Roaming/Typora/typora-user-images/image-20220502165127756.png)

注意: 用户定义的变量无需对其进行声明或初始化，只不过没有赋值时候获取到的值为NULL。

演示示例:

```sql
-- 赋值
set @myname = 'itcast';
set @myage := 10 ;
set @mygender := '男',@myhobby := 'java';
select @mycolor := 'red';
select count(*) into @mycount from tb_user;
-- 使用
select @myname,@myage,@mygender,@myhobby;
select @mycolor , @mycount;
```

> 运行结果：

![image-20220502165205919](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021652211.png)

![image-20220502165217929](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021652136.png)

#### 局部变量

局部变量 ==是根据需要定义的在局部生效的变量==，访问之前，需要DECLARE声明。==可用作存储过程内的局部变量，局部变量的范围是在其声明的BEGIN ... END块中。==

1). 声明

```sql
1 DECLARE 变量名 变量类型 [DEFAULT ... ] ;
```

变量类型就是数据库字段类型：INT、BIGINT、CHAR、VARCHAR、DATE、TIME等。

2). 赋值

```sql
SET 变量名 = 值 ;
SET 变量名 := 值 ;
SELECT 字段名 INTO 变量名 FROM 表名 ... ;
```

> 演示示例:

```sql
-- 声明局部变量 - declare
-- 赋值
create procedure p2()
begin
declare stu_count int default 0 ;
select count(*) into stu_count from student;
select stu_count;
end;
call p2();
```



### if判断

> 介绍

if 用于做条件判断，具体的语法结构为：

```sql
IF 条件1 THEN
.....
ELSEIF 条件2 THEN -- 可选
.....
ELSE -- 可选
.....
END IF;
```

在if条件判断的结构中，ELSE IF 结构可以有多个，也可以没有。 ELSE结构可以有，也可以没有。

> 案例：

根据定义的分数score变量，判定当前分数对应的分数等级。

```sql
score >= 85分，等级为优秀。
score >= 60分 且 score < 85分，等级为及格。
score < 60分，等级为不及格。
```

`具体写法:`

```sql
create procedure p3()
begin
declare score int default 58 ;
declare result varchar( 10 );
if score >= 85 then
set result := '优秀';
elseif score >= 60 then
set result := '及格';
else
set result := '不及格';
end if;
select result;
end;
call p3();
```

> 运行结果：

![image-20220502194416161](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205021944405.png)

### 参数

> 介绍：

| **类型** | **含义**                                     | **备注** |
| -------- | -------------------------------------------- | -------- |
| IN       | 该类参数作为输入，也就是需要调用时传入值     | 默认     |
| OUT      | 该类参数作为输出，也就是该参数可以作为返回值 |          |
| INOUT    | 既可以作为输入参数，也可以作为输出参数       |          |

> 用法：

案例一

根据传入参数score，判定当前分数对应的分数等级，并返回。

```sql
score >= 85分，等级为优秀。
score >= 60分 且 score < 85分，等级为及格。
score < 60分，等级为不及格。
```



```sql
create procedure p4(in score int, out result varchar( 10 ))
begin
if score >= 85 then
set result := '优秀';
elseif score >= 60 then
set result := '及格';
else
set result := '不及格';
end if;
end;

-- 定义用户变量 @result来接收返回的数据, 用户变量可以不用声明
call p4( 18 , @result);
select @result;
```

案例二：

将**传入**的200分制的分数，进行换算，换算成百分制，然后**返回**。 

```sql
create procedure p5(inout score double)
begin
set score := score * 0.5;
end;
set @score = 198 ;
call p5(@score);
select @score;
```



### case

>  介绍

case有两种语法格式：

语法 1 ：

```sql
-- 含义： 当case_value的值为 when_value1时，执行statement_list1，当值为 when_value2时，
执行statement_list2， 否则就执行 statement_list
CASE case_value
WHEN when_value1 THEN statement_list1
[ WHEN when_value2 THEN statement_list2] ...
[ ELSE statement_list ]
END CASE;
```

语法2 ：

```sql
-- 含义： 当条件search_condition1成立时，执行statement_list1，当条件search_condition2成
立时，执行statement_list2， 否则就执行 statement_list
CASE
WHEN search_condition1 THEN statement_list1
[WHEN search_condition2 THEN statement_list2] ...
[ELSE statement_list]
END CASE;
```

> 案例：

根据传入的月份，判定月份所属的季节（要求采用case结构）。

- 1-3月份，为第一季度
- 4-6月份，为第二季度
- 7-9月份，为第三季度
- 10-12月份，为第四季度

```sql
create procedure p6(in month int)
begin
declare result varchar( 10 );
case
when month >= 1 and month <= 3 then
set result := '第一季度';
when month >= 4 and month <= 6 then
set result := '第二季度';
when month >= 7 and month <= 9 then
set result := '第三季度';
when month >= 10 and month <= 12 then
set result := '第四季度';
else
set result := '非法参数';
end case ;
select concat('您输入的月份为: ',month, ', 所属的季度为: ',result);
end;
call p6( 16 );

写法二：
DELIMITER $
CREATE PROCEDURE p7(IN MONTH INT,OUT a  VARCHAR( 10 ))
BEGIN
CASE
WHEN MONTH >= 1 AND MONTH <= 3 THEN
SET  := '第一季度';
WHEN MONTH >= 4 AND MONTH <= 6 THEN
SET a := '第二季度';
WHEN MONTH >= 7 AND MONTH <= 9 THEN
SET a := '第三季度';
WHEN MONTH >= 10 AND MONTH <= 12 THEN
SET a := '第四季度';
ELSE
SET a := '非法参数';
END CASE ;
END$
CALL p7( 16,@a );
SELECT @a;
```

![image-20220502205448289](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205022054627.png)

### while

1). 介绍

while 循环是有条件的循环控制语句。满足条件后，再执行循环体中的SQL语句。具体语法为：

```sql
-- 先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
WHILE 条件 DO
SQL逻辑...
END WHILE;
```

2). 案例

计算从 1 累加到n的值，n为传入的参数值。

```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行减1 , 如果n减到0, 则退出循环
create procedure p7(in n int)
begin
declare total int default 0 ;
while n> 0 do
set total := total + n;
set n := n - 1 ;
end while;
select total;
end;
call p7( 100 );
```

### repeat

1). 介绍

repeat是有条件的循环控制语句, 当满足until声明的条件的时候，则退出循环 。具体语法为：

```sql
-- 先执行一次逻辑，然后判定UNTIL条件是否满足，如果满足，则退出。如果不满足，则继续下一次循环  这个循环控制语句一定会执行一次
REPEAT
SQL逻辑...
UNTIL 条件
END REPEAT;
```

2). 案例

计算从 1 累加到n的值，n为传入的参数值。(使用repeat实现)

```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环
create procedure p8(in n int)
begin
declare total int default 0 ;
repeat
set total := total + n;
set n := n - 1 ;
until n <= 0
end repeat;
select total;
call p8( 10 );
call p8( 100 );
```

### loop

1). 介绍

LOOP 实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。

LOOP可以配合一下两个语句使用：

- LEAVE ：配合循环使用，退出循环。
- ITERATE：必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环。

```sql
[begin_label:] LOOP
SQL逻辑...
END LOOP [end_label];
```

```sql
LEAVE label; -- 退出指定标记的循环体
ITERATE label; -- 直接进入下一次循环
```

上述语法中出现的 begin_label，end_label，label 指的都是我们所自定义的标记。

2). 案例一

计算从 1 累加到n的值，n为传入的参数值。

```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环 ----> leave xx
create procedure p9(in n int)
begin
declare total int default 0 ;
sum:loop
if n<= 0 then
leave sum;
end if;
set total := total + n;
set n := n - 1 ;
end loop sum;
select total;
end;
call p9( 100 );
```

案例二

计算从 1 到n之间的偶数累加的值，n为传入的参数值。

```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环 ----> leave xx
-- C. 如果当次累加的数据是奇数, 则直接进入下一次循环. --------> iterate xx
create procedure p10(in n int)
begin
declare total int default 0 ;
sum:loop
if n<= 0 then
leave sum;
end if;
if n% 2 = 1 then
set n := n - 1 ;
iterate sum;
end if;
set total := total + n;
set n := n - 1 ;
end loop sum;
select total;
end;
call p10( 100 );
```

### 游标

1). 介绍

游标（CURSOR）是用来存储查询结果集的`数据类型` , 在存储过程和函数中可以使用游标对结果集进行循环的处理。游标的使用包括游标的声明、OPEN、FETCH 和 CLOSE，其语法分别如下。

A. 声明游标

```sql
DECLARE 游标名称 CURSOR FOR 查询语句 ;
```

B. 打开游标

```sql
OPEN 游标名称 ;
```

C. 获取游标记录

```sql
FETCH 游标名称 INTO 变量 [, 变量 ] ;
```

D. 关闭游标

```sql
CLOSE 游标名称 ;
```

2). 案例

根据传入的参数uage，来查询用户表tb_user中，所有的用户年龄小于等于uage的用户姓名
（name）和专业（profession），并将用户的姓名和专业插入到所创建的一张新表
(id,name,profession)中。

```sql
-- 逻辑:
-- A. 声明游标, 存储查询结果集
-- B. 准备: 创建表结构
-- C. 开启游标
-- D. 获取游标中的记录
-- E. 插入数据到新表中
-- F. 关闭游标
create procedure p11(in uage int)
begin
declare uname varchar( 100 );
declare upro varchar( 100 );
declare u_cursor cursor for select name,profession from tb_user where age <=
uage;
drop table if exists tb_user_pro;
create table if not exists tb_user_pro(
id int primary key auto_increment,
name varchar( 100 ),
profession varchar( 100 )
);
open u_cursor;
while true do
fetch u_cursor into uname,upro;
insert into tb_user_pro values (null, uname, upro);
end while;
close u_cursor;
end;
call p11( 30 );
```

上述的存储过程，最终我们在调用的过程中，会报错，之所以报错是因为上面的while循环中，并没有退出条件。==当游标的数据集获取完毕之后，再次获取数据，就会报错，从而终止了程序的执行。==

![image-20220503085323867](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205030853014.png)

但是此时，tb_user_pro表结构及其数据都已经插入成功了，我们可以直接刷新表结构，检查表结构中的数据。

![image-20220503085348703](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205030853952.png)

上述的功能，虽然我们实现了，但是逻辑并不完善，而且程序执行完毕，获取不到数据，数据库还报错。 接下来，我们就需要来完成这个存储过程，并且解决这个问题。

要想解决这个问题，就需要通过MySQL中提供的==条件处理程序 Handler 来解决。==



### 条件处理程序

1). 介绍

条件处理程序（Handler）可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤。具体
语法为：

```sql
DECLARE handler_action HANDLER FOR condition_value [, condition_value]
... statement ;

handler_action 的取值：
CONTINUE: 继续执行当前程序
EXIT: 终止执行当前程序

condition_value 的取值：
SQLSTATE sqlstate_value: 状态码，如 02000
SQLWARNING: 所有以 01 开头的SQLSTATE代码的简写
NOT FOUND: 所有以 02 开头的SQLSTATE代码的简写
SQLEXCEPTION: 所有没有被SQLWARNING 或 NOT FOUND捕获的SQLSTATE代码的简写
```

2). 案例

我们继续来完成在上一小节提出的这个需求，并解决其中的问题。

根据传入的参数uage，来查询用户表tb_user中，所有的用户年龄小于等于uage的用户姓名
（name）和专业（profession），并将用户的姓名和专业插入到所创建的一张新表
(id,name,profession)中。

```sql
-- 逻辑:
-- A. 声明游标, 存储查询结果集
-- B. 准备: 创建表结构
-- C. 开启游标
-- D. 获取游标中的记录
-- E. 插入数据到新表中
-- F. 关闭游标
create procedure p11(in uage int)
begin
declare uname varchar( 100 );
declare upro varchar( 100 );
declare u_cursor cursor for select name,profession from tb_user where age <=
uage;
-- 声明条件处理程序 ： 当SQL语句执行抛出的状态码为 02000 时，将关闭游标u_cursor，并退出
declare exit handler for SQLSTATE '02000' close u_cursor;
drop table if exists tb_user_pro;
create table if not exists tb_user_pro(
id int primary key auto_increment,
name varchar( 100 ),
profession varchar( 100 )
);
open u_cursor;
while true do
fetch u_cursor into uname,upro;
insert into tb_user_pro values (null, uname, upro);
end while;
close u_cursor;
end;
call p11( 30 );
```

B. 通过SQLSTATE的代码简写方式 NOT FOUND

02 开头的状态码，代码简写为 NOT FOUND

```sql
create procedure p12(in uage int)
begin
declare uname varchar( 100 );
declare upro varchar( 100 );
declare u_cursor cursor for select name,profession from tb_user where age <=
uage;
-- 声明条件处理程序 ： 当SQL语句执行抛出的状态码为 02 开头时，将关闭游标u_cursor，并退出
declare exit handler for not found close u_cursor;
drop table if exists tb_user_pro;
create table if not exists tb_user_pro(
id int primary key auto_increment,
DETERMINISTIC：相同的输入参数总是产生相同的结果
name varchar( 100 ),
profession varchar( 100 )
);
open u_cursor;
while true do
fetch u_cursor into uname,upro;
insert into tb_user_pro values (null, uname, upro);
end while;
close u_cursor;
end;
call p12( 30 );
```

具体的错误状态码，可以参考官方文档：

```sql
https://dev.mysql.com/doc/refman/8.0/en/declare-handler.html
https://dev.mysql.com/doc/mysql-errors/8.0/en/server-error-reference.html
```

### 存储函数

1). 介绍

存储函数是有返回值的存储过程，存储函数的参数只能是IN类型的。具体语法如下：

```sql
CREATE FUNCTION 存储函数名称 ([ 参数列表 ])
RETURNS type [characteristic ...]
BEGIN
-- SQL语句
RETURN ...;
END ;
```

characteristic说明：

- DETERMINISTIC：相同的输入参数总是产生相同的结果
- NO SQL ：不包含 SQL 语句。
- READS SQL DATA：包含读取数据的语句，但不包含写入数据的语句。

2). 案例

计算从 1 累加到n的值，n为传入的参数值。

```sql
DELIMITER $
CREATE FUNCTION fun1(n INT)
RETURNS INT DETERMINISTIC
BEGIN

DECLARE total INT DEFAULT 0 ;
WHILE n> 0 DO
SET total := total + n;
SET n := n - 1 ;
END WHILE;
RETURN total;
END $
SELECT fun1( 50 );
```

在mysql8.0版本中binlog默认是开启的，一旦开启了，mysql就要求在定义存储过程时，需要指定
characteristic特性，否则就会报如下错误：

![image-20220503091437640](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205030914899.png)

==指定characteristic为DETERMINISTIC后，运行结果如下==

![image-20220503093529811](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205030935084.png)

## 触发器

### 介绍

触发器是与表有关的数据库对象，指在insert/update/delete之前(`BEFORE`)或之后(`AFTER`)，触发触发器执行定义的SQL语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性, 日志记录 , 数据校验等操作 。

使用别名`OLD`和`NEW`来引用对应表中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发。

==行级触发器就是指执行涉及多少条语句，触发器就触发多少次，语级触发器是指不管执行涉及多少条语句，触发器都只触发一次。==

| 触发器类型                                     | NEW 和 OLD                                              |
| ---------------------------------------------- | ------------------------------------------------------- |
| NSERT 型触发器                                 | NEW 表示将要或者已经新增的数据                          |
| UPDATE 型触发器                                | OLD 表示修改之前的数据 , NEW 表示将要或已经修改后的数据 |
| DELETE 型触发器 OLD 表示将要或者已经删除的数据 | OLD 表示将要或者已经删除的数据                          |

### 语法

1). 创建

```sql
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON tbl_name FOR EACH ROW -- 行级触发器
BEGIN
trigger_stmt ;
END;
```

2). 查看

```sql
1 SHOW TRIGGERS ;
```

3). 删除

```sql
DROP TRIGGER [schema_name.]trigger_name ; -- 如果没有指定 schema_name，默认为当前数
据库 
```

> 案例

通过触发器记录 tb_user 表的数据变更日志，将变更日志插入到日志表user_logs中, 包含增加,修改 , 删除 ;

表结构准备:

```sql
-- 准备工作 : 日志表 user_logs
create table user_logs(
id int( 11 ) not null auto_increment,
operation varchar( 20 ) not null comment '操作类型, insert/update/delete',
operate_time datetime not null comment '操作时间',
operate_id int( 11 ) not null comment '操作的ID',
operate_params varchar( 500 ) comment '操作参数',
primary key(`id`)
)engine=innodb default charset=utf8;
```

### A. 插入数据触发器

```sql
create trigger tb_user_insert_trigger
after insert on tb_user for each row
begin
insert into user_logs(id, operation, operate_time, operate_id, operate_params)
VALUES
(null, 'insert', now(), new.id, concat('插入的数据内容为:
id=',new.id,',name=',new.name, ', phone=', NEW.phone, ', email=', NEW.email, ',
profession=', NEW.profession));
end;
```

> 测试

```sql
-- 查看
show triggers ;
-- 插入数据到tb_user
insert into tb_user(id, name, phone, email, profession, age, gender, status,
createtime) VALUES ( 25 ,'三皇子','18809091212','erhuangzi@163.com','软件工
程', 23 ,'1','1',now());
```

测试完毕之后，检查日志表中的数据是否可以正常插入，以及插入数据的正确性。

![image-20220503103512752](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031035976.png)

### B. 修改数据触发器

```sql
create trigger tb_user_update_trigger
after update on tb_user for each row
begin
insert into user_logs(id, operation, operate_time, operate_id, operate_params)
VALUES
(null, 'update', now(), new.id,
concat('更新之前的数据: id=',old.id,',name=',old.name, ', phone=',
old.phone, ', email=', old.email, ', profession=', old.profession,
' | 更新之后的数据: id=',new.id,',name=',new.name, ', phone=',
NEW.phone, ', email=', NEW.email, ', profession=', NEW.profession));
end;
```

> 测试

```sql
-- 查看
show triggers ;
-- 更新
update tb_user set profession = '会计' where id = 23 ;
update tb_user set profession = '会计' where id <= 5 ;  这条语句触发器触发了五次下图可看出
```

测试完毕之后，检查日志表中的数据是否可以正常插入，以及插入数据的正确性。

![image-20220503103646684](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031036945.png)

### C. 删除数据触发器

```sql
create trigger tb_user_delete_trigger
after delete on tb_user for each row
begin
insert into user_logs(id, operation, operate_time, operate_id, operate_params)
VALUES
(null, 'delete', now(), old.id,
concat('删除之前的数据: id=',old.id,',name=',old.name, ', phone=',
old.phone, ', email=', old.email, ', profession=', old.profession));
end;
```

> 测试:

测试完毕之后，检查日志表中的数据是否可以正常插入，以及插入数据的正确性。



## 锁

### 概述

锁是计算机==协调多个进程或线程并发访问某一资源的机制==。在数据库中，除传统的计算资源（CPU、RAM、I/O）的争用以外，==数据也是一种供许多用户共享的资源==。如何保证==数据并发访问的一致性、有效性是所有数据库必须解决的一个问题==，`锁冲突也是影响数据库并发访问性能的一个重要因素`。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。

MySQL中的锁，按照锁的粒度分，分为以下三类：

- 全局锁：锁定数据库中的所有表。
- 表级锁：每次操作锁住整张表。
- 行级锁：每次操作锁住对应的行数据。



### 全局锁

####  介绍

全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，已经更新操作的事务提交语句都将被阻塞（`回滚事务可以执行`）。

==**其典型的使用场景是做全库的逻辑备份，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性。**==



为什么全库逻辑备份，就需要加全就锁呢？

> A. 我们一起先来分析一下不加全局锁，可能存在的问题。

假设在数据库中存在这样三张表: tb_stock 库存表，tb_order 订单表，tb_orderlog 订单日志表。

![image-20220503115541427](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031155568.png)

- 在进行数据备份时，先备份了tb_stock库存表。
- 然后接下来，在业务系统中，执行了下单操作，扣减库存，生成订单（更新tb_stock表，插入tb_order表）。
- 然后再执行备份 tb_order表的逻辑。
- 业务中执行插入订单日志操作。
- 最后，又备份了tb_orderlog表。

此时备份出来的数据，是存在问题的。因为备份出来的数据，tb_stock表与tb_order表的数据不一致(`有最新操作的订单信息,但是库存数没减`)。

那如何来规避这种问题呢? 此时就可以借助于MySQL的全局锁来解决。

> B. 再来分析一下加了全局锁后的情况

![image-20220503115706482](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031157836.png)

对数据库进行进行逻辑备份之前，先对整个数据库加上全局锁，一旦加了全局锁之后，其他的DDL、DML全部都处于阻塞状态，但是可以执行DQL语句，也就是处于只读状态，而数据备份就是查询操作。那么数据在进行逻辑备份的过程中，数据库中的数据就是不会发生变化的，这样就保证了数据的一致性和完整性。

#### 语法

1). 加全局锁

```sql
flush tables with read lock ;           
```

2). 数据备份

```sql
mysqldump -uroot –p1234 itcast > itcast.sql  这是本地数据库备份语句 远程数据库备份语句看下面的案例语句 其实就是多加一个ip
```

数据备份的相关指令, 在后面MySQL管理章节, 还会详细讲解.

3). 释放锁

```
unlock tables ;
```

#### 特点

数据库中加全局锁，是一个比较重的操作，存在以下问题：

- 如果在主库上备份，那么在备份期间都不能执行更新，业务基本上就得停摆。
- 如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志（binlog），会导
  致主从延迟。

在InnoDB引擎中，我们可以在备份时加上参数 --single-transaction 参数来完成不加锁的一致
性数据备份。

```sql
mysqldump --single-transaction -uroot –p123456 itcast > itcast.sql   这个利用的是快照实现的
```

> 案例：开启三个客户端 先执行上面的客户端语句，再执行下面左边的，发现阻塞了。

![image-20220503123546692](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031235957.png)

开启一个窗口，执行备份指令：

![image-20220503124039401](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031240535.png)

查看：

![image-20220503124114899](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031241373.png)

释放锁查看：

![image-20220503124840376](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031248636.png)

==**Ps：如果我们开启了一个事务，执行了update语句，但是还未提交，那么我们加个全局锁去备份。是备份不了的。会一直阻塞。直到你回滚了事务。**==



### 表级锁

#### 介绍

表级锁，每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。应用在MyISAM、
InnoDB、BDB等存储引擎中。

对于表级锁，主要分为以下三类：

- 表锁
- 元数据锁（meta data lock，MDL）
- 意向锁

#### 表锁

对于表锁，分为两类：

- 表共享读锁（read lock）
- 表独占写锁（write lock）

语法：

- 加锁：lock tables 表名... read/write。
- 释放锁：unlock tables / 客户端断开连接 。

特点:

A. 读锁

![image-20220503135715543](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031357849.png)

左侧为客户端一，对指定表加了读锁，不会影响右侧客户端二的读，但是会阻塞右侧客户端的写，同时左侧的客户端也无法写。

> 测试

![image-20220503135813099](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031358894.png)

B. 写锁

![image-20220503135842418](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031358965.png)

左侧为客户端一，对指定表加了写锁，会阻塞右侧客户端的读和写。

> 测试：

![image-20220503135904547](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031359080.png)

==**结论: 读锁不会阻塞其他客户端的读，但是会阻塞写。写锁既会阻塞其他客户端的读，又会阻塞其他客户端的写。**==



####  元数据锁

meta data lock , 元数据锁，简写MDL。

MDL加锁过程==是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。==MDL锁主要作用是`在表上有活动事务的时候，不可以对元数据进行写入操作`。 **为了避免DML与
DDL冲突，保证读写的正确性。**

这里的元数据，大家可以简单理解为就是一张表的表结构。 也就是说，某一张表涉及到未提交的事务
时，是不能够修改这张表的表结构的。

在MySQL5.5中引入了MDL，当对一张表进行`增删改查`的时候，加`MDL读锁(共享)`；当对`表结构进行变
更操作`的时候，`加MDL写锁(排他)。`

常见的SQL操作时，所添加的元数据锁：  

![image-20220504173402948](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205041734250.png)

开启事务 然后select* from xxx或者SELECT * FROM xxx  LOCK IN SHARE MODE; 就上了一个SHARED_READ锁

开启事务，然后UPDATE（也可以是insert/delete） xxx SET `password`='1111' WHERE id=1或者SELECT * FROM xxx FOR UPDATE; ; 就上了一个SHARED_WRITE锁



当一个客户端加了个表锁（比如读锁，其他客户端可以进行事务操作。`但是这个事务操作里面的sql只能是select 否则一直阻塞`）

当一个客户端加了个表锁（比如写锁，其他客户端可以进行事务操作。`但是这个事务操作里面的sql会一直阻塞`）

> 演示：

当执行SELECT、INSERT、UPDATE、DELETE等语句时，添加的是元数据共享锁（SHARED_READ /SHARED_WRITE），之间是兼容的。



当执行SELECT语句时，添加的是元数据共享锁（SHARED_READ），会阻塞元数据排他锁（EXCLUSIVE），之间是互斥的。

![image-20220503140553817](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031405168.png)

我们可以通过下面的SQL，来查看数据库中的`元数据锁的情况`：

```sql
select object_type,object_schema,object_name,lock_type,lock_duration from performance_schema.metadata_locks ;
```

我们在操作过程中，可以通过上述的SQL语句，==来查看元数据锁的加锁情况。==

![image-20220503143416978](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031434278.png)

再开一个事务

![image-20220504162529942](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205041625499.png)

接着执行`alter table score drop column java`会发现出现阻塞，原因是存在元数据锁。如上图两个。

我们提交事务，上图的元数据锁就会释放。再执行`alter table score drop column java`就不会报错啦。



#### 意向锁

1). 介绍

在InnoDB中引入了`意向锁`，使得表锁不用检查每行
数据是否加锁，使用意向锁来减少对行锁的检查。

假如没有意向锁，客户端一对表加了行锁后，客户端二如何给表加表锁呢，来通过示意图简单分析一下：

首先客户端一，开启一个事务，然后执行DML操作，在执行DML语句时，会对涉及到的行加行锁。

![image-20220503144634932](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031446050.png)

当客户端二，想对这张表加表锁时，会检查当前表是否有对应的行锁，如果没有，则添加表锁，此时就会从第一行数据，检查到最后一行数据，效率较低。

![image-20220503144710951](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031447059.png)

有了意向锁之后 :

客户端一，在执行DML操作时，会对涉及的行加行锁，同时也会对该表加上意向锁。

![image-20220503144729549](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031447656.png)

而其他客户端，在对这张表加表锁的时候，会根据该表上所加的意向锁来判定是否可以成功加表锁，而不用逐行判断行锁情况了。

![image-20220503144758382](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031447479.png)

2). 分类

- 意向共享锁(IS): 由语句select ... lock in share mode添加 。 与表锁共享锁(read)兼容，与表锁排他锁(write)互斥。
- 意向排他锁(IX): 由insert、update、delete、select...for update添加 。与表锁共享锁(read)及排他锁(write)都互斥，意向锁之间不会互斥。

==**一旦事务提交了，意向共享锁、意向排他锁，都会自动释放。**==

可以通过以下SQL，查看`意向锁及行锁的加锁情况`：

```sql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```

> 演示：

A. 意向共享锁与表读锁是兼容的  `下面那个是行锁`

==当我们select ....lock in share mode添加行锁的时候。系统会帮我们为涉及到的表加一个意向共享锁。如图的IS即是==

![image-20220503144935035](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031449231.png)

B. 意向排他锁与表读锁、写锁都是互斥的

![image-20220503145004172](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031450475.png)



### 行级锁

#### 介绍

行级锁，每次操作`锁住对应的行数据。锁定粒度最小，发生锁冲突的概率最低，并发度最高`。应用==InnoDB==存储引擎中。

InnoDB的数据是基于索引(`聚集索引`)组织的【聚集索引除了存主键还存行数据】，行锁是通过==对索引上的索引项加锁来实现的，而不是对记录加的锁==。对于行级锁，主要分为以下三类：

- 行锁（Record Lock）：锁定单个行记录的锁，防止其他事务对此行进行update和delete。在==RC(读已提交)、RR（可重复读）==隔离级别下都支持。

![image-20220503150300646](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031503744.png)

- 间隙锁（Gap Lock）：==锁定索引记录间隙（不含该记录），确保索引记录间隙不变，防止其他事务在这个间隙进行insert，产生幻读。在RR隔离级别下都支持。==

![image-20220503150322981](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031503075.png)

- 临键锁（Next-Key Lock）：==行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap。 在RR隔离级别下支持。==

![image-20220503150343006](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031503101.png)



#### 行锁

1). 介绍

InnoDB实现了以下两种类型的行锁：

- 共享锁（S）：允许一个事务去读一行，阻止其他事务获得相同数据集的排它锁。
- 排他锁（X）：允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁。

> 两种行锁的兼容情况如下:

![image-20220503153102555](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031531667.png)

常见的SQL语句，在执行时，所加的行锁如下：

![image-20220503153124959](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031531070.png)

2). 演示

- ==针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁。==
- ==InnoDB的行锁是针对于索引加的锁，不通过索引条件检索数据，那么InnoDB将对表中的所有记录加锁，此时 就会升级为表锁。==



可以通过以下SQL，查看意向锁及行锁的加锁情况：

```sql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;   mysql8里面才能使用这个语句因为data_locks只有在8版本才会出现。5版本的在其他地方
```

**示例演示**

数据准备

```sql
CREATE TABLE `stu` (
`id` int NOT NULL PRIMARY KEY AUTO_INCREMENT,
`name` varchar( 255 ) DEFAULT NULL,
`age` int NOT NULL
) ENGINE = InnoDB CHARACTER SET = utf8mb4;
INSERT INTO `stu` VALUES ( 1 , 'tom', 1 );
INSERT INTO `stu` VALUES ( 3 , 'cat', 3 );
INSERT INTO `stu` VALUES ( 8 , 'rose', 8 );
INSERT INTO `stu` VALUES ( 11 , 'jetty', 11 );
INSERT INTO `stu` VALUES ( 19 , 'lily', 19 );
INSERT INTO `stu` VALUES ( 25 , 'luci', 25 );
```

演示行锁的时候，我们就通过上面这张表来演示一下。

> A. 普通的select语句，执行时，不会加锁。

![image-20220503153354413](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031533626.png)

> B. select...lock in share mode，加共享锁，共享锁与共享锁之间兼容。

==加了某条记录的表共享锁mysql会自动帮我们加上一个意向共享锁。事务提交后，行锁消失后，这个意向共享锁也会消失。==

![image-20220503153429669](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031534990.png)

![image-20220503153437722](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031534859.png)

> 共享锁与排他锁之间互斥。

![image-20220503153458426](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031534745.png)

客户端一获取的是id为1这行的共享锁，客户端二是可以获取id为3这行的排它锁的，因为不是同一行数据。 而如果客户端二想获取id为1这行的排他锁，会处于阻塞状态，因为共享锁与排他锁之间互斥。

> C. 排它锁与排他锁之间互斥

![image-20220503153534747](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031535906.png)

当客户端一，执行update语句，会为id为1的记录加排他锁； 客户端二，如果也执行update语句更新id为1的数据，也要为id为1的数据加排他锁，但是客户端二会处于阻塞状态，因为排他锁之间是互斥的。 直到客户端一，把事务提交了，才会把这一行的行锁释放，此时客户端二，解除阻塞。

> D. 无索引行锁升级为表锁

stu表中数据如下:

![image-20220503153626772](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031536935.png)

我们在两个客户端中执行如下操作:

![image-20220503153643796](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031536000.png)

在客户端一中，开启事务，并执行update语句，更新name为Lily的数据，也就是id为19的记录 。

然后在客户端二中更新id为3的记录，却不能直接执行，会处于阻塞状态，为什么呢？

原因就是因为此时，客户端一，根据name字段进行更新时，name字段是没有索引的，如果没有索引，此时行锁会升级为表锁(==因为行锁是对索引项加的锁，而name没有索引==)。



接下来，我们再针对name字段建立索引，索引建立之后，再次做一个测试：

![image-20220503153722186](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205031537458.png)

此时我们可以看到，客户端一，开启事务，然后依然是根据name进行更新。而客户端二，在更新id为3的数据时，更新成功，并未进入阻塞状态。 这样就说明，我们根据索引字段进行更新操作，就可以避免行锁升级为表锁的情况。



#### 间隙锁&临键锁

默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读（`也只能解决部分幻读`）。

- ==索引上的等值查询(唯一索引)，给不存在的记录加锁时, 优化为间隙锁 。==
- `索引上的等值查询(普通索引)【因为普通索引可能存在多个相同的值】，向右遍历B+树时最后一个值不满足查询需求时，next-key lock 退化为间隙锁。` ==主键索引不是普通索引而是聚集索引==
- 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。

==**注意：间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。**==

案例：

> A. 索引上的等值查询(唯一索引)，给不存在的记录加锁时, 优化为间隙锁 。

![image-20220503202516174](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205032025653.png)

> B. 索引上的等值查询(非唯一普通索引)，向右遍历时最后一个值不满足查询需求时，next-key lock 退化为间隙锁

介绍分析一下：

我们知道InnoDB的B+树索引，叶子节点是有序的双向链表。 假如，我们要根据这个二级索引查询值为18的数据，并加上共享锁，我们是只锁定18这一行就可以了吗？ 并不是，因为是非唯一索引，这个结构中可能有多个18的存在，所以，在加锁时会继续往后找，找到一个不满足条件的值（当前案例中也就是29）。==此时会对18加临键锁（锁住是18及16-18的间隙），并对19-29之间的间隙加锁。==

![image-20220503204311861](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205032043016.png)

![image-20220503204322906](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205032043452.png)

==**第四个锁锁的是3和7之间的间隙，第三个锁锁的是3这条记录，第二个锁锁的是1-3之间的间隙以及3这条记录**==

> C. 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。

![image-20220503204405669](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205032044068.png)

查询的条件为id>=19，并添加共享锁。 此时我们可以根据数据库表中现有的数据，将数据分为三个部分：

[19]  对应第二个锁

(19,25]  对应第四个锁

(25,+∞] 对应第三个锁

==**所以数据库数据在加锁是，就是将19加了行锁，25的临键锁（包含25及19-25之间的间隙），正无穷的临键锁(正无穷及之前的间隙)。**==



==**小柯总结：间隙锁就是只锁间隙。行锁就是只锁某条记录。临键锁既锁记录也锁间隙。相当于间隙锁+行锁。**==



## InnoDB引擎

### **逻辑存储结构**

InnoDB的逻辑存储结构如下图所示:

![image-20220504201215553](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042012942.png)



1). 表空间

表空间是InnoDB存储引擎逻辑结构的最高层， 如果用户启用了参数 innodb_file_per_table(在8.0版本中默认开启) ，则每张表都会有一个表空间（xxx.ibd），一个mysql实例可以对应多个表空间，用于存储记录、索引等数据。（`表空间在/var/lib/mysql下`（==反正我是用docker装的==））



2). 段

段，分为数据段（Leaf node segment）、索引段（Non-leaf node segment）、回滚段（Rollback segment），InnoDB是索引组织表，数据段就是B+树的叶子节点， 索引段即为B+树的非叶子节点。段用来管理多个Extent（区）。

3). 区

区，表空间的单元结构，每个区的大小为1M。 默认情况下， InnoDB存储引擎页大小为16K， 即一个区中一共有64个连续的页。

4). 页

页，是InnoDB 存储引擎磁盘管理的最小单元，每个页的大小默认为 16KB。为了保证页的连续性，InnoDB 存储引擎每次从磁盘申请 4-5 个区。

5). 行

行，InnoDB 存储引擎数据是按行进行存放的。在行中，默认有两个隐藏字段：

Trx_id：==每次对某条记录进行改动时，都会把对应的事务id赋值给trx_id隐藏列。==

Roll_pointer：每次对某条引记录进行改动时，都会把旧的版本写入到undo日志中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。



### **架构** 

**概述**

MySQL5.5 版本开始，默认使用InnoDB存储引擎，它擅长事务处理，具有崩溃恢复特性，在日常开发中使用非常广泛。下面是InnoDB架构图，左侧为内存结构，右侧为磁盘结构。

![image-20220504203639188](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042036415.png)

#### **内存结构**

![image-20220504203659990](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042037241.png)

在左侧的内存结构中，主要分为这么四大块儿： Buffer Pool、Change Buffer、Adaptive Hash Index、Log Buffer。 接下来介绍一下这四个部分。

> Buffer Pool

InnoDB存储引擎==基于磁盘文件存储==，访问物理硬盘和在内存中进行访问，速度相差很大，为了尽可能弥补这两者之间的I/O效率的差值，就需要把经常使用的数据加载到缓冲池中，避免每次访问都进行磁盘I/O。 

在InnoDB的缓冲池中不仅缓存了索引页和数据页，还包含了undo页、插入缓存、自适应哈希索引以及InnoDB的锁信息等等。

缓冲池 Buffer Pool，是主内存中的一个区域，里面可以==缓存磁盘上经常操作的真实数据==，在执行增删改查操作时，先操作缓冲池中的数据，==然后再以一定频率刷新到磁盘，从而减少磁盘IO，加快处理速度。==

缓冲池以Page页为单位，底层采用链表数据结构管理Page。根据状态，将Page分为三种类型：

• free page：空闲page，未被使用。

• clean page：被使用page，数据没有被修改过。

• dirty page：脏页，被使用page，数据被修改过，也中数据与磁盘的数据产生了不一致。

在专用服务器上，通常将多达80％的物理内存分配给缓冲池 。参数设置： show variables like 'innodb_buffer_pool_size';

![image-20220504203737249](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042037459.png)



> Change Buffer

Change Buffer，更改缓冲区（针对于非唯一二级索引页），在执行DML语句时，如果这些数据Page没有在Buffer Pool中，不会直接操作磁盘，而会将数据变更存在更改缓冲区 Change Buffer中，在未来数据被读取时，再将数据合并恢复到Buffer Pool中，再将合并后的数据刷新到磁盘中。

Change Buffer的意义是什么呢?

先来看一幅图，这个是二级索引的结构图：

![image-20220504204349567](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042043830.png)

与聚集索引不同，二级索引通常是非唯一的，并且以相对随机的顺序插入二级索引。同样，删除和更新可能会影响索引树中不相邻的二级索引页，如果每一次都操作磁盘，会造成大量的磁盘IO。有了ChangeBuffer之后，我们可以在缓冲池中进行合并处理，减少磁盘IO。





> Adaptive Hash Index

自适应hash索引，用于优化对Buffer Pool数据的查询。MySQL的innoDB引擎中虽然没有直接支持hash索引，但是给我们提供了一个功能就是这个自适应hash索引。因为前面我们讲到过，hash索引在进行等值匹配时，一般性能是要高于B+树的，因为hash索引一般只需要一次IO即可（`前提是不存在hash碰撞`），而B+树，可能需要几次匹配，所以hash索引的效率要高，==但是hash索引又不适合做范围查询、模糊匹配等。==

InnoDB存储引擎会监控对表上各索引页的查询，如果观察到在特定的条件下hash索引可以提升速度，则建立hash索引，称之为自适应hash索引。

**自适应哈希索引，无需人工干预，是系统根据情况自动完成。**

参数： adaptive_hash_index



> Log Buffer

Log Buffer：日志缓冲区，用来保存要写入到磁盘中的log日志数据（redo log 、undo log），默认大小为 16MB，日志缓冲区的日志会定期刷新到磁盘中。如果需要更新、插入或删除许多行的事务，增加日志缓冲区的大小可以节省磁盘 I/O。

参数:

innodb_log_buffer_size：缓冲区大小

innodb_flush_log_at_trx_commit：日志刷新到磁盘时机，取值主要包含以下三个：

1: 日志在每次事务提交时写入并刷新到磁盘，默认值。

0: 每秒将日志写入并刷新到磁盘一次。

2: 日志在每次事务提交后写入，并每秒刷新到磁盘一次。

![image-20220504205432478](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042054956.png)



 

#### **磁盘结构**

接下来，再来看看InnoDB体系结构的右边部分，也就是磁盘结构：

![image-20220504212313137](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042123349.png)

> System Tablespace

系统表空间是更改缓冲区的存储区域。如果表是在系统表空间而不是每个表文件或通用表空间中创建的，它也可能包含表和索引数据。(在MySQL5.x版本中还包含InnoDB数据字典、undolog等)

参数：innodb_data_file_path

![image-20220504212333712](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042123084.png)

系统表空间，默认的文件名叫 ibdata1。（`默认在/var/lib/mysql下`）

>  File-Per-Table Tablespaces
>

如果开启了innodb_file_per_table开关 ，则每个表的文件表空间包含单个InnoDB表的数据和索引 ，并存储在文件系统上的单个数据文件中。

开关参数：innodb_file_per_table ，该参数默认开启。

![image-20220504212402071](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042124343.png)

那也就是说，我们每创建一个表，都会产生一个表空间文件，如图：

![image-20220504212420135](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042124397.png)

> General Tablespaces

通用表空间，需要通过 CREATE TABLESPACE 语法创建通用表空间，在创建表时，可以指定该表空间。

A. 创建表空间

```sql
CREATE TABLESPACE ts_name ADD DATAFILE 'file_name' ENGINE = engine_name;
```

![image-20220504212451235](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042124452.png)

B. 创建表时指定表空间

```sql
CREATE TABLE xxx ... TABLESPACE ts_name;
```

![image-20220504212522451](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042125898.png)

> Undo Tablespaces

撤销表空间[`用于事务的回滚`]，MySQL实例在初始化时会自动创建两个默认的undo表空间（初始大小16M），用于存储undo log日志。

> Temporary Tablespaces

InnoDB 使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据。

> Doublewrite Buffer Files

双写缓冲区，innoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据页写入双写缓冲区文件中，便于系统异常时恢复数据

![](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042125917.png)

> Redo Log

重做日志，是用来实现事务的持久性。该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log）,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都会存到该redo log中, 用于在刷新脏页到磁盘时,发生错误时, 进行数据恢复使用。以循环方式写入重做日志文件，涉及两个文件：`默认在/var/lib/mysql下`

![image-20220504212618013](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205042126189.png)



### 后台线程

![image-20220505093208162](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205050932421.png)

在InnoDB的后台线程中，分为4类，分别是：Master Thread 、IO Thread、Purge Thread、Page Cleaner Thread。

>  Master Thread
>

核心后台线程，负责调度其他线程，负责将缓冲池中的数据异步刷新到磁盘中, 保持数据的一致性，还包括脏页的刷新、合并插入缓存、undo页的回收 。

> IO Thread

在InnoDB存储引擎中大量使用了AIO来处理IO请求, 这样可以极大地提高数据库的性能，而IO Thread主要负责这些IO请求的回调。

![image-20220505093354757](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205050933063.png)

我们可以通过以下的这条指令，查看到InnoDB的状态信息，其中就包含IO Thread信息

```sql
show engine innodb status \G; 
```

![image-20220505093453886](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205050934357.png)

>  Purge Thread

主要用于回收事务已经提交了的undo log，在事务提交之后，undo log可能不用了，就用它来回收。

> Page Cleaner Thread

协助 Master Thread 刷新脏页到磁盘的线程，它可以减轻 Master Thread 的工作压力，减少阻塞。





## 事务原理

### 事务基础

> 事务

事务 是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

> 在默认的隔离级别下（RR）来做个演示

a客户端：

```sql
BEGIN
UPDATE `user` SET PASSWORD='1q11' WHERE id=1;
```

a客户端查看

![image-20220505095318636](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205050953852.png)

b 客户端查看：SELECT * FROM `user`

![image-20220505095356756](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205050953970.png)

a客户端提交后，b客户端再次查看 SELECT * FROM `user`

![image-20220505095432424](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205050954588.png)

> 特性

• 原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。

• 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。

• 隔离性（Isolation）：==数据库系统提供的隔离机制，保证事务在不受外部并发（`加锁阻塞`）操作影响的独立环境下运行。==

• 持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

`那实际上，我们研究事务的原理，就是研究MySQL的InnoDB引擎是如何保证事务的这四大特性的。`

![image-20220505093756734](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205050937045.png)

而对于这四大特性，实际上分为两个部分。 其中的`原子性`、`一致性`、`持久化`，实际上是由InnoDB中的两份日志来保证的，一份是`redo log`日志，一份是`undo log`日志。 而`隔离性是通过数据库的锁，加上MVCC来保证的。`

![image-20220505093821073](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205050938226.png)

我们在讲解事务原理的时候，主要就是来研究一下redolog，undolog以及MVCC。



### redo log

`重做日志`，记录的是事务提交时==数据页的物理修改==，==是用来实现事务的持久性==。该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log file）,前者是在`内存`中，后者在`磁盘`中。当事务提交之后会把所有修改信息都存到该日志文件中, ==用于在刷新脏页到磁盘,发生错误时, 进行数据恢复使用。如果没有redolog==，可能会存在什么问题的？ 

我们一起来分析一下。

我们知道，在InnoDB引擎中的内存结构中，==主要的内存区域就是缓冲池，在缓冲池中缓存了很多的数据页==。 当我们在一个事务中，执行多个增删改的操作时，InnoDB引擎会先操作缓冲池中的数据，如果缓冲区没有对应的数据，会通过后台线程将磁盘中的数据加载出来，存放在缓冲区中，然后将缓冲池中的数据修改，修改后的数据页我们称为脏页。 而脏页则会在一定的时机，通过后台线程刷新到磁盘中，从而保证缓冲区与磁盘的数据一致。 ==而缓冲区的脏页数据并不是实时刷新的，而是一段时间之后将缓冲区的数据刷新到磁盘中==，==假如刷新到磁盘的过程出错了，而提示给用户事务提交成功，而数据却没有持久化下来，这就出现问题了，没有保证事务的持久性。==

![image-20220505100005318](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051000521.png)

那么，如何解决上述的问题呢？ 在InnoDB中提供了一份日志 redo log，接下来我们再来分析一下，通过redo log如何解决这个问题。

![image-20220505100026025](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051000305.png)

==有了redo log之后，当对缓冲区的数据进行增删改之后，会首先将操作的数据页的变化，记录在redo log buffer中。在事务提交时，会将redo log buffer中的数据刷新到redo log磁盘文件中。过一段时间之后，如果刷新缓冲区的脏页到磁盘时，发生错误，此时就可以借助于redo log进行数据恢复，这样就保证了事务的持久性。 而如果脏页成功刷新到磁盘 或 或者涉及到的数据已经落盘，此时redo log就没有作用了，就可以删除了，所以存在的两个redolog文件是循环写的。==

==那为什么每一次提交事务，要刷新redo log 到磁盘中呢，而不是直接将buffer pool中的脏页刷新到磁盘呢 ?因为在业务操作中，我们操作数据一般都是随机读写磁盘的，而不是顺序读写磁盘。 而redo log在往磁盘文件中写入数据，由于是日志文件，所以都是顺序写的。顺序写的效率，要远大于随机写。 这种先写日志的方式，称之为 WAL（Write-Ahead Logging）。==



### undo log

==回滚日志，用于记录数据被修改前的信息 , 作用包含两个 : 提供回滚(保证事务的原子性) 和MVCC(多版本并发控制) 。undo log和redo log记录物理日志不一样，它是逻辑日志。可以认为当delete一条记录时，undolog中会记录一条对应的insert记录，反之亦然，update一条记录时，它记录一条对应相反的update记录。当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。==

Undo log销毁：undo log在事务执行时产生，事务提交时，`并不会立即删除undo log，因为这些日志可能还用于MVCC`。

Undo log存储：undo log采用段的方式进行管理和记录，存放在前面介绍的 rollback segment回滚段中，内部包含1024个undo log segment。 



### 前提知识：

> 当前读

==读取的是记录的最新版本==，会对读取的记录进行==加锁==。对于我们日常的操作，如：`select ... lock in share mode(共享锁)，select ...for update、update、insert、delete(排他锁)都是一种当前读(也就是行锁)。`

> 测试：

![image-20220505101153176](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051011560.png)

==在测试中我们可以看到，即使是在默认的RR隔离级别下，事务A中依然可以读取到事务B最新提交的内容，因为在查询语句后面加上了 lock in share mode 共享锁，此时是当前读操作。当然，当我们加排他锁的时候，也是当前读操作。==

> 快照读

简单的select（不加锁）就是快照读，快照读，读取的是==记录数据的可见版本==，有可能是历史数据，不加锁，是==非阻塞读。==

• Read Committed：每次select，都生成一个快照读。

• Repeatable Read：开启事务后第一个select语句才是快照读的地方。

• Serializable：快照读会退化为当前读。

> 测试

![image-20220505101254766](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051012170.png)

在测试中,我们看到即使事务B提交了数据,事务A中也查询不到。 原因就是因为普通的select是快照读，而在当前默认的RR隔离级别下，开启事务后第一个select语句才是快照读的地方，后面执行相同的select语句都是从快照中获取数据，可能不是当前的最新数据，==这样也就保证了可重复读。==



### MVCC

全称 Multi-Version Concurrency Control，==多版本并发控制==。==指维护一个数据的多个版本，使得读写操作没有冲突，快照读为MySQL实现MVCC提供了一个非阻塞读功能==。MVCC的具体实现，还需要`依赖`于数据库记录中的`三个隐式字段`、`undo log日志`、`readView`。接下来，我们再来介绍一下InnoDB引擎的表中涉及到的隐藏字段 、undolog 以及 readview，从而来介绍一下MVCC的原理。



#### 隐藏字段

![image-20220505104745411](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051047634.png)

当我们创建了上面的这张表，我们在查看表结构的时候，就可以显式的看到这三个字段。 实际上除了这三个字段以外，InnoDB还会自动的给我们添加三个隐藏字段及其含义分别是：

![image-20220505104829607](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051048758.png)

上述的前两个字段是肯定会添加的， 是否添加最后一个字段DB_ROW_ID，得看当前表有没有主键，如果有主键，则不会添加该隐藏字段。

> 测试

1). 查看有主键的表 stu进入服务器中的 `data目录` , 查看stu的表结构信息, 通过如下指令:

```sql
ibd2sdi stu.ibd(指令8版本才能用)
```

查看到的表结构信息中，有一栏 columns，在其中我们会看到处理我们建表时指定的字段以外，还有额外的两个字段 分别是：DB_TRX_ID 、 DB_ROLL_PTR ，因为该表有主键，所以没有DB_ROW_ID隐藏字段

![image-20220505105244278](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051052612.png)

#### undo log

##### 介绍:

回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志。==当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除==。 `而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会立即被删除。`



##### 版本链

有一张表原始数据为:

![image-20220505120811703](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051208058.png)

- DB_TRX_ID : ==代表最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID，是自增的。==
- DB_ROLL_PTR ： 由于这条数据是才插入的，没有被更新过，所以该字段值为null。

然后，有四个并发事务同时在访问这张表。

> A. 第一步

![image-20220505120846671](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051208884.png)

当事务2执行第一条修改语句时，会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

![image-20220505120907132](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051209437.png)

> B.第二步

![image-20220505120928510](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051209745.png)

当事务3执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

![image-20220505120952211](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051209549.png)

C. 第三步

![image-20220505121006553](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051210710.png)

当事务4执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

![image-20220505121025956](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051210154.png)

==**最终我们发现，不同事务或相同事务对同一条记录进行修改，会导致该记录的undolog生成一条记录版本链表，链表的头部是最新的旧记录，链表尾部是最早的旧记录。**==



#### readview

==ReadView（读视图）是 快照读 SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事务（未提交的）id。==

ReadView中包含了四个核心字段：

![image-20220505121831920](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051218250.png)

而在readview中就规定了版本链数据的访问规则：

==trx_id 代表当前undolog版本链对应事务ID。==

![image-20220505121851186](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051218418.png)

不同的隔离级别，生成ReadView的时机不同：

- READ COMMITTED ：==在事务中每一次执行快照读时生成ReadView。==
- REPEATABLE READ：==仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。==

#### 原理分析

##### RC隔离级别

`RC隔离级`别下，在事务中`每一次执行快照读时生成ReadView`。我们就来分析事务5中，两次快照读读取数据，是如何获取数据的?

在事务5中，查询了两次id为30的记录，由于隔离级别为Read Committed，所以每一次进行快照读都会生成一个ReadView，那么两次生成的ReadView如下。

![image-20220505124005183](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051240586.png)

那么这两次快照读在获取数据时，==就需要根据所生成的ReadView以及ReadView的版本链访问规则，到undolog版本链中匹配数据，最终决定此次快照读返回的数据。==

A. 先来看第一次快照读具体的读取过程：

![image-20220505124035373](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051240606.png)

![image-20220505124048776](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051240937.png)

在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

- 先匹配 ![image-20220505124129399](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051241639.png)这条记录，这条记录对应的trx_id为4，也就是将4带入右侧的匹配规则中。 ①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。

- 再匹配第二条 ，![image-20220505124257512](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051242778.png)这条记录对应的trx_id为3，也就是将3带入右侧的匹配规则中。①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。

- 再匹配第三条 ![image-20220505124333132](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051243323.png)

  ，这条记录对应的trx_id为2，也就是将2带入右侧的匹配规则中。①不满足 ②满足 终止匹配，此次快照读，返回的数据就是版本链中记录的这条数据。

B. 再来看第二次快照读具体的读取过程:

![image-20220505124402539](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051244704.png)

![image-20220505124414510](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051244742.png)

在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

- 先匹配 ![image-20220505124445800](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051244127.png)这条记录，这条记录对应的trx_id为4，也就是将4带入右侧的匹配规则中。 ①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第二条 ![image-20220505124504739](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051245962.png)

，这条记录对应的trx_id为3，也就是将3带入右侧的匹配规则中。①不满足 ②满足 。终止匹配，此次快照读，返回的数据就是版本链中记录的这条数据



##### RR隔离级别

RR隔离级别下，仅在事务中`第一次`执行快照读时生成ReadView，后续复用该ReadView。 而`RR 是可重复读`，在一个事务中，执行`两次相同的select语句，查询到的结果是一样的`。

那MySQL是如何做到可重复读的呢? 我们简单分析一下就知道了

![image-20220505124615443](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051246663.png)

==**我们看到，在RR隔离级别下，只是在事务中第一次快照读时生成ReadView，后续都是复用该ReadView，那么既然ReadView都一样， ReadView的版本链匹配规则也一样， 那么最终快照读返回的结果也是一样的。**==

`所以呢，MVCC的实现原理就是通过 InnoDB表的隐藏字段、UndoLog 版本链、ReadView来实现的。`

`而MVCC + 锁，则实现了事务的隔离性。 而一致性则是由redolog 与 undolog保证。`

![image-20220505124653586](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051246808.png)



## MySQL管理

### 系统数据库

Mysql数据库安装完成后，自带了一下四个数据库，具体作用如下：

![image-20220505164147971](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205051641160.png)

### 常用工具

#### **mysql**（客户端工具）

该mysql不是指mysql服务，而是指mysql的客户端工具。

![image-20220505210247844](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052102162.png)

-e选项可以在Mysql客户端执行SQL语句，而不用连接到MySQL数据库再执行。

> 示例

```sql
mysql -uroot –p123456 db01 -e "select * from stu";
```

![image-20220505210022336](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052100853.png)

![image-20220505211428216](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052114424.png)

#### mysqladmin（管理工具）

mysqladmin 是一个执行`管理操作的客户端程序`。可以用它来检查服务器的`配置`和`当前状态`、`创建并删除数据库`等。

```sql
通过帮助文档查看选项： 
mysqladmin --help
```

![image-20220505210353327](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052103860.png)

![image-20220505210424527](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052104768.png)

> 案例：

```sql
mysqladmin -uroot –p1234 drop 'test01'; 
mysqladmin -uroot –p1234 version;
```

#### **mysqlbinlog**（二进制查看工具）

由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到`mysqlbinlog 日志管理工具`

![image-20220505210532383](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052105707.png)

> 案例：

A. 查看 binlog.000008这个二进制文件中的数据信息`此二进制文件在/var/lib/mysql下`

![image-20220505210610778](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052106349.png)

上述查看到的二进制日志文件数据信息量太多了，不方便查询。 我们可以加上一个参数 -s 来显示简单格式。

![image-20220505210632853](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052106509.png)

#### **mysqlshow（数据库查找工具）**

mysqlshow 客户端对象查找工具，用来很快地`查找`存在`哪些数据库`、`数据库中的表`、`表中的列或者索引`。

![image-20220505210721105](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052107323.png)

> 示例：

A. 查询每个数据库的表的数量及表中记录的数量

```sql
mysqlshow -uroot -p1234 --count
```

此目录为`/var/lib/mysql`

![image-20220505210848338](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052108715.png)

B. 查看数据库db01的统计信息

```sql
mysqlshow -uroot -p1234 db01 --count
```

![image-20220505213340638](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052133027.png)

C. 查看数据库db01中的course表的信息

```sql
mysqlshow -uroot -p1234 db01 course --count
```

![image-20220505211015621](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052110904.png)



D. 查看数据库db01中的course表的id字段的信息

```sql
mysqlshow -uroot -p1234 db01 course id --count
```

![image-20220505211053868](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052110277.png)



#### **mysqldump（数据库备份工具）**

mysqldump 客户端工具用来`备份数据库`或`在不同数据库之间进行数据迁移`。备份内容包含`创建表`，及`插入表的SQL语句`。

![image-20220505214820209](../AppData/Roaming/Typora/typora-user-images/image-20220505214820209.png)

A. 备份drug数据库

```sql
mysqldump -uroot -p123456 drug > drug01.sql
```

![image-20220505215819702](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052158909.png)

可以直接打开drug01.sql，来查看备份出来的数据到底什么样。

![image-20220505215946619](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052159935.png)

备份出来的数据包含：

- 删除表的语句
- 创建表的语句
- 数据插入语句

如果我们在数据备份时，不需要创建表，或者不需要备份数据，只需要备份表结构，都可以通过对应的参数来实现。



B. 备份db01数据库中的表数据，不备份表结构(-t)

```sql
mysqldump -uroot -p123456 -t drug > drug02.sql
```

![image-20220505220205051](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052202599.png)

打开drugb02.sql ，来查看备份的数据，只有insert语句，没有备份表结构。

![image-20220505220355204](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052203471.png)

C. 将drug数据库的表的表结构与数据分开备份(-T)

```sql
mysqldump -uroot -p123456 -T /root drug（库） problem(表)
```

![image-20220505220538992](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052205471.png)

执行上述指令，会出错，数据不能完成备份，原因是因为我们所指定的数据存放目录/root，MySQL认为是不安全的，需要存储在MySQL信任的目录下。那么，哪个目录才是MySQL信任的目录呢，可以查看一下系统变量 secure_file_priv 。执行结果如下（`这个是黑马老师的`）：

![image-20220505215143587](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052151840.png)

我用docker部署的mysql，这玩意是null，null的话意味不能备份写出txt文件,解决方法：

[(14条消息) MySQL - 34Docker上出现secure-file-priv为null_小沈曰的博客-CSDN博客](https://blog.csdn.net/shentian885/article/details/106584000?ops_request_misc=%7B%22request%5Fid%22%3A%22165180798116781435448698%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=165180798116781435448698&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-4-106584000-null-null.142^v9^pc_search_result_cache,157^v4^new_style&utm_term=如何解决docker下mysql的secure_file_priv为null&spm=1018.2226.3001.4187)

如果用的是宝塔安装mysql：直接在下图这里手动添加即可。

![image-20220506145840884](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205061458134.png)

按照上面的博客，我指定的路径如下：

![image-20220506124534764](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205061245088.png)

解决bug以后

```sql
mysqldump -uroot -p123456 -T /root drug（库） problem(表)
```

![image-20220506130931336](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205061309654.png)

上述的文件 problem.sql 中记录的就是表结构文件（`没有数据`），而 problem.txt 就是表数据文件，但是需要注意表数据文件，并不是记录一条条的insert语句，而是按照一定的格式记录表结构中的数据。如下。

![image-20220510111149071](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205101111610.png)

#### **mysqlimport/source（数据导入工具）**

1). mysqlimport

mysqlimport 是客户端`数据导入工具`，用来`导入mysqldump 加 -T 参数后导出的文本文件`

![image-20220505215252821](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052152180.png)

先删除score表的数据，然后执行下面指令

![image-20220510112606419](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205101126686.png)

2). source

如果需要导入sql文件,可以使用mysql中的source 指令 : 

![image-20220505215320155](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202205052153458.png)





# 数据类型

## 整型

| 类型名称      | 取值范围                                  | 大小    |
| ------------- | ----------------------------------------- | ------- |
| TINYINT       | -128〜127                                 | 1个字节 |
| SMALLINT      | -32768〜32767                             | 2个宇节 |
| MEDIUMINT     | -8388608〜8388607                         | 3个字节 |
| INT (INTEGHR) | -2147483648〜2147483647                   | 4个字节 |
| BIGINT        | -9223372036854775808〜9223372036854775807 | 8个字节 |

无符号在数据类型后加 unsigned 关键字。

## 浮点型

| 类型名称            | 说明               | 存储需求   |
| ------------------- | ------------------ | ---------- |
| FLOAT               | 单精度浮点数       | 4 个字节   |
| DOUBLE              | 双精度浮点数       | 8 个字节   |
| DECIMAL (M, D)，DEC | 压缩的“严格”定点数 | M+2 个字节 |

## 日期和时间

| 类型名称  | 日期格式            | 日期范围                                          | 存储需求 |
| --------- | ------------------- | ------------------------------------------------- | -------- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1 个字节 |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3 个字节 |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3 个字节 |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8 个字节 |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC | 4 个字节 |

## 字符串

| 类型名称   | 说明                                         | 存储需求                                                   |
| ---------- | -------------------------------------------- | ---------------------------------------------------------- |
| CHAR(M)    | 固定长度非二进制字符串                       | M 字节，1<=M<=255                                          |
| VARCHAR(M) | 变长非二进制字符串                           | L+1字节，在此，L< = M和 1<=M<=255                          |
| TINYTEXT   | 非常小的非二进制字符串                       | L+1字节，在此，L<2^8                                       |
| TEXT       | 小的非二进制字符串                           | L+2字节，在此，L<2^16                                      |
| MEDIUMTEXT | 中等大小的非二进制字符串                     | L+3字节，在此，L<2^24                                      |
| LONGTEXT   | 大的非二进制字符串                           | L+4字节，在此，L<2^32                                      |
| ENUM       | 枚举类型，只能有一个枚举字符串值             | 1或2个字节，取决于枚举值的数目 (最大值为65535)             |
| SET        | 一个设置，字符串对象可以有零个或 多个SET成员 | 1、2、3、4或8个字节，取决于集合 成员的数量（最多64个成员） |

## 二进制类型

| 类型名称       | 说明                 | 存储需求               |
| -------------- | -------------------- | ---------------------- |
| BIT(M)         | 位字段类型           | 大约 (M+7)/8 字节      |
| BINARY(M)      | 固定长度二进制字符串 | M 字节                 |
| VARBINARY (M)  | 可变长度二进制字符串 | M+1 字节               |
| TINYBLOB (M)   | 非常小的BLOB         | L+1 字节，在此，L<2^8  |
| BLOB (M)       | 小 BLOB              | L+2 字节，在此，L<2^16 |
| MEDIUMBLOB (M) | 中等大小的BLOB       | L+3 字节，在此，L<2^24 |
| LONGBLOB (M)   | 非常大的BLOB         | L+4 字节，在此，L<2^32 |

# 权限一览表

> 具体权限的作用详见[官方文档](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html "官方文档")

GRANT 和 REVOKE 允许的静态权限

| Privilege                                                    | Grant Table Column           | Context                               |
| :----------------------------------------------------------- | :--------------------------- | :------------------------------------ |
| [`ALL [PRIVILEGES]`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_all) | Synonym for “all privileges” | Server administration                 |
| [`ALTER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_alter) | `Alter_priv`                 | Tables                                |
| [`ALTER ROUTINE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_alter-routine) | `Alter_routine_priv`         | Stored routines                       |
| [`CREATE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create) | `Create_priv`                | Databases, tables, or indexes         |
| [`CREATE ROLE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-role) | `Create_role_priv`           | Server administration                 |
| [`CREATE ROUTINE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-routine) | `Create_routine_priv`        | Stored routines                       |
| [`CREATE TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-tablespace) | `Create_tablespace_priv`     | Server administration                 |
| [`CREATE TEMPORARY TABLES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-temporary-tables) | `Create_tmp_table_priv`      | Tables                                |
| [`CREATE USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-user) | `Create_user_priv`           | Server administration                 |
| [`CREATE VIEW`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_create-view) | `Create_view_priv`           | Views                                 |
| [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_delete) | `Delete_priv`                | Tables                                |
| [`DROP`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_drop) | `Drop_priv`                  | Databases, tables, or views           |
| [`DROP ROLE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_drop-role) | `Drop_role_priv`             | Server administration                 |
| [`EVENT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_event) | `Event_priv`                 | Databases                             |
| [`EXECUTE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_execute) | `Execute_priv`               | Stored routines                       |
| [`FILE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_file) | `File_priv`                  | File access on server host            |
| [`GRANT OPTION`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_grant-option) | `Grant_priv`                 | Databases, tables, or stored routines |
| [`INDEX`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_index) | `Index_priv`                 | Tables                                |
| [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_insert) | `Insert_priv`                | Tables or columns                     |
| [`LOCK TABLES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_lock-tables) | `Lock_tables_priv`           | Databases                             |
| [`PROCESS`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_process) | `Process_priv`               | Server administration                 |
| [`PROXY`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_proxy) | See `proxies_priv` table     | Server administration                 |
| [`REFERENCES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_references) | `References_priv`            | Databases or tables                   |
| [`RELOAD`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_reload) | `Reload_priv`                | Server administration                 |
| [`REPLICATION CLIENT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_replication-client) | `Repl_client_priv`           | Server administration                 |
| [`REPLICATION SLAVE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_replication-slave) | `Repl_slave_priv`            | Server administration                 |
| [`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_select) | `Select_priv`                | Tables or columns                     |
| [`SHOW DATABASES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_show-databases) | `Show_db_priv`               | Server administration                 |
| [`SHOW VIEW`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_show-view) | `Show_view_priv`             | Views                                 |
| [`SHUTDOWN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_shutdown) | `Shutdown_priv`              | Server administration                 |
| [`SUPER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_super) | `Super_priv`                 | Server administration                 |
| [`TRIGGER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_trigger) | `Trigger_priv`               | Tables                                |
| [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_update) | `Update_priv`                | Tables or columns                     |
| [`USAGE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_usage) | Synonym for “no privileges”  | Server administration                 |

GRANT 和 REVOKE 允许的动态权限

| Privilege                                                    | Context                                           |
| :----------------------------------------------------------- | :------------------------------------------------ |
| [`APPLICATION_PASSWORD_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_application-password-admin) | Dual password administration                      |
| [`AUDIT_ABORT_EXEMPT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_audit-abort-exempt) | Allow queries blocked by audit log filter         |
| [`AUDIT_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_audit-admin) | Audit log administration                          |
| [`AUTHENTICATION_POLICY_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_authentication-policy-admin) | Authentication administration                     |
| [`BACKUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_backup-admin) | Backup administration                             |
| [`BINLOG_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_binlog-admin) | Backup and Replication administration             |
| [`BINLOG_ENCRYPTION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_binlog-encryption-admin) | Backup and Replication administration             |
| [`CLONE_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_clone-admin) | Clone administration                              |
| [`CONNECTION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_connection-admin) | Server administration                             |
| [`ENCRYPTION_KEY_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_encryption-key-admin) | Server administration                             |
| [`FIREWALL_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_firewall-admin) | Firewall administration                           |
| [`FIREWALL_EXEMPT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_firewall-exempt) | Firewall administration                           |
| [`FIREWALL_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_firewall-user) | Firewall administration                           |
| [`FLUSH_OPTIMIZER_COSTS`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_flush-optimizer-costs) | Server administration                             |
| [`FLUSH_STATUS`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_flush-status) | Server administration                             |
| [`FLUSH_TABLES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_flush-tables) | Server administration                             |
| [`FLUSH_USER_RESOURCES`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_flush-user-resources) | Server administration                             |
| [`GROUP_REPLICATION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_group-replication-admin) | Replication administration                        |
| [`GROUP_REPLICATION_STREAM`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_group-replication-stream) | Replication administration                        |
| [`INNODB_REDO_LOG_ARCHIVE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_innodb-redo-log-archive) | Redo log archiving administration                 |
| [`NDB_STORED_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_ndb-stored-user) | NDB Cluster                                       |
| [`PASSWORDLESS_USER_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_passwordless-user-admin) | Authentication administration                     |
| [`PERSIST_RO_VARIABLES_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_persist-ro-variables-admin) | Server administration                             |
| [`REPLICATION_APPLIER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_replication-applier) | `PRIVILEGE_CHECKS_USER` for a replication channel |
| [`REPLICATION_SLAVE_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_replication-slave-admin) | Replication administration                        |
| [`RESOURCE_GROUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_resource-group-admin) | Resource group administration                     |
| [`RESOURCE_GROUP_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_resource-group-user) | Resource group administration                     |
| [`ROLE_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_role-admin) | Server administration                             |
| [`SESSION_VARIABLES_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_session-variables-admin) | Server administration                             |
| [`SET_USER_ID`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_set-user-id) | Server administration                             |
| [`SHOW_ROUTINE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_show-routine) | Server administration                             |
| [`SYSTEM_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_system-user) | Server administration                             |
| [`SYSTEM_VARIABLES_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_system-variables-admin) | Server administration                             |
| [`TABLE_ENCRYPTION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_table-encryption-admin) | Server administration                             |
| [`VERSION_TOKEN_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_version-token-admin) | Server administration                             |
| [`XA_RECOVER_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_xa-recover-admin) | Server administration                             |

# 图形化界面工具

- Workbench(免费): http://dev.mysql.com/downloads/workbench/
- navicat(收费，试用版30天): https://www.navicat.com/en/download/navicat-for-mysql
- Sequel Pro(开源免费，仅支持Mac OS): http://www.sequelpro.com/
- HeidiSQL(免费): http://www.heidisql.com/
- phpMyAdmin(免费): https://www.phpmyadmin.net/
- SQLyog: https://sqlyog.en.softonic.com/

# 安装

# 小技巧

1. 在SQL语句之后加上`\G`会将结果的表格形式转换成行文本形式
2. 查看Mysql数据库占用空间：
```mysql
SELECT table_schema "Database Name"
     , SUM(data_length + index_length) / (1024 * 1024) "Database Size in MB"
FROM information_schema.TABLES
GROUP BY table_schema;
```