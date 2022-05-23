# KotLin

## 基础

### 变量 常量

```kt
package com.org.kotlin
//常量
const val PI = 3.1415;

fun main() {
    //可变类型变量
    //可省略类型，有类型推断
    var age = 10//var age:Int = 10
    age = 3
    //不可变类型变量 相当于java中的final
    val name = "张三"
}
```

### 基本数据类型

kotlin基本数据类型有Byte、Short、Int、Long、Float、Double等，他们是一个类，在编译后会转成java基本数据类型byte,short,int,long,float,double

### 类型转换

```kt
package com.org.kotlin

fun main() {
    val i = "99".toInt()
    val a = 99.toString()
    val b = 99.toDouble()
    val c = 99.0.toInt()
    val d = 99.toFloat()
    val e = 99.0f.toInt()
    println("$i $a $b $c $d $e")//99 99 99.0 99 99.0 99
}
```

### 数组

```kt
package com.org.kotlin

fun main() {
    //初始化数组方法 其他基本类型类推
    val list = intArrayOf(1, 2, 3)

    //查
    println(list[0])//1
    //越界处理
    println(list.elementAtOrNull(999))//null
    //改
    list[0] = 2

    //对象类型 元素为字符串或者对象
    val list2 = arrayOf("1", "2")
    val list3 = arrayOf(A(), A())
}

class A() {
}
```

### 解构赋值

```kt
package com.org.kotlin

fun main() {
    //初始化数组方法
    val list = intArrayOf(1, 2, 3)
    //解构赋值
    val (a, b, c) = list
    println("$a $b $c");//1 2 3

    //解构赋值指定位置
    val (_, d, _) = list
    println("$d");//2
}
```

### 遍历

```kt
package com.org.kotlin

fun main() {
    //初始化数组方法
    val list = intArrayOf(1, 2, 3)
    //增强for循环
    list.forEach {
        println(it) //1 2 3
    }
    //for in
    for (item in list) {
        println(item) //1 2 3
    }
    //有下标的forEach
    list.forEachIndexed { index, item ->
        println("$index $item")
    }
}
```

### range表达式

```kt
package com.org.kotlin

fun main() {
    var age = 20

    //范围内 闭合
    if (age in 10..20) {
        println(">=10 && <= 20")
    }

    age = 9

    //范围外 开
    if (age !in 10..20) {
        println("<10 && > 20")
    }
}
```

### when表达式

就是switch

```kt
package com.org.kotlin

fun main() {
    val age = 20

    //when表达式是有返回值的,这里是main函数所以不能在when里面return，如果在其他函数是可以的
    val res = when (age) {
        1 -> println("1")
        2 -> println("2")
        10, 20 -> println(">=10,<=20") // 10,20
        in 21..30 -> print("21,30") //..可以用until代替  in 21 until  30 -> print("21,30")
        else -> {
            print("其他值")
        }
    }

    println(res)//kotlin.Unit
}
```

### Unit类代替void返回值

为空的话返回了Unit对象

```kt
package com.org.kotlin

fun main() {
    fun voidFn() {
    }
    
    println(voidFn())//kotlin.Unit
}
```

### 字符串模板

```kt
package com.org.kotlin

fun main() {
    val age = 20
    val name = "张三"

    println("age=${age + 1},name=$name")//age=21,name=张三
}
```

### 三目运算符

```kt
package com.org.kotlin

fun main() {
    val age = (Math.random() * 30).toInt()

    println(if (age > 18) {
        println("成年了")
    } else {
        println("未成年")
    })//kotlin.Unit if表示也会返回空...

    val str = if (age > 18) {
        "成年了"
    } else {
        "未成年"
    }
    println(str)
}
```

### 空判断

和flutter的dart语言类似,也可以用if判断不为空后调用

```kt
package com.org.kotlin

fun main() {
    //?如果为空则不处理?后面的
    val name1: String? = null
    println(name1?.length) //null

    //?如果为空，则执行?:后面的处理方式
    val name2: String? = null
    println(name2?.length ?: "name2为空") //name2为空

    //?如果不为空，则执行let后面的方法体
    val name3: String? = "张三"
    println(name3?.let {
        //it指像name3
        println(it.length)//2
        it //可以写返回值
    })//张三

    //!!如果为空则报错
    val name4: String? = null
    //这个就是断言assert 保证不为空的情况下这样写
    println(name4!!.length) //kotlin.KotlinNullPointerException
}
```

### 异常

