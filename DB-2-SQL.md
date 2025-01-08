# 问题记录



# 待归档区



## 1.3SQL简介

**SQL：结构化查询语言（操作关系型数据通用的标准）**

<font color='blue'>MySQ与Oracle的方言差异？</font>

### 1.3.1SQL通用语法

SQL语句可以单行或者多行书写，以分号结尾

SQL语句可以使用空格/缩进来增强语句的可阅读性

MySQL数据库的SQL语句不区分大小写（<font color='blue'>有区分的数据库吗吗</font>），建议语句大写，表名、字段名蛇形命名法

单行注释： -- 注释内容（--与注释内容由空格）或者#注释内容；

多行注释:：/* 注释内容 */

### 1.3.2分类

SQL语句根据其功能分为四大类：DDL、DML、DQL、DCL

| 分类 | 全称                       | 说明                                                   |
| ---- | -------------------------- | ------------------------------------------------------ |
| DDL  | Data Difinition Language   | 数据定义语言，用来定义数据库（数据库，表，字段）       |
| DML  | Data Manipulation Language | 数据操作语言，用来对数据库表中的数据进行增删改         |
| DQL  | Data Query Language        | 数据查询语言，用来查询数据库中表的记录                 |
| DCL  | Data Control Language      | 数据控制语言，用来创建数据库用户、控制数据库的访问权限 |



# 2数据库设计-DDL

## 2.1数据库操作

DDL全称Data Definition Language（数据定义语言）用来定义数据库对象

DDL中数据库常见操作：查询，创建，使用，删除。

**查询所有数据库**

```sql
SHOW DATABASES;
```

**使用指定数据库**

```sql
USE 数据库名;
```

**查询当前使用数据库**

```sql
SELECT DATABASE();
```

**创建数据库：**数据库的名称一旦确定不能改变

```sql
CREATE DATABASE [IF NOT EXISTS] 数据库名;
```

**删除数据库：**一般项目初始化可以用（项目上线之后绝对不能用）

```sql
DROP DATABASE [IF EXISTS] 数据库名;
```

**查询数据库中各表的信息**

```sql
SHOW TABLE STATUS;
```



## 2.2表操作

### 2.2.1数据类型

MySQL中的数据类型有很多，主要分为三类：数值类型、字符串类型、日期时间类型。

- 数值类型：
  1. tinyint（1字节byte）(-128~127 )整数值
  2. int（4字节byte）(-2147483648，2147483647)大整数值
  3. bigint（8字节byte）（-2^63，2^63-1）极大整数值【可以用作id类的定义】
  4. double（8字节byte）(-1.7976931348623157 E+308，1.7976931348623157 E+308)额外：double（m,n）m：最长位数 n：小数点后位数
  5. <font color='blue'>decimal类型为什么不用，为什么它计算可以能会double类型少损失精度为什么不用</font>
- 字符串类型：
  1. char（0-255 bytes）定长字符串，额外：char（10）最多只能存10个字符，不足10个字符，占用10个字符空间（<font color='blue'>char型最大可声明多大？255/3？？</font>）
  2. varchar（0-65535 bytes）变长字符串，额外：varchar（10）最多只能存储10个字符，不足10个字符，按照实际长度存储
- 日期时间类型：
  1. date（0-3 byte）（1000-01-01至9999-12-31）（格式：YYYY-MM-DD）日期类型，数据库会校验。2010-5-6可以存入，<font color='blue'>底层校验原理，性能？</font>
  2. datetime（0-8 byte）（1000-01-01 00:00:00至9999-12-31 23:59:59）（格式：YYYY-MM-DD HH:MM:SS）日期时间类型

### 2.2.2约束类型

概念：所谓约束就是作用到表中字段上的规则，用于限制存储在表中的数据。

作用：用来保证数据库当中数据的正确性，有效性和完整性。后边学习会验证这些

在MySQL数据库当中，提供了以下5种约束：

| 约束     | 关键字      | 描述                                         |
| -------- | ----------- | -------------------------------------------- |
| 非空约束 | NOT NULL    | 限制该字段不能为null                         |
| 唯一约束 | UNIQUE      | 保证字段的所有数据都是唯一的、不重复的       |
| 主键约束 | PRIMARY KEY | 主键是一行数据的唯一表示，要求非空且唯一     |
| 默认约束 | DEFAULT     | 保存数据时，如果未指定该字段值，则采用默认值 |
| 外键约束 | FOREIGN KEY | 让两张表建立连接，保证数据的一致性和完整性   |

> 约束时作用于表中字段上的，可以在创建、修改表的时候添加约束

### 2.2.3创建表

- 创建表语法：


```sql
CREATE TABLE 表名(
	字段1名字 字段1类型 [约束] [COMMENT '字段1注释内容'],
	字段2名字 字段2类型 [约束] [COMMENT '字段2注释内容'],
	...
	字段n名字 字段n类型 [约束] [COMMENT '字段n注释内容'],
)[ENGINE=引擎名字][COMMENT '表注释内容'];
```

- 创建表代码：


```sql
USE cp_410_ddl;
CREATE TABLE emps(
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT COMMENT'员工id',
    username VARCHAR(20) NOT NULL UNIQUE COMMENT '用户名',
    name VARCHAR(10) NOT NULL COMMENT '姓名',
    gender CHAR(1) DEFAULT '男' COMMENT'性别',
    image VARCHAR(255) COMMENT '图片URL',
    job VARCHAR(10) COMMENT '职位',
    join_time DATE NOT NULL COMMENT '加入时间',
    create_time DATETIME NOT NULL COMMENT '创建时间',
    update_time DATETIME NOT NULL COMMENT '最后一次更新时间'
) COMMENT'员工信息表';
```

