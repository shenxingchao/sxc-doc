# Mysql
## 准备工作
### 安装mysql
参考[wnmp安装](/md/Php?id=wnmp搭建)中的mysql下载服务安装和服务启动 windows可设置成开机自启动

[多个版本安装](https://blog.csdn.net/wudinaniya/article/details/82455431)

!> 最好安装mysql-5.7.34  因为他支持中文全文索引

### 设置环境变量
环境变量-用户变量-Path 添加  D:\mysql-5.7.34\bin

### 连接到Mysql
打开cmd
```powershell
mysql -h localhost -P 3306 -u root -p ;
```
> 初始密码去 D:\mysql\data\xxx.err 文件里 搜索 temporary password 为临时密码 登录后修改密码,参考[这里](/md/Mysql?id=访问控制)

###  退出连接
```powershell
quit or exit
```
### 下载工具
[Navicat Premium 15需要断网激活](https://pan.baidu.com/s/1dvZ-tTLXnhTlXXh9ag0o7Q )  
提取码：c12z [激活教程](https://www.jianshu.com/p/aca31d8f4c5b)  
[Navicat Premium 免安装旧版](https://pan.baidu.com/s/1QZc-mvzc0mPCeTKKiylxtQ)  
提取码: 1wxg

### 书写规范
SQL语句和大小写 请注意，SQL语句不区分大小写，因此
SELECT与select是相同的。同样，写成Select也没有关系。
许多SQL开发人员喜欢对所有SQL关键字使用大写，而对所有
列和表名使用小写，这样做使代码更易于阅读和调试。——来自MySql必知必会

## 使用
### 选择数据库
```sql
USE dbname
```
输出
```sql
Database changed
```
### 显示所有数据库
```sql
SHOW DATABASES;
```
输出
```sql
+--------------------+
| Database           |
+--------------------+
| information_schema |
| test               |
+--------------------+
2 rows in set (0.00 sec)
```

### 显示当前数据库所有表格
```sql
SHOW TABLES;
```
输出
```sql
+-----------------------------+
| Tables_in_test              |
+-----------------------------+
| nc_acl                      |
+-----------------------------+
23 rows in set (0.00 sec)
```

### 显示指定表格的所有列
```sql
SHOW COLUMNS FROM user;
```
or
```sql
DESCRIBE user;
```
输出结果相同
```sql
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(255) | NO   |     |         |                |
+-------+--------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```

### 查看数据库运行状态
```sql
SHOW STATUS
```
如查看mysql启动后的运行时间  
[可查看项](https://dev.mysql.com/doc/refman/5.7/en/server-status-variables.html)
```sql
SHOW STATUS LIKE 'Uptime';
```
输出
```sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Uptime        | 3621  |
+---------------+-------+
1 row in set (0.00 sec)
```

## 排序规则

排序规格可以作用于整个数据库、表、字段，例如账号字段设计为utf8bin 验证码字段设计为utf8_general_ci

utf8bin 区分大小写

utf8_general_ci 不区分大小写

utf8mb4 可存储4位的emoji表情

## 基本数据类型

tinyint 无需这设置大小，因为宽度为固定值 4（含一位符号位），取值范围是 -128 ~ 127 的整型数据。存储大小为1个字节，所以在建表的时候，无论是tinyint(100)还是tinyint(1),最终都是tinyint(4)，故无需设置其宽度

float/double 不推荐，在计算SUM(column) 会丢失精度，直接用decimal

varchar 最大长度21844，（65535-1-2）/3=21844  因为utf8是3个字节表示一位，65535是最大字节数(65535并不是一个很精确的上限，可以继续缩小这个上限。65535个字节包括所有字段的长度，变长字段的长度标识（每个变长字段额外使用1或者2个字节记录实际数据长度）、NULL标识位的累计,NULL标识位，如果varchar字段定义中带有default null允许列空,则需要需要1bit来标识，每8个bits的标识组成一个字段。一张表中存在N个varchar字段，那么需要（N+7）/8 （取整）bytes存储所有的NULL标识位。)；如果是GBK，2个字节表示1位，那就是（65535-1-2）/2；【前提是有且只有一个字段varchar;有多个varchar字段，或者或者有非空的字段，那所有varchar加起来的和也不等于21844，还会变小，没找到公式】

## 初始化数据
创建数据库

```sql
CREATE DATABASE dbname;
USE dbname;
```
创建用户表
```sql
CREATE TABLE `user` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT COMMENT 'id',
  `name` varchar(255) NOT NULL DEFAULT '' COMMENT 'name',
  `age` int(255) unsigned DEFAULT '0' COMMENT 'age',
  PRIMARY KEY (`id`),
  KEY `age_index` (`age`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
INSERT INTO `dbname`.`user` (`id`, `name`, `age`) VALUES (1, '张三', 1);
INSERT INTO `dbname`.`user` (`id`, `name`, `age`) VALUES (2, '李四', 1);
INSERT INTO `dbname`.`user` (`id`, `name`, `age`) VALUES (3, '李四', 2);
```
创建英文用户表
```sql
CREATE TABLE `en_user` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT COMMENT 'id',
  `name` varchar(255) NOT NULL DEFAULT '' COMMENT 'name',
  `age` int(255) unsigned NOT NULL DEFAULT '0' COMMENT 'age',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
INSERT INTO `dbname`.`en_user` (`id`, `name`, `age`) VALUES (1, 'zhangsan', 1);
INSERT INTO `dbname`.`en_user` (`id`, `name`, `age`) VALUES (2, 'lisi', 1);
INSERT INTO `dbname`.`en_user` (`id`, `name`, `age`) VALUES (3, 'ZHANGSAN', 2);
```
创建地址表
```sql
CREATE TABLE `user_address` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT COMMENT 'id',
  `user_id` int(11) unsigned NOT NULL DEFAULT '0' COMMENT 'user id外键',
  `address` varchar(255) NOT NULL DEFAULT '' COMMENT '地址',
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`),
  FULLTEXT KEY `address` (`address`) WITH PARSER `ngram`,
  CONSTRAINT `user_id` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
INSERT INTO `dbname`.`user_address` (`id`, `user_id`, `address`) VALUES (1, 1, '上海浦东');
INSERT INTO `dbname`.`user_address` (`id`, `user_id`, `address`) VALUES (2, 3, '浙江杭州');
```


## 查询

### SELECT
语法 SELECT 字段名1，字段名2 FROM 表名
```sql
SELECT id,name FROM `user`;
```
输出
```sql
+----+------+
| id | name |
+----+------+
|  1 | 张三 |
|  2 | 李四 |
+----+------+
```

### 查询全部

```sql
SELECT * FROM `user`;
```
输出结果和上面相同

### DISTINCT关键字

作用于所有列，聚合结果中所有指定列相同的数据，不能作用于单列
假如user表如下
```
+----+------+-----+
| id | name | age |
+----+------+-----+
|  1 | 张三 |   1 |
|  2 | 李四 |   1 |
|  3 | 李四 |   2 |
```
输入
```sql
SELECT DISTINCT name,age FROM `user`;
```
输出
```sql
+------+-----+
| name | age |
+------+-----+
| 张三 |   1 |
| 李四 |   1 |
| 李四 |   2 |
```
两个李四的age字段不一样  所以没有聚合 除非2个age都为2才能聚合
试着把age去掉，输入
```sql
SELECT DISTINCT name FROM `user`;
```
输出
```sql
+------+
| name |
+------+
| 张三 |
| 李四 |
+------+
```
### LIMIT

语法 LIMIT 从index行开始,取number行
```sql
SELECT name FROM `user` LIMIT 1,2;
```
or
```sql
SELECT name FROM `user` LIMIT 2 OFFSET 1;
```
一般用前一种比较多

### ORDER BY

语法 ORDER BY 字段名 ASC | DESC; 不加默认为ASC
下面的语句是先按id降序，再按age升序排列，注意这里排序字段的顺序
```sql
SELECT id,name,age FROM `user` ORDER BY id DESC,age ASC;
```
输出
```sql
+----+------+-----+
| id | name | age |
+----+------+-----+
|  3 | 李四 |   2 |
|  2 | 李四 |   1 |
|  1 | 张三 |   1 |
```
!> ORDER BY 必须在 LIMIT 之前

### WHERE
查找id为3的记录
```sql
SELECT * FROM `user` WHERE id = 3;
```
输出
```sql
+----+------+-----+
| id | name | age |
+----+------+-----+
|  3 | 李四 |   2 |
+----+------+-----+
1 row in set (0.03 sec)
```

WHERE 条件操作符

| 操作符 | 描述     |
| ------ | -------- |
| =      | 等于     |
| <>     | 不等于   |
| !=     | 不等于   |
| <      | 小于     |
| <=     | 小于等于 |
| >      | 大于     |
| >=     | 大于等于 |

!> 注意mysql查询等值不区分英文大小写
```sql
SELECT * FROM `en_user` WHERE name = 'ZHANGSAN';
```
输出
```sql
+----+----------+-----+
| id | name     | age |
+----+----------+-----+
|  1 | zhangsan |   1 |
|  3 | ZHANGSAN |   2 |
+----+----------+-----+
2 rows in set (0.00 sec)
```

### BETWEEN
语法 BETWEEN 开始范围 AND 结束范围
```sql
SELECT * FROM `user` WHERE id BETWEEN 1 AND 2;
```
输出
```sql
+----+------+-----+
| id | name | age |
+----+------+-----+
|  1 | 张三 |   1 |
|  2 | 李四 |   1 |
+----+------+-----+
```
看到查询结果是包含边界值的

### IS NULL
判断age是NULL值的 不是字符串的null，这里的NULL表示空值就是没有值
```sql
SELECT * FROM `user` WHERE age IS NULL;
```
输出
```sql
+----+------+------+
| id | name | age  |
+----+------+------+
|  3 | 李四 | NULL |
+----+------+------+
1 row in set (0.00 sec)
```

### AND
查询姓名是李四且年龄为1的记录
```sql
SELECT * FROM `user` WHERE name = '李四' AND age = 1;
```
输出
```sql
+----+------+------+
| id | name | age  |
+----+------+------+
|  2 | 李四 |    1 |
+----+------+------+
1 row in set (0.00 sec)
```

### OR
查询姓名是李四或者年龄为1的记录
```sql
SELECT * FROM `user` WHERE name = '李四' OR age = 1;
```
输出
```sql
+----+------+------+
| id | name | age  |
+----+------+------+
|  1 | 张三 |    1 |
|  2 | 李四 |    1 |
|  3 | 李四 |    2 |
+----+------+------+
3 rows in set (0.00 sec)
```

!> AND 和 OR 执行顺序是 先执行 AND 后执行 OR，如果想先执行OR 用()括起来

### IN
查询年龄为1和2的记录
```sql
SELECT * FROM `user` WHERE age IN(1,2);
```
输出
```sql
+----+------+------+
| id | name | age  |
+----+------+------+
|  1 | 张三 |    1 |
|  2 | 李四 |    1 |
|  3 | 李四 |    2 |
+----+------+------+
3 rows in set (0.00 sec)
```

> IN 能完成与 OR 相同的功能  age=1 OR age=2 相当于 age IN(1,2) 

### NOT
取反操作
只可用于 IN\BETWEEN\EXISTS 取反
查询年龄不为1和2的记录
```sql
SELECT * FROM `user` WHERE age NOT IN(1,2);
```
输出
```sql
Empty set (0.00 sec)
```

### LIKE
语法 LIKE '%查询条件值%'
百分号可省略，省略一边表示查询值以一边开始模糊匹配
查询姓李的记录
```sql
SELECT * FROM `user` WHERE name LIKE '%李%';
```
输出
```sql
+----+------+------+
| id | name | age  |
+----+------+------+
|  2 | 李四 |    1 |
|  3 | 李四 |    2 |
+----+------+------+
2 rows in set (0.00 sec)
```

!> 百分号可以匹配任意东西，除了NULL值，还有一个下划线通配符 _ ，只能匹配单个字符，不常用

!> LIKE里的百分号都省略时即不使用通配符，例如下面这个例子将不会返回数据，但是LIKE '李四' 又能匹配全部,因为LIKE匹配的是整个串而不是一个字,所以不加百分号就要用完整的值

```sql
SELECT * FROM `user` WHERE name LIKE '李';
```
输出
```sql
Empty set (0.00 sec)
```

### REGEXP
语法 REGEXP '正则表达式'
```sql
SELECT * FROM `user` WHERE name REGEXP '李';
```
输出
```sql
+----+------+------+
| id | name | age  |
+----+------+------+
|  2 | 李四 |    1 |
|  3 | 李四 |    2 |
+----+------+------+
2 rows in set (0.06 sec)
```
!> 正则可以匹配任意数据，但是使用正则会影响性能，需要注意的是mysql正则表达式的转义字符是双斜杠\\\\而不是\，一个由MYSQL解释 一个由正则表达式库解释

### AS
别名
```sql
SELECT name AS username FROM `user` AS u;
```
输出
```sql
+----------+
| username |
+----------+
| 张三     |
| 李四     |
| 李四     |
+----------+
3 rows in set (0.03 sec)
```

### GROUP BY
聚合指定字段的记录即数据分组
如按年龄分组查询  年龄为1的有两条 年龄为2的有一条
```sql
SELECT name,age,COUNT(*) AS row_count FROM `user` GROUP BY age;
```
输出
```sql
+------+------+-----------+
| name | age  | row_count |
+------+------+-----------+
| 张三 |    1 |         2 |
| 李四 |    2 |         1 |
+------+------+-----------+
2 rows in set (0.00 sec)
2 rows in set (0.00 sec)
```

!> 同样是聚合,区别 DISTINCT 是作用于所有列，而 GROUP BY 作用于一个字段
!> NULL值也会作为一个分组出现
!> GROUP BY 必须在ORDER BY之前

### HAVING
过滤 GROUP BY分组后的数据,支持所有WHERE操作符
如按年龄分组查询 筛选每组个数大于1的数据
```sql
SELECT name,age,COUNT(*) AS row_count FROM `user` GROUP BY age HAVING(row_count > 1);
```
输出
```sql
+------+------+-----------+
| name | age  | row_count |
+------+------+-----------+
| 张三 |    1 |         2 |
+------+------+-----------+
1 row in set (0.00 sec)
```

!> WHERE是过滤没分组的数据

### USING
用于简化联合查询
```sql
SELECT * FROM user INNER JOIN (SELECT id FROM user) AS u ON user.id = u.id;
```
上面的语句可以用下面的代替
```sql
SELECT * FROM user INNER JOIN (SELECT id FROM user) AS u USING(id);
```

### 子查询
利用子查询的结果集作为父查询的条件
```sql
SELECT*FROM `user` WHERE id IN (
SELECT id FROM `user` WHERE id BETWEEN 1 AND 2);
```
输出
```sql
+----+------+------+
| id | name | age  |
+----+------+------+
|  1 | 张三 |    1 |
|  2 | 李四 |    1 |
+----+------+------+
2 rows in set (0.02 sec)
```

### EXISTS
查询判断条件 不返回数据 只返回布尔类型
如找出有地址的用户
```sql
SELECT
	* 
FROM
	`user` u 
WHERE
	EXISTS ( SELECT * FROM `user_address` ua WHERE u.id = ua.user_id );
```
输出
```sql
+----+--------+------+
| id | name   | age  |
+----+--------+------+
|  1 | 张三   |    1 |
|  3 | 李五   |    2 |
+----+--------+------+
2 rows in set (0.00 sec)
```

### INNER JOIN
内联查询，查询2个表都有的部分
查询user表有地址的用户
```sql
SELECT
	u.*,
	ua.address 
FROM
	`user` AS u
	INNER JOIN `user_address` AS ua ON u.id = ua.user_id;
```
输出
```sql
+----+------+------+----------+
| id | name | age  | address  |
+----+------+------+----------+
|  1 | 张三 |    1 | 上海浦东 |
|  3 | 李四 |    2 | 浙江杭州 |
+----+------+------+----------+
2 rows in set (0.00 sec)
```

### LEFT JOIN
左外连接查询 全称 LEFT OUTER JOIN
查询主表全部数据 连接表没有的数据填充NULL
```sql
SELECT
	u.*,
	ua.address 
FROM
	`user` AS u
	LEFT JOIN `user_address` AS ua ON u.id = ua.user_id 
WHERE
	u.id < 4 
ORDER BY
	u.id;
```
输出
```sql
+----+------+------+----------+
| id | name | age  | address  |
+----+------+------+----------+
|  1 | 张三 |    1 | 上海浦东 |
|  2 | 李四 |    1 | NULL     |
|  3 | 李四 |    2 | 浙江杭州 |
+----+------+------+----------+
3 rows in set (0.00 sec)
```

### RIGHT JOIN
右外连接查询 全称 RIGHT OUTER JOIN
查询连接表全部数据
```sql
SELECT
	u.*,
	ua.address 
FROM
	`user` AS u
	RIGHT JOIN `user_address` AS ua ON u.id = ua.user_id 
WHERE
	u.id < 4 
ORDER BY
	u.id;
```
输出
```sql
+------+------+------+----------+
| id   | name | age  | address  |
+------+------+------+----------+
|    1 | 张三 |    1 | 上海浦东 |
|    3 | 李四 |    2 | 浙江杭州 |
+------+------+------+----------+
2 rows in set (0.00 sec)
```

### UNION
联合查询 将两条查询语句的结果集合并在一起返回，并剔除重复数据
```sql
SELECT * FROM `user` WHERE name = '李四' AND age = 2 UNION
SELECT * FROM `user` WHERE name = "张三" ORDER BY id;
```
默认情况下会剔除重复数据，如果想返回所有数据使用 UNION ALL

### FULLTEXT
全文索引
语法 MATCH(列1,列2) AGAINST(搜索表达式1,搜索表达式2)
mysql5.7.6开始支持中文的全文索引 所以尽量用新版本，或者使用[Sphinx](https://www.runoob.com/w3cnote/sphinx-sql-search-engine.html)
新增索引 或者在navicat 新增索引下面的解析器里填 ngram
```sql
ALTER TABLE `user_address` ADD FULLTEXT INDEX address ( `address` ) WITH PARSER ngram;
```
搜索
```sql
SELECT * FROM `user_address` WHERE MATCH(address) AGAINST("杭州");
```
输出
```sql
+----+---------+----------+
| id | user_id | address  |
+----+---------+----------+
|  2 |       3 | 浙江杭州 |
+----+---------+----------+
1 row in set (0.00 sec)
```
!> mysql 全文索引以词为基础 就是分词的不是分字的 例如搜 AGAINST("杭") 就搜不到了

### 联合索引
有多个WHERE AND 条件的，即可用联合索引
例如
```sql
SELECT * FROM `user` WHERE name = "张三" AND age > 1;
```
即可添加联合索引
```sql
ALTER TABLE `user` ADD KEY `name_age_index` (`name`,`age`) USING BTREE;
```

## 运算
支持+ - * /
```sql
SELECT name,age * 2  FROM `user`;
```
输出
```sql
+------+---------+
| name | age * 2 |
+------+---------+
| 张三 |       2 |
| 李四 |       2 |
| 李四 |       4 |
+------+---------+
3 rows in set (0.02 sec)
```

## 函数
### CONCAT
拼接字段符
语法 CONCAT (filed1,filed2,...)
查询输出拼接好的字符串
```sql
SELECT CONCAT(name,'今年',age,'岁') AS name FROM `user`;
```
输出
```sql
+-------------+
| name        |
+-------------+
| 张三今年1岁 |
| 李四今年1岁 |
| 李四今年2岁 |
+-------------+
3 rows in set (0.02 sec)
```

### TRIM\LTRIM\RTRIM
去除 左右两边空格 \ 左空格 \ 右空格
```sql
SELECT TRIM(name) FROM `user`;
```
输出
```sql
+------------+
| TRIM(name) |
+------------+
| 张三       |
| 李四       |
| 李四       |
+------------+
3 rows in set (0.01 sec)
```

### UPPER\LOWER
全部字母转大写 \ 转小写
```sql
SELECT UPPER(name) AS name FROM `en_user`;
```
输出
```sql
+----------+
| name     |
+----------+
| ZHANGSAN |
| LISI     |
| ZHANGSAN |
+----------+
3 rows in set (0.00 sec)
```

### LENGTH
计算字符串长度
```sql
SELECT LENGTH(name) AS name_length FROM `user`;
```
输出
```sql
+-------------+
| name_length |
+-------------+
|           6 |
|           6 |
|           6 |
+-------------+
3 rows in set (0.01 sec)
```

### LEFT\RIGHT
返回索引为1左边的字符
下面取名字的姓
```sql
SELECT LEFT(name,1) AS xing FROM `user`;
```
输出
```sql
+------+
| xing |
+------+
| 张   |
| 李   |
| 李   |
+------+
3 rows in set (0.00 sec)
```

### SUBSTRING
语法 SUBSTRING(str,n,m)
截取字符串str 从索引n开始后的m个字符
```sql
SELECT SUBSTRING(name,2,1) AS ming FROM `user`;
```
输出
```sql
+------+
| ming |
+------+
| 三   |
| 四   |
| 四   |
+------+
3 rows in set (0.00 sec)
```
!>  n是从1开始的注意一点

### SUBSTRING_INDEX
语法 SUBSTRING_INDEX(str,find_str,count)
截取指定字符串str中find_str出现次数之前或之后的字符串，不包括他自己
```sql
SELECT SUBSTRING_INDEX(name,'n',1) AS ming FROM `en_user`;
```
输出
```sql
+----------+
| ming     |
+----------+
| zha      |
| lisi     |
| ZHANGSAN |
+----------+
```

### 日期函数
| 函数名        | 描述                           |
| ------------- | ------------------------------ |
| ADDDATE()     | 增加一个日期（天、周等）       |
| ADDTIME()     | 增加一个时间（时、分等）       |
| CURTIME()     | 返回当前时间                   |
| DATE()        | 返回日期时间的日期部分         |
| DATEDIFF()    | 计算两个日期之差               |
| DATE_ADD()    | 高度灵活的日期运算函数         |
| DATE_FORMAT() | 返回一个格式化的日期或时间串   |
| DAY()         | 返回一个日期的天数部分         |
| DAYOFWEEK()   | 对于一个日期，返回对应的星期几 |
| HOUR()        | 返回一个时间的小时部分         |
| MINUTE()      | 返回一个时间的分钟部分         |
| MONTH()       | 返回一个日期的月份部分         |
| NOW()         | 返回当前日期和时间             |
| SECOND()      | 返回一个时间的秒部分           |
| TIME()        | 返回一个日期时间的时间部分     |
| YEAR()        | 返回一个日期的年份部分         |
如当前时间格式化输出
```sql
SELECT DATE_FORMAT(CURTIME(),'%y年%m月%d日 %H:%i:%s') AS create_date FROM `en_user`;
```
输出
```sql
+-----------------------+
| create_date           |
+-----------------------+
| 21年09月13日 15:36:58 |
| 21年09月13日 15:36:58 |
| 21年09月13日 15:36:58 |
+-----------------------+
3 rows in set (0.00 sec)
```

### 数学方法
| 函数名  | 描述                |
| ------- | ------------------- |
| ABS()   | 返回一个数的绝对值  |
| COS()   | 返回一个角度的余弦  |
| EXP()   | 返回一个数的指数值  |
| MOD()   | 返回除操作的余数    |
| FLOOR() | 向下取整 即舍弃小数 |
| ROUND() | 向上取整 即四舍五入 |
| PI()    | 返回圆周率          |
| RAND()  | 返回一个 0~1 随机数 |
| SIN()   | 返回一个角度的正弦  |
| SQRT()  | 返回一个数的平方根  |
| TAN()   | 返回一个角度的正切  |
如年龄随机一下
```sql
SELECT name,ROUND(RAND() * 100) AS age FROM `user`;
```
输出
```sql
+------+-----+
| name | age |
+------+-----+
| 张三 |  26 |
| 李四 |  61 |
| 李四 |  29 |
+------+-----+
3 rows in set (0.00 sec)
```

### 统计函数
| 函数名  | 描述             |
| ------- | ---------------- |
| AVG()   | 返回某列的平均值 |
| COUNT() | 返回某列的行数   |
| MAX()   | 返回某列的最大值 |
| MIN()   | 返回某列的最小值 |
| SUM()   | 返回某列值之和   |
如取平均年龄
```sql
SELECT AVG(age) AS age_avg FROM `user`;
```
输出
```sql
+---------+
| age_avg |
+---------+
|  1.3333 |
+---------+
1 row in set (0.00 sec)
```

!> 这里COUNT 有2种用法 一种是统计所有行的数量（有没有NULL都被统计） 另一种是统计一个字段不为NULL值数量

统计所有行数量
```sql
SELECT COUNT(*) AS row_count FROM `user`;
```
输出
```sql
+-----------+
| row_count |
+-----------+
|         3 |
+-----------+
1 row in set (0.00 sec)
```
统计有年龄的行数量
```sql
SELECT COUNT(age) AS has_age_count FROM `user`;
```
输出
```sql
+---------------+
| has_age_count |
+---------------+
|             3 |
+---------------+
1 row in set (0.00 sec)
```

## 新增
### INSERT
语法 INSERT INTO table_name (字段1,字段2) VALUES ("值1","值2")
返回影响行数
```sql
INSERT INTO `user` (name,age) VALUES ("张三",4);
```
输出
```sql
Query OK, 1 row affected (0.06 sec)
```

### LOW_PRIORITY
优先级  
降低INSERT INTO 语句的优先级，也适用于UPDATE和DELETE语句，可提高SELECT语句的效率
```sql
INSERT LOW_PRIORITY INTO `user` (name,age) VALUES ("张三",4);
```

### 插入多行
语法 INSERT INTO table_name (字段1,字段2) VALUES ("值1","值2"),("值3","值4")
```sql
INSERT INTO `user` (name,age) VALUES ("张三",5),("张三",6);
```
输出
```sql
Query OK, 2 rows affected (0.04 sec)
Records: 2  Duplicates: 0  Warnings: 0
```

### INSERT SELECT
将查询的结果集作为数据插入到表格
```sql
INSERT INTO `user` (name,age) SELECT name,age FROM user;
```
输出
```sql
Query OK, 8 rows affected (0.03 sec)
Records: 8  Duplicates: 0  Warnings: 0
```
!> SELETE 选择的列名不一定要和插入的列名一致，只要顺序对应即可

## 更新
### UPDATE
语法 UPDATE table_name SET 字段1 = "值1",字段2 = "值2" WHERE 条件
返回影响行数
```sql
UPDATE `user` SET name="李五",age = 4 WHERE name = "李四" AND age = 2;
```
输出
```sql
Query OK, 2 rows affected (0.05 sec)
Rows matched: 2  Changed: 2  Warnings: 0
```

## 删除
### DELETE
语法 DELETE FROM table_name WHERE 条件
```sql
DELETE FROM `user` WHERE id>3;
```
输出
```sql
Query OK, 13 rows affected (0.15 sec)
```

## 表操作
### CREATE
```sql
CREATE TABLE IF NOT EXISTS `user`(
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT COMMENT 'id',
  `name` varchar(255) NOT NULL DEFAULT '' COMMENT 'name',
  `age` int(255) unsigned DEFAULT '0' COMMENT 'age',
  PRIMARY KEY (`id`),
  KEY `age_index` (`age`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

### ALTER
语法 ALTER TABLE table_name 操作
新增列
```sql
ALTER TABLE `user` ADD tel VARCHAR(13) NOT NULL DEFAULT '' COMMENT '电话';
```
删除列
```sql
ALTER TABLE `user` DROP tel;
```
定义外键
```sql
ALTER TABLE `user_address` ADD CONSTRAINT `user_id` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`);
```
### DROP
删除表  
语法 DROP TABLE table_name

### RENAME
重命名表  
语法 RENAME TABLE table_name To new_table_name

### TRUNCATE
清空表格并重写创建
```sql
TRUNCATE `user_address`;
```
!> 有关联表的主表不能够被清空提示：Cannot truncate a table referenced in a foreign key constraint 

## 视图
### 创建视图
视图就是虚拟表 只包含查询语句，用来简化复杂SQL语句

```sql
CREATE VIEW user_view AS
SELECT u.*,ua.address FROM `user` AS u LEFT JOIN user_address AS ua ON u.id = ua.user_id;
```
!> 在navicat 中 创建视图按钮可省略 CREATE VIEW user_view AS

### 使用视图构建的虚拟表查询
```sql
SELECT * FROM user_view;
```
输出
```sql
+----+--------+------+--------------+
| id | name   | age  | address      |
+----+--------+------+--------------+
|  1 | 张三   |    1 | 上海浦东     |
|  3 | 李四   |    2 | 浙江杭州     |
|  2 | 李四   |    1 | NULL         |
+----+--------+------+--------------+
3 rows in set (0.02 sec)
```

## 存储过程
### 什么是存储过程
就是保存SQL语句的集合

### 使用
navicat里使用 函数菜单——新建函数——过程 来创建存储过程
```sql
CREATE PROCEDURE `test`(n INT)
BEGIN
	SELECT * FROM `user` WHERE id<n;
	SELECT * FROM `user_address` WHERE id<n;
END
```
需要返回变量可以用 下面2种
IN 和 OUT 分别表示传入参数和返回参数
```sql
CREATE PROCEDURE `test`(IN n INT,OUT total INT)
BEGIN
	SELECT COUNT(*) INTO total FROM `user` WHERE id<n;
END
```
或
```sql
CREATE PROCEDURE `test`(IN n INT)
BEGIN
	SELECT COUNT(*) INTO @total FROM `user` WHERE id<n;
  SELECT @total;
END
```


### 百万条数据插入方法
利用MYSQL执行常量AUTOCOMMIT设置为0，阻止他每次执行sql都提交
```sql
CREATE PROCEDURE `addData`(n int)
BEGIN
	DECLARE i INT DEFAULT 0;
	SET AUTOCOMMIT = 0;
	WHILE i<n DO
		INSERT INTO user(`name`,`age`) VALUES ('随机数据',ROUND(RAND()*100));
		SET i = i+1;
	END WHILE;
	SET AUTOCOMMIT = 1;  
END
```
使用事务
```sql
CREATE PROCEDURE `addData`(n INT)
BEGIN
	#Routine body goes here...
	DECLARE error INT DEFAULT 0;
	DECLARE i INT DEFAULT 0;
	DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET error = 1;
		START TRANSACTION;
			WHILE i<n DO
				INSERT INTO user(`name`,`age`) VALUES ('随机数据',ROUND(RAND()*100));
				SET i = i+1;
			END WHILE;
			IF error = 1 THEN
				ROLLBACK;
			ELSE
				COMMIT;
			END If;
-- 			返回结果集
	SELECT error;
END
```

## 触发器
### 创建
```sql
CREATE TRIGGER update_trigger AFTER INSERT ON `user` FOR EACH ROW
UPDATE `user_address` SET address = "上海郊区" WHERE id = 1;
```

### 删除
```sql
DROP TRIGGER update_trigger;
```

## 维护
### 备份
windows可以使用navicate自动备份

### 导出
1. 使用navicate导出（不推荐，导出的数据是一条一条插入的）  

2. 使用命令（推荐）
    ```
    # 首先查看服务器数据库数值 max_allowed_packet net_buffer_length
    show variables like 'max_allowed_packet';
    show variables like 'net_buffer_length';
    # 然后根据这两个数值，导出本地数据库
    # 虽然默认开启了extended-insert合并参数的，但是如果数据超过1M，也会生成多个insert。将net_buffer_length 的值调高，这样导出的insert合并的条数多。
    mysqldump -u root -p -h localhost -P 3306 datebase_name -e --max_allowed_packet=4194304 --net_buffer_length=1024000 > mysqldump.sql
    ```

### 导入
1. 方式1
   ```sql
   mysql -u root -p -h 服务器数据库ip -P 3306 datebase_name < C:\mysqldump.sql
   ```

2. 方式2(推荐)
    ```sql
    mysql -u root -p -h 服务器数据库ip -P 3306
    USE datebase_name;
    SET names utf8;
    SOURCE sql文件路径
    ```

### 检查
检查表是否有错误 操作不可在表高频繁状态下执行
```sql
CHECK TABLE table_name;
```

### 分析
操作不可在表高频繁状态下执行
ANALYZE TABLE user;

## 访问控制
### 查看权限
```sql
USE mysql;
SHOW GRANTS FOR root;
```

### 修改密码
```sql
USE mysql;
SET PASSWORD FOR root = Password("");
FLUSH PRIVILEGES;
EXIT;
```

## 性能优化
### 查询优化
1. 使用EXPLAIN
   ![calc](../images/explain.png)  
2. SELECT * 最好不用，除非需要所有的列，因为检索不需要的列会降低性能
3. LIKE '%xxx%' %放在搜索模式 'xxx' 最前面查询最慢，不会走索引
4. 查询使用 ORDER BY 变慢
    查询慢的语句
    ```sql
    SELECT * FROM `user` ORDER BY age,id LIMIT 1000000,2;
    ```
    输出
    ```sql
    > 时间: 1.176s
    ```
    优化上面的语句(推荐)
    ```sql
    SELECT
        *
    FROM
        `user` INNER JOIN ( SELECT id FROM `user` ORDER BY age,id LIMIT 1000000,2 ) AS u USING ( id );
    ```
    输出
    ```sql
    > 时间: 0.171s
    ```
    上面的写法还能用自连接代替（非常慢）
    ```sql
    SELECT
        u1.name,
        u1.age,
        u2.id 
    FROM
        `user` AS u1,
        `user` AS u2 
    WHERE
        u1.id = u2.id 
    ORDER BY
        u1.age,
        u1.id 
        LIMIT 1000000,
        2;
    ```
    输出
    ```
    > 时间: 13.634s
    ```

    另外一个例子
    优化前
    ```sql
    SELECT
        u.*,
        ua.address 
    FROM
        `user` AS u
        LEFT JOIN `user_address` AS ua ON u.id = ua.user_id 
    ORDER BY
        u.id
    LIMIT 100000,20;
    ```
    输出
    ```sql
    > 时间: 1.954s
    ```
    优化后
    ```sql
    SELECT
        u.*,
        ua.address 
    FROM
        ( SELECT * FROM `user` ORDER BY id LIMIT 100000, 20 ) AS u
        LEFT JOIN `user_address` AS ua ON u.id = ua.user_id;
    ```
    输出
    ```sql
    > 时间: 0.02s
    ```
    可以看到优化速度提升了一倍
5. 尽量不写没有WHERE的SQL语句，除非你需要所有数据
6. 连表查询连接的表越多，性能下降越厉害
7. 优化LIMIT语句
    使用索引覆盖
    ```sql
    SELECT * FROM `user` ORDER BY id LIMIT 1000000,2;
    ```
    输出
    ```sql
    +---------+--------------+------+
    | id      | name         | age  |
    +---------+--------------+------+
    | 1000001 | 随机数据     |   33 |
    | 1000002 | 随机数据     |   98 |
    +---------+--------------+------+
    2 rows in set (0.20 sec)
    ```
    优化后
    ```sql
    SELECT * FROM `user` WHERE id >= (
        SELECT id FROM `user` ORDER BY id LIMIT 1000000,1) ORDER BY id LIMIT 2;
    ```
    输出
    ```sql
    +---------+--------------+------+
    | id      | name         | age  |
    +---------+--------------+------+
    | 1000001 | 随机数据     |   33 |
    | 1000002 | 随机数据     |   98 |
    +---------+--------------+------+
    2 rows in set (0.16 sec)
    ```
8. 排序结果不一样
    ```sql
    SELECT * FROM `user` WHERE age > 10 ORDER BY age LIMIT 1000000,2;
    ```
    ```sql
    SELECT id FROM `user` WHERE age > 10 ORDER BY age LIMIT 1000000,2;
    ```
    如果age有多个重复值，那么结果将不一样，解决办法是再加上id字段
    ```sql
    SELECT * FROM `user` WHERE age > 10 ORDER BY age,id LIMIT 1000000,2;
    ```
9. 启用慢查询（版本mysql5.7）
    my.ini文件里面设置，不同版本配置不同
    ```ini
    #开启慢查询日志
    slow_query_log = on
    #指定日志文件保存路径
    slow_query_log_file = D:/mysql-5.7.34/slow_query.log
    #指定达到多少秒才算慢查询
    long_query_time = 1
    #记录没有使用索引的查询语句
    log_queries_not_using_indexes=on
    ```
10. 尽量避免使用 IN 和 NOT IN，会扫描全表，使用 EXISTS 或者 BETWEEN 代替
11. 尽量避免使用 OR 用 UNION 代替
12. 如果 GROUP BY 的结果不需要排序 那么显示的加上 GROUP BY NULL 会提高效率
13. UNION 除非要消除重复行 不然用UNION ALL代替
14. 尽量使用数字 如1，2 tinyintl类型 代表男女 字符串会降低查询和连接的性能

## 名词
### 主键
确定一条记录的唯一标识，一个表只能有一个主键，主键可以由多列组合而成
### 外键
外键是某个表中的一列，它对应主表的主键值
### BTREE
索引方法 B树索引 平衡多路查找树