kotlin不会像java一样去检查异常，你可以手动调用一些检查的方法去检查异常

```kt
package com.org.kotlin

fun main() {
    //?如果为空则不处理?后面的
    val name: String? = null

    //被动抛出异常
    try {
        println(name!!.length)
    } catch (e: Exception) {
        println(e)//kotlin.KotlinNullPointerException
    } finally {
        println("finally")
    }

    //主动抛出异常
    try {
        name?.length ?: throw RuntimeException("变量为空了")
    } catch (e: RuntimeException) {
        println(e.message)//变量为空了
    }

    try {
        //这一句主动检查异常，可以让下面的name不用加额外的判断符号
        checkNotNull(name)
        println(name.length)
    } catch (e: IllegalStateException) {
        println(e)//java.lang.IllegalStateException: Required value was null.
    }
}
```

## 函数

```kt
package com.org.kotlin

fun main() {
    println(fnName("张三"))//张三
    println(fnName2("李四"))//李四
    println(fnName3())//乖乖，默认参数
    println(fnName4(null))//null
    println(fnName(name = "张三"))//给指定的参数传值,叫具名参数传值
    println(fnVarArg("张三", "8岁", "男"))//8岁 男 张三
    fnVoid("张三");//无返回值
}

//不写private默认public
private fun fnName(name: String): String {
    return name
}

//上面的函数编译成java后是这样的
//private static final String fnName(String name) {
//    return name;
//}

//=号赋值 返回类型确定的话，可省略返回值类型
private fun fnName2(name: String) = name

//默认参数赋值
private fun fnName3(name: String = "乖乖，默认参数") = name

//如果要允许类型可以为空则需要加? 这里允许参数为空 返回值也为空
private fun fnName4(name: String?): String? {
    return null
}

//可变参数
fun fnVarArg(name: String, vararg value: String): String {
    value.forEach {
        println(it)
    }
    return name
}

//无返回值可省略:Unit（单元） 相当于java void
fun fnVoid(name: String): Unit {
    println("无返回值")
}
```

### 匿名函数Lambda

匿名函数就是Lambda

```kt
package com.org.kotlin

fun main() {
    //声明匿名函数函数 函数变量名：(参数类型)->返回值类型 = {接收参数1，接收参数2...->方法体 返回值}
    //tips:冒号声明必须写返回类型
    val fn: (String, Int) -> String = { name: String, age: Int ->
        println("$name $age")
        name//返回值不用写return
    }
    fn("张三", 18)//张三 18
    //只有一个参数 会默认赋值给it变量
    val fn2: (String) -> String = {
        println(it)
        it
    }
    fn2("李四")//李四
    //类型推断 转换第一个fn 省略=左边的类型声明
    val fn3 = { name: String, age: Int ->
        println("$name $age")
        name
    }
    fn3("王五", 18)//王五 18
    //类型推断 转换第一个fn 省略=右边的类型声明
    val fn4: (String, Int) -> String = { name, age ->
        println("$name $age")
        name//返回值不用写return
    }
    fn4("赵六", 18)//赵六 18
    //返回值为空的匿名函数  fn5:(String,Int)->Unit
    val fn5 = { name: String, age: Int ->
        println("$name $age")
    }
    println(fn5("孙七", 18))//孙七 18  kotlin.Unit 可以看到返回空
}
```

### 函数参数和内联关键字和函数引用

```kt
package com.org.kotlin

fun main() {
    fn("张三", interfaceFn = { hobby: String ->
        println(hobby)//回调传参
    })
    //这种就是上面这种方式的简写 (这里就是相当java于new了一个接口，并实现了接口的方法)
    fn("张三") { hobby: String ->
        println(hobby)//回调传参
    }
    //任何用lambda作为参数的可直接替换为函数 ::interfaceFn叫做函数引用
    fn("张三", ::interfaceFn)//回调传参
}

//一个外部函数去替换lambda
fun interfaceFn(hobby: String): Unit {
    println(hobby)
}

//如果函数参数里有类似与这种lambda函数参数的，（就是java里的接口多态，这里传个接口类型，下面调用接口方法，回传参数到接口对象里）,
//加上inline 则编译成java会把整个函数逻辑都放到main函数里去 这样相当于不用创建对象，提高效率
public inline fun fn(name: String, interfaceFn: (String) -> Unit) {
    interfaceFn("回调传参")
}
```

### 函数作为返回值

**匿名函数作为返回值**

