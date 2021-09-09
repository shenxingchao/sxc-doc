# Mysql
## 准备工作
### 安装mysql
参考[wnmp安装](/md/Php?id=wnmp搭建)中的mysql下载服务安装和服务启动 windows可设置成开机自启动

### 设置环境变量
环境变量-用户变量-Path 添加  D:\mysql-5.6.43\bin

### 连接到Mysql
打开cmd
```powershell
mysql -u root -p ;
```
> 初始密码为空
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

作用于所有列，聚合所有指定列相同的数据，不能作用于单列
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







## 性能优化
### 查询优化
1. SELECT * 最好不用，除非需要所有的列，因为检索不需要的列会降低性能 
2. LIKE '%xxx%' LIKE 相对于其他查询更慢 %放在搜索模式 'xxx' 最前面查询最慢