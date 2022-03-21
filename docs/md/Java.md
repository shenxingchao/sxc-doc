# Java

## let’s go

java se = 桌面软件

java ee  = web

android kotlin  = 手机 

## jdk

java JDK 包含了java开发工具(java、javac等)和jre(java运行环境)

jre = JVM(java虚拟机) + Java核心类库

## 环境变量

直接我的电脑系统变量Path配置 jdk bin目录和 jre bin目录(根本不需要什么JAVA_HOME这种的，如果还要配置jdk下的其他目录，直接加就行了,简单明了)

```java
D:\jdk1.8.0_191\bin
D:\jdk1.8.0_191\jre\bin
```

输入cmd 输入 java -version查看版本

## HelloWorld

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.print("hello world!");
    }
}
```

## 编译

javac HelloWorld.java

## 运行

java HelloWorld

```java
PS D:\sxc\javastudy> java HelloWorld
hello world!
```

## args

这里的String[] args 和python里的*args差不多 不确定个数的参数打包数组

如何查看

```java
public class HelloWorld {
    public static void main(String[] args) {
        for (int i = 0; i < args.length; i++) {
            System.out.println(args[i]);
        }
        System.out.print("hello world!");
    }
}
```

编译运行查看输出

```java
PS D:\sxc\javastudy> javac HelloWorld.java
PS D:\sxc\javastudy> java HelloWorld -s -c
-s
-c
hello world!
```

tips：public修饰类有且仅有一个 文件名按public类名来命名,main方法可以写在非public类中

## VsCode运行配置

```json
{
  "code-runner.runInTerminal": true,
  "code-runner.executorMap": {
      "java": "chcp 936 && cd $dir && javac -encoding utf-8 $fileName && java $fileNameWithoutExt",
  },
}
```

## api文档

[https://www.matools.com/api/java8](https://www.matools.com/api/java8)

[https://docs.oracle.com/javase/8/docs/api/](https://docs.oracle.com/javase/8/docs/api/)

[https://devdocs.io/](https://devdocs.io/)

## 常用数据类型

```java
public class VarType {
    public static void main(String[] args) {
        // 整形
        int a = 123;
        int b = 345;
        System.out.println(a);
        System.out.println(b);
        //长整形
        long la = 123456;
        System.out.println(la);
        // 单精度浮点数
        float c = 3.14f;
        System.out.println(c);
        // 双精度浮点数
        double d = 3.1415926;
        System.out.println(d);
        // 字符串ps:不是基本数据类型
        String str = "hello world";
        System.out.println(str);
        // 单个字符 单引号
        char word = 97;
        System.out.println(word);
        // 布尔值
        boolean boolTrue = true;
        boolean boolFalse = false;
        System.out.println(boolTrue);
        System.out.println(boolFalse);
    }
}
```

## 类型转换

```java
public class VarType {
    public static void main(String[] args) {
        // 强制类型转换
        byte a = 97;
        byte b = 2;
        byte c = (byte) (a + b);
        char word = (char) (a + b);
        // int转String
        String str = Integer.toString(1000);
        // double转String
        String str2 = Double.toString(1000.0);
        // String转int
        int num = Integer.parseInt(str);
        System.out.println(c);// 输出99
        System.out.println(word);// 输出c
        System.out.println(str);// 输出1000
        System.out.println(str2);// 输出1000.0
        System.out.println(num);// 输出1000
        // +号强转
        System.out.println(num + "");// 输出1000

    }
}
```

## 逻辑运算符

```java
public class AndOr {
    public static void main(String[] args) {
        int a = 1, b = 3;
        // && 短路规则 => 前面的条件如果为false后面的就不执行
        if (a > b && ++b == 4) {
            // ++b没执行
        }
        System.out.println("b=" + b);
        // & 不遵循短路规则 => 前面的如果是false那后面的也执行
        a = 1;
        b = 3;
        if (a > b & ++b == 4) {
            // ++b执行了
        }
        System.out.println("b=" + b);

    }
}
```

## 条件语句

```java
public class IfElse {
    public static void main(String[] args) {
        boolean a = true;
        boolean b = false;
        if (a) {
            System.out.println("a");
        } else if (!b) {
            System.out.println("因为a满足了不会输出,a满足了就跳出这个if else if else代码块了");
        } else {
            System.out.println("因为a满足了不会输出,else不会输出的");
        }
    }
}
```

## 循环

### for循环计1~100的和

```java
public class SumNumber {
    public static void main(String[] args) {
        int sum = 0;
        for (int i = 1; i < 101; i++) {
            sum+=i;
        }
        System.out.println(sum);
    }
}
```

### for循环输出列表

```java
public class Demo {
    public static void main(String[] args) {
        int[] arr = { 1, 2, 3, 4 };
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
    }
}
```

### for循环打印99乘法表

```java
public class Demo {
    public static void main(String[] args) {
        for (int i = 1, j = 1; i <= 9; i++) {
            for (j = 1; j <= i; j++) {
                System.out.print(i + "*" + j + " = " + i * j + "  ");
            }
            System.out.println("\n");
        }
    }
}
```

### while循环最好是用在不确定循环次数的时,也可计算0-100的和

```java
public class Demo {
    public static void main(String[] args) {
        int sum = 0;
        int i = 1;
        while (true) {
            if (i <= 100) {
                sum += i;
                i++;
            } else {
                break;
            }
        }
        System.out.println(sum);
    }
}
```

练习：

**打印金字塔**

```
     *
    ***
   *****
  *******
 *********