```kt
package com.org.kotlin

fun main() {
    val lambdaFn = fn("调用函数")//调用函数
    //使用这个匿名函数
    println(lambdaFn("张三", 18))//张三 18
}

//这里的返回值是lambda函数
fun fn(value: String): (String, Int) -> String {
    println(value)
    return { name: String, age: Int ->
        "$name $age"
    }
}
```

上面这一段就相当于下面这段在函数里调函数而已

```kt
package com.org.kotlin

fun main() {
    println(fn("调用函数","张三", 18))
}

fun fn2(name: String, age: Int): String {
    return "$name $age"
}

//这里的返回值是lambda函数
fun fn(value: String, name: String, age: Int): String {
    println(value)
    return fn2(name, age)
}
```

## 特殊类类型

### Nothing

无法正常返回的方法就返回 Nothing

```kotlin
package com.org.kotlin

fun main() {
    println(A().fn()) //kotlin.NotImplementedError: An operation is not implemented: Not yet implemented
    fn()
}

interface MyInterface {
    fun fn()
}

class A : MyInterface {
    override fun fn() {
        TODO("Not yet implemented") //因为要抛异常，注定无法正常返回，所以返回 Nothing 类型
    }
}

//或者是执行的时候程序会一直陷入循环中无法正常返回,所以返回 Nothing 类型
fun fn(): Nothing {
    while (true) {
        println("死循环")
    }
}
```

### 任意类型Any

超类 相当于 java中的Object

```kt
package com.org.kotlin

fun main() {
    //定义任意类型变量
    var a: Any = 3;
    a = "三";

    //定义任意类型返回值
    val fn: (String, Int) -> Any = { name: String, age: Int ->
        if (age > 18) {
            println("成年")
        } else {
            false
        }
    }

    println(fn("张三", 18))//false
}
```

## 内置api

### 字符串操作

**正则替换**

```kt
package com.org.kotlin

fun main() {
    var name = "张三李四王五"
    //正则替换 后面{}是lambda传值，可以迭代匹配到的每个元素 进行替换
    name = name.replace(Regex("[三四五]")) {
        if (it.value == "三") {
            "四"
        } else if (it.value == "四") {
            "三"
        } else {
            it.value
        }
    }
    println(name)//张四李三王五
}
```

### Filter过滤

**过滤删除**

```kt
package com.org.kotlin

fun main() {
    //创建集合
    val list = mutableListOf<String>("1", "2", "3")
    list.removeIf {
        it.contains("1")
    }
    println(list)//[2, 3]
}
```

**查找单个值**

```kt
package com.org.kotlin

fun main() {
    //创建集合
    val list = mutableListOf<String>("1", "2", "3")
    val value = list.find {
        it.contains("2")
    }
    println(value)//2
}
```

**查找返回集合**

```kt
package com.org.kotlin

fun main() {
    //创建集合
    val list = mutableListOf<String>("1", "2", "2", "3")
    val list2 = list.filter {
        it.contains("2")
    }
    println(list2)//[2, 2]
}
```

## 作用域函数

### let

见空判断let使用方法，一般用于简化 判断不为空后执行代码这个操作

### apply

apply⼀般⽤于⼀个对象实例初始化的时候，需要对对象中的属性进⾏赋值

```kt
package com.org.kotlin

fun main() {
    var person = Person(null, null)

    //apply里的this就是变量本身
    person = person.apply {
        println(person === this) //true
    }
    //返回值也是他本身
    println(person)//com.org.kotlin.Person@330bedb4 还是Person对象

    //apply可以链式调用
    person.apply {
        name = "张三"
    }.apply {
        age = 18
    }
    println("${person.name} ${person.age}")//张三 18
}

//属性和构造方法合并的写法
class Person(var name: String?, var age: Int?) {
}
```

### run

```kt
package com.org.kotlin

fun main() {
    //类似于java代码块 可以让变量名重复定义
    run {
        var name = "张三"
    }
    run {
        var name = "李四"
    }

    //最后一行作为一个函数值返回,可以避免专门定义一个函数
    var name = run {
        val name = "王五"
        name
    }
}
```

### also

also和apply一样返回他本身，类似于java需要把值赋给一个值后判断的这种用also

```kt
package com.org.kotlin

import java.io.File

fun main() {
    //项目根目录存放1.txt
    val file = File("./1.txt")
    val reader = file.bufferedReader()
    var line: String?
    //also和apply一样返回他本身 类似于这种处理用also
    while (reader.readLine().also {
                //在这里把读到的行赋值给line
                line = it
            } != null) {
        println(line)
    }
}
```

