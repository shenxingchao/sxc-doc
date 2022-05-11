# JavaWeb

## IDEA

### 添加jar包

将jar包放在libs文件夹下，右键as library,选择项目即可

删除 File - Project Structure - Modules - Dependencies 选中要删除的包remove即可


## JDBC

需要将mysql-connector-java.jar包导入,mysql5.7可以直接用8.0版本的，也支持

[下载地址](https://dev.mysql.com/downloads/connector/j/) 选择Platform Independent

### 步骤

db.properties文件内容见 *写入配置文件加载(推荐)*

1. 注册驱动，加载Driver类
2. 获取连接，得到Connection
3. 执行sql
4. 释放资源

```java
import java.io.*;
import java.sql.*;
import java.util.Properties;

public class Demo {
    public static void main(String[] args) throws SQLException, IOException, ClassNotFoundException {
        //新建Properties对象
        Properties properties = new Properties();
        //获取文件输入流
        BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("db.properties"));
        //加载输入流到properties对象
        properties.load(bufferedInputStream);
        //加载Driver时自动完成注册
        Class.forName(properties.getProperty("driver"));
        //得到连接
        Connection connection = DriverManager.getConnection(properties.getProperty("url"), properties.getProperty("user"), properties.getProperty("password"));
        //执行sql statement.execute 返回值不代表执行成功或失败，如果执行的是查询语句，返回的是ResultSet则返回true，其他查询或更新语句返回false
        Statement statement = connection.createStatement();
        int i = statement.executeUpdate("INSERT INTO `user` (name,age) VALUES ('张四',4)");
        if (i > 0) {
            System.out.println("新增一条数据");
        }
        ResultSet resultSet = statement.executeQuery("SELECT * FROM `user`");
        while (resultSet.next()) {
            int id = resultSet.getInt(1);
            String name = resultSet.getString(2);
            int age = resultSet.getInt(3);
            System.out.println(id + "\t" + name + "\t" + age);
        }
        //关闭连接
        resultSet.close();
        statement.close();
        connection.close();
        //关闭流
        bufferedInputStream.close();
    }
}
```

### 连接方式

原始方式

```java
import com.mysql.jdbc.Driver;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

public class Demo {
    public static void main(String[] args) throws SQLException {
        //得到驱动
        Driver driver = new Driver();
        //得到连接
        Properties properties = new Properties();
        properties.put("user", "root");
        properties.put("password", "");
        Connection connection = driver.connect("jdbc:mysql://localhost:3307/dbname?useSSL=false&&rewriteBatchedStatements=true&&characterEncoding=utf-8", properties);
        //关闭连接
        connection.close();
    }
}
```

反射方式

```java
import com.mysql.jdbc.Driver;

import java.lang.reflect.InvocationTargetException;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

public class Demo {
    public static void main(String[] args) throws SQLException, ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
        //得到驱动
        Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");
        Driver driver = (Driver) aClass.getConstructor().newInstance();
        //得到连接
        Properties properties = new Properties();
        properties.put("user", "root");
        properties.put("password", "");
        Connection connection = driver.connect("jdbc:mysql://localhost:3307/dbname?useSSL=false&&rewriteBatchedStatements=true&&characterEncoding=utf-8", properties);
        //关闭连接
        connection.close();
    }
}
```

DriverManager方式

```java
import com.mysql.jdbc.Driver;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Demo {
    public static void main(String[] args) throws SQLException, ClassNotFoundException, IllegalAccessException, InstantiationException {
        //得到驱动
        Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");
        Driver driver = (Driver) aClass.newInstance();
        //注册驱动
        DriverManager.registerDriver(driver);
        //得到连接
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3307/dbname?useSSL=false&&rewriteBatchedStatements=true&&characterEncoding=utf-8", "root", "");
        //关闭连接
        connection.close();
    }
}
```

加载Driver时自动完成注册

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Demo {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        //加载Driver时自动完成注册
        Class.forName("com.mysql.jdbc.Driver");
        //得到连接
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3307/dbname?useSSL=false&&rewriteBatchedStatements=true&&characterEncoding=utf-8", "root", "");
        //关闭连接
        connection.close();
    }
}
```

全自动加载

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Demo {
    public static void main(String[] args) throws SQLException {
        //在mysql-connector-java.jar/META-INF/services/java.sql.Driver自动注册了，所以不加载都可以
        //得到连接
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3307/dbname?useSSL=false&&rewriteBatchedStatements=true&&characterEncoding=utf-8", "root", "");
        //关闭连接
        connection.close();
    }
}
```

写入配置文件加载(推荐)

db.properties

```properties
user=root
password=
url=jdbc:mysql://localhost:3307/dbname?useSSL=false&&rewriteBatchedStatements=true&&characterEncoding=utf-8
driver=com.mysql.jdbc.Driver
```

tips:useSSL 去除mysql高版本ssl警告；rewriteBatchedStatements 使用批处理功能；characterEncoding 编码格式