### 2.2.4删除表

删除表语法

```sql
DROP TABLE 表名;
```

删除表代码

```sql
DROP TABLE emps;
```

### 2.2.5查询表

**查询当前数据库下所有的表名称**

```sql
SHOW TABLES;
```

**查询表结构**

```sql
DESC EMPS;
```

**查询建表的语句**（由数据库生成，不一定能对应）

```sql
SHOW CREATE TABLE emps;
```



```sql
CREATE TABLE `emp_100w` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `username` varchar(20) NOT NULL COMMENT '用户名',
  `password` varchar(32) DEFAULT '123456' COMMENT '密码',
  `name` varchar(10) NOT NULL COMMENT '姓名',
  `gender` tinyint(3) unsigned NOT NULL COMMENT '性别, 说明: 1 男, 2 女',
  `image` varchar(300) DEFAULT NULL COMMENT '图像',
  `job` tinyint(3) unsigned DEFAULT NULL COMMENT '职位, 说明: 1 班主任,2 讲师, 3 学工主管, 4 教研主管, 5 咨询师',
  `entrydate` date DEFAULT NULL COMMENT '入职时间',
  `dept_id` int(10) unsigned DEFAULT NULL COMMENT '部门ID',
  `create_time` datetime NOT NULL COMMENT '创建时间',
  `update_time` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `username` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=5194242 DEFAULT CHARSET=utf8 COMMENT='员工表
```

> `在MySQL中是转义符，表示其中内容是数据库名，表名，字段名，不是关键字

### 2.2.5修改表

> 关于表结构的修改操作，工作中一般都是直接基于图形化界面操作，

**1.修改表名**

- 语法：

```sql
RENAME TABLE 原表名 TO 新表名；
```

- 代码：

```sql
RENAME TABLE emps TO emp;
```

**2.添加字段**

- 注意事项：<font color='blue'>新增字段会默认在表的末尾列，不能调整位置，为什么？</font>

- 语法：

```sql
ALTER TABLE 表名 ADD 字段名 类型（长度） [COMMENT '注释内容'] [约束]
```

- 代码：

```sql
ALTER TABLE emps ADD slary double(8,2) COMMENT '薪水';
```

**3.修改字段类型**

- 语法：

```sql
ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）;
```

- 代码：

```sql
ALTER TABLE emps MODIFY slary double(10,2);
```

**4.修改字段名字：**（未提供单独的修改字段名的方法，不写新约束，保留之前的约束）

- 语法：

```sql
ALTER TABLE 表名 CHANGE 原字段名 新字段名 类型（长度）[COMMENT '注释内容'] [约束];
```

- 代码：

```sql
ALTER TABLE emps CHANGE  id new_id BIGINT;
```

**5.删除字段：**

- 语法：

```sql
ALTER TABLE 表名 DROP COLUMN 字段名;
```

- 代码：

```sql
ALTER TABLE emps DROP COLUMN slary;
```

# 3.数据库操作-DML

DML：Data Manipulation Language（数据库操作语言）用来对数据库的记录进行增删改操作。

## 3.1增加数据

**1.向全部字段添加数据（单条）**

- 语法：

```sql
INSERT INTO 表名 VALUES(值1,值2,...);
```

- 代码：

```sql
INSERT INTO emps VALUES(NULL,'LXL','李小龙','男',NULL,'讲师','2020-10-10',NOW(),NOW());
```

**2.向指定字段添加数据（单条）**

- 语法：

```sql
INSERT INTO 表名 (字段名1,字段名2) VALUES (值1,值2); 
```

- 代码：

```sql
INSERT INTO emps (id,username,name,gender,image,job,join_time,create_time, update_time) VALUES (NULL,'ZXH','张小花','女',NULL,'保洁','2022-5-6',NOW(),NOW());
```

**4.向全部字段添加数据（批量）**

- 语法：

```sql
INSERT INTO 表名 VALUES (值1,值2,...),(值1,值2,...); 
```

- 代码：

```sql
INSERT INTO emps VALUES (NULL,'LTZ','刘铁柱',NULL,NULL,'财务','2012-04-15',NOW(),NOW()),
                        (NULL,'ZAM','赵爱民',NULL,NULL,'销售','2015-05-25',NOW(),NOW());
```

**4.向指定字段添加数据（批量）**

- 语法：

```sql
INSERT INTO 表名 (字段名1,字段名2) VALUES (值1,值2),(值1,值2);
```

- 代码：

```sql
INSERT INTO emps (name,gender) VALUES ('刘铁柱','男'),('赵爱民','男');
```

## 3.2修改数据

注意事项：

如果没有WHERE则是修改全部的，但是DataGrip会提醒Unsafe Query。

语法：

```sql
UPDATE 表名 SET 字段名1=值1 , 字段名2=值2,....[WHERE 条件]
```

代码：

```sql
UPDATE emps SET job='行政主管',join_time='2000-5-6' WHERE ID=4;
```



## 3.3删除数据

注意事项：

如果没有WHERE则是删除全部的，但是DataGrip会提醒Unsafe Query。

语法：

```sql
DELETE FROM 表名 [WHERE 条件]
```

代码：

```sql
DELETE FROM emps WHERE ID=4;
```