### with

省略对象变量名调用方法

```kt
package com.org.kotlin

fun main() {
    var person = Person()
    //一般赋值
    person.name = "张三"
    person.age = 18

    //使用with赋值
    with(person) {
        name = "李四"
        age = 20
    }
    println(person)//Person(name='李四', age=20)
}

class Person() {
    var name: String = "";
    var age: Int = 0;
    override fun toString(): String {
        return "Person(name='$name', age=$age)"
    }
}
```

## 集合

### List

**不可变集合listOf**

```kt
package com.org.kotlin

fun main() {
    //创建集合
    var list = listOf<String>("1", "2", "3")
    //查
    println(list[0])//1
    //防止越界
    println(list.getOrNull(100))//null
    //增
    list = list.plus("4");
    //删除值等于
    list = list.minus("2")
    //删除值等于
    list = list.dropWhile {
        it == "1"
    }
    //输出
    println(list)//[3, 4]

    //增
    list = list + "5"
    //删
    list = list - "3"
    println(list)//[4, 5]
}
```

**可变集合mutableListOf**

```kt
package com.org.kotlin

fun main() {
    //创建集合
    val list = mutableListOf<String>("1", "2", "3")
    //查
    println(list[0])//1
    //防止越界
    println(list.getOrNull(100))//null
    //增
    list.add("4");
    //删除值等于
    list.remove("2")
    //删除值等于
    list.removeAt(0)
    //输出
    println(list)//[3, 4]

    list += "5"
    list -= "3"
    println(list)//[4, 5]
}
```

**可变集合arrayListOf**

```kt
package com.org.kotlin

fun main() {
    //创建集合
    val list = arrayListOf<String>("1", "2", "3")
}
```

**List去重**

```kt
package com.org.kotlin

fun main() {
    //创建集合
    val list = mutableListOf<String>("1", "1", "2", "3")
    //List转Set集合去重
    println(list.toSet())//[1, 2, 3]
    //List转Set集合去重后转回List
    println(list.toSet().toList())//[1, 2, 3]
    //快捷函数去重
    print(list.distinct()) //[1, 2, 3] 底层toMutableSet.toList()
}
```

### Set集合

元素不可重复的集合

```kt
package com.org.kotlin

fun main() {
    //初始化Set集合
    val set = setOf<Int>(1, 1, 2, 3)
    println(set)//[1, 2, 3]
    //查
    println(set.elementAt(0))//1
    //越界处理
    println(set.elementAtOrNull(999))//null

    //可变set
    val set2 = mutableSetOf<Int>(1, 1, 2, 3)
    //增
    set2.add(4)
    //删除
    set2.remove(1)
    //操作符
    set2 += 4
    set2 -= 1
    println(set2)//[2, 3, 4]
}
```

### Map

**可变Map mutableMapOf**

```kt
package com.org.kotlin

fun main() {
    //初始化不可变Map
    val map = mapOf<String, Int>()

    //可变map xx to xx =》键值对
    val map2 = mutableMapOf<String, Any>("hobby" to "lol", "address" to "SH")
    //增
    map2["name"] = "张三";
    map2["age"] = 15;
    //操作符添加
    map2 += "name" to "李四"

    //删
    map2.remove("name")
    //改
    map2.replace("age", 18)
    //查
    println(map2["age"])//18
    //不存在默认值处理
    println(map2["name"])//null
    println(map2.getOrDefault("name", "张三"))//张三
    println(map2)//{hobby=lol, address=SH, age=18}
}
```

**可变hashMap**

```kt
package com.org.kotlin

fun main() {
    //可变hashMap
    val map = hashMapOf<String, Any>()
}
```

**map遍历**

```kt
package com.org.kotlin

fun main() {
    val map = mutableMapOf<String, Any>("hobby" to "lol", "address" to "SH")
    //遍历
    map.forEach {
        println(it.key)
        println(it.value)
    }

    map.forEach { (index: String, item: Any) ->
        println(index)
        println(item)
    }

    for (mutableEntry in map) {
        println(mutableEntry.key)
        println(mutableEntry.value)
    }
}
```

## 面向对象

### 类

**声明类**