```java
import java.io.*;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class Demo {
    public static void main(String[] args) throws SQLException, IOException, ClassNotFoundException {
        //新建Properties对象
        Properties properties = new Properties();
        //获取文件输入流
        BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("db.properties"));
        //加载输入流到properties对象
        properties.load(bufferedInputStream);
        //加载Driver时自动完成注册
        Class.forName(properties.getProperty("driver"));
        //得到连接
        Connection connection = DriverManager.getConnection(properties.getProperty("url"), properties.getProperty("user"), properties.getProperty("password"));
        //关闭连接
        connection.close();
        //关闭流
        bufferedInputStream.close();
    }
}
```

### 预处理

PreparedStatement 效率高，不存在sql注入风险

因为Statement存在sql注入风险，所以不用

```java
import java.io.*;
import java.sql.*;
import java.util.Properties;

public class Demo {
    public static void main(String[] args) throws SQLException, IOException, ClassNotFoundException {
        //新建Properties对象
        Properties properties = new Properties();
        //获取文件输入流
        BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("db.properties"));
        //加载输入流到properties对象
        properties.load(bufferedInputStream);
        //加载Driver时自动完成注册
        Class.forName(properties.getProperty("driver"));
        //得到连接
        Connection connection = DriverManager.getConnection(properties.getProperty("url"), properties.getProperty("user"), properties.getProperty("password"));
        //预处理sql
        PreparedStatement preparedStatement = connection.prepareStatement("SELECT * FROM `user` where id = ?");
        //设置 占位符？处的值 注意这里的参数顺序从1开始，第二个表示参数值
        //或者作为对象设置setObject(1, 2)也可以
        preparedStatement.setInt(1, 2);
        //执行sql查询 其他增删改使用executeUpdate
        ResultSet resultSet = preparedStatement.executeQuery();
        while (resultSet.next()) {
            int id = resultSet.getInt(1);//也可以改成resultSet.getInt("id") 或者 Object id = resultSet.getObject("id")
            String name = resultSet.getString(2);
            int age = resultSet.getInt(3);
            System.out.println(id + "\t" + name + "\t" + age);//2	李四	1
        }
        //关闭连接
        resultSet.close();
        preparedStatement.close();
        connection.close();
        //关闭流
        bufferedInputStream.close();
    }
}
```

### 封装工具类及调用

JDBCUtil

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.sql.*;
import java.util.Properties;

public class JDBCUtil {
    //连接
    public static Connection connection;

