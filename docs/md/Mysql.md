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
#### 简单查询

**1.查询用户表的姓名和id**

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

**2.查询用户表全部**

```sql
SELECT * FROM `user`;
```
输出结果和上面相同

**3.DISTINCT关键字**

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




## 性能优化
### 查询优化
1. SELECT * 最好不用，除非需要所有的列，因为检索不需要的列会降低性能 