```kt
package com.org.kotlin

fun main() {
    val a = A("张三", 18)
    println(a.age)//19
    B("张三", 10, "3班").fn()//主构造和初始化代码块被调用 初始化代码块被调用 次构造方法被调用 成员方法
}

class A {
    //默认声明为public属性编译器会产生getter和setter，并将属性私有化
    var name = ""

    //get set的底层逻辑
    var age = 0
        get() = field + 1
        set(value) {
            field = value
        }

    //私有的不会产生getter和setter
    private var sex = "男"

    constructor(name: String, age: Int) {
        this.name = name
        this.age = age
    }
}

//上面的公有属性和构造器可合并为这个
class B(var name: String, var age: Int) {
    private var sex = "男"

    //初始化代码块 类似于java 代码块{}
    init {
        println("主构造和初始化代码块被调用")
    }

    //如果还有其他次构造函数  this(name, age)必须加上 相当于调用主构造函数B(var name: String, var age: Int)
    constructor(name: String, age: Int, classNo: String) : this(name, age) {
        println("次构造方法被调用")
    }

    fun fn() {
        println("成员方法")
    }
}
```

**成员变量延迟初始化手动赋值lateinit**

```kt
package com.org.kotlin

fun main() {
    val a = A("张三", 18)
    //println(a.classNo)//未初始化先调用会报错 lateinit property classNo has not been initialized
    a.classNo = "2班"
    println(a.classNo)
}

class A(var name: String, var age: Int) {
    //先声明，不初始化
    lateinit var classNo: String
}
```

**成员变量延迟初始化，自动赋值by lazy**

```kt
package com.org.kotlin

fun main() {
    //实例化对象，没有加载
    val a = A("张三", 18)//没加载
    //用的时候才加载
    println(a.classNo)//加载了 2班
    //值加载一次
    println(a.classNo)//2班
}

class A(var name: String, var age: Int) {
    //用的时候才加载
    val classNo: String by lazy {
        println("加载了")
        "2班"
    }
}
```

### 继承和重写

```kt
package com.org.kotlin

fun main() {
    //实例化对象，和java一样可以父类引用指向子类对象
    val b: A = B("张三", 18)
    b.fn()//子类重写父类方法
}

//默认不加修饰符是public final
//open是剔除final修饰符
open class A(var name: String, var age: Int) {
    //默认不加修饰符是public final
    //加了open剔除final后可重写
    open fun fn() {
        println("父类成员方法")
    }
}

//B继承A
open class B(name: String, age: Int) : A(name, age) {
    //重写
    override fun fn() {
        println("子类重写父类方法")
    }
}
```

### 向上转型和向下转型

```kt
package com.org.kotlin

fun main() {
    //向上转型 - 父类引用指向子类对象,且会自动遗失父类没有的方法
    val a: A = B("张三", 18)
    a.fn()//子类重写父类方法
    a.parentFn()//父类方法
    //a.sonFn();//报错，自动遗失父类没有的方法
    //向下转型
    val b: Any = B("张三", 18)
    //判断类型后执行 is 就是java里的instanceof
    if (b is B) {
        b.fn()//子类重写父类方法
        b.sonFn();//子类方法
    }
    //或者强转后执行
    (b as B).fn()
    //强转一次后，后面的可以省略。。。
    b.sonFn()
}

open class A(var name: String, var age: Int) {
    open fun fn() {
        println("父类成员方法")
    }

    fun parentFn() {
        println("父类方法")
    }
}

//B继承A
open class B(name: String, age: Int) : A(name, age) {
    override fun fn() {
        println("子类重写父类方法")
    }

    fun sonFn() {
        println("子类方法")
    }
}
```

### object

可用于单例和匿名内部类

**用于单例**

```kt
package com.org.kotlin

fun main() {
    //相当于调用了A.INSTANCE.hashCode 编译后底层饿汉式单例
    println(A.hashCode())//856419764
    println(A.hashCode())//856419764
    print(A.name)
}

//object代替class 那他就是一个单例
object A {
    var name: String = "张三"
    var age: Int = 18
}
```

**匿名内部类**

```kt
package com.org.kotlin

fun main() {
    //匿名内部类 类型可省略
    val a: A = object : A {
        override fun fn() {
            println("实现了A接口fn")
        }
    }
    //匿名内部类形参，实现多态
    B().fn(a)//实现了A接口fn

    //如果是java的接口才能这么简写!!
    val b: Runnable = Runnable {
        println("实现了Runnable run方法")
    }
}

interface A {
    abstract fun fn()
}

class B() {
    fun fn(a: A) {
        a.fn()
    }
}
```