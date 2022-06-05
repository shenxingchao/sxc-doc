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
    //for in
    for (item in list) {
        println(item) //1 2 3
    }
    //增强for循环
    list.forEach {
        println(it) //1 2 3
    }
    //有下标的forEach
    list.forEachIndexed { index, item ->
        println("$index $item")
    }
    //使用索引遍历 indices返回0..2
    for (index in list.indices) {
        println(list[index])
    }
    //迭代器
    val iterator = list.iterator()
    while (iterator.hasNext()) {
        println(iterator.next())
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

kotlin1.6开始需要强制穷举when所有情况

```kt
package com.org.kotlin

fun main() {
    val age = 20

    //when表达式是有返回值的,这里是main函数所以不能在when里面return，如果在其他函数是可以的
    val res = when (age) {
        1 -> println("1")
        2 -> println("2")
        10, 20 -> println(">=10,<=20") // 10,20
        in 21..30 -> print("21,30") //..可以用until代替  in 21 until  30 -> print("21,30"),类似的还有downTo 
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
    //if-not-null 缩写
    println(name1?.length) //null

    //?如果为空，则执行?:后面的处理方式
    //if-not-null-else 缩写
    val name2: String? = null
    println(name2?.length ?: "name2为空") //name2为空

    //?如果不为空，则执行let后面的方法体
    //if not null 执行代码
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

### kotlin注解

**@file:JvmName**

```kt
//这行加到开头，可以改变kt文件编译后的类名
@file:JvmName("User")
```

**@JvmStatic

```kt
package com.org.kotlin

fun main() {
}

class A {
    companion object {
        //编译成java后是私有的，如果java代码想要调用，则需要加一个注解
        @JvmField
        val name = "张三"

        //编译成java后是私有的
        @JvmStatic
        fun fn() {
            println("方法")
        }
    }
}
```

### is操作符

判断对象，并在该判断后的方法体内自动推断类型

```kt
fun main() {
    val str: Any = "哈哈"
    if (str is String) {//不仅是这个if里面，在整个main函数都已经明确类型
        println(str.length)
    }
}
```

### 将代码标记为未完成

添加此方法，执行到此会报异常

```kt
TODO()
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

### .()语法糖

```kt
package com.org.kotlin

fun main() {
    val fn1: String.(Int) -> String = {
        println(this)
        println(it)
        this.repeat(it)
    }
    val fn2: (String, Int) -> String = { str, num ->
        println(str)
        println(num)
        str.repeat(num)
    }
}
```

### ::语法糖

函数引用或者变量引用

```kt
package com.org.kotlin

fun main() {
    val a = A("张三", 18)
    //a::fn是函数引用
    println(a::name)
    fn(a, a::fn)
}

class A(var name: String, var age: Int) {
    fun fn(): String {
        return "成员方法"
    }
}

/**
 * methodFn这里是函数传参
 */
fun fn(a: A, methodFn: () -> String) {
    println(a.fn())//普通调用
    println(methodFn())//通过函数引用调用
}
```

### 函数参数和inline内联关键字和函数引用

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

### 扩展函数

实现一个let来举例说明扩展函数，任何类型都是可以加扩展函数的

**通俗点就是可以用.的方法调用**

**扩展函数支持链式调用,就可以实现db.select{xxx}.field{xxx}类似的功能了**

```kt
package com.org.kotlin

fun main() {
    val s = "张三".let {
        println(it)
        "我是返回值1"
    }
    println(s)

    val s1 = "张三".myLet {
        println(it)
        "我是返回值2"
    }
    println(s1)

    val bool = "张三".myLet {
        if (it == "张三")
            "是的"
        else
            "不是"
    }.myLet {
        it == "是的"
    }

    println(bool)
}


//1.inline提高效率
//2.输入类型I 任意类型 如输入"张三".
//3.myLet扩展函数名
//4.传参lambda转化为代码块{}
//5.输出this就是输入参数I本身
//6.输出类型O根据lambda返回值类型确定类型 return lambda(this) 就是lambda函数转换成代码块{}的返回值，代码块的最后一行，返回任意类型
inline fun <I, O> I.myLet(lambda: (I) -> O): O {
    println(this is I)//true
    return lambda(this)
}
```

### 中缀表达式

函数可以省略· 并且可以链式调用

```kt
package com.org.kotlin

fun main() {
    //内置的中缀表达式函数 将1和10组合成二元元组
    println(1 to 10)//(1, 10)
    //将表达式
    println("张三".fn("李四"))//张三和李四
    //简化成
    println("张三" fn "李四")//张三和李四
    println("张三" fn 18 fn "李四" fn 20 fn "王五" fn 3)//张三和18和李四和20和王五和3
}

//infix 中缀表达式 可以让函数省略.调用,I,O 可以输入任何类型
infix fun <I, O> I.fn(param: O): String {
    return this.toString() + "和" + param.toString()
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

**检测元素是否在可迭代对象中**

```kt
package com.org.kotlin

fun main() {
    val list = listOf<String>("张三", "李四", "王五")
    println("张三" in list)
}
```

### zip

合并可迭代对象的每个位置的元素为一个二元元组

```kt
package com.org.kotlin

fun main() {
    val list = listOf<Int>(1, 2, 3)
    val list1 = listOf<String>("张三", "李四", "王五")
    val list2 = listOf<String>("10", "20", "30")


    val zip: List<Pair<Pair<Int, String>, String>> = list.zip(list1).zip(list2)
    zip.forEachIndexed { index, item ->
        println("$item $index")
    }
    /*
    ((1, 张三), 10) 0
    ((2, 李四), 20) 1
    ((3, 王五), 30) 2
     */
}
```

## 作用域函数

### let

一般用于简化 判断不为空后执行代码这个操作

```kt
package com.org.kotlin

fun main() {
    //?如果不为空，则执行let后面的方法体
    val name3: String? = "张三"
    println(name3?.let {
        //it指像name3
        println(it.length)//2
        it //可以写返回值
    })//张三
}
```

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

## 高阶函数

### map

map可以讲一个可迭代对象的每个元素处理后返回，相当于forEach遍历并处理后返回

```kt
package com.org.kotlin

fun main() {
    val list = listOf<String>("张三", "李四", "王五")
    //map最后一行是返回值
    //map可以讲一个可迭代对象的每个元素处理后返回 相当于forEach遍历并处理后返回
    val map = list.map {
        it + "是学生"
    }.map {
        "$it，在学生上课"
    }
    println(map)//[张三是学生，在学生上课, 李四是学生，在学生上课, 王五是学生，在学生上课]
}
```

### any

查找元素在可迭代对象中是否大于等于1,返回布尔值

```kt
package com.org.kotlin

fun main() {
    val list = listOf("张三", "李四", "王五")
    //查找元素在可迭代对象中是否大于等于1,返回布尔值
    val bool = list.any {
        it == "张三"
    }
    println(bool)//true
}
```

### all

```kt
package com.org.kotlin

fun main() {
    val list = listOf("张三", "李四", "王五")
    //查找所有元素在可迭代对象中是否满足条件，返回布尔值
    val bool = list.all {
        it.length == 2
    }
    println(bool)//true
}
```

### any

查找元素在可迭代对象中是否不存在,不存在返回true

```kt
package com.org.kotlin

fun main() {
    val list = listOf("张三", "李四", "王五")
    //查找元素在可迭代对象中是否不存在
    val bool = list.none{
        it == "赵四"
    }
    println(bool)//true
}
```

### count

统计元素在可迭代对象的次数

```kt
package com.org.kotlin

fun main() {
    val list = listOf("张三", "李四", "王五")
    //统计元素在可迭代对象的次数
    val count = list.count {
        it == "张三"
    }
    println(count)//1
}
```

### groupBy

分组统计 只适用于数组 分为聚合和不聚合

```kt
package com.org.kotlin

fun main() {
    val mutableList = mutableListOf<Person>()
    mutableList.add(Person("张三", 18))
    mutableList.add(Person("李四", 18))
    mutableList.add(Person("王五", 19))

    //按年龄分组 如果重复 it用最后一个重复的
    val associateByList = mutableList.associateBy {
        it.age
    }//{18=Person(name=李四, age=18), 19=Person(name=王五, age=19)}
    println(associateByList)
    //按年龄分组 组使用list存储
    val groupList = mutableList.groupBy {
        it.age
    }//{18=[Person(name=张三, age=18), Person(name=李四, age=18)], 19=[Person(name=王五, age=19)]}
    println(groupList)
    //按年龄分组后并统计
    val groupList2 = mutableList.groupBy {
        it.age
    }.map {
        it.key to it.value.size
    }//[(18, 2), (19, 1)]
    println(groupList2)

}

data class Person(var name: String, var age: Int)
```

### partition

分组并拆分成2个集合

```kt
package com.org.kotlin

fun main() {
    val mutableList = mutableListOf<Person>()
    mutableList.add(Person("张三", 18))
    mutableList.add(Person("李四", 18))
    mutableList.add(Person("王五", 19))

    //按条件分组后结构成2个可迭代对象
    val (prevList, nextList) = mutableList.partition {
        it.age == 19
    }
    println(prevList)//[Person(name=王五, age=19)]
    println(nextList)//[Person(name=张三, age=18), Person(name=李四, age=18)]
}

data class Person(var name: String, var age: Int)
```

### sortedBy

排序

```kt
package com.org.kotlin

fun main() {
    val mutableList = mutableListOf<Person>()
    mutableList.add(Person("张三", 18))
    mutableList.add(Person("李四", 18))
    mutableList.add(Person("王五", 19))
    //按年龄倒序排序  也可以直接用sortedByDescending倒序排序
    val sortedList = mutableList.sortedBy {
        //-号表示倒序
        -it.age
    }
    println(sortedList)//[Person(name=王五, age=19), Person(name=张三, age=18), Person(name=李四, age=18)]
}

data class Person(var name: String, var age: Int)
```

### getOrElse

检验下标为index的元素存不存在，存在则返回本身，不存在则执行后面的代码

```kt
package com.org.kotlin

fun main() {
    val mutableList = mutableListOf<Person>()
    mutableList.add(Person("张三", 18))
    mutableList.add(Person("李四", 18))
    mutableList.add(Person("王五", 19))

    //检验下标为index的元素存不存在，存在则返回本身，不存在则执行后面的代码
    val isExit = mutableList.getOrElse(4) {
        println("不存在")
        //这里直接返回false
        false
    }
    println(isExit)//false
}

data class Person(var name: String, var age: Int)
```

### flatMap

将二维数组中的每个集合拆成一维数组

```kt
package com.org.kotlin

fun main() {
    val list1 = listOf("张三", "李四")
    val list2 = listOf("王五", "赵六")
    val listQR = listOf(list1, list2)
    println(listQR)//[[张三, 李四], [王五, 赵六]]
    //两种用法是一样的
    val list = listQR.flatMap { it }
    //val list = listQR.flatten()
    println(list)//[张三, 李四, 王五, 赵六]
}
```



### takeIf

用于判断对象某个属性是否为空,或者用于某个变量检查后，再对对象进行处理

如果代码块predicate里面返回为true，则返回这个对象本身，否则返回空

```kt
package com.org.kotlin

fun main() {
    val person: Person = Person("", 18)

    //takeIf判断
    val person2 = person.takeIf {
        //it指代本身
        it.name != "" //这里如果名字为空就是false返回null
    }
    println(person2) //null

    //用于某个变量检查
    val bool = true;
    var person3 = person.takeIf { bool }?.apply {
        person.name = "张三"
    }
    println(person3?.name)//张三

    //上面的代码相当于
    if (bool) {
        person3 = person.apply {
            person.name = "张三"
        }
    } else {
        person3 = null
    }
}

class Person(var name: String, var age: Int)
```

### RxJava

接收任意的值，并对其处理，可实现监听一个观察者监听者

RxJava就是观察者，他的扩展函数就是监听者

```kt
package com.org.kotlin

fun main() {
    fn {
        //最后一行是返回值
        "张三"
        //第一个fn返回的是Rxjava(action()) 这个对象
    }.fn1 {
        //然后这里使用Rxjava(action())对象扩展方法fn1
        println(this)
    }.fn2 {
        //第二种用it  lambda: (I) -> O 这里的I
        println(it)
    }
}

//RxJava操作符中转站类
class Rxjava<I>(var value: I) {
    init {
        println(value) //这里传进来张三
    }
}

//然后对这个类进行扩展操作即可 传入一个Rxjava<I>对象
//I.()的意思是 (I) 就是一种lambda的写法
inline fun <I, O> Rxjava<I>.fn1(lambda: I.() -> O) = Rxjava(lambda(value))

inline fun <I, O> Rxjava<I>.fn2(lambda: (I) -> O) = Rxjava(lambda(value))

//这样的表达式可以用来监视变量 lambda()是创建一个lambda会返回{}里的最后一行
inline fun <O> fn(lambda: () -> O) = Rxjava(lambda())
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

## 元组

kotlin没有多个数据的元组

### Pair

二个数据泛型元组

```kt
package com.org.kotlin

fun main() {
    val pair = Pair(1, 2);
    val pair1 = Pair("name", "张三");
}
```

### Triple

三个数据泛型元组

```kt
package com.org.kotlin

fun main() {
    val pair = Triple(1, 2, "third");
    val pair1 = Triple("name", "张三", 18);
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

### 成员变量延迟初始化手动赋值lateinit

只能用于引用类型String等，不能用于Int等基础类型，且不能为null

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

### 成员变量延迟初始化自动赋值bylazy

和懒汉很像

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

### object单例和匿名内部类

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

### 伴生对象

companion object

字面意思 伴随对象产生而产生，直接用类名调用，就是静态static的意思

```kt
package com.org.kotlin

fun main() {
    //可以直接类名调用了
    println(A.name)//张三
    A.fn()//静态方法
}

class A {
    //伴生对象 在这个代码块里面声明的为java中的静态变量
    companion object {
        var name = "张三"
        var age = 18

        fun fn() {
            println("静态方法")
        }
    }
}
```

### 内部类

```kt
package com.org.kotlin

fun main() {
    A().getInnerFn()

    //非静态内部类
    A().Inner().fnInner()
    //静态内部类
    A.Inner2().fnInner()
}

class A {
    var name = "张三"
    var age = 18

    //这里的相当于静态属性
    companion object {
        var classNo = "2班"
    }

    //内部类加inner去除编译后的static修饰符,否则只能访问静态属性
    inner class Inner {
        fun fnInner() {
            println("内部类方法访问外部非静态属性$name $age")
        }
    }

    //默认是静态内部类 编译后会加上static关键字
    class Inner2 {
        fun fnInner() {
            println("内部类方法访问外部静态属性$classNo")
        }
    }

    fun getInnerFn() {
        Inner().fnInner()
        Inner2().fnInner()
    }
}
```

### 数据类

加上data关键字 就是orm类(数据库对象关系映射)

```kt
package com.org.kotlin

fun main() {
    //看下数据类和普通类的对比
    //1.数据类型重写了toString()方法
    println(UserNormal(1, "张三", 18).toString())//com.org.kotlin.UserNormal@610455d6
    println(User(1, "张三", 18).toString())//User(userId=1, name=张三, age=18)
    //2.数据类支持结构赋值
    //普通类解构需要加component方法 不然报错
    val (userId1, name1, age1) = UserNormal(1, "张三", 18);
    println("$userId1 $name1 $age1")
    //数据类自动添加了解构返回方法
    val (userId2, name2, age2) = User(1, "张三", 18);
    println("$userId2 $name2 $age2")
    //3.支持对象copy 底层是new 一个主构造方法...
    val user = User(1, "张三", 18).copy(userId = 10, name = "王五", age = 1)
    println(user)//User(userId=10, name=王五, age=1)
}

class UserNormal(var userId: Int, var name: String, var age: Int) {

    //这几个方法，只是为了演示普通类不能解构
    operator fun component1(): Any {
        return userId
    }

    operator fun component2(): Any {
        return name
    }

    operator fun component3(): Any {
        return age
    }
}

//java bean就是orm类（数据库表关系对象）这么声明
data class User(var userId: Int, var name: String, var age: Int)

//多表
data class UserAddress(var userId: Int, var name: String, var age: Int, var address: String)
```

### operator重载运算符

既可以重载系统内置的运算符 > = < 等等，还可以用来对象的解构 见**数据类**部分

```kt
package com.org.kotlin

fun main() {
    //普通调用
    println(A(3).plus(A(4)))

    //将+号对应的运算符plus给重载了 就可以实现对象相加  其他的运算符也要一样
    println(A(3) + A(4))
}

class A(var num: Int) {
    //利用重载运算符 重载系统内置的加号 + 
    operator fun plus(a: A): Int {
        return this.num + a.num
    }
}
```

### 枚举类

enum class

比较java多了一个class关键字,强调他是一个类

```kt
package com.org.kotlin

fun main() {
    println(A.RED);// RED
    println(A.GREEN);// GREEN
    println(B.RED);// RED
    //枚举对象名称
    println(A.RED.name)//RED
    //枚举对象对应的编号
    println(A.RED.ordinal)//0
    //打印所有枚举
    println(B.values().contentToString())//[RED, GREEN, BLUE]
    // 将字符串转成已有枚举常量对象
    println(B.valueOf("RED"));// RED
}

//属性私有化，防止外部更改
enum class A(private var color: String) {
    RED("红色"),
    GREEN("绿色"),
    BLUE("蓝色")
}

// 最简单的枚举类
enum class B {
    RED,
    GREEN,
    BLUE
}
```

### 密封类

枚举类的扩展，可代替枚举，主要用于when判断多个对象，不需要写else的情况

主要因为他的子类可以创建多个，而枚举类是不行的（枚举类也不需要写else）

```kt
package com.org.kotlin

fun main() {
    //密封类不能被初始化,自己是抽象类
    //Color();Sealed types cannot be instantiated

    //一般用法
    val a1 = if (Math.random() > 0.5) B() else C()
    var res1 = when (a1) {
        is B -> "B"
        is C -> "C"
        //强行写else
        else -> "没有对象需要判断了"
    }
    //使用密封类后不需要写else了
    val a2 = if (Math.random() > 0.5) Color2.Red("宝马") else Color2.Blue("奔驰")
    var res2 = when (a2) {
        is Color2.Blue -> a2.name
        is Color2.Red -> a2.name
        //不需要写else了
    }
}

//用于when举例
interface A {}
class B : A {}
class C : A {}

interface Car {
}

//密封类可以继承。而枚举类不行
sealed class Color : Car {
    //object 让它变成单例 可以重用 这就和枚举一样了 一般不这么用 : Color()是继承父类的意思
    object Red : Color()
    object Blue : Color()
}

sealed class Color2 : Car {
    //一般声明为class 可以创建多个子类实例
    data class Red(var name: String) : Color2()
    data class Blue(var name: String) : Color2()
}
```

### 接口

接口可以多实现，类是单继承

接口中定义的未赋值变量和方法都要在实现类中实现

```kt
package com.org.kotlin

fun main() {
    C("张三", 18).fnA()
    C("李四", 19).fnA()
    println(C("张三", 18).classNo)
}

interface A {
    var name: String
    val classNo: String
        get() = "默认值只能这样赋值"

    fun fnA()
}

interface B {
    var age: Int
    fun fnB()
}

open class D

//接口可以多实现，类是单继承
class C(override var name: String, override var age: Int) : A, B, D() {
    override fun fnA() {
        println("实现A接口方法")
    }

    override fun fnB() {
        println("实现B接口方法")
    }
}
```

### 抽象类

```kt
package com.org.kotlin

fun main() {
}

abstract class A {
    abstract fun fn()
    fun fnA() {
        println("抽象类A的方法")
    }
}

class B : A() {
    override fun fn() {
        println("实现抽象类A的方法")
    }
}
```

### 泛型

```kt
package com.org.kotlin

fun main() {
    val person = Person<Int>(null)
    //表明泛型是Int
    person.age = 18;
    //下面的是String就报错了
    //person.age = "20"
    // 需要传泛型里的类型<Int>
}

//泛型类
class Person<T>(var age: T?) {
    // 泛型方法返回值泛型+形参泛型
    fun <E> getAge(age: E): E {
        return age
    }
}
```

**泛型继承**

```kt
package com.org.kotlin

fun main() {
    //只能传A类型或A类型的子类型
    val person = Person(A())
    person.a = B();
    person.a = null;//对象传null也没报错..

    //person.a = 18;//报错
}

open class A
open class B : A()

class Person<T : A>(var a: T?)
```

**泛型优化reified**

使用reified关键字修饰泛型T，可拿到T对象的反射类

```kt
package com.org.kotlin

fun main() {
    fn("张三")
}

//使用reified关键字修饰泛型T，可拿到T对象的反射类 Type.class，不然拿不到
inline fun <reified T> fn(obj: T) =
    when (T::class) {
        String::class -> {
            println("String")
        }
        Int::class -> {
            println("Int")
        }
        else -> {
            println("其他类型")
        }
    }
```

### 协变和逆变

out和in操作符

out只能被读取，不能修改 out在Kotlin中叫协变。

in只能修改，不能读取 in在Kotlin中叫逆变

也可以定义一个父子类(B:A) 在函数参数多态中 去验证

协变 子类可以传给父类 类似java <? extend A>

逆变 父类可以传给子类 类似java <? super B>

```kt
package com.org.kotlin

fun main() {
    //假如用第一个方法复制整个数组，我传入一个Int类型数组复制到Number数组就错了  Number是T的父类
    val srcArr = arrayOf<Int>(1, 2, 3)
    val destArr = Array<Number>(3) { 1 }
    //copyFn(destArr,srcArr)//这行报错
    copyFnOut(destArr, srcArr)
    copyFnIn(destArr, srcArr)
}

//不使用操作符，传入了T=>Number   T=>Int 两个T不一致了
fun <T> copyFn(destArr: Array<T>, srcArr: Array<T>) {
    srcArr.forEachIndexed { index, item -> destArr[index] = item }
}

//使用协变操作符out T 此泛型只能被读取，不能修改了，srcArr所以取出来一定是T  相当于T=>Number   out T=>Number  协变把Int看成父类Number
fun <T> copyFnOut(destArr: Array<T>, srcArr: Array<out T>) {
    srcArr.forEachIndexed { index, item -> destArr[index] = item }
}

//使用逆变操作符in T 此泛型只能修改，不能读取 相当于 in T =>Int T=>Int  逆变把Number看成Int的子类了
fun <T> copyFnIn(destArr: Array<in T>, srcArr: Array<T>) {
    srcArr.forEachIndexed { index, item -> destArr[index] = item }
}
```

## 协程

类似于线程，对线程进行了优化，更加轻量，协程比JVM线程的资源消耗更少

测试协程需要创建android app并在gradle里面添加依赖

### 添加库

[kotlinx.coroutines 官方库](https://github.com/Kotlin/kotlinx.coroutines/blob/master/README.md#using-in-your-projects)

app的build.gradle 添加

```kt
dependencies {
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.6.2"
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.2"
}
```

### 第一个协程

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.delay
import kotlinx.coroutines.launch
import kotlinx.coroutines.runBlocking

//运行协程需要放在runBlocking里面 叫做CoroutineScope协程作用域
fun main() = runBlocking {
    //launch启动一个协程
    launch { fn() }
    println("我先执行")
}

//一个协程函数需要声明suspend
suspend fun fn() {
    delay(5000)
    println("协程执行完毕")
}
```

### 线程等待和线程挂起

线程等待**runBlocking**

线程挂起**coroutineScope**

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.coroutineScope
import kotlinx.coroutines.delay
import kotlinx.coroutines.launch
import kotlinx.coroutines.runBlocking

//运行协程需要放在runBlocking里面 叫做CoroutineScope协程作用域
fun main() = runBlocking {
    fn()
    println("我被阻塞了")
}

//一个协程函数需要声明suspend  coroutineScope 独立的协程作用域类似于runBlocking
//runBlocking方法阻塞当前线程等待用于普通函数(不需要加suspend关键字)，而coroutineScope用于挂起函数(suspend关键字函数)
suspend fun fn() = coroutineScope {
    //launch启动一个协程 可以启动多个并行执行（同时执行）
    launch {
        delay(5000)
        println("协程执行完毕")
    }
    //和上面的协程并发执行了
    println("我先执行")
}
```

### 协程等待

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.coroutineScope
import kotlinx.coroutines.delay
import kotlinx.coroutines.launch
import kotlinx.coroutines.runBlocking

fun main() = runBlocking {
    fn()
}

suspend fun fn() = coroutineScope {
    //launch启动一个协程 可以启动多个并行执行（同时执行）
    val task = launch {
        delay(5000)
        println("协程执行完毕")
    }
    println("我先执行")
    //等待任务执行
    task.join()
    println("我被阻塞了")
}
```

### async和await

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*

fun main() = runBlocking {
    fn()
}

suspend fun fn() = coroutineScope {
    //launch启动一个协程 可以启动多个并行执行（同时执行）
    val task = async {
        delay(5000)
        println("协程执行完毕")
    }
    println("我先执行")
    //等待任务执行
    task.await()
    println("我被阻塞了")
}
```

并发请求得到返回值

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*
import kotlin.system.measureTimeMillis


fun main() = runBlocking {
    val time = measureTimeMillis {
        //直接这样同步顺序执行
        println("执行getFn1")
        val i = getFn1()
        println("执行getFn2")
        val j = getFn2()
    }
    println(time)//4023 花费4秒

    val time2 = measureTimeMillis {
        //异步并发执行 async
        //async函数有一个lazy参数 start = CoroutineStart.LAZY 可以延迟启动
        //可以手动调用start启动或者是在使用.await时启动他
        val i = async {
            println("执行getFn1")
            getFn1()
        }
        val j = async {
            println("执行getFn2")
            getFn2()
        }
        println(i.await() + j.await())
    }
    println(time2)//2045 花费2秒
}

suspend fun getFn1(): Int {
    delay(2000)
    return 10;
}

suspend fun getFn2(): Int {
    delay(2000)
    return 10;
}
```

### async还是launch

launch可启动新协程而不将结果返回给调用方。任何被视为“一劳永逸”的工作都可以使用 launch 来启动

async会启动一个新的协程，并允许您使用一个名为 await 的挂起函数返回结果

### 协程通道通信

用于协程间共享数据

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*
import kotlinx.coroutines.channels.Channel


fun main() = runBlocking {
    fn()
}

suspend fun fn() = coroutineScope {
    val channel = Channel<String>()
    launch {
        delay(2000)
        //发送
        channel.send("协程A执行完毕")
    }
    launch {
        delay(2000)
        //发送
        channel.send("协程B执行完毕")
    }
    //开一个协程用于接收信息
    launch {
        while (true) {
            //等待阻塞接收
            val receive = channel.receive()
            println("接收信息 $receive")
        }
    }
    println("我先执行")
}
```

### 取消协程

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*


fun main() = runBlocking {
    fn()
}

suspend fun fn() = coroutineScope {
    val task = launch {
        delay(2000)
        //就是this.isActive 是协程作用域的属性 CoroutineScope.isActive
        if (isActive) {
            //发送
            println("我被取消了,不会执行")
        }
    }
    //手动取消了 安全取消需要协程域执行的代码放在isActive里面
    task.cancel()
    //取消后会立即执行后面的代码，这个时候已经执行的协程代码可能还在执行，并未停止，这个时候可以用cancelAndJoin()代替 就是先取消，然后等待，可以保证已经执行的任务完成，保证代码的健壮性。
    println("取消完毕")
}
```

### 自动取消协程

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*


fun main() = runBlocking {
    fn()
}

suspend fun fn() = coroutineScope {
    //设置超时1秒，自动取消withTimeout这个会异常CancellationException
    //withTimeoutOrNull取消不会报异常，返回null，执行完毕返回最后一行
    val result = withTimeoutOrNull(1000) {
        delay(2000)
        //就是this.isActive 是协程作用域的属性 CoroutineScope.isActive
        if (isActive) {
            //发送
            println("执行任务")
        }
        "执行完毕，没有超时"
    }
    println(result)//null
}
```

### 协程参数（上下文对象）

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*

fun main(): Unit = runBlocking {
    launch { // 父协程的上下文，主 runBlocking 协程
        println("我后执行")
        delay(5000)
    }
    println("主线程执行1")
    launch(Dispatchers.Unconfined) { // 非受限的 - 将和主线程一起工作
        println("我先执行")
        delay(5000)
    }
    println("主线程执行2")

    //主线程执行1
    //我先执行
    //主线程执行2
    //我后执行
    //结论
    //launch(Dispatchers.Unconfined)会按顺序执行，
    //launch会等到主线程执行完毕才开始
}
```

### withContext

指定协程的运行线程 

Dispatchers.Main  

Dispatchers.Unconfined 

Dispatchers.IO 

Dispatchers.Defualt

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*

fun main(): Unit = runBlocking {
    var res = async {
        println("线程调度Main" + Thread.currentThread().name)
        delay(1000)
    }
    println("主线程1")
    withContext(Dispatchers.Default) {
        println("线程调度Default" + Thread.currentThread().name)
        delay(1000)
    }
    println("主线程2")
    withContext(Dispatchers.Unconfined) {
        println("线程调度Unconfined" + Thread.currentThread().name)
        delay(1000)
    }
    println("主线程3")
    withContext(Dispatchers.IO) {
        println("线程调度IO" + Thread.currentThread().name)
        delay(1000)
    }
    println("主线程4")
    //主线程1
    //线程调度Mainmain
    //线程调度DefaultDefaultDispatcher-worker-1
    //主线程2
    //线程调度Unconfinedmain
    //主线程3
    //线程调度IODefaultDispatcher-worker-1
    //主线程4
}
```

### 子协程

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*

fun main(): Unit = runBlocking {
    launch { // 父协程的上下文，主 runBlocking 协程
        println("父协程启动")
        launch {
            println("子协程启动")
            delay(5000)
            //子协程
            println("子协程执行完毕")
        }
        delay(10000)
        println("父协程执行完毕")
    }
    println("主线程执行完毕")

    //主线程执行完毕
    //父协程启动
    //子协程启动
    //子协程执行完毕
    //父协程执行完毕
    //结论，注意一点，子线程是顺序执行的，不会等到父协程执行完毕才开始
    //和主线程执行完毕，再启动父协程不一样
}
```

### 协程命名

CoroutineName

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*

fun main(): Unit = runBlocking(CoroutineName("主runBlocking协程")) {
    launch(CoroutineName("父协程启动")) {
        var sonJob = async(CoroutineName("子协程启动")) {
            delay(3000)
            //子协程
            println("子协程执行完毕")
        }
        delay(10000)
        println("父协程执行完毕")
    }
}
```

### 作用域

一般安装使用主线程作用域即可，全局的作用域是跟随应用生命周期的

```kt
class MyAndroidActivity {
    private val scope = MainScope() //等同于CoroutineScope(Dispatchers.Main + SupervisorJob()）

    override fun onDestroy() {
        super.onDestroy()
        scope.cancel()
    }
}
```

### 流flow

就是类似于监听变量

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*
import kotlinx.coroutines.flow.flow

fun main() = runBlocking {
    flow {
        delay(100)
        println("执行了")
        emit("回调值") // emit next value
    }.collect{
        //相当于监听者,开启监听上面的代码才会被执行
        //监听到值
        println(it)
    }
}
```

直接监听变量

asFlow

```kt
package com.example.kotlin_android_demo

import kotlinx.coroutines.*
import kotlinx.coroutines.flow.asFlow

fun main() = runBlocking {
    arrayOf("张三","李四").asFlow().collect {
        println(it)
    }
}
```

## json库

### 依赖

build

```
buildscript {
    dependencies {
        ...
        classpath "org.jetbrains.kotlin:kotlin-serialization:1.6.21"
        ...
    }
}

```

app

```
plugins {
    id 'org.jetbrains.kotlin.plugin.serialization'
}
...
dependencies {
    ...
    implementation "org.jetbrains.kotlinx:kotlinx-serialization-json:1.3.3"
}
```

### 使用

```kt
package com.example.kotlin_android_demo

import kotlinx.serialization.Serializable
import kotlinx.serialization.decodeFromString
import kotlinx.serialization.encodeToString
import kotlinx.serialization.json.Json

fun main() {
    val mutableList = mutableListOf<Person>()
    mutableList.add(Person("张三", 18))
    mutableList.add(Person("李四", 18))
    mutableList.add(Person("王五", 19))

    val jsonStr = Json.encodeToString(mutableList)
    println(jsonStr)//[{"name":"张三","age":18},{"name":"李四","age":18},{"name":"王五","age":19}]
    val mutableList2 = Json.decodeFromString<Person>("""{"name":"张三","age":18}""")
    println(mutableList2)//Person(name=张三, age=18)
}

@Serializable
data class Person(var name: String, var age: Int)
```

## 设计模式

### 单例

饿汉式

```kt
package com.org.kotlin

fun main() {
    SingleInstance.instance // 单例初始化了,只初始化一次
    SingleInstance.instance // 这句不输出了
}

class SingleInstance private constructor() {
    // 构造方法私有化 防止用户直接new初始化类
    init {
        println("单例初始化了,只初始化一次")
    }

    companion object {
        // 获取单例
        // 单例对象初始化
        val instance = SingleInstance()
    }
}
```

或者直接

```kt
package com.org.kotlin

fun main() {
    SingleInstance // 单例初始化了,只初始化一次
    SingleInstance // 这句不输出了
}

object SingleInstance{
    init {
        println("单例初始化了,只初始化一次")
    }
}
```

**懒汉式**

```kt
package com.org.kotlin

fun main() {
    SingleInstance.instance // 单例初始化了,只初始化一次
    SingleInstance.instance // 这句不输出了
}

class SingleInstance private constructor() {
    // 构造方法私有化 防止用户直接new初始化类
    init {
        println("单例初始化了,只初始化一次")
    }

    companion object {
        // 获取单例，单例对象初始化
        // 单例对象
        @get:Synchronized
        var instance: SingleInstance? = null
            get() {
                if (field == null) {
                    field = SingleInstance()
                }
                return field
            }
            private set
    }
}
```

**双重锁机制**

```kt
package com.org.kotlin

fun main() {
    SingleInstance.instance // 单例初始化了,只初始化一次
    SingleInstance.instance // 这句不输出了
}

class SingleInstance private constructor() {
    // 构造方法私有化 防止用户直接new初始化类
    init {
        println("单例初始化了,只初始化一次")
    }

    companion object {
        val instance: SingleInstance by lazy(mode = LazyThreadSafetyMode.SYNCHRONIZED) {
            SingleInstance()
        }
    }
}
```

### 代理模式by

by关键字就是java中的代理模式

kotlin有接口代理和属性代理

接口代理和java一样，属性代理是通过代理类的set和get方法去设置

**by lazy**

by lazy区别于lateinit ，属性代理是使用时初始化（先写好初始化代码）  lateinit使用前初始化（使用前赋值）


```kt
package com.org.kotlin

fun main() {
    val a = A()
    println(a.name)//注释这行代码发现不初始化，所以是懒加载
    a.hobby = "lol"
    println(a.hobby)
}

class A {
    val name: String by lazy {
        println("使用时初始化了")
        "张三"
    }
    lateinit var hobby: String;
}
```

**by observable**

相当于vue watch监视一个属性，没啥亮点

## kotlin安卓项目搭建

### idea环境搭建流程

1. idea需要2022版本 旧版不能热更新代码 [破解流程](https://www.exception.site/article/29) [补丁](https://pan.baidu.com/s/1uYLHHKGIcWqSrl9Je9991g ) 提取码：1234
2. idea需要下载kotlin和android插件 否则第三步出不来
3. idea Project Structure =》SDKS=》添加android sdk根目录，设置好api构建版本 使用最新版本
4. 模拟器或者真机adb连接上
5. 真机无线调试方法:开发者模式-》点击无线调试出现弹窗-》 adb pair 配对地址：端口-》输入配对码(只需配对一次)-》然后在adb connect 无线调试界面的地址和端口号 [参考](https://blog.csdn.net/weixin_42089228/article/details/124362840)
6. adb连上后，点击idea运行按钮即可，第二个按钮就是即时刷新
7. 布局现在不用学xml了，不学！
8. apk运行安装到手机出现  应用不能安装 在gradle.properties(项目根目录或者gradle全局配置目录 ~/.gradle/)文件中添加android.injected.testOnly=false
9. 发布Build--》 Generate Signed Bundle / APK 连生成key都有

### build:gradle

[镜像库](https://developer.aliyun.com/mvn/guide)

```
buildscript {
    ...
    repositories {
        //google()
        //mavenCentral()
        maven { url('https://maven.aliyun.com/repository/central') }//mavenCentral()
        maven { url('https://maven.aliyun.com/repository/public') }//jcenter()
        maven { url('https://maven.aliyun.com/repository/google') }//google()
    }
    ...
}
```

### 一些问题

**添加main函数无法运行**

解决办法，把app的build.gradle SDK改成30即可

```
android {
    compileSdk 30

    defaultConfig {
        ...
        targetSdk 30
        ...
    }
}
```



## JetpackCompose

android函数声明式UI框架，和flutter类似就是了

### DSL手写声明式组件

理解了这个再了解内置组件就容易多啦

```kt
package com.org.kotlin

fun main() {
    AppBar("标题") {
        Text("内容") {
        }
    }
    Text("内容") {
    }
    ItemList {
        items(10) {
            Text("内容 $it") {
            }
        }
    }
    CustomComponent {
        Text("自定义插槽内容") {
        }
    }
}

//下面的注解是来自compose源代码，复制的
@MustBeDocumented
@Retention(AnnotationRetention.BINARY)
@Target(
    AnnotationTarget.FUNCTION,
    AnnotationTarget.TYPE,
    AnnotationTarget.TYPE_PARAMETER,
    AnnotationTarget.PROPERTY_GETTER
)
annotation class Composable

//每个组件的作用域
interface Scope
object AppBarScope : Scope
object TextScope : Scope
object ItemListScope : Scope {
    //这是一个DLS方法
    inline fun items(num: Int, content: @Composable ItemListItemScope.(it: Int) -> Unit) {
        for (i in 0..num) {
            content(ItemListItemScope, i)
        }
    }
}
object ItemListItemScope : Scope

@Composable
fun AppBar(title: String = "", lambda: @Composable AppBarScope.() -> Unit) {
    println(title)
    lambda(AppBarScope)
}

@Composable
fun Text(text: String = "", lambda: @Composable TextScope.() -> Unit) {
    println(text)
    lambda(TextScope)
}

@Composable
fun ItemList(lambda: @Composable ItemListScope.() -> Unit) {
    println("列表项")
    lambda(ItemListScope)
}

@Composable
fun CustomComponent(content: @Composable Scope.() -> Unit) {
    Text("自定义组件") {
        content()
    }
}
```

### slot

lambda尾随实现插槽功能

```kt
@Composable
fun DemoComponent(title: String = "", slot: @Composable () -> Unit) {
    Column {
        Text(title)
        slot()
    }
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    DemoComponent(title = "组件名称") {
        Text("插槽内容")
    }
}
```

### material组件文档

[material组件文档](https://developer.android.google.cn/reference/kotlin/androidx/compose/material/package-summary#overview)

[实验性组件库](https://google.github.io/accompanist)，正式发布会从该库移除，并加入compose的

### 创建

Idea-File-New-Project-Android-Empty Compose Activity

### 注解

@Composable 

使函数成为可组合函数,就是加上这个注解就是一个UI组件了

@Preview(showBackground = true,...)

加上这个注解 这个组件就可以在设计窗口，加在@Composable上方

### 修饰符

修改size\padding\width\height\等样式用修饰符modifier = Modifier.xxx.xxx,可以链式调用。需要注意的是先后顺序很重要

[所有修饰符文档](https://developer.android.google.cn/jetpack/compose/modifiers-list#Actions)

```text
wrapContentWidth/wrapContentHeight 内容根据父容器定位
background 背景色
border 边框
padding 外边距或者是内边距
width 宽 传入IntrinsicSize.Min可以强制限制子元素宽
height 高 传入IntrinsicSize.Min可以强制限制子元素高
size 尺寸大小
clip(RoundedCornerShape(20.dp) 裁剪
```

```kt
@Composable
fun DemoComponent() {
    Button(
        modifier = Modifier
            .padding(10.dp)
            .background(Color.Red)
            //第一个相当于margin
            .padding(10.dp)
            .border(1.dp, Color.Green),
            //第二个相当于padding
        onClick = {}) {
        Text("按钮")
    }
}
```

### 空修饰符

封装组件可能会用到

```kt
@Composable
fun PhotographerCard(modifier: Modifier = Modifier) {
    Row(modifier) { ... }
}
```

### 修饰符合并

和链式调用其实一样，只不过多了一个作用域可以在里面执行别的逻辑

```kt
@Composable
fun DemoComponent() {
    Text(text = "标题标题", modifier = Modifier.composed {
        background(Color.Red).size(50.dp)
    })
}
```

### 设置activity内容

```kt
setContent {
    各种且套组件
    ...
}
```

### Text组件

style可以在主题文件里预先定义好全局的样式风格，然后拿过来用就行了

```kt
Text("张三", fontSize = 14.sp, fontWeight = FontWeight.Bold, color = MaterialTheme.colors.primary, maxLines = 2, style = MaterialTheme.typography.h1)
```

### Button

按钮组件，内部提供一个插槽，内容都放在内部

```kt
@Composable
fun DemoComponent() {
    Button(onClick = {}) {
        Text("普通按钮")
    }
    TextButton(onClick = {}) {
        Text("纯文字按钮")
    }
    OutlinedButton(onClick = {}){
        Text("线框按钮")
    }
}
```

### Icon

[material-icons图标库搜索](https://joe1900.github.io/MDI/)

app的build.gradle

```
implementation "androidx.compose.material:material-icons-extended:$compose_version"
```

```kt
Icon(Icons.Filled.ArrowBack, contentDescription = null)
```

### Colum

列

```kt
//对齐方式
Column(verticalArrangement = Arrangement.SpaceBetween) {
    Text(text = "张三")
    Text(text = "李四")
}
```

### Box

堆叠布局及宽度100%或者高度100%或者填满

```kt
@Composable
fun DemoComponent() {
    //堆叠布局
    Box(modifier = Modifier.fillMaxSize(), contentAlignment = Alignment.Center) {
        //填满父级
        Surface(modifier = Modifier.fillMaxSize(), color = Color.Red) { }
        //横向填满
        Surface(modifier = Modifier.fillMaxWidth().height(10.0.dp), color = Color.Green) { }
        //纵向填满
        Surface(modifier = Modifier.fillMaxHeight().width(10.0.dp), color = Color.Blue) { }
    }
}
```


### Image

图片放到res/drawable和R.drawable.test的test名称一样就可以了...

```kt
Image(
    painter = painterResource(R.drawable.test),
    contentDescription = "Contact profile picture",
    modifier = Modifier.size(20.dp).clip(CircleShape)//裁剪
)
```

**加载网络图片**

首先需要添加网络权限

```xml
<!-- AndroidManifest.xml -->
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.kotlin_android_demo">
    <uses-permission android:name="android.permission.INTERNET" />
...
```

app的build.gradle添加编译依赖

https://github.com/coil-kt/coil

```
implementation "io.coil-kt:coil-compose:2.1.0"
```

```kt
@Composable
fun DemoComponent() {
    AsyncImage(
        model = "http://www.ay1.cc/img?w=640&h=640&c=f60f60",
        placeholder = painterResource(R.drawable.test),
        contentDescription = null,
        modifier = Modifier.size(100.dp),
    )
}
```

### Row

行

```kt
//边距，对齐方式水平，对齐方式垂直
Row(modifier = Modifier.padding(start = 0.5.dp), horizontalArrangement = Arrangement.SpaceBetween, verticalAlignment = Alignment.CenterVertically) {
    Text("张三")
    Text("李四")
}
```

### Spacer

空白间距

```kt
//水平间距
Spacer(modifier = Modifier.width(8.dp))
//垂直间距
Spacer(modifier = Modifier.height(4.dp))
```

### TextField

输入框

```kt
@Composable
fun DemoComponent() {
    Column {
        TextField(value = "张三", onValueChange = {})
        //一般用下面这个有边框的
        OutlinedTextField(value = "张三", onValueChange = {})
    }
}
```

### Surface

在容器表面覆盖一层,也可用来占位

```kt
//遮罩大小，形状（MaterialTheme.shapes.small是Shape.kt预先定义好的圆角的..[指定圆角：RoundedCornerShape(10.dp)；指定形状：CircleShape]）阴影深度，背景颜色
Surface(modifier = Modifier.size(40.dp), shape = MaterialTheme.shapes.small, elevation = 4.dp, color = MaterialTheme.colors.primary)
```

### TopAppBar

顶部导航栏

```kt
@Composable
fun DemoComponent() {
    TopAppBar(
        title = {
            Text(text = "Page title", maxLines = 2)
        },
        navigationIcon = {
            IconButton(onClick = {}){
                Text(text = "图标")
            }
        }
    )
}
```

### BottomNavigation

底部路由导航栏

```kt
@Composable
fun DemoComponent() {
    //局部状态，用于记录当前点击的索引...
    var selectedItem by remember { mutableStateOf(0) }

    class NavigationItem(var label: String, var icon: ImageVector)

    val navigationItems = listOf<NavigationItem>(
        NavigationItem("首页", Icons.Filled.Home),
        NavigationItem("我的", Icons.Filled.Person),
    )

    BottomNavigation {
        navigationItems.forEachIndexed { index, item ->
            BottomNavigationItem(
                icon = { Icon(item.icon, contentDescription = null) },
                label = { Text(item.label) },
                selected = selectedItem == index,
                onClick = {
                    selectedItem = index
                })
        }
    }
}
```

### scaffold

脚手架

```kt
@Composable
fun DemoComponent() {
    Scaffold(
        topBar = {
            TopAppBar(
                title = {
                    Text(text = "标题", maxLines = 2)
                },
                navigationIcon = {
                    IconButton(onClick = {}) {
                        Icon(Icons.Filled.ArrowBack, contentDescription = null)
                    }
                },
                //右侧操作栏
                actions = {
                    IconButton(onClick = {}) {
                        Icon(Icons.Filled.Done, contentDescription = null)
                    }
                }
            )
        },
        bottomBar = {
            BottomNavigation {
                BottomNavigationItem(
                    icon = { Icon(Icons.Filled.Home, contentDescription = null) },
                    selected = true,
                    onClick = {})
                BottomNavigationItem(
                    icon = { Icon(Icons.Filled.Person, contentDescription = null) },
                    selected = false,
                    onClick = {})
            }
        }
    ) { innerPadding ->
        //innerPadding 内容边距 PaddingValues(start=0.0.dp, top=0.0.dp, end=0.0.dp, bottom=0.0.dp
        BodyContent(Modifier.padding(innerPadding))
    }
}

@Composable
private fun BodyContent(modifier: Modifier = Modifier) {
    Column(modifier = Modifier.padding(8.dp)) {
        Text(text = "正文内容")
    }
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    DemoComponent()
}
```

### LazyColumn

长列表,性能更好

[案例来自](https://developer.android.google.cn/jetpack/compose/tutorial) LazyColumn 包含一个 items 子项。它接受 List 作为参数，并且其 lambda 会收到我们命名为 message 的参数（可以随意为其命名），它是 Message 的实例。简而言之，系统会针对提供的 List 的每个项调用此 lambda

```kt
@Composable
fun CardList(messages: MutableList<Message>) {
    LazyColumn {
        items(messages) { message ->
            Card(message)
        }
    }
}
```

### scroll

页面内容可滚动

```kt
@Composable
fun DemoComponent() {
    val scrollState = rememberScrollState()
    Column(modifier = Modifier.verticalScroll(scrollState).fillMaxWidth()) {
        repeat(100) {
            Text("列表内容 $it")
        }
    }
}
```

**LazyColumn控制滚动位置**

```kt
@Composable
fun DemoComponent() {
    Column {
        val listSize = 100
        val scrollState = rememberLazyListState()
        val coroutineScope = rememberCoroutineScope()
        Button(onClick = {
            coroutineScope.launch {
                // 0 is the first item index
                scrollState.animateScrollToItem(0)
            }
        }) {
            Text("回到顶部")
        }
        Button(onClick = {
            coroutineScope.launch {
                // listSize - 1 is the last index of the list
                scrollState.animateScrollToItem(listSize - 1)
            }
        }) {
            Text("滚动到底部")
        }
        LazyColumn(state = scrollState) {
            items(100) {
                Text("item $it")
            }
        }
    }
}
```

**Column控制滚动位置**

```kt
@Composable
fun DemoComponent() {
    val scrollState = rememberScrollState()
    val coroutineScope = rememberCoroutineScope()
    Column(modifier = Modifier.verticalScroll(scrollState).fillMaxWidth()) {
        Button(onClick = {
            coroutineScope.launch {
                scrollState.animateScrollTo(3000)
            }
        }) {
            Text("滚动到底部")
        }
        repeat(100) {
            Text("列表内容 $it", modifier = Modifier.height(30.dp))
        }
        Button(onClick = {
            coroutineScope.launch {
                // 0 is the first item index
                scrollState.scrollTo(0)
            }
        }) {
            Text("回到顶部")
        }
    }
}
```

### click

组件添加点击事件

```kt
@Composable
fun DemoComponent() {
    //如果要点击到内边距，则click需要加在内边距前面
    Text("张三", Modifier.clickable(onClick = {}).padding(20.dp))
}
```

### flex布局

使用Row Cloumn即可实现，另外需要注意的是flex:1使用Modifier.weight(1f)实现即可

### 自定义插槽

类似vue插槽，利用尾随lambda实现，就是最后一项是lambda表达式

```kt
@Composable
fun DemoComponent(content: @Composable () -> Unit) {
    Button(onClick = {}) {
        content()//注意这里的括号
    }
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    DemoComponent {
        Text("插槽内容")
    }
}
```

### 简单的状态管理

案例，控制一个列表项展开与合并

```kt
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            DefaultPreview(
            )
        }
    }
}

data class Message(var name: String, var age: Int)

@Composable
fun CardList(messages: MutableList<Message>) {
    LazyColumn {
        items(messages) { message ->
            Card(message)
        }
    }
}

@Composable
fun Card(msg: Message) {
    Row(
        modifier = Modifier.padding(start = 0.5.dp),
        horizontalArrangement = Arrangement.SpaceBetween,
        verticalAlignment = Alignment.CenterVertically
    ) {
        Text(
            "张三",
            color = MaterialTheme.colors.primary,
            style = MaterialTheme.typography.subtitle1,
        )
        Image(
            painter = painterResource(R.drawable.test),
            contentDescription = "Contact profile picture",
            modifier = Modifier
                .size(40.dp)
                .clip(RectangleShape)
                .border(1.5.dp, MaterialTheme.colors.secondary, RectangleShape)
        )
        Spacer(modifier = Modifier.width(8.dp))

        //创建一个状态 需要通过remember
        var isExpanded by remember { mutableStateOf(false) }
        //状态控制颜色变化并添加动画
        val surfaceColor by animateColorAsState(
            if (isExpanded) MaterialTheme.colors.primary else MaterialTheme.colors.surface,
        )

        Column(verticalArrangement = Arrangement.SpaceBetween, modifier = Modifier.clickable {
            isExpanded = !isExpanded
        }) {

            Text(text = "Hello ${msg.name}!")
            Spacer(modifier = Modifier.height(4.dp))
            if(isExpanded){
                Surface(shape = MaterialTheme.shapes.small, elevation = 4.dp, color = surfaceColor) {
                    Text(text = "Hello ${msg.age}!")
                }
            }
        }
    }
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    val list = mutableListOf<Message>()
    val message = Message("张三4", 18)
    list.add(message);
    list.add(message);
    CardList(list)
}
```

### MutableState

局部状态管理的几种方式 一般使用第三种即可 带set和get

remember在屏幕旋转时会丢失状态,使用rememberSaveable代替即可

tips:遇到```ype 'TypeVariable(T)' has no method 'getValue(Nothing?, KProperty<*>)' and thus it cannot serve as a delegate```

导入```import androidx.compose.runtime.*```即可

```kt
val state = remember { mutableStateOf(deafult) }
val (value,setValue) = remember { mutableStateOf(deafult) }
val value by remenber { mutableStateOf(deafult) }
```

实现输入框输入文字双向绑定，案例

```kt
@Composable
fun DemoComponent() {
    var text by remember { mutableStateOf("张三") }
    Column {
        Text(text)
        TextField(value = text, onValueChange = {
            text = it
        })
    }
}
```

### ViewModel全局状态管理

以一个文章列表添加删除显示为例

需要的依赖用于状态管理

app的build.gradle

```
implementation 'androidx.lifecycle:lifecycle-runtime-ktx:2.4.0'
implementation 'androidx.lifecycle:lifecycle-viewmodel-ktx:2.4.0'
implementation 'androidx.activity:activity-ktx:1.4.0'
```

```kt
//文字数据类
data class Article(var id: Int, var title: String, var content: String)

//相当于一个article store
class ArticleViewModel : ViewModel() {
    //存储在store的状态变量  类型是LiveData
    var articleList = mutableStateListOf<Article>()
        //私有化setter 就不能使用articleViewModel.articleList = mutableStateListOf<Article>(Article(1,"标题","内容"))来赋值了
        private set

    //来个页面标题
    var title = mutableStateOf<String>("默认页面标题")

    fun addItem(item: Article) {
        articleList.add(item)
    }

    fun removeItem(item: Article) {
        articleList.remove(item)
    }
}


class MainActivity : ComponentActivity() {
    //获取一个store实例
    private val articleViewModel by viewModels<ArticleViewModel>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Scaffold(
                topBar = {
                    TopAppBar(
                        title = {
                            Text(text = articleViewModel.title.value, maxLines = 2)
                        }
                    )
                },
            ) {
                Surface {
                    DemoComponent(articleViewModel)
                }
            }
        }
    }
}

//主结构
@Composable
fun DemoComponent(articleViewModel: ArticleViewModel) {
    ArticleListComponent(
        articleViewModel.articleList,
        onAddItem = articleViewModel::addItem,
        onRemoveItem = articleViewModel::removeItem
    )
}


//列表组件
@Composable
fun ArticleListComponent(
    items: List<Article>,
    onAddItem: (Article) -> Unit,
    onRemoveItem: (Article) -> Unit
) {
    //渲染
    Box(modifier = Modifier.fillMaxSize(), contentAlignment = Alignment.BottomCenter) {
        LazyColumn(modifier = Modifier.padding(bottom = 60.dp).fillMaxSize()) {
            items(items.size) {
                val (id, title, content) = items[it];
                //创建一个状态 key是id 类似于vue对循环中的当前记录添加一个是否显示的状态
                var isExpanded by remember(id) { mutableStateOf(false) }
                Column {
                    Row(modifier = Modifier.fillMaxWidth().clickable {
                        isExpanded = !isExpanded
                    }, horizontalArrangement = Arrangement.SpaceBetween) {
                        Text(text = "$id $title")
                        Button(onClick = {
                            onRemoveItem(items[it])
                        }) { Text("删除") }
                    }
                    if (isExpanded) {
                        Text(content, maxLines = 2, overflow = TextOverflow.Ellipsis)
                    }
                }
            }
        }
        BottomBar(onAddItem)
    }
}

@Composable
private fun BottomBar(onAddItem: (Article) -> Unit) {
    Button(modifier = Modifier.height(60.dp).background(Color.Transparent), onClick = {
        val id = (Math.random() * 100).toInt();
        onAddItem(
            Article(
                id, "标题文字", "文章详细内容文章详细内容文章详细内容文章详细内容文章详细内容文章详细内容文章详细内容" +
                        "文章详细内容文章详细内容文章详细内容文章详细内容文章详细内容"
            )
        )
    }) { Text("添加新闻") }
}
```

### 组件内使用协程

rememberCoroutineScope.launch

用于 网络请求\IO读写\弹窗\滚动控制\动画\触摸 所有可能阻塞主线程的情况

这个协程方法，不用导入kotlin协程包...

```kt
@Composable
fun DemoComponent() {
    val scope = rememberCoroutineScope()
    scope.launch {
        println("组件内启动一个协程")
    }
}
```

### 生命周期

**LaunchedEffect**

组件首次加载的时候调用的协程，类似mounted，一些http请求就可以放在这里执行咯

### 局部状态作用域

compositionLocalOf

自带的主题设置方式就是使用的CompositionLocalProvider

可以让其在所有子层组件及嵌套组件共享变量,且作用域外的值不会受到影响,并且只能在CompositionLocalProvider
改变变量的值

如果有个数据需要在一个组件嵌套中层层传递，就用这个

类似于值变化的用compositionLocalOf，不会经常变化是用staticCompositionLocalOf

```kt
...
//在外层定义一个类似于全局变量，然后可以在任何嵌套层调用，就不需要层层传递了
val localName = compositionLocalOf {
    // 设置默认值
    "红色"
}

@Composable
fun OutComponent() {
    Text(localName.current)
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    Column {
        Text("主题", fontSize = 30.sp, fontWeight = FontWeight.Bold)
        Text("当前主题", fontSize = 14.sp)
        //CompositionLocalProvider可以让其在所有子层组件及嵌套组件共享变量
        CompositionLocalProvider(localName provides "蓝色") {
            Text(localName.current)//蓝色
            //不然在这里要传递这个参数，有了这个就不需要传下去了
            OutComponent()//蓝色
        }
        //外部的值并没有刷新
        Text(localName.current)//红色
        CompositionLocalProvider { //重新创建一个作用域，里面的还是默认的
            Text(localName.current)//红色
            OutComponent()//红色
        }
    }
}
```

### 单值动画

就是单个值改变的动画

下面是一个切换tab栏改变正文背景颜色

animateColorAsState 颜色动画状态

animateSizeAsState 尺寸动画状态

...其他点出来即可

```kt
@Composable
fun DemoComponent() {
    var tabIndexState by remember { mutableStateOf(0) }
    val titles = listOf("选项1", "选项2", "选项3")
    val backgroundColor by animateColorAsState(
        when (tabIndexState) {
            0 -> MaterialTheme.colors.primary
            1 -> MaterialTheme.colors.secondary
            2 -> MaterialTheme.colors.background
            else -> {
                MaterialTheme.colors.background
            }
        }
    )
    Column {
        TabRow(selectedTabIndex = tabIndexState) {
            titles.forEachIndexed { index, title ->
                Tab(
                    text = { Text(title) },
                    selected = tabIndexState == index,
                    onClick = { tabIndexState = index }
                )
            }
        }
        Surface(modifier = Modifier.fillMaxSize(),color = backgroundColor) {
            when (tabIndexState) {
                0 -> Text("选项1")
                1 -> Text("选项2")
                2 -> Text("选项3")
            }
        }
    }
}
```

### 尺寸大小变化动画

animateContentSize

给大小变化的外部添加点击事件，给大小变化的容器添加animateContentSize()修饰

```kt
@Composable
fun DemoComponent() {
    var isExpanded by rememberSaveable { mutableStateOf(false) }

    Surface(
        modifier = Modifier
            .fillMaxWidth()
            .padding(8.dp)
            .clickable {
                isExpanded = !isExpanded
            },
        color = Color.Red
    ) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .animateContentSize()
        ) {
            Text(
                if (isExpanded) {
                    "隐藏"
                } else {
                    "显示"
                }
            )
            if (isExpanded) {
                Surface(
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(100.dp),
                    color = Color.Red
                ) { }
            }
        }
    }
}


@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    DemoComponent()
}
```

### 显示隐藏动画

直接用AnimatedVisibility 代替if即可

实验性api 都要加上@ExperimentalAnimationApi注解

```kt
@ExperimentalAnimationApi
class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            DefaultPreview()
        }
    }
}

@ExperimentalAnimationApi
@Composable
fun DemoComponent() {
    var isExpanded by rememberSaveable { mutableStateOf(false) }

    Column {
        Button(onClick = {
            isExpanded = !isExpanded
        }) {
            Text(
                if (isExpanded) {
                    "隐藏"
                } else {
                    "显示"
                }
            )
        }
        //直接用AnimatedVisibility 代替if即可
        AnimatedVisibility(
            visible = isExpanded,
            //进入动画 这里可能需要自己调，不是重点
            enter = slideInVertically(
                initialOffsetY = {
                    -it
                }, animationSpec = tween(
                    durationMillis = 3000, easing = LinearOutSlowInEasing
                )
            ),
            //退出动画
            exit = slideOutVertically(
                targetOffsetY = {
                    -it
                }, animationSpec = tween(
                    durationMillis = 300, easing = FastOutLinearInEasing
                )
            )
        ) {
            Surface(modifier = Modifier.fillMaxWidth().height(100.dp), color = Color.Red) { }
        }
    }
}


@ExperimentalAnimationApi
@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    DemoComponent()
}
```

### 多值动画

有多个值一起改变的动画

下面是一个tab选项卡指示器移动且背景颜色更改的动画

```kt
@Composable
fun DemoComponent() {
    var tabIndexState by remember { mutableStateOf(0) }
    val titles = listOf("选项1", "选项2", "选项3")
    //Transition对象
    val transition = updateTransition(targetState = tabIndexState, label = "选项卡指示器切换动画Transition")
    //颜色动画状态
    val backgroundColor by transition.animateColor(label = "指示器边框颜色动画状态") {
        when (tabIndexState) {
            0 -> MaterialTheme.colors.primary
            1 -> MaterialTheme.colors.secondary
            2 -> MaterialTheme.colors.background
            else -> {
                MaterialTheme.colors.background
            }
        }
    }
    Column {
        TabRow(
            selectedTabIndex = tabIndexState,
            backgroundColor = backgroundColor,
            indicator = {
                //[TabPosition(left=0.0.dp, right=145.45454.dp, width=145.45454.dp)
                    tabPosition ->
                val indicatorLeft by transition.animateDp(
                    //自定义运动快慢
                    transitionSpec = {
                        //左边到右边慢  右边到左边快
                        if (0 isTransitioningTo 2) {
                            //stiffness: Float速度快慢
                            spring(stiffness = Spring.StiffnessVeryLow)
                        } else {
                            spring(stiffness = Spring.StiffnessMedium)
                        }
                    },
                    label = "指示器左边位置动画"
                ) {
                    tabPosition[tabIndexState].left
                }
                val indicatorRight by transition.animateDp(
                    //自定义运动快慢
                    transitionSpec = {
                        //左边到右边快  右边到左边慢
                        if (0 isTransitioningTo 2) {
                            spring(stiffness = Spring.StiffnessMedium)
                        } else {
                            spring(stiffness = Spring.StiffnessVeryLow)
                        }
                    },
                    label = "指示器右边位置动画"
                ) {
                    tabPosition[tabIndexState].right
                }
                //拿到这两个值然后添加指示器组件
                Box(
                    modifier = Modifier
                        //填满整行 并放置到开始位置
                        .fillMaxSize()
                        .wrapContentSize(align = Alignment.CenterStart)
                        //偏移属性
                        .offset(indicatorLeft)
                        //宽度
                        .width(indicatorRight - indicatorLeft)
                        //高度填满，不然没内容
                        .fillMaxHeight()
                        //边框+圆角
                        .border(width = 1.dp, color = Color.Red, shape = RoundedCornerShape(4.dp))
                )
            }
        ) {
            titles.forEachIndexed { index, title ->
                Tab(
                    text = { Text(title) },
                    selected = tabIndexState == index,
                    onClick = { tabIndexState = index }
                )
            }
        }
        Surface(modifier = Modifier.fillMaxSize()) {
            when (tabIndexState) {
                0 -> Text("选项1")
                1 -> Text("选项2")
                2 -> Text("选项3")
            }
        }
    }
}
```

### 无限循环动画

```kt
@Composable
fun DemoComponent() {
    //无限循环动画对象
    val infiniteTransition = rememberInfiniteTransition()
    val color by infiniteTransition.animateColor(
        //初始颜色
        initialValue = Color.Red,
        //结束颜色
        targetValue = Color.Blue,
        //动画规则
        animationSpec = infiniteRepeatable(
            //keyframes 帧动画 还有其他的 tween 在AnimationSpec接口
            animation = keyframes {
                //持续时间
                durationMillis = 5000
                //延时
                delayMillis = 1000
                //下面这句话的意思是在第3000毫秒时刻为绿色，相当于插帧
                Color.Green at 2500
            },
            //循环模式 反向
            repeatMode = RepeatMode.Reverse
        )
    )
    val size by infiniteTransition.animateValue(
        //初始颜色
        initialValue = 50.dp,
        //结束颜色
        targetValue = 100.dp,
        typeConverter = TwoWayConverter(
            convertToVector = {
                //dp 转 AnimationVector对象  这里只有一个值所以是1D，这个变化的值可以是对象
                AnimationVector1D(it.value)
            },
            convertFromVector = {
                //AnimationVector对象 转dp
                it.value.dp
            }
        ),
        //动画规则
        animationSpec = infiniteRepeatable(
            animation = tween(
                durationMillis = 5000,
                delayMillis = 1000,
                easing = LinearEasing
            ),
            //循环模式 反向
            repeatMode = RepeatMode.Reverse
        )
    )
    Surface(
        modifier = Modifier.size(size),
        shape = RoundedCornerShape(10.dp),
        color = color
    ) { }
}
```

**上面的优化版**

这里不知道为什么颜色会不停闪动，还没修复，这种动画不要加颜色，非常的垃圾，会闪烁

```kt
//非常注意这里color是Long类型，因为animateValue改变的是值不是对象Color就是对象了 Dp也是
data class AnimScaleColor(var color: Long = 0xFFFF0000, var size: Float = 50f)

@Composable
fun DemoComponent() {

    //无限循环动画对象
    val infiniteTransition = rememberInfiniteTransition()
    val animScaleColor by infiniteTransition.animateValue(
        //初始颜色
        initialValue = AnimScaleColor(0xFFFF0000, 50f),
        //结束颜色
        targetValue = AnimScaleColor(0xFF0000FF, 100f),
        //类型转换
        typeConverter = TwoWayConverter(
            convertToVector = {
                AnimationVector2D(it.color.toFloat(), it.size)
            },
            convertFromVector = {
                AnimScaleColor(it.v1.toLong(), it.v2)
            }
        ),
        //动画规则
        animationSpec = infiniteRepeatable(
            animation = tween(
                durationMillis = 2000,
                easing = LinearEasing
            ),
            //循环模式 反向
            repeatMode = RepeatMode.Reverse
        )
    )
    Surface(
        modifier = Modifier.size(animScaleColor.size.dp),
        shape = RoundedCornerShape(10.dp),
        color = Color(animScaleColor.color)
    ) { }
}
```

### 滑动删除动画

看看就行，基本用不到

```kt
@Composable
fun DemoComponent() {
    Surface(
        color = Color.Red,
        //pointerInput作用于可以接收触摸事件 相当于在这个元素上开启监听了
        //Unit "滑动删除key" 随便是啥反正是key
        //composed 只是为了有一个代码块去定义一些变量
        modifier = Modifier.composed {
            //用来记录控件滑动偏移量
            val offsetX = remember { Animatable(0f) }
            //创建一个协程域监听触摸事件
            val cScope = rememberCoroutineScope()
            pointerInput(Unit) {
                //用于计算多拽后的目标位置
                val decay = splineBasedDecay<Float>(this)
                //开启协程监听手势变化
                cScope.launch {
                    while (true) {
                        //阻塞获取按下事件
                        val pointerId = awaitPointerEventScope {
                            //第一次按下
                            awaitFirstDown().id
                        }
                        //中断之前的所有滑动动画,重新监听
                        offsetX.stop()
                        //用于记录拖拽的速度
                        val velocityTracker = VelocityTracker()
                        //阻塞获取拖动事件
                        awaitPointerEventScope {
                            //使用上面按下的Id去拖动
                            horizontalDrag(pointerId) {
                                //记录拖拽后的偏移位置
                                val horizontalDragOffset = offsetX.value + it.positionChange().x
                                //启动协程去设置偏移量状态
                                launch {
                                    //snapTo对齐到的意思 是一个挂起函数需要放在协程域当中
                                    offsetX.snapTo(horizontalDragOffset)
                                }
                                //记录拖拽速度
                                velocityTracker.addPosition(it.uptimeMillis, it.position)

                                it.consumePositionChange()
                            }
                        }
                        //获取到拖拽的水平方向上的速度
                        val velocity = velocityTracker.calculateVelocity().x
                        //计算出拖拽后可以滑动到的最终位置
                        val targetOffsetX = decay.calculateTargetValue(offsetX.value, velocity)
                        //限制拖拽的边界 这个边界值也是有问题的
                        offsetX.updateBounds(
                            lowerBound = -size.width.toFloat(),
                            upperBound = size.width.toFloat()
                        )
                        //启动一个协程用于判断现在拖动后的偏移位置
                        launch {
                            //拖拽后可能是否能画出边界
                            if (targetOffsetX.absoluteValue <= size.width) {
                                //不能
                                offsetX.animateTo(targetValue = 0f, initialVelocity = velocity)
                            } else {
                                //能，则直接滑动到最终位置
                                offsetX.animateDecay(velocity, decay)
                                println("删除动画执行结束，移除元素")
                            }
                        }
                    }
                }
            }.fillMaxWidth().height(100.dp).offset(offsetX.value.dp, 0.dp)
        }) {}
}
```

### 路由导航

如果自定义AppBar要放搜索框，直接使用插槽的TopAppBar即可，就是没标题的那个

依赖

```
implementation 'androidx.navigation:navigation-compose:2.4.0'
```

```kt
//定义路由列表 密封类，类似枚举
sealed class Screen(val route: String, val label: String, var icon: ImageVector? = null) {
    //底部导航栏路由
    object HomeComponent : Screen("Home", "首页", Icons.Filled.Home)
    object UserComponent : Screen("User", "我的", Icons.Filled.Person)

    //对象传参
    object UserDetailComponent : Screen("User/UserDetail/{userObj}", "我的")

    //带参数的路由 如果要传递数据类对象 那需要将数据类序列化，并转json传参，接收参数处decode
    object ArticleListComponent : Screen("Article/ArticleList/{articleCatId}", "文章列表")
    object ArticleDetailComponent : Screen("Article/ArticleDetail/{articleId}", "文章详情")
}

@Serializable
data class Person(var name: String, var age: Int)

//底部导航栏组件
@Composable
fun BottomBar(navController: NavHostController) {
    val navigationItems = listOf(
        Screen.HomeComponent,
        Screen.UserComponent
    )
    //获取到当前导航的节点状态
    val navBackStackEntry by navController.currentBackStackEntryAsState()
    //获取到当前导航的节点对象
    val currentDestination = navBackStackEntry?.destination
    //显示导航
    BottomNavigation {
        run {
            navigationItems.forEach { screen ->
                BottomNavigationItem(
                    icon = { Icon(screen.icon!!, contentDescription = null) },
                    label = { Text(screen.label) },
                    //hierarchy 可以处理路由嵌套的情况
                    selected = currentDestination?.hierarchy?.any { it.route == screen.route } == true,
                    onClick = {
                        //点击切换到导航
                        navController.navigate(screen.route) {
                            //清除启动路由外的所有路由
                            popUpTo(navController.graph.findStartDestination().id) {
                                saveState = true
                            }
                            //避免重复点击相同路由 反复加入
                            launchSingleTop = true
                            //重新选择之前的路由，缓存了。
                            restoreState = true
                        }
                    })
            }
        }
    }
}

//自定义AppBar组件 标题是居中的
@Composable
fun AppBar(
    navController: NavHostController,
    title: String = "",
    showBackIcon: Boolean = false,
    rowScope: @Composable (() -> Unit)? = null
) {
    TopAppBar(
        navigationIcon = {
            if (showBackIcon) {
                IconButton(onClick = {
                    navController.popBackStack()
                }) {
                    Icon(Icons.Filled.ArrowBack, contentDescription = null)
                }
            }
        },
        title = {
            Row(
                modifier = Modifier.fillMaxSize(),
                horizontalArrangement = Arrangement.Center,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Text(
                    text = title,
                    maxLines = 2,
                    textAlign = TextAlign.Center,
                    overflow = TextOverflow.Ellipsis
                )
            }
        },
        //右侧操作栏
        actions = {
            if (rowScope == null) {
                Spacer(
                    Modifier.fillMaxHeight()
                        .width(72.dp - 4.dp)
                )
            } else {
                rowScope()
            }
        }
    )
}

@Composable
fun DemoComponent() {
    //导航控制器状态对象
    val navController = rememberNavController()
    Scaffold {
        //根据导航控制器，返回不同的导航
        NavHost(navController = navController, startDestination = Screen.HomeComponent.route) {
            composable(Screen.HomeComponent.route) { HomeComponent(navController) }
            composable(Screen.UserComponent.route) { UserComponent(navController) }
            composable(Screen.UserDetailComponent.route) { UserDetailComponent(navController) }
            composable(Screen.ArticleListComponent.route) { ArticleListComponent(navController) }
            composable(Screen.ArticleDetailComponent.route) { ArticleDetailComponent(navController) }
        }
    }
}

//主页  navController 参数保证每个屏幕都能使用导航
@Composable
fun HomeComponent(navController: NavHostController) {
    Scaffold(
        topBar = {
            AppBar(
                navController = navController,
                title = Screen.HomeComponent.label
            )
        },
        //底部导航
        bottomBar = {
            //创建导航栏列表
            BottomBar(navController)
        }
    ) {
        Column {
            Text("主页")
            Text("跳转到文章列表", modifier = Modifier.clickable {
                //跳转到文章列表
                navController.navigate(Screen.ArticleListComponent.route)
            })
        }
    }
}

//个人中心
@Composable
fun UserComponent(navController: NavHostController) {
    Scaffold(
        //底部导航
        bottomBar = {
            //创建导航栏列表
            BottomBar(navController)
        }
    ) {
        Column {
            Text("个人中心")
            Text("对象传参跳转", modifier = Modifier.clickable {
                //跳转到用户详情
                navController.navigate("User/UserDetail/" + Json.encodeToString(Person("张三", 18)))
            })
        }
    }
}

//用户详情
@Composable
fun UserDetailComponent(navController: NavHostController) {
    //获取用户详情
    val userObjStr = navController.currentBackStackEntry?.arguments?.get("userObj")
    val userObj: Person? = userObjStr?.let {
        Json.decodeFromString<Person>(userObjStr as String)
    }

    Scaffold(
        topBar = {
            AppBar(
                navController = navController,
                title = Screen.UserDetailComponent.label,
                showBackIcon = true
            )
        },
    ) {
        Column {
            Text("用户详情" + userObj?.name)
        }
    }
}

//文章列表
@Composable
fun ArticleListComponent(navController: NavHostController) {
    Scaffold(
        topBar = {
            AppBar(
                navController = navController,
                title = Screen.ArticleListComponent.label,
                showBackIcon = true
            )
        },
    ) {
        Column {
            Text("文字列表")
            Text("跳转到文章详情", modifier = Modifier.clickable {
                //跳转到文章列表
                navController.navigate("Article/ArticleDetail/333")
            })
        }
    }
}

//文章详情
@Composable
fun ArticleDetailComponent(navController: NavHostController) {
    //获取详情
    val id = navController.currentBackStackEntry?.arguments?.get("articleId").apply {}
    Scaffold(
        topBar = {
            AppBar(
                navController = navController,
                title = Screen.ArticleDetailComponent.label,
                showBackIcon = true,
                rowScope = {
                    IconButton(onClick = {}) {
                        Icon(Icons.Filled.Done, contentDescription = null)
                    }
                })
        },
    ) {
        Column {
            Text("文章详情id：$id")
            Text("跳到指定页面,并移除堆栈指定路由之上的所有路由(用的不多)", modifier = Modifier.clickable {
                //跳转到首页
                navController.navigate(Screen.HomeComponent.route) {
                    //移除文章列表上的所有路由，这里移除了文章详情，跳到到首页返回后只能返回到文章列表
                    popUpTo(Screen.ArticleListComponent.route)
                }
            })
            Text("跳到指定页面,并移除堆栈指定路由之上的所有路由,包括指定路由(可用，相当于移除所有路由，只保留一个路由)", modifier = Modifier.clickable {
                //跳转到首页
                navController.navigate(Screen.HomeComponent.route) {
                    //移除文章列表上的所有路由，这里移除了文章详情，跳到到首页返回后只能返回到文章列表
                    popUpTo(Screen.HomeComponent.route) {
                        inclusive = true
                    }
                }
            })
            Text("重复点击同一个路由，避免多个重复路由在顶部(可用)", modifier = Modifier.clickable {
                //跳转到首页
                navController.navigate("Article/ArticleDetail/333") {
                    launchSingleTop = true
                }
            })
        }
    }
}
```

### 状态栏

依赖

```
//用于设置状态栏
implementation 'com.google.accompanist:accompanist-systemuicontroller:0.23.1'
//用于获取状态栏高度 沉浸式状态栏需要
implementation 'com.google.accompanist:accompanist-insets:0.23.1'
```

**设置导航栏颜色**

```kt
@Composable
fun DemoComponent1() {
    //系统UI设置控制器
    val systemUiController = rememberSystemUiController()
    //使用白色图标
    val useDarkIcons = false
    //背景颜色
    val backgroundColor = MaterialTheme.colors.primarySurface
    //设置效果
    SideEffect {
        systemUiController.setSystemBarsColor(
            color = backgroundColor,
            darkIcons = useDarkIcons
        )
    }
    Scaffold(
        topBar = {
            TopAppBar(
                title = {
                    Text(text = "Page title", maxLines = 2)
                },
                navigationIcon = {
                    IconButton(onClick = {}) {
                        Text(text = "图标")
                    }
                }
            )
        },
    ) {
        LazyColumn(modifier = Modifier.fillMaxSize()) {
            items(100) {
                Text("item $it")
            }
        }
    }
}
```

**沉浸式状态栏**

遇到大坑，官方的修饰符没有，且第三方组件里使用还要包裹一层，另外如果有底部导航栏，会挡住内容，还得加上底部导航栏高度

MainActivity开启沉浸式状态栏 一般如果是图片的话已经可以了,如果顶部是文字，还得加上状态栏高度

```kt
class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        //设置沉浸式状态栏
        WindowCompat.setDecorFitsSystemWindows(window, false)
        //设置沉浸式状态栏
        super.onCreate(savedInstanceState)
        setContent {
            DefaultPreview()
        }
    }
}
```

主屏幕设置状态栏颜色，以及兼容问题

```kt
@Composable
fun DemoComponent() {
    //系统UI设置控制器
    val systemUiController = rememberSystemUiController()
    //使用白色图标
    val useDarkIcons = true
    //背景颜色
    val backgroundColor = Color.Transparent
    //设置效果
    SideEffect {
        systemUiController.setSystemBarsColor(
            color = backgroundColor,
            darkIcons = useDarkIcons
        )
    }
    //得用这个ProvideWindowInsets 不然statusBarsHeight()不生效
    ProvideWindowInsets {
        Scaffold(bottomBar = {
            BottomNavigation {
                BottomNavigationItem(
                    icon = {},
                    label = { Text("底部导航栏") },
                    selected = true,
                    onClick = {})
            }
        }) {
            //paddingValues这个值就是底部导航栏的值
                paddingValues ->
            val scrollState = rememberScrollState()
            //.padding(paddingValues) 即可解决底部挡住问题
            Column(modifier = Modifier.verticalScroll(scrollState).fillMaxWidth().padding(paddingValues)) {
                Spacer(
                    //设置状态栏高度
                    modifier = Modifier.statusBarsHeight()
                        .fillMaxWidth().background(
                            //渐变色
                            brush = Brush.linearGradient(
                                colors = listOf(
                                    colors.primary,
                                    colors.primaryVariant,
                                )
                            )
                        )
                )
                TopAppBar(
                    title = {
                        Text(text = "Page title", maxLines = 2)
                    },
                    navigationIcon = {
                        IconButton(onClick = {}) {
                            Text(text = "图标")
                        }
                    },
                    //渐变色
                    modifier = Modifier.background(
                        brush = Brush.linearGradient(
                            colors = listOf(
                                colors.primary,
                                colors.primaryVariant,
                            )
                        )
                    ),
                    backgroundColor = Color.Transparent,
                    contentColor = Color.White,
                    elevation = 0.dp
                )
                repeat(100) {
                    Text("列表内容 $it")
                }

                //Spacer(modifier = Modifier.navigationBarsHeight(56.dp))
            }
        }
    }
}
```