***********
```

```java
public class Demo {
    public static void main(String[] args) {
        int line = 6;
        for (int i = 1; i <= line; i++) {
            for (int j = 0; j < line - i; j++) {
                System.out.print(" ");
            }
            for (int j = 0; j < i*2-1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

**打印镂空金字塔**

```
         *
        * *
       *   *
      *     *
     *       *
    *         *
   *           *
  *             *
 *               *
*******************
```

```java
public class Demo {
    public static void main(String[] args) {
        int line = 10;
        for (int i = 1; i <= line; i++) {
            for (int j = 0; j < line - i; j++) {
                System.out.print(" ");
            }
            for (int j = 0; j < i * 2 - 1; j++) {
                if (i > 1 && i < line) {
                    if (j == 0 || j == i * 2 - 2) {
                        System.out.print("*");
                    } else {
                        System.out.print(" ");
                    }
                } else {
                    System.out.print("*");
                }
            }
            System.out.println();
        }
    }
}
```

### break用法

这里记录一下break可以跳出for循环标签，其他语言类似，continue也可以跳到指定标签运行

```java
public class Demo {
    public static void main(String[] args) {
        outer: for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (j > 0)
                    break outer;
            }
            System.out.println("我不会输出，因为已经跳出最外层循环outer");
        }
    }
}
```

## 数组

### 基本操作

```java
import java.util.Arrays;

public class Demo {
    public static void main(String[] args) {
        // 定义一个数组
        double[] arr = { 1, 5, 1, 3.4, 2, 50 };
        // 或者 这种初始化是长度位6的空数组
        double[] arr1 = new double[6];
        // 牛逼初始化
        double[] arr2 = new double[] { 1, 5, 1, 3.4, 2, 50 };
        // 定义一个二维数组
        // 静态赋值
        double[][] arr3 = { { 1, 2.0 }, { 3.0 } };
        // 声明一个空数组
        double[][] arr4 = new double[6][1];
        // 动态赋值数组长度
        double[][] arr5 = new double[6][];
        // 打印某个值
        System.out.println(arr[0]);// 输出1.0
        // 赋值
        arr[0] = 3;
        // 打印数组
        System.out.println(Arrays.toString(arr)); // 输出[3.0, 5.0, 1.0, 3.4, 2.0, 50.0]
        // 遍历数组
        for (double item : arr) {
            System.out.print(item); // 输出3.05.01.03.42.050.0
        }
        System.out.println();
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]); // 输出3.05.01.03.42.050.0
        }
        System.out.println();
        // 数组长度
        System.out.println(arr.length);// 输出6

    }
}
```

### 常用方法

```java
import java.util.Arrays;

public class Demo {
    public static void main(String[] args) {
        // 定义一个数组
        double[] arr = { 1, 5, 1, 3.4, 2, 50 };
        // 复制拷贝一个数组
        double[] arr2 = arr.clone();
        System.out.println(Arrays.toString(arr2));// 输出[1.0, 5.0, 1.0, 3.4, 2.0, 50.0]
        // 数组反转
        double[] arrReverse = new double[arr.length];
        for (int i = 0; i < arr.length; i++) {
            arrReverse[i] = arr[arr.length - 1 - i];
        }
        System.out.println(Arrays.toString(arrReverse));// 输出[50.0, 2.0, 3.4, 1.0, 5.0, 1.0]
    }
}
```

**二维数组案例**

输出10行的杨辉三角

```java
import java.util.Arrays;

public class Demo {
    public static void main(String[] args) {
        // 定义一个数组
        int[][] arr = new int[10][];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = new int[i + 1];
            for (int j = 0; j < i + 1; j++) {
                if (j == 0 || j == i) {
                    arr[i][j] = 1;
                } else {
                    arr[i][j] = arr[i - 1][j - 1] + arr[i - 1][j];
                }
            }
        }
        for (int[] item : arr) {
            System.out.println(Arrays.toString(item));
        }
    }
}
```

```
[1]
[1, 1]
[1, 2, 1]
[1, 3, 3, 1]
[1, 4, 6, 4, 1]
[1, 5, 10, 10, 5, 1]
[1, 6, 15, 20, 15, 6, 1]
[1, 7, 21, 35, 35, 21, 7, 1]
[1, 8, 28, 56, 70, 56, 28, 8, 1]
[1, 9, 36, 84, 126, 126, 84, 36, 9, 1]
```

## 类

### 创建类

```java
public class Demo {

    public static void main(String[] args) {
        Person person1 = new Person("小白", 3);
        Person person2 = new Person("小花", 100);
        System.out.println(person1.getCat());// 输出 小白 3
        System.out.println(person2.getCat());// 输出 小花 100
    }
}

class Person {
    private String name = "";
    private int age = 0;

    // 构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getCat() {
        return this.name + " " + this.age;
    }
}
```

### 变量

全局变量（成员方法\属性）有默认值

局部变量指在代码块中定义的，使用前必须赋值

### 构造方法

构造方法和类名同名，他既然是方法，也具有方法重载

```java
public class Demo {

    public static void main(String[] args) {
        Person person = new Person();
        System.out.println(person.getCat());// 姓名 0
    }
}

class Person {
    private String name = "";
    private int age = 0;

    public Person() {
        // 调用本类里的构造方法
        this("姓名", 0);
    }

    // 构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getCat() {
        return this.name + " " + this.age;
    }
}
```

### 静态方法

```java
public class Demo {

    public static void main(String[] args) {
        Person.staticFn("直接调用");
    }
}

class Person {
    public static void staticFn(String content) {
        System.out.println(content);
    }
}
```

### 传参方式

分为引用传递和值传递

基本数据类型：int double等都是值传递（值的拷贝）  值传递不会改变原变量的值

引用数据类型：数组、对象等是引用传递（地址拷贝），在方法里改变值会影响原变量的值

另外字符串虽然是引用数据类型（但是他是不可变的对象final，String赋值的过程相当于new了一个String对象） 

tips:任何方法的调用会创建新的栈空间，如果在这个新的空间里创建同名对象，这个对象和实参是2个不同的对象

```java
import java.util.Arrays;

public class Demo {

    public static void main(String[] args) {
        int num = 1;
        int[] intArr = { 1, 2, 3, 4 };
        String str = "hello world!";
        changeNum(num);
        changeArr(intArr);
        changeStr(str);
        System.out.println(num);//1 基本数据类型是值传递 所以原变量不变
        System.out.println(Arrays.toString(intArr));// [-1, 2, 3, 4] 数组是引用传递，所以值发生了改变
        System.out.println(str);//hello world!String也是值传递 所以原变量不变
    }

    public static void changeNum(int num) {
        num = -1;
    }

    public static void changeArr(int[] intArr) {
        intArr[0] = -1;
    }

    public static void changeStr(String str) {
        str = "hello";//注意这里本质是new 了一个String对象 相当于在栈开辟了新空间，和main里的str是2个String对象了
    }
}
```

### 可变参数

可变参数 不确定参数的个数时使用 ，需要放在形参的最后

```java
import java.util.Arrays;

public class Demo {

    public static void main(String[] args) {
        Person.argsFn(1, 2, 3);// [1, 2, 3]
    }
}

class Person {

    /**
     * 
     * @param nums 可变参数 不确定参数的个数时使用 相当于int[] nums
     */
    public static void argsFn(int... nums) {
        System.out.println(Arrays.toString(nums));
    }
}
```

### 方法重载

方法的重载就是同一个类中方法名相同，形参不同（返回类型不构成重载的条件，只有返回类型不一样，不算重载）

```java
public class Demo {

    public static void main(String[] args) {
        Person.overloadFn();// 我没有形参，是overloadFn方法
        Person.overloadFn("我多了个形参，是overloadFn方法的重载");// 我多了个形参，是overloadFn方法的重载
    }
}

class Person {
    public static void overloadFn() {
        System.out.println("我没有形参，是overloadFn方法");
    }

    public static void overloadFn(String content) {
        System.out.println(content);
    }
}
```

### 返回值

返回包装类

```java
public class Demo {

    public static void main(String[] args) {
        double a = 0;
        double b = 1;
        Calc calc = new Calc();
        Double res = calc.div(a, b);
        System.out.println(res);
    }
}

class Calc {
    /**
     * 除法运算
     * @param a 除数
     * @param b 被除数
     * @return Double 是因为结果里可能包含null，所以要用包装类返回，因为类对象可以是null，不用double是因为是基本数据类型，不能为null
     */
    public Double div(double a, double b) {
        if (a == 0) {
            return null;
        } else {
            return a / b;
        }
    }
}
```

### 包

创建包

规范 com.公司名.项目名.模块名

新建文件夹com → 新建文件PackageDemo.java

```java
package com;

public class PackageDemo {
    
}
```

使用

```java
import com.PackageDemo;

public class Demo {
    public static void main(String[] args) {
      PackageDemo packageDemo = new PackageDemo();
    }
}
```

### 访问修饰符

| 访问修饰符         | 同类 | 同包 | 子类          | 不同包 |
| :----------------- | :--- | :--- | :------------ | :----- |
| public             | √    | √    | √             | √      |
| protected          | √    | √    | √             | ×      |
| 默认（什么也不加） | √    | √    | ×不同包 √同包 | ×      |
| private            | √    | ×    | ×             | ×      |

### 封装

属性私有化 设置or获取数据通过方法

```java
public class Demo {
    public static void main(String[] args) {
        new Person("小白", 18);
    }
}

class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        setName(name);
        setAge(age);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

### 继承和多态

继承 子类和父类有共同属性或方法时使用，java是单继承

多态 重写父类的方法

```java
public class Demo {
    public static void main(String[] args) {
        Person p = new Person("小白", 18);
        // 继承后可调用父类的方法
        p.setAddress("上海金融大厦");
        System.out.println(p.getAddress());// 输出上海金融大厦
        // 重写父类方法 多态
        System.out.println(p.getCompanyName());// 输出阿里巴巴公司
    }
}

/**
 * 公司类
 */
class Company {
    private String address = "浙江省杭州市";
    private String companyName = "阿里巴巴";

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getCompanyName() {
        return companyName;
    }

    public void setCompanyName(String companyName) {
        this.companyName = companyName;
    }

}

/**
 * Person 继承 公司类
 */
class Person extends Company {
    private String name;
    private int age;

    public Person() {
        super();// 虽然会默认调，最好写一下，因为假如父类没有无参构造器（定义了一个构造器）,那这里就会报错
    }

    public Person(String name, int age) {
        super();
        setName(name);
        setAge(age);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    // 重写父类的方法 称之为多态。。。
    @Override
    public String getCompanyName() {
        // super.访问父类属性或方法
        return super.getCompanyName() + "公司";
    }
}
```

## 算法

### 冒泡排序

相邻两个数比较，进行arr.length-1轮比较，每轮比较的次数都会少一次

```java
import java.util.Arrays;

public class Demo {
    public static void main(String[] args) {
        // 定义一个数组
        double[] arr = { 1, 5, 1, 3.4, 2, 50 };
        // 冒泡排序从小到大
        double temp;
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - 1 - i; j++) {
                // 如果前面的比后面的大 则交换
                if (arr[j] > arr[j + 1]) {
                    temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
                // 每一轮的结果是
                System.out.println(Arrays.toString(arr));
            }
        }
        System.out.println(Arrays.toString(arr));//输出[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
    }
}
```

第一轮

```
[1.0, 5.0, 1.0, 3.4, 2.0, 50.0]
[1.0, 1.0, 5.0, 3.4, 2.0, 50.0]
[1.0, 1.0, 3.4, 5.0, 2.0, 50.0]
[1.0, 1.0, 3.4, 2.0, 5.0, 50.0]
[1.0, 1.0, 3.4, 2.0, 5.0, 50.0]
```

第二轮

```
[1.0, 1.0, 3.4, 2.0, 5.0, 50.0]
[1.0, 1.0, 3.4, 2.0, 5.0, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
```

第三轮

```
[1.0, 1.0, 3.4, 2.0, 5.0, 50.0]
[1.0, 1.0, 3.4, 2.0, 5.0, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
```

第四轮

```
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
```

第五轮

```
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
```

### 选择排序

每次和最小值比较，选出最小的值放在第i个位置，共进行arr.length-2轮

```java
import java.util.Arrays;

public class Demo {
    public static void main(String[] args) {
        // 定义一个数组
        double[] arr = { 1, 5, 1, 3.4, 2, 50 };
        // 选择排序从小到大
        double min;
        for (int i = 0; i < arr.length - 1; i++) {
            min = arr[i];
            for (int j = i + 1; j < arr.length - 1; j++) {
                if (arr[j] < min) {
                    min = arr[j];
                    arr[j] = arr[i];
                    arr[i] = min;
                }
                System.out.println(Arrays.toString(arr));
            }
        }
        System.out.println(Arrays.toString(arr));// 输出[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
    }
}
```

第一轮 注意一共只有四轮

```
[1.0, 5.0, 1.0, 3.4, 2.0, 50.0]
[1.0, 5.0, 1.0, 3.4, 2.0, 50.0]
[1.0, 5.0, 1.0, 3.4, 2.0, 50.0]
[1.0, 5.0, 1.0, 3.4, 2.0, 50.0]
```

第二轮

```
[1.0, 1.0, 5.0, 3.4, 2.0, 50.0]
[1.0, 1.0, 5.0, 3.4, 2.0, 50.0]
[1.0, 1.0, 5.0, 3.4, 2.0, 50.0]
```

第三轮

```
[1.0, 1.0, 3.4, 5.0, 2.0, 50.0]
[1.0, 1.0, 2.0, 5.0, 3.4, 50.0]
```

第四轮

```
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
```

### 插入排序

每次取第i个数和他前面的有序列表对比，插入到相应为止，共进行arr.length - 1

```java
import java.util.Arrays;

public class Demo {
    public static void main(String[] args) {
        // 定义一个数组
        double[] arr = { 1, 5, 1, 3.4, 2, 50 };
        // 插入排序从小到大
        double min;
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < i + 1; j++) {
                min = arr[i + 1];
                if (min < arr[j]) {
                    arr[i + 1] = arr[j];
                    arr[j] = min;
                }
                System.out.println(Arrays.toString(arr));
            }
        }
        System.out.println(Arrays.toString(arr));// 输出[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
    }
}
```

第一轮

```
[1.0, 5.0, 1.0, 3.4, 2.0, 50.0]
```

第二轮

```
[1.0, 5.0, 1.0, 3.4, 2.0, 50.0]
[1.0, 1.0, 5.0, 3.4, 2.0, 50.0]
```

第三轮

```
[1.0, 1.0, 5.0, 3.4, 2.0, 50.0]
[1.0, 1.0, 5.0, 3.4, 2.0, 50.0]
[1.0, 1.0, 3.4, 5.0, 2.0, 50.0]
```

第四轮

```
[1.0, 1.0, 3.4, 5.0, 2.0, 50.0]
[1.0, 1.0, 3.4, 5.0, 2.0, 50.0]
[1.0, 1.0, 2.0, 5.0, 3.4, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
```

第五轮

```
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
[1.0, 1.0, 2.0, 3.4, 5.0, 50.0]
```

## 内置类

### Scanner获取用户输入

```java
import java.util.Scanner;

public class GetInput {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入a");
        int a = Integer.parseInt(scanner.next());
        System.out.println("请输入b");
        int b = Integer.parseInt(scanner.next());
        System.out.println("请输入c");
        int c = scanner.nextInt();
        System.out.println(a * b * c);
        scanner.close();
    }
}
```

### Math数学方法

**随机整数**

```java
public class Demo {
    public static void main(String[] args) {
        // 0~10随机整数
        System.out.println((int)Math.ceil(Math.random() * 10));
    }
}
```

### Random

随机整数

```java
import java.util.Random;

public class Demo {

    public static void main(String[] args) {
        Random random = new Random();
        // 输出0-10的随机整数
        System.out.println(random.nextInt(10));
    }
}
```

### String

**equals**

判断两个字符串是否相等 被判断的变量放在传参里 可以防止空指针

```java
public class Demo {
    public static void main(String[] args) {
        String str = "hello";
        if("hello".equals(str)){
            System.out.println("相等");
        }
    }
}
```