    //静态代码块只会加载一次
    static {
        //新建Properties对象
        Properties properties = new Properties();
        try {
            //获取文件输入流
            BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("db.properties"));
            //加载输入流到properties对象
            properties.load(bufferedInputStream);
            //加载Driver时自动完成注册
            Class.forName(properties.getProperty("driver"));
            //得到连接
            connection = DriverManager.getConnection(properties.getProperty("url"), properties.getProperty("user"), properties.getProperty("password"));
            //关闭流
            bufferedInputStream.close();
        } catch (Exception e) {
            //本来这里抛出的是编译异常，现在转成运行异常，在运行阶段通过jvm抛出异常，调用者也可以手动处理该异常
            throw new RuntimeException(e);
        }
    }

    /**
     * 关闭并释放资源
     *
     * @param resultSet         结果集
     * @param preparedStatement 预处理
     * @param closeConnection   连接是否关闭
     */
    public static void close(ResultSet resultSet, PreparedStatement preparedStatement, Boolean closeConnection) {
        //关闭连接
        try {
            if (resultSet != null) {
                //关闭结果集释放资源
                resultSet.close();
            }
            if (preparedStatement != null) {
                //关闭预处理释放资源
                preparedStatement.close();
            }
            if (closeConnection != null && closeConnection) {
                //关闭连接释放资源
                connection.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

调用

```java
import java.sql.*;

public class Demo {
    public static void main(String[] args) throws SQLException {
        //得到连接
        Connection connection = JDBCUtil.connection;
        //预处理sql
        PreparedStatement preparedStatement = connection.prepareStatement("SELECT * FROM `user` where id = ?");
        preparedStatement.setInt(1, 2);
        //执行sql查询 其他增删改使用executeUpdate
        ResultSet resultSet = preparedStatement.executeQuery();
        while (resultSet.next()) {
            int id = resultSet.getInt("id");
            String name = resultSet.getString("name");
            int age = resultSet.getInt("age");
            System.out.println(id + "\t" + name + "\t" + age);//2	李四	1
        }
        //关闭连接
        JDBCUtil.close(resultSet, preparedStatement, true);
    }
}
```

### 事务

```java
import java.sql.*;

public class Demo {
    public static void main(String[] args) throws SQLException {
        //得到连接
        Connection connection = JDBCUtil.connection;
        //设置为不提交,相当于开启事务
        connection.setAutoCommit(false);
        //预处理sql
        PreparedStatement preparedStatement = connection.prepareStatement("UPDATE `user` SET name=?,age = ? WHERE name = ? AND age = ?");
        preparedStatement.setString(1, "李五");
        preparedStatement.setInt(2, 4);
        preparedStatement.setString(3, "李四");
        preparedStatement.setInt(4, 2);
        //将上面这条语句改为下面这条就不会执行成功,且抛出异常
        //preparedStatement.setString(4, "二");
        try {
            //执行sql
            int i = preparedStatement.executeUpdate();
            //提交事务
            connection.commit();
            if (i > 0) {
                System.out.println("更新成功");
            }
        } catch (Exception e) {
            connection.rollback();
            throw new RuntimeException(e);
        }
        //关闭连接
        JDBCUtil.close(null, preparedStatement, true);
    }
}
```

### sql批处理

批量处理减少网络开销和编译次数，提高效率；

tips:事务和批处理的区别，事务在数据库层执行，消耗数据库内存；批处理在客户端层执行，消耗客户端内存。测试过批量添加50W条数据比事务快6.8倍，如果可以选择批处理更好。

```java
import java.sql.*;

public class Demo {
    public static void main(String[] args) throws SQLException {
        //得到连接
        Connection connection = JDBCUtil.connection;
        //预处理sql
        PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO `user` (name,age) VALUES (?,?)");
        //添加批处理
        for (int i = 0; i < 10; i++) {
            preparedStatement.setString(1, "张三");
            preparedStatement.setInt(2, i);
            preparedStatement.addBatch();
        }
        //执行sql批处理
        preparedStatement.executeBatch();
        //清空sql批处理
        preparedStatement.clearBatch();
        //关闭连接
        JDBCUtil.close(null, preparedStatement, true);
    }
}
```

### 连接池

**c3p0**

[下载地址](https://sourceforge.net/projects/c3p0/?source=navbar) 直接下载即可

下载并将c3p0-0.9.5.5.jar包和mchange-commons-java-0.2.19.jar包添加到项目

使用代码设置来连接

```java

import com.mchange.v2.c3p0.ComboPooledDataSource;

import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.util.Properties;
import java.sql.*;

public class Demo {
    public static void main(String[] args) throws Exception {
        //创建数据源对象
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
        //新建Properties对象
        Properties properties = new Properties();
        //获取文件输入流
        BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("db.properties"));
        //加载输入流到properties对象
        properties.load(bufferedInputStream);
        //关闭流
        bufferedInputStream.close();
        //设置驱动
        comboPooledDataSource.setDriverClass(properties.getProperty("driver"));
        //设置协议
        comboPooledDataSource.setJdbcUrl(properties.getProperty("url"));
        //设置用户名
        comboPooledDataSource.setUser(properties.getProperty("user"));
        //设置密码
        comboPooledDataSource.setPassword(properties.getProperty("password"));
        //初始化连接池数量
        comboPooledDataSource.setInitialPoolSize(10);
        //设置最大连接数
        comboPooledDataSource.setMaxPoolSize(50);
        //得到连接 这里会打印一堆log信息，暂时不管他
        Connection connection = comboPooledDataSource.getConnection();
        //关闭
        connection.close();
    }
}
```

使用配置文件

配置文件必须命名为c3p0-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
    <!-- 默认配置，只可以出现一次 -->
    <default-config>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">
            <![CDATA[jdbc:mysql://localhost:3307/dbname?useSSL=false&&rewriteBatchedStatements=true&&characterEncoding=utf-8]]>
        </property>
        <property name="user">root</property>
        <property name="password"></property>
        <!--每次增长连接数-->
        <property name="acquireIncrement">3</property>
        <!--初始化连接数-->
        <property name="initialPoolSize">3</property>
        <!--最大连接数-->
        <property name="maxPoolSize">15</property>
        <!--最小连接数-->
        <property name="minPoolSize">3</property>
        <!--可连接的最多命令对象数-->
        <property name="maxStatements">5</property>
        <!--每个连接对象可连接的最多命令对象数-->
        <property name="maxStatementsPerConnection">2</property>
    </default-config>
    <!-- 名称配置 -->
    <named-config name="dataSource">
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">
            <![CDATA[jdbc:mysql://localhost:3307/dbname?useSSL=false&&rewriteBatchedStatements=true&&characterEncoding=utf-8]]>
        </property>
        <property name="user">root</property>
        <property name="password"></property>
        <!--每次增长连接数-->
        <property name="acquireIncrement">3</property>
        <!--初始化连接数-->
        <property name="initialPoolSize">3</property>
        <!--最大连接数-->
        <property name="maxPoolSize">15</property>
        <!--最小连接数-->
        <property name="minPoolSize">3</property>
        <!--可连接的最多命令对象数-->
        <property name="maxStatements">5</property>
        <!--每个连接对象可连接的最多命令对象数-->
        <property name="maxStatementsPerConnection">2</property>
    </named-config>
</c3p0-config>
```

使用

```java

import com.mchange.v2.c3p0.ComboPooledDataSource;

import java.sql.*;

public class Demo {
    public static void main(String[] args) throws SQLException {
        //创建数据源对象 使用默认配置的话可以省略dataSource
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource("dataSource");
        //得到连接 这里会打印一堆log信息，暂时不管他
        Connection connection = comboPooledDataSource.getConnection();
        //关闭
        connection.close();
    }
}
```