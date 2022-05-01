# MySQL

# 1 初始 mySql

```txt
只会写代码，数据库，混饭吃；
操作系统 数据结构与算法，不错的程序员；
离散数学，数字电路，体系结构，编译原理 +实战经验，高级程序员
```

javaEE 企业级Java开发，web

- 前端：（页面，展示，数据）
- 后台：（连接点：连接数据库JDBC ，连接前端（控制，控制视图跳转，和给前端传递数据））
- 数据库：（存数据，txt,Excel.word)

## 1.1 什么是数据库：

概念：数据仓库，一个软件，安装在操作系统之上

作用：存储数据，管理数据

## 1.2 数据库分类

关系型数据库：（SQL）

- MySql ， Oracle，Sql Server，DB2，SQLite
- 通过表和表之间，行和列之间的关系进行数据的存储，

非关系型数据库：(NoSQL) Not Only SQL

- Redis, MongDB
- 对象存储，通过对象自身的属性来决定。

**DBMS(数据库管理系统)**

- 数据库的管理软件，科学有效的管理我们的数据，维护和获取数据；
  - DBMS: 管理和操作数据
  - 数据库：存储数据
- MySql本质上就是一个DBMS，因为不仅可以存储数据，还可以管理和操作数据

## 1.3 安装Mysql

- 安装**压缩包版本**更简单
- sc delete mysql 清空服务，就是把mysql的配置全删掉，然后就可以重新配置

## 1.4 安装 SQLyog

- 可视化软件
- 企业版注册名与注册码：
  - kuangshen
  - 8d8120df-a5c3-4989-8f47-5afc79c56e7c
- 每一个sqlyog的执行过程，本质上都对应sql语句，可以历史记录里查看

安装完后：

- 新建一个数据库school
- 自己添加多条记录

## 1.5 命令行操作

```sql
mysql -uroot -p123456 --连接数据库
update mysql.user set authentication_string=password("123456") where user="root" and host = "localhost";--修改用户密码
flush privileges;--刷新权限
--所有的sql语句都用;结尾
show datebases;--查看所有数据库
use school;--use 数据库名 切换数据库
show tables;--查看数据库中所有的表
describe student;--显示数据库中所有的表信息
create database student1;--创建一个数据库
exit;--退出连接

--单行注释（SQL 本来的注释）
/*
sql的多行注释
*/


```

## 1.6 数据库语言分类

数据库xxx语言

DDL 定义

DML 操作

DQL 查询

DCL 控制

1. **数据查询语言**DQL（Data Query Language）：SELECT、WHERE、ORDER BY、GROUP BY、HAVING；
2. **数据定义语言**DDL（Data Definition Language）：CREATE、ALTER、DROP；
3. **数据操作语言**DML（Data Manipulation Language）：INSERT、UNPATE、DELETE；
4. **事务处理语言**TPL（Transaction Process Language）：COMMIT、ROLLBACK；
5. **数据控制语言**DCL（Data Control Language）：GRANT、REVOKE。

# 2 操作数据库

- 操作数据库->操作数据库中的表->操作数据库表中的数据
- mysql关键字**不区分大小写**

## 2.1 操作数据库（了解）

1、创建数据库

```sql
create database [if not exists] joker;
```

2、删除数据库

```sql
DROP DATABASE IF EXISTS joker;
```

3、使用数据库

```sql
--tab上面的`,如果你的表名或者字段名是一个特殊字符，就需要带
use `school`;
```

4、查看数据库

```sql
show databases;
```

**学习方法**

- 对照sqlyog可视化历史记录查看sql
- 固定的语法或关键字必须记住

## 2.2 数据库的数据类型（列类型）

- 只列出了常用的

**1. 数值类型**：

| tinyint   | 十分小的数据                           | 1字节 |
| --------- | -------------------------------------- | ----- |
| smallint  | 较小的数据                             | 2字节 |
| mediumint | 中等的数据                             | 3字节 |
| int       | 标准的整数**常用**                     | 4字节 |
| bigint    | 较大的数据                             | 8字节 |
| float     | 浮点数                                 | 4字节 |
| double    | 浮点数，两个浮点数都存在精度问题       | 8字节 |
| decimal   | 字符串形式的浮点数，金融计算的时候使用 |       |

**2. 字符串类型**

| char         | 字符串固定大小的                               | 0~255   |
| ------------ | ---------------------------------------------- | ------- |
| **varchar ** | 可变字符串，**最常用的变量**，对应Java的String | 0~65535 |
| tinytext     | 微型文本,写博客就够用                          | 2^8 - 1 |
| text         | 文本串，写一本书都够用,**常用保存大文本**      | 2^16 -1 |

**3. 时间日期**

| date      | YYYY-MM-DD,            | 日期格式               |
| --------- | ---------------------- | ---------------------- |
| time      | HH:mm:ss               | 时间格式               |
| datetime  | YYYY-MM-DD HH:mm:ss    | **最常用**             |
| timestamp | 1970.1.1到现在的毫秒数 | 时间戳，**也较为常用** |
| year      | 年份表示               |                        |

**4. null**

- 没有值，未知
- 注意，不要使用NULL进行运算，使用了结果也为NULL

## 2.3 数据库的字段属性(重点)

- unsigned
  - 是无符号的整数
  - 声明了该列不能为负数
- zerofill
  - 0填充
  - 不足的位数，用0来填充
  - 比如，int(3) , 5……005
- 自增（auto_increment)
  - 自动在上一条记录的基础上 +1（默认），也可以+其它
  - 通常用来设计唯一的主键，比如 index,必须是整数类型
  - 可以自定义设计主键的起始值和步长
- 非空 (NULL 和NOT NULL)
  - 假设设置为NOT NULL ，如果不给他赋值，就会报错
  - NULL，如果不填写值，默认就是null
  -  
- 默认
  - 设置默认的值
  - 比如，sex ,默认值为 男，如果不指定该列的值，则默认为该值

拓展：（看看就好）

```sql
/*每一个表，都必须存在一下五个字段，未来做项目用的，表示一个记录存在的意义
id 主键
`version`乐观锁
is_delete 伪删除 1
gmt_create 创建时间
gmt_update 修改时间
*/
```

## 2.4 创建数据库表（重点）

```sql

-- 注意点，使用英文(), 表的名称 和字段 尽量用 ``括起来
-- 字符串使用单引号括起来
-- 所有的语句后面加 ,(英文的),最后一个不用加
-- PRIMARY KEY 主键，一般一个表里只有一个唯一的主键

CREATE TABLE IF NOT EXISTS `joker`(
`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
`sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别',
`birthday` DATETIME DEFAULT NULL COMMENT '生日',
`address` VARCHAR(100) DEFAULT NULL COMMENT '地址',
`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
PRIMARY KEY(id)
)ENGINE=INNODB DEFAULT CHARSET=utf8

```

总结格式：

```sql
create table [if not exists] `表名`(
	`字段名` 列类型 [属性] [索引] [注释],
    `字段名` 列类型 [属性] [索引] [注释],
	......
	`字段名` 列类型 [属性] [索引] [注释]
)[表类型][字符集设置][注释]
```

常用命令

```sql
SHOW CREATE DATABASE school-- 查看创建数据库的语句
SHOW CREATE TABLE joker-- 查看创建joker表的定义语句
DESC joker-- 显示表的结构
```

## 2.5 数据库表的类型

```sql
-- 关于数据库引擎
INNODB 默认使用
MYISAM 早些年使用的
其它不用管
```

### INNODB 与MYISAM

|              | MYISAM | INNODB                        |
| ------------ | ------ | ----------------------------- |
| 事务支持     | 不支持 | 支持                          |
| 数据行锁定   | 不支持 | 支持                          |
| 外键约束     | 不支持 | 支持                          |
| 全文索引     | 支持   | mysql 5.7以后才支持全英文索引 |
| 表空间的大小 | 较小   | 较大，约为前者的2倍           |

常规的使用操作：

- MYISAM	节约空间，速度较快

- INNODB     安全性高，事务处理，多表多用户操作

> 在物理空间存在的位置

- 所有的数据库文件都存在data目录下，一个文件夹就对应一个数据库

- 本质还是文件存储

> Mysql 引擎在物理文件上的区别

- InnoDB 在数据库表中只有一个*.frm 文件，以及上级目录下的 ibdata1 文件
- MYISAM 对应的文件
  - *.frm 表结构的定义文件
  - *.MYD 数据文件（data)
  - *.MYI 索引文件 （index）

> 设置数据库表的字符集编码

```sql
charset=utf8
```

- 不设置的话，会是mysql默认的字符集编码~（不管是啥，反正不支持中文）

- MySQL的默认编码是Latin1 ,不支持中文

- 可以在 my.ini 中配置默认的编码（不建议，因为如果文件到别人电脑里，就不一定能运行）

  ```sql
  character-set-server=utf8
  ```

## 2.6 修改删除表

> 修改表

```sql
-- 修改表名 ALTER TABLE 旧表名 RENAME AS 新表名;
ALTER TABLE student RENAME AS student1;
-- 增加表的字段 ALTER TABLE 表名 ADD 字段名 列属性;
ALTER TABLE student1 ADD sex VARCHAR(10);
-- 修改表的字段，有两种（重命名，修改约束）
-- ALTER TABLE 表名 MODIFY 字段名 列属性 [];
ALTER TABLE student1 MODIFY age VARCHAR(10);-- 修改字段的约束
-- ALTER TABLE 表名 CHANGE 旧字段名 新字段名 列属性 [];
ALTER TABLE student1 CHANGE age age1 INT(3);-- 字段重命名

-- 删除表的字段 ALTER TABLE 表名 DROP 字段名;
ALTER TABLE student1 DROP age1;
```



> 删除表

```sql
-- 删除表 (如果存在再删除）
DROP TABLE IF EXISTS student1;
```

**所有的删除和创建操作尽量加上判断，以免报错**

**注意**：

- 字段名 使用 ``包裹
- 注释：
  - 行注释 -- 
  - 多行注释/**/
  - sql 关键字大小写不敏感，建议小写
  - 所有的符号用英文的

# 3、MySQL的数据管理

## 3.1 外键（了解）

> 方式一：在创建表的时候添加外键

```sql
CREATE TABLE IF NOT EXISTS `grade`(
`gradeId` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年纪id',
`gradeName` VARCHAR(10) NOT NULL COMMENT '年纪名称',
PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
-- joker 表的gradeid字段 要去引用grade表的gradeid
-- 定义外键key
-- 给这个外键添加约束 （执行引用） refereces 引用
CREATE TABLE `joker`(
  `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
  `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
  `sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别',
  `birthday` DATETIME DEFAULT NULL COMMENT '生日',
  `address` VARCHAR(100) DEFAULT NULL COMMENT '地址',
  `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
  `gradeid` INT(10) NOT NULL COMMENT '学生年纪',
  PRIMARY KEY (`id`),
  KEY `FK_gradeid` (`gradeid`),
  CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(`gradeId`)
  
) ENGINE=INNODB DEFAULT CHARSET=utf8
```

> 方式二：创建表之后添加外键约束

```sql
ALTER TABLE `joker`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY(`gradeid`) REFERENCES `grade`(`gradeid`);
```

总结：

- 以上的操作都是物理外键，数据库级别的外键，不建议使用，避免数据库过多造成困扰
- 要删除有外键关系的表，必须先删除引用的表，再删除被引用的表

最佳实践:

- 数据库就是单纯的表，只用来存数据，只有行（数据）和列（字段）
- 我们想使用多张表的数据，想使用外键（用程序实现）

## 3.2 DML（操作）（全记）

- 数据库意义：数据存储，数据管理
- DML：数据库操作语言

### insert（添加/插入）

```sql
-- 插入语句
-- insert into 表名([字段名1，字段名2，...]) values(`值1`),(`值2`),(`值3`)...
INSERT INTO `grade`(`gradeName`) VALUES('dayi')

-- 写插入语句数据和字段一定要一一对应
-- 插入多个字段
insert into `grade`(`gradename`)
values(`大一`),(`大二`)

insert into `joker`(`name`,`pwd`,`sex`)
values (`zhang`,`dkdj`,`女`),(`li`,`ddd`,`nan`)
```

注意事项：

- 字段和字段之间使用英文逗号隔开
- 字段是可以省略的，但是后面的值必须一一对应，不能少
- 可以同时插入多条数据，values 后面的值，需要使用 , 隔开即values(),(),()......

### update(修改)

```sql
-- 语法
-- update 表名 set 列名=值,[列名1=值2],... where [条件]
UPDATE `joker` SET `name`='陈',sex='男' WHERE id = 1;
--where 可以省略，但是就会修改所有的列值，千万不要
```

**条件**：where子句 运算符

**操作符**会返回布尔值:

| **操作符**       | 含义     | 范围  | 结果  |
| ---------------- | -------- | ----- | ----- |
| =                | 等于     | 5=6   | false |
| <> 或 !=         | 不等于   | 5 !=6 | true  |
| >                | 大于     |       |       |
| <                |          |       |       |
| <=               |          |       |       |
| >=               |          |       |       |
| between...and... | 某个范围 |       |       |
| and              | 和       |       |       |
| or               | 或       |       |       |

语法：update 表名 set column_name=value,[column_name2=value],... where [条件]

注意：

- column_name 是数据库的列，尽量带上``

- 删选的条件，如果没有指定，则会修改所有的列

- value可以是一个具体的值，也可以是一个变量（用的很少），比如时间，

- ```sql
  UPDATE `joker` SET `birthday`=CURRENT_TIME WHERE id = 1;
  ```

- 多个设置的属性使用英文逗号隔开

### delete(删除)

语法：delete from 表名 [where 条件]

```sql
delete from `joker`--避免这样写，会全部删除
delete from `joker` where id = 1;--删除指定数据

```

> TRUNCATE 命令

作用：完全清空一个数据库表，表的结构和索引约束不会变

```sql
truncate `joker`;-- 清空joker表
```

#### delete 与 truncate 的区别

- 相同点：都能删除数据，不会删除表结构
- 不同：
  - truncate
    - 重新设置自增列，自增计数器会归零
    - 不会影响事务

```sql
-- 测试delete 与 truncate 的区别
CREATE TABLE `test`(
`id` INT(10) NOT NULL AUTO_INCREMENT,
`column` VARCHAR(20) NOT NULL,
PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO `test`(`column`) VALUES ('1'),('2'),('3')

DELETE FROM `test` -- 不会影响自增
TRUNCATE TABLE `test`-- 自增会归零


```

delete 删除的问题：（了解即可），会重启数据库，产生的现象：

- InnoDB 自增列会重1开始，（因为存在内存当中的，断电即失）
- MyISAM 继续从上一个自增量开始 （存在文件中的，不会丢失）

## 3.3 DQL（查询）（最重点）

Data Query Language

- 所有的查询操作都用它 **select**

### select 与 as

```sql
-- select
SELECT * FROM grade
-- 查询指定字段
SELECT `studentno`,`studentname` FROM student

-- as 别名，字段和表都可以
SELECT `studentno` AS 学号,`studentname` AS 姓名 FROM student
-- 函数 Concat(a,b) 拼接字符串
SELECT CONCAT('姓名: ',studentname) AS 新名字 FROM student

--as 可以省略，
```

### 去重 distinct

作用：select 查询出来的结果中重复的语句

```sql
select distinct `studentno` from result
```

### 数据库的列（表达式）

```sql
select version() --查询系统版本（函数）
select 100*3-1 as 计算结果;--用来计算（表达式）
select @@auto_increment_increment --查询自增的步长（变量）

-- 学员成绩+1分查看
select 'studentno','studentresult' + 1 as '加分后' from result
```

总结：

- 数据库中的**表达式**：文本值，列，null,函数，计算表达式，系统变量。。。
- select 表达式 from 表

### where 条件子句

作用：检索数据中符合条件的值

搜索的条件由一个或多个表达式组成，结果都是返回布尔值

#### 逻辑运算符

| 运算符     | 语法                  | 描述                   |
| ---------- | --------------------- | ---------------------- |
| and   &&   | a and b          a&&b | 逻辑与                 |
| or    \|\| |                       | 逻辑或                 |
| not !      | not a      ! a        | 逻辑非，真为假，假为真 |

**尽量使用英文字母**



#### 模糊查询：比较运算符

| 运算符             | 语法               | 描述                            |
| ------------------ | ------------------ | ------------------------------- |
| IS NULL            | a is null          | 操作符为null,结果为真           |
| IS NOT NULL        | a is not null      |                                 |
| BETWEEN ...AND ... | a between b and c  |                                 |
| **Like**           | a like b           |                                 |
| In                 | a in (a1,a2,a3...) | a 为a1,a2,a3 中的一个，结果为真 |

```sql
-- like 的操作
--like 结合 % （表示0到任意个数的字符），_（表示一个字符）
-- 查询姓陈的
select `studentno`,`studentname` from `student`
where studentname like '陈%'

-- 查询姓陈的,后面只有一个字的
select `studentno`,`studentname` from `student`
where studentname like '陈_'

-- 查询姓陈的,后面只有2个字的
select `studentno`,`studentname` from `student`
where studentname like '陈__'

-- 查询名字中有陈字的
select `studentno`,`studentname` from `student`
where studentname like '%陈%'

-- in(具体的一个或多个值) 操作
-- 查询1001，1002，1003号学生
select `studentno`,`studentname` from `student`
where studentno in (1001,1002,1003)


```



### 联表查询

#### join 对比

| 操作       | 描述                                       |
| ---------- | ------------------------------------------ |
| inner join | 如果表中至少有一个匹配，就返回行           |
| left join  | 会从左表中返回所有的值，即使右表中没有匹配 |
| right join | 会从右表中返回所有的值，即使左表中没有匹配 |

注意：

- join（连接的表） on （判断的条件）  是固定搭配 ，连接查询
- where 是等值查询
- 结果是一样的

```sql

```

解题思路：

- 确定查询哪些数据，select...

- 哪几个表，from...join....on 交叉条件

- 假设存在一种多张表查询，先查询两张表，然后慢慢加

  ```sql
  select s.studentno,studentname,subjectName,studentresult
  from student s
  right join result r
  on r.studentno = s.studentno
  inner join subject sub
  on r.subjectno = sub.subjectno
  ```



#### 自连接

- 自己的表和自己的表连接，核心：一张表拆为两张一样的表即可

```sql信息
-- 查询父子信息
select a.categroname '父',b.catagoryname '子'
from catagory a,catagory b
where a.catagoryid = b.pid
```

### 分页和排序

#### 排序 order by...

order by

ASC 升序，DESC 降序





#### 分页 limit

limit

为什么要分页：缓解数据库压力，不分页，就是瀑布流，

语法：limit 查询起始下标，pageSize

### 子查询

本质：在where语句中嵌套一个子查询语句

查询过程是**由里及外**

where (select * from)

```sql
-- 查询 课程为高等数学-2 且分数不小于80分的同学的姓名和学号
-- 方式一：使用连接查询
SELECT DISTINCT s.studentno,studentname
FROM student s
INNER JOIN result r
ON s.studentno=r.studentno
INNER JOIN `subject` sub
ON sub.subjectno=r.subjectno
WHERE subjectname = '高等数学-2' AND studentresult>=80

-- 方式二：使用子查询 执行过程，由里及外
SELECT DISTINCT s.studentno,studentname
FROM student s
INNER JOIN `result` r
ON s.studentno=r.studentno
WHERE `studentresult` >= 80 AND subjectno = (
SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'
)
-- 再改造子查询
SELECT studentno,studentname
FROM student
WHERE studentno IN(
SELECT studentno FROM result WHERE studentresult >= 80 AND subjectno = (
SELECT studentno FROM `subject` WHERE subjectname = '高等数学-2'
)
)
```

### 分组和过滤（group by , having）

```sql
-- 查询不同课程的平均分，最高分，最低分，平均分大于>80
-- 高版本要用any_value(subjectname)
SELECT subjectname ,AVG(studentresult) AS 平均分 ,MAX(studentresult) AS 最高分,MIN(studentresult) AS 最低分
FROM result r
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
GROUP BY r.subjectno -- 通过什么字段来分组
HAVING 平均分> 80
```

### select 小结

语法：

- 顺序不能变

```sql
select [all |distinct]
{* | talbe.* |table.field1[as alias1],[...]}
from table_name[as table_alias]
[left|right|inner join table_name2] --联查查询
[where ...]
[group by ...]
[having...]
[order by ...]
[limit ...]
```

官方文档

```sql
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
    [HIGH_PRIORITY]
    [STRAIGHT_JOIN]
    [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
    [SQL_CACHE | SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
    select_expr [, select_expr] ...
    [into_option]
    [FROM table_references
      [PARTITION partition_list]]
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [HAVING where_condition]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
    [PROCEDURE procedure_name(argument_list)]
    [into_option]
    [FOR UPDATE | LOCK IN SHARE MODE]

into_option: {
    INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        export_options
  | INTO DUMPFILE 'file_name'
  | INTO var_name [, var_name] ...
}
```





# 4 mysql常用函数

### 1、常用函数

- 常用函数并不怎么常用 
- 一般会在程序里面实现

```sql
-- 数学运算
select abs(-8)  -- 求绝对值
select ceiling(10.7)  -- 向上取整
select floor(10.7)  -- 向下取整
select rand() -- 返回0~1之间的随机数
select sign(-3)-- 判断一个数的符号 0-0，负数返回-1，正数返回1

-- 字符串函数
select char_length('hello') -- 字符串长度
select concat('wo','love','you') -- 拼接字符串
select insert('I LOVE YOU THREE THOUSANDS TIMES',2,4,' REALLY LOVE')-- 查询，替换，INSERT(old_str,pos,length,new_str)
select lower('DFDFAF') -- 转成小写
select upper('fdkfdsf')-- 转成大写
select instr('Iloveyou','o') -- instr(str,substr) 返回substr 在str中第一次出现的位置
select replace('I love you','love','like') -- 替换出现的指定字符串 replace(str,from_str,to_str)
select substr('I love you',3,4)-- 返回指定的字符串，substr(str,pos,len)
select reverse('I love you')-- 反转

-- 查询姓周的同学，周》邹
select replace(studentname,'周','邹') from `student` 
where studentname like '周%'

-- 时间和日期函数（记住）
select current_date()-- 当前日期
SELECT curdate()-- 当前日期
SELECT now()-- 当前时间
SELECT localtime()
SELECT sysdate()
SELECT year(NOw())

SELECT month(now())
SELECT day(NOW())
SELECT hour(NOW())
SELECT minute(NOW())
SELECT second(now())
SELECT
-- 系统
SELECT system_user()
SELECT user()
select version()
```



### 2、聚合函数（常用）

```sql
-- 聚合函数
-- count()
SELECT COUNT('borndate') FROM student; -- count(字段) 会忽略所有的null值
SELECT COUNT(*) FROM student; -- count(字段) 不会忽略null值，本质，计算行数，会算所有列
SELECT COUNT(1) FROM student; -- count(字段) 不会忽略null值 本质，计算行数，但只计算一列

SELECT SUM(studentresult) AS 总分 FROM student;-- 总和
SELECT AVG(studentresult) AS 平均分 FROM student;-- 平均值
SELECT MAX(studentresult) AS 最高分 FROM student;-- 最大
SELECT MIN(studentresult) AS 最小分 FROM student;-- 最小
```

### 3、数据库级别的MD5加密（扩展）

什么是md5 ：主要是增强算法强度和不可逆性

MD5不可逆，具体的值对应的MD5 是一样的

MD5 破解网站原理，背后有一个字典，MD5 加密后的值，返回加密前的值。所以稍微复杂一点，就不行了

```sql
-- md5
CREATE TABLE `testmd5`(
`id` INT(10) NOT NULL ,
`name` VARCHAR(20) NOT NULL,
`pwd` VARCHAR(20) NOT NULL,
PRIMARY KEY (`id`)

)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO testmd5 VALUES (1,'kdfkdf','fdfkdf'),(2,'kdfkdf','fdfkdf'),(3,'kdfkdf','fdfkdf')

-- 加密
UPDATE testmd5 SET pwd=MD5(pwd) -- 加密了所有的密码

-- 插入的时候加密
INSERT INTO testmd5 VALUES (4,'hhh',MD5('19902929'))
-- 如何校验：将用户传进来的密码，进行MD5 加密，然后比对加密后的值

SELECT * FROM testmd5 WHERE `name`='hhh' AND pwd=MD5('19902929')
```

# 5 事务（transaction）

## 5.1 什么是事务

- 要么都成功，要么都失败

---------

1. sql  执行 A 给B转账，
2. sql 执行 B收到A的钱

------------

将一组sql放在一个批次去执行

- **事务原则**：ACID 原则，（原子性，一致性，隔离性，持久性 ）
  - 原子性（atomicity）：不可分割操作
  - 一致性 (consistency)：最终一致性，比如A+B的钱总数不变
  - 持久性 (durability)  ：事务没有提交，恢复到原状；事务已经提交，持久化到数据库。即事务一旦提交就不可逆。
  - 隔离性 (isolation)：针对对用户同时操作，比如 还有C给B转帐，就会隔离A-B,C-B,两个事务
- 隔离所导致的问题：
  - 脏读：一个事务读取了另一个事务未提交的数据
  - 不可重复读：在一个事务内读取表中的某一行数据，多次读取结果不同（这不一定是错误，只是某些场合不对）
  - 虚读（幻读）：在一个事务内，读取到了别的事务插入的数据，导致前后读取不一致

## 5.2 执行事务

- mysql 是默认开启 事务自动提交的

  ```sql
  -- mysql 是默认开启 事务自动提交的
  SET autocommit=0 -- 关闭
  SET autocommit=1 -- 开启，默认的
  
  -- 手动处理事务
  SET autocommit=0 -- 第一步，关闭自动提交
  -- 事务开启
  START TRANSACTION  -- 标记一个事务的开始，从这个之后的sql,都在同一个事务内
  INSERT 。。。
  
  -- 提交： 持久化（成功就提交）
  COMMIT
  -- 回滚：回到原来的样子（失败就回滚）
  ROLLBACK
  -- 事务结束
  SET autocommit=1 -- 最后一步，开启自动提交
  
  -- 了解 ，可以在事务中使用保存点
  SAVEPOINT 保存点名 -- 设置一个事务的保存点
  ROLLBACK TO SAVEPOINT 保存点名 -- 回滚到保存点名
  RELEASE SAVEPOINT 保存点名 -- 撤销保存点
  ```

  

> 模拟场景



# 6、索引

## 6.1 索引的分类

- 主键索引 primary key
  - 唯一的标识，主键不可重复，只能有一个列
- 唯一索引  unique key
  - 避免重复的列出现，可以重复，多个列都可标识为唯一索引
- 常规索引 key|index
  - 默认的，可以用 index 或 key 来设置
- 全文索引 FullText
  - 在特点的数据库引擎下才有，MyISAM
  - 快速定位数据

> 基础语法

```sql
SHOW INDEX FROM student -- 显示所有的索引信息
-- 增加一个全文索引 索引名（`列名`）
 ALTER TABLE school.student ADD FULLTEXT INDEX `studentname`(`studentname`)
 --方式三：创建一个索引
 create index id_student_name on student(`studentname`);
 
 --  explain 分析sql的执行状况
 EXPLAIN SELECT * FROM student -- 普通索引，即非全文索引
 EXPLAIN SELECT * FROM student WHERE MATCH(studentname) AGAINST('刘')-- 全文索引

```

## 6.2 测试索引

- 索引在小数据量的时候，区别不打，但，在大数据量的时候，区别十分明显
- 



## 6.3 索引原则

- 索引不是越多越好
- 不要对经常变动的数据加索引
- 小数据量的表不需要加索引
- 索引一般加在常用来查询的字段上

> 索引的数据结构

Btree:InnoDB 的默认数据结构

阅读：http://blog.codinglabs.org/articles/theory-of-mysql-index.html





# 7、权限管理里和备份

## 7.1 权限管理

- 用户表：mysql.user
- 本质：读这张表进行增删改查
- 权限管理的基本操作

```sql
-- 权限管理
-- 创建用户 CREATE USER 用户名 IDENTIFIED BY '密码'
CREATE USER joker IDENTIFIED BY '123456'
-- 修改密码 （当前用户）
SET PASSWORD =PASSWORD('123456')
-- 修改密码 （指定用户）
SET PASSWORD FOR joker =PASSWORD('123456')

-- 重命名
RENAME USER joker TO joker2

-- 用户授权 all privileges 全部的权限，库.表
-- all privileges 除了给别人授权，其它都能干
GRANT ALL PRIVILEGES *.* TO joker2
-- 查询权限
SHOW GRANTS FOR joker2 -- 查看指定用户的权限
SHOW GRANTS FOR root@localhost 
-- 撤销权限 revoke 哪些权限，在哪个库，给谁撤销
REVOKE ALL PRIVILEGES ON *.* FROM joker2
-- 删除用户
DROP USER joker2
```



## 7.2 数据库备份

为什么备份：

- 保重数据不丢失
- 数据转移

mysql 数据库备份的方式

- 直接拷贝物理文件

- 在sqlyog 可视化工具中，手动导出

- 使用命令行导出 mysqldump 命令行使用

  - ```bash
    # mysql dump -h主机 -u 用户名 -p 密码 数据库 [表名1 表名2 ...] > 物理磁盘位置/文件名
    mysqldump -hlocalhost -uroot -p123456 shcool student >D:/a.sql
    
    # 导入
    # 登录的情况下
    # source 备份文件
    source d:/a.sql
    
    
    ```

  - 



# 8、规范数据库设计

## 8.1 为什么要设计数据库



## 8.2 三大范式

**第一范式**：

- 原子性：要求数据库表的每一列都是不可分割的原子操作

**第二范式**

- 前提：满足第一范式
- 每张表只描述一件事情

**第三范式**

- 前提：满足第二范式
- 第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关



拓展：

阿里手册里，关联查询的表不得超过三张表

**规范性和性能的问题：**

- 考虑商业化的需求和目标，（成本，用户体验），数据库的性能更加重要
- 在规范性能的问题的时候，需要适当的考虑一下规范性
- 故意给某些表增加一些冗余的字段。（从多表查询中变为单表查询）
- 故意增加一些计算列（ 从大数据量降低为小数据量的查询）

# 9、JDBC （重点）

## 9.1数据库驱动

- 我们的程序 会通过 数据库驱动和数据库打交道

## 9.2 JDBC

- 对于开发人员来说，只需要掌握JDBC接口的操作即可，就可以实现数据库与程序的连接
- 需要导入的包：
  - java.sql
  - javax.sql
  - 还需要导入一个数据库驱动包 mysql-connector-java-5.1.47.jar (后面直接在maven导入即可）
- 

## 9.3 第一个jdbc 程序

步骤：

1. 加载驱动 Class.forname()
2. 连接数据库 DriverManager
3. 获得执行sql的对象 Statement
4. 获得返回的结果集 Resultset
5. 释放连接 close()

```sql
-- 建数据库表
CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci;

USE jdbcStudy;

CREATE TABLE `users`(
	id INT PRIMARY KEY,
	NAME VARCHAR(40),
	PASSWORD VARCHAR(40),
	email VARCHAR(60),
	birthday DATE`jdbcstudy`
);

INSERT INTO `users`(id,NAME,PASSWORD,email,birthday)
VALUES(1,'zhansan','123456','zs@sina.com','1980-12-04'),
(2,'lisi','123456','lisi@sina.com','1981-12-04'),
(3,'wangwu','123456','wangwu@sina.com','1979-12-04')
```



```java
public class jdbcDemo01 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1. 加载驱动
        Class.forName("com.mysql.jdbc.Driver");//固定写法，加载驱动
        //2. 用户信息和url
        //useUnicode=true&characterEncoding=utf8&useSSL=true
        String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=true";
        String name = "root";
        String password = "123456";
        //3. 连接成功，数据库对象 Connection 代表数据库
        Connection connection = DriverManager.getConnection(url, name, password);
        //4. 执行sql的对象 Statement
        Statement statement = connection.createStatement();
        //5. 执行sql的对象，去执行sql，可能存在结果
        //所有的删除和插入都叫更新update()
        String sql = "SELECT * FROM users";
        ResultSet resultSet = statement.executeQuery(sql);//返回结果集，是一个链表的形式
        //结果集中封装了我们查询的全部的结果

        while(resultSet.next()){
            System.out.println("id="+resultSet.getObject("id"));
            System.out.println("name="+resultSet.getObject("NAME"));
            System.out.println("pwd="+resultSet.getObject("PASSWORD"));
            System.out.println("email="+resultSet.getObject("email"));
            System.out.println("birthday="+resultSet.getObject("birthday"));

        }

        //6. 释放连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}

```

> DriverManager

```java
//原本的写法是 DriverManager.registerDriver(new com.mysql.jdbc.Driver());,但最好用下面一种
        Class.forName("com.mysql.jdbc.Driver");//固定写法，加载驱动
```

> URL

```java
String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=true";

//mysql --3306
//jdbc:mysql://主机地址:端口号/数据库名？参数1&参数2&参数3
//oracle --1521
//jdbc:oracle:thin:@localhost:1521:sid
```

> connection

```java
// connection 代表数据库
// 可以做数据库级别可以做的事情
//数据库设置自动提交
//事务提交
//事务回滚
connection.commit();
connection.rollback();
connection.setAutoCommit();
...
```



> Statement 执行SQL的对象， PrepareStatement 也是执行SQL的对象

```java
String sql = "select * from users";//编写SQL

statement.executeQuery();//查询操作，返回ResultSet
statement.execute();//执行任何SQL，但效率更低
statement.executeUpdate();//更新，插入，删除，都用这个，返回一个受影响的行数
```

> ResultSet 查询的结果集，封装了所有的查询结果

获得指定的数据类型

```java
resultSet.getObject();//在不知道类型的情况下使用
resultSet.getString();//知道列的类型就用指定类型
resultSet.getFloat();
resultSet.getInt();
```

遍历

```java
resultSet.beforeFirst();// 移动到最前面
resultSet.afterLast();// 移动到最后面
resultSet.next();//移动到下一个数据
resultSet.previous();//移动到前一行
resultSet.absolute(row);//移动到指定行
```

> 释放资源

```java
//6. 释放连接
resultSet.close();
statement.close();
connection.close();//十分耗资源，用完关掉
```

## 9.4 statement 对象

- 存在SQL注入问题，不安全，不推荐使用



### 代码实现

1. 提取工具类

   ```java
   /**
    * JDBC连接工具类
    */
   public class JdbcUtils {
       private static String driver = null;
       private static String url = null;
       private static String username = null;
       private static String password = null;
   
       static {
           try {
               InputStream in = Files.newInputStream(Paths.get("src/com/joker/database.properties"));
               Properties properties = new Properties();
               properties.load(in);
   
               driver = properties.getProperty("driver");
               url = properties.getProperty("url");
               username = properties.getProperty("username");
               password = properties.getProperty("password");
               //1 驱动只用加载一次
               Class.forName(driver);
           } catch (Exception e){
               e.printStackTrace();
           }
       }
   
       //获取连接
       public static Connection getConnection() throws SQLException {
           return DriverManager.getConnection(url, username, password);
       }
       //释放资源
       public static void release(Connection conn, Statement statement, ResultSet resultSet){
           if (resultSet != null){
               try {
                   resultSet.close();
               } catch (SQLException throwables) {
                   throwables.printStackTrace();
               }
           }
           if (statement != null){
               try {
                   statement.close();
               } catch (SQLException throwables) {
                   throwables.printStackTrace();
               }
           }
           if (conn != null){
               try {
                   conn.close();
               } catch (SQLException throwables) {
                   throwables.printStackTrace();
               }
           }
       }
   }
   ```

   

2. 编写增删改的方法，executeUpdate()

   ```java
   public class TestInsert {
       public static void main(String[] args) {
   
           Connection conn = null;
           Statement statement = null;
           ResultSet resultSet = null;
           try {
               conn = JdbcUtils.getConnection();
               statement = conn.createStatement();
               String sql = "INSERT INTO `users` (id,`NAME`,`PASSWORD`,`email`,birthday)\n" +
                       "VALUES(6,'joker','123456','abc@qq.com','2021-12-12')";
               int i = statement.executeUpdate(sql);
               if (i>0){
                   System.out.println("插入成功");
               }
           } catch (Exception e) {
               e.printStackTrace();
           } finally {
               JdbcUtils.release(conn,statement,resultSet);
           }
       }
   }
   ```

   ```java
   public class TestDelete {
       public static void main(String[] args) {
   
           Connection conn = null;
           Statement statement = null;
           ResultSet resultSet = null;
           try {
               conn = JdbcUtils.getConnection();
               statement = conn.createStatement();
               String sql = "delete from users where id = 4";
               int i = statement.executeUpdate(sql);
               if (i>0){
                   System.out.println("删除成功");
               }
           } catch (Exception e) {
               e.printStackTrace();
           } finally {
               JdbcUtils.release(conn,statement,resultSet);
           }
       }
   }
   ```

   ```java
   public class TestUpdate {
       public static void main(String[] args) {
   
           Connection conn = null;
           Statement statement = null;
           ResultSet resultSet = null;
           try {
               conn = JdbcUtils.getConnection();
               statement = conn.createStatement();
               String sql = "update users set `NAME`='monster' ,`email`='hhhh@joker.com' where id = 1";
               int i = statement.executeUpdate(sql);
               if (i>0){
                   System.out.println("更新成功");
               }
           } catch (Exception e) {
               e.printStackTrace();
           } finally {
               JdbcUtils.release(conn,statement,resultSet);
           }
       }
   }
   ```

   

3. 查询 executeQuery()

   ```java
   public class TestSelect {
   
           public static void main(String[] args) {
   
               Connection conn = null;
               Statement statement = null;
               ResultSet resultSet = null;
               try {
                   conn = JdbcUtils.getConnection();
                   statement = conn.createStatement();
                   String sql = "select * from users where id =1";
                   resultSet= statement.executeQuery(sql);
                  while (resultSet.next()){
                      System.out.println(resultSet.getString("NAME"));
                  }
               } catch (Exception e) {
                   e.printStackTrace();
               } finally {
                   JdbcUtils.release(conn,statement,resultSet);
               }
           }
   }
   ```

   

### SQL 注入问题

- sql存在漏洞，会被攻击导致数据泄露

- 本质：SQL会被拼接 or

  ```java
  /**
   * SQL注入问题示例
   */
  public class SqlInject {
      public static void main(String[] args) {
  //        login("monster","123456");//正常登录
            login(" 'or'1=1"," 'or'1=1");//这就是典型的sql注入问题
      }
  
      // 登录业务
      public static void login(String username,String password) {
          Connection conn = null;
          Statement statement = null;
          ResultSet resultSet = null;
          try {
              conn = JdbcUtils.getConnection();
              statement = conn.createStatement();
              String sql = "select * from users where `NAME`='"+username+"' and `password`='"+password+"'";
              resultSet= statement.executeQuery(sql);
              while (resultSet.next()){
                  System.out.println(resultSet.getString("NAME"));
                  System.out.println(resultSet.getString("password"));
  
              }
          } catch (Exception e) {
              e.printStackTrace();
          } finally {
              JdbcUtils.release(conn,statement,resultSet);
          }
      }
  
  }
  
  ```

  

## 9.5 PreparedStatement 对象

- PreparedStatement  可以防止SQL 注入，并且效率更高，推荐使用

- PreparedStatement  防SQL注入的本质：把传递进来的参数当做字符，假设存在转移字符，直接忽略，比如说 ' 会被直接转义

**插入**

```java
/**
 * preparedStatement 的使用
 */
public class TestInsert {
    public static void main(String[] args) {

        Connection conn = null;
        PreparedStatement statement = null;

        try {
            conn = JdbcUtils.getConnection();
            //与 Statement 的区别
            // 使用 ? 占位符代替参数
            String sql = "INSERT INTO `users` (id,`NAME`,`PASSWORD`,`email`,birthday) values(?,?,?,?,?)";
//                    "VALUES(6,'joker','123456','abc@qq.com','2021-12-12')";
            statement = conn.prepareStatement(sql);//预编译SQL ，先写sql,然后不执行
            //手动给参数赋值
            statement.setInt(1,7);
            statement.setString(2,"evil");
            statement.setString(3,"12345");
            statement.setString(4,"kjj@dd.com");
            //注意点：sql.Date 数据库 java.sql.Date
            //      util.Date Java  new Date().getTime() 获得时间戳
            statement.setDate(5,new java.sql.Date(new java.util.Date().getTime()));
            //执行
            int i = statement.executeUpdate();
            if (i>0){
                System.out.println("插入成功");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(conn,statement,null);
        }
    }
}

```

**删除**

```java
/**
 * PreparedStatement
 */
public class TestDelete {
    public static void main(String[] args) {

        Connection conn = null;
        PreparedStatement statement = null;

        try {
            conn = JdbcUtils.getConnection();
            //与 Statement 的区别
            // 使用 ? 占位符代替参数
            String sql = "delete from users where id=?";
            statement = conn.prepareStatement(sql);//预编译SQL ，先写sql,然后不执行
            //手动给参数赋值
            statement.setInt(1,7);
            //执行
            int i = statement.executeUpdate();
            if (i>0){
                System.out.println("删除成功");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(conn,statement,null);
        }
    }
}
```

**更新**

```java
/**
 * PreparedStatement
 */
public class TestUpdate {
    public static void main(String[] args) {

        Connection conn = null;
        PreparedStatement statement = null;

        try {
            conn = JdbcUtils.getConnection();
            //与 Statement 的区别
            // 使用 ? 占位符代替参数
            String sql = "update users set `NAME`=? where id=?";
            statement = conn.prepareStatement(sql);//预编译SQL ，先写sql,然后不执行
            //手动给参数赋值
            statement.setString(1,"joker2");
            statement.setInt(2,5);
            //执行
            int i = statement.executeUpdate();
            if (i>0){
                System.out.println("更新成功");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(conn,statement,null);
        }
    }

}

```



**查询**

```java
/**
 * PreparedStatement
 */
public class TestSelect {
    public static void main(String[] args) {
        Connection conn=null;
        PreparedStatement preparedStatement=null;
        ResultSet resultSet = null;

        try {
            conn = JdbcUtils.getConnection();
            String sql = "select * from users where id=?";

            preparedStatement = conn.prepareStatement(sql);

            preparedStatement.setInt(1,1);
            resultSet = preparedStatement.executeQuery();
            while (resultSet.next()){
                System.out.println(resultSet.getString("NAME"));
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JdbcUtils.release(conn,preparedStatement,resultSet);
        }

    }
}

```



## 9.6 idea处理事务

```java
public class TestTransaction01 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement ps = null;


        try {
            conn = JdbcUtils.getConnection();
            //关闭数据库的自动提交，会自动开启事务
            conn.setAutoCommit(false);

            String sql_1 = "update account set money = money-100 where name = 'A'";
            ps = conn.prepareStatement(sql_1);
            ps.executeUpdate();

//            int x=1/0;//报错，

            String sql_2 = "update account set money = money+100 where name = 'B'";
            ps = conn.prepareStatement(sql_2);
            ps.executeUpdate();

            //业务完毕，提交事务
            conn.commit();
            conn.setAutoCommit(true);
            System.out.println("转账成功");
        } catch (SQLException throwables) {
            try {
                conn.rollback();//如果失败，则回滚事务,不写也可以，因为失败默认回滚
            } catch (SQLException e) {
                e.printStackTrace();
            }
            throwables.printStackTrace();
        } finally {
            JdbcUtils.release(conn,ps,null);
        }
    }
}
```

## 9.7 数据库连接池

正常过程：数据库连接--执行完毕--释放

连接--释放 十分浪费资源

**池化技术**：准备一些预先的资源，过来就连接预先准备好的

一般的连接数为100

设定：

最小连接数：10

最大连接数：15

等待超时：100ms

编写连接池，实现一个接口 DataSource

> 开源数据源实现

- DBCP
- C3P0
- Druid:(阿里巴巴的)

使用了这些数据库连接池后，我们在项目开发中就不需要编写连接数据库的代码了

### DBCP

- 需要用到的jar包
  - commons-dbcp-1.4.jar
  - commons-pool-1.6.jar

### C3P0

需要的jar包

- ​	c3p0-0.9.5.5
- mchange-commons-java-0.2.19



> 总结

无论使用什么数据源，本质还是一样的，DataSource 接口不会变，方法就不会变

### Druid

- 在springBoot的时候学习，很牛
- 

# 10 视图

## 10.1 概念

 **定义**

​        视图，虚拟表，从一个表或多个表中查询出来的表，作用和真实表一样，包含一系列带有行和列的数据。视图表中，用户可以使用SELECT语句查询数据，也可以使用INSERT、UPDATE、DELETE修改记录，视图可以使用户操作方便，并保障数据库系统安全。

和临时表很像，但临时表不会被保存，而视图是**保存下来**的表。

**特点**

- 优点
  - 简单化，数据所见即所得。
  - 安全性，用户只能查询或修改他们所能见到的数据。
  - 逻辑独立性，可以屏蔽真实表结构变化带来的影响。
- 缺点
  - 性能相对较差，简单的查询也会变得稍显复杂。
  - 修改不方便，特别是复杂的聚合视图基本无法修改。

## 10.2 视图操作

### 1、创建视图(create)

```sql
-- 创建temp_info 的视图
-- 语法：create view 视图名 as 查询数据源表结构
create view temp_info
as
select empoyee_ed , firts_name from t_employees;
```

### 2、使用视图(select )

- 使用视图可以简化查询的操作，将来视图应用最多的地方也是**查询。**

```sql
select * from temp_info where employe_id = 1;
```

### 3、视图的修改(alter)

- 方式一：`CREATE OR REPLACE VIEW 视图名 AS 查询语句`
  - 方式一是在不明确视图是否存在时使用，如果存在则修改，否则创建
- 方式二：`ALTER VIEW 视图名 AS 查询语句`
  - 方式二是明确存在时进行修改。

无论哪种方式都需要拼接一个完整查询语句。

```sql
-- 方式一，如果视图存在则修改，反之创建。
CREATE OR REPLACE VIEW temp_info
AS
SELECT employee_id FROM t_employees;
-- 方式二，对已存在的视图进行修改
ALTER VIEW t_emp_info
AS
SELECT employee_id FROM t_employees;
```

### 4、视图的删除（drop）

```sql
drop view temp_info;
```

## 10.3 视图的 注意事项

- 视图不会独立存储数据，原表发生改变，视图也发生改变。没有优化任何查询性能。
- 如果视图包含以下结构中的一种，则视图不可更新：
  - 聚合函数的结果
  - DISTINCT去重后的结果
  - GROUP BY分组后的结果
  - HAVING筛选过滤后的结果
  - UNION、UNION ALL联合后的结果







# 11、SQL 的优化



# 12 Mysql 的补充

## 12.1 straight join







# THE END