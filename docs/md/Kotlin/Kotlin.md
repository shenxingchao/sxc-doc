# Kotlin

# 基础

## 变量 常量

```kt
package com.org.kotlin
//常量
const val PI = 3.1415

fun main() {
    //可变类型变量
    //可省略类型，有类型推断
    var age = 10//var age:Int = 10
    age = 3
    //不可变类型变量 相当于java中的final
    val name = "张三"
}
```

## 基本数据类型

kotlin基本数据类型有Byte、Short、Int、Long、Float、Double等，他们是一个类，在编译后会转成java基本数据类型byte,short,int,long,float,double

## 类型转换

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

## 数组

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

## 解构赋值

```kt
package com.org.kotlin

fun main() {
    //初始化数组方法
    val list = intArrayOf(1, 2, 3)
    //解构赋值
    val (a, b, c) = list
    println("$a $b $c")//1 2 3

    //解构赋值指定位置
    val (_, d, _) = list
    println("$d")//2
}
```

## 遍历

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

## range表达式

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

## when表达式

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

## Unit类代替void返回值

为空的话返回了Unit对象

```kt
package com.org.kotlin

fun main() {
    fun voidFn() {
    }
    
    println(voidFn())//kotlin.Unit
}
```

## 字符串模板

```kt
package com.org.kotlin

fun main() {
    val age = 20
    val name = "张三"

    println("age=${age + 1},name=$name")//age=21,name=张三
}
```

## 三目运算符

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

## 空判断

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

## 异常

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

## kotlin注解

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

## is操作符

判断对象，并在该判断后的方法体内自动推断类型

```kt
fun main() {
    val str: Any = "哈哈"
    if (str is String) {//不仅是这个if里面，在整个main函数都已经明确类型
        println(str.length)
    }
}
```

## 将代码标记为未完成

添加此方法，执行到此会报异常

```kt
TODO()
```

# 函数

```kt
package com.org.kotlin

fun main() {
    println(fnName("张三"))//张三
    println(fnName2("李四"))//李四
    println(fnName3())//乖乖，默认参数
    println(fnName4(null))//null
    println(fnName(name = "张三"))//给指定的参数传值,叫具名参数传值
    println(fnVarArg("张三", "8岁", "男"))//8岁 男 张三
    fnVoid("张三")//无返回值
}

//不写private默认public
private fun fnName(name: String): String {
    return name
}

//上面的函数编译成java后是这样的
//private static final String fnName(String name) {
//    return name
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

## 匿名函数Lambda

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

## .()语法糖

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

## ::语法糖

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

## 函数参数和inline内联关键字和函数引用

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

## 函数作为返回值

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

## 扩展函数

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

## 中缀表达式

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

# 特殊类类型

## Nothing

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

## 任意类型Any

超类 相当于 java中的Object

```kt
package com.org.kotlin

fun main() {
    //定义任意类型变量
    var a: Any = 3
    a = "三"

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

# 内置api

## 字符串操作

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

## Filter过滤

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

## zip

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

# 作用域函数

## let

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

## apply

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

## run

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

## also

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

## with

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
    var name: String = ""
    var age: Int = 0
    override fun toString(): String {
        return "Person(name='$name', age=$age)"
    }
}
```

# 高阶函数

## map

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

## any

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

## all

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

## any

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

## count

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

## groupBy

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

## partition

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

## sortedBy

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

## getOrElse

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

## flatMap

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



## takeIf

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
    val bool = true
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

## RxJava

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

# 集合

## List

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
    list = list.plus("4")
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
    list.add("4")
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

## Set集合

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

## Map

**可变Map mutableMapOf**

```kt
package com.org.kotlin

fun main() {
    //初始化不可变Map
    val map = mapOf<String, Int>()

    //可变map xx to xx =》键值对
    val map2 = mutableMapOf<String, Any>("hobby" to "lol", "address" to "SH")
    //增
    map2["name"] = "张三"
    map2["age"] = 15
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

# 元组

kotlin没有多个数据的元组

## Pair

二个数据泛型元组

```kt
package com.org.kotlin

fun main() {
    val pair = Pair(1, 2)
    val pair1 = Pair("name", "张三")
}
```

## Triple

三个数据泛型元组

```kt
package com.org.kotlin

fun main() {
    val pair = Triple(1, 2, "third")
    val pair1 = Triple("name", "张三", 18)
}
```

# 面向对象

## 类

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

## 成员变量延迟初始化手动赋值lateinit

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

## 成员变量延迟初始化自动赋值bylazy

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

## 继承和重写

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

## 向上转型和向下转型

```kt
package com.org.kotlin

fun main() {
    //向上转型 - 父类引用指向子类对象,且会自动遗失父类没有的方法
    val a: A = B("张三", 18)
    a.fn()//子类重写父类方法
    a.parentFn()//父类方法
    //a.sonFn()//报错，自动遗失父类没有的方法
    //向下转型
    val b: Any = B("张三", 18)
    //判断类型后执行 is 就是java里的instanceof
    if (b is B) {
        b.fn()//子类重写父类方法
        b.sonFn()//子类方法
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

## object单例和匿名内部类

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

## 伴生对象

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

## 内部类

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

## 数据类

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
    val (userId1, name1, age1) = UserNormal(1, "张三", 18)
    println("$userId1 $name1 $age1")
    //数据类自动添加了解构返回方法
    val (userId2, name2, age2) = User(1, "张三", 18)
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

## operator重载运算符

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

## 枚举类

enum class

比较java多了一个class关键字,强调他是一个类

```kt
package com.org.kotlin

fun main() {
    println(A.RED)// RED
    println(A.GREEN)// GREEN
    println(B.RED)// RED
    //枚举对象名称
    println(A.RED.name)//RED
    //枚举对象对应的编号
    println(A.RED.ordinal)//0
    //打印所有枚举
    println(B.values().contentToString())//[RED, GREEN, BLUE]
    // 将字符串转成已有枚举常量对象
    println(B.valueOf("RED"))// RED
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

## 密封类

枚举类的扩展，可代替枚举，主要用于when判断多个对象，不需要写else的情况

主要因为他的子类可以创建多个，而枚举类是不行的（枚举类也不需要写else）

```kt
package com.org.kotlin

fun main() {
    //密封类不能被初始化,自己是抽象类
    //Color()//Sealed types cannot be instantiated

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

## 接口

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

## 抽象类

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

## 泛型

```kt
package com.org.kotlin

fun main() {
    val person = Person<Int>(null)
    //表明泛型是Int
    person.age = 18
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
    person.a = B()
    person.a = null//对象传null也没报错..

    //person.a = 18//报错
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

## 协变和逆变

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

# 协程

类似于线程，对线程进行了优化，更加轻量，协程比JVM线程的资源消耗更少

测试协程需要创建android app并在gradle里面添加依赖

**协程是运行在线程之上的,可在不同线程间调度**

## 添加库

[kotlinx.coroutines 官方库](https://github.com/Kotlin/kotlinx.coroutines/blob/master/README.md#using-in-your-projects)

app的build.gradle 添加

```kt
dependencies {
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.6.2"
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.2"
}
```

## 第一个协程

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

//一个挂起函数需要声明suspend（只是提示作用，真正的开启协程在协程作用域里） 在他的lambda里面可以启动多个并发协程
suspend fun fn() {
    delay(5000)
    println("协程执行完毕")
}
```

tips:挂起函数的本质就是切线程，协程的本质就是在线程上的一层封装

## 线程等待和线程挂起

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

## 协程等待

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

## async和await

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
    return 10
}

suspend fun getFn2(): Int {
    delay(2000)
    return 10
}
```

## async还是launch

launch可启动新协程而不将结果返回给调用方。任何被视为“一劳永逸”的工作都可以使用 launch 来启动

async会启动一个新的协程，并允许您使用一个名为 await 的挂起函数返回结果

## 协程通道通信

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

## 取消协程

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

## 自动取消协程

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

## 非受限的协程

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

## withContext

指定协程的运行线程 

**withContext 在一个lanuch或者是async里面 只是用来切换上下文的**

**一般的协程Context 由两部分组成**

Job() + Dispatchers.Main 子协程异常会导致父协程结束

SupervisorJob() + Dispatchers.Main 子协程异常不会导致父协程结束，一般是这个

tips:包括andoird里的MainScope，ViewModle的viewModelScope

**下面是4种调度器**

Dispatchers.Main  UI线程，UI绘制，动画，都是在这里面

Dispatchers.Unconfined 基本不用

Dispatchers.IO 网络通信,socket一些耗时操作都在这里面

Dispatchers.Defualt  和IO共享线程池， 用来计算型任务（涉及到循环n次的计算）

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

## 子协程

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

## 协程命名

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

## 作用域

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

## 流flow

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

# json库

## 依赖

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

高版本gradle使用

```
plugins {
    id 'org.jetbrains.kotlin.plugin.serialization' version '1.6.21'
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

## 使用

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

# 设计模式

## 单例

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

## 代理模式by

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
    lateinit var hobby: String
}
```

**by observable**

相当于vue watch监视一个属性，没啥亮点

# kotlin安卓项目搭建

## idea环境搭建流程

1. idea需要2022版本 旧版不能热更新代码 [破解流程](https://www.exception.site/article/29) [补丁](https://pan.baidu.com/s/1uYLHHKGIcWqSrl9Je9991g ) 提取码：1234
2. idea需要下载kotlin和android插件 否则第三步出不来
3. idea Project Structure =》SDKS=》添加android sdk根目录，设置好api构建版本 使用最新版本
4. 模拟器或者真机adb连接上
5. 真机无线调试方法:开发者模式-》点击无线调试出现弹窗-》 adb pair 配对地址：端口-》输入配对码(只需配对一次)-》然后在adb connect 无线调试界面的地址和端口号 [参考](https://blog.csdn.net/weixin_42089228/article/details/124362840)
6. adb连上后，点击idea运行按钮即可，第二个按钮就是即时刷新
7. 布局现在不用学xml了，不学！
8. apk运行安装到手机出现  应用不能安装 在gradle.properties(项目根目录或者gradle全局配置目录 ~/.gradle/)文件中添加android.injected.testOnly=false
9. 发布Build--》 Generate Signed Bundle / APK 连生成key都有

## build:gradle

[镜像库](https://developer.aliyun.com/mvn/guide)

compose项目好像不用加

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

## 一些问题

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



# JetpackCompose

android函数声明式UI框架，和flutter类似就是了

## DSL手写声明式组件

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

## material组件文档

[material组件文档](https://developer.android.google.cn/reference/kotlin/androidx/compose/material/package-summary#overview)

[material3组件文档](https://developer.android.google.cn/reference/kotlin/androidx/compose/material3/package-summary#materialtheme) 后面的案例都用这个演示

[实验性组件库](https://google.github.io/accompanist)，正式发布会从该库移除，并加入compose的

## Idea创建

Idea-File-New-Project-Android-Empty Compose Activity

## As创建项目 

使用最新版androidStudio

1. [下载](https://developer.android.google.cn/studio)
2. new Project 选择Empty Compose Activity([Material3](https://developer.android.google.cn/jetpack/androidx/releases/compose-material3#kts))  等待依赖库下载完毕
3. 修改项目根目录build里[compose版本1.2.0-beta01](https://developer.android.google.cn/jetpack/androidx/releases/compose)，直接用最新的 kotlin版本直接最新1.6.21
4. 配置注释搜索space
5. 配置字体大小主题 白色 14 16
6. 配置自动导入Auto Import kotlin 选中第一个
7. 配置格式化快键键Keymap 搜索format
8. 配置跳转编辑快键键Keymap 搜索navigate

## slot

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

## 注解

@Composable 

使函数成为可组合函数,就是加上这个注解就是一个UI组件了

@Preview(showBackground = true,...)

加上这个注解 这个组件就可以在设计窗口，加在@Composable上方

## 修饰符

修改size\padding\width\height\等样式用修饰符modifier = Modifier.xxx.xxx,可以链式调用。需要注意的是先后顺序很重要

[所有修饰符文档](https://developer.android.google.cn/jetpack/compose/modifiers-list#Actions)

```text
wrapContentWidth/wrapContentHeight 内容根据父容器定位
background 背景色 慎用，一般用Surface设置颜色
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

## 空修饰符

封装组件可能会用到

```kt
@Composable
fun PhotographerCard(modifier: Modifier = Modifier) {
    Row(modifier) { ... }
}
```

## 设置activity内容

```kt
setContent {
    各种且套组件
    ...
}
```

## Text

style可以在主题文件里预先定义好全局的样式风格，然后拿过来用就行了

```kt
Text("张三", fontSize = 14.sp, fontWeight = FontWeight.Bold, color = MaterialTheme.colorScheme.primary, maxLines = 2, style = MaterialTheme.typography.labelLarge)
```

## Button

按钮组件，内部提供一个插槽，内容都放在内部

```kt
Column {
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

## Icon

[material-icons图标库搜索](https://joe1900.github.io/MDI/)

app的build.gradle

```
implementation "androidx.compose.material:material-icons-extended:$compose_version"
```

```kt
Icon(Icons.Filled.ArrowBack, contentDescription = null)
```

## Colum

列

```kt
//对齐方式
Column(verticalArrangement = Arrangement.SpaceBetween) {
    Text(text = "张三")
    Text(text = "李四")
}
```

## Box

堆叠布局及宽度100%或者高度100%或者填满

```kt
//堆叠布局
Box(modifier = Modifier.fillMaxSize(), contentAlignment = Alignment.Center) {
    //填满父级
    Surface(modifier = Modifier.fillMaxSize(), color = Color.Red) { }
    //横向填满
    Surface(modifier = Modifier.fillMaxWidth().height(10.0.dp), color = Color.Green) { }
    //纵向填满
    Surface(modifier = Modifier.fillMaxHeight().width(10.0.dp), color = Color.Blue) { }
}
```


## Image

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
    //加载不出来是因为设置了代理没开fiddler...
    AsyncImage(
        model = "http://www.ay1.cc/img?w=640&h=640&c=f60f60",
        placeholder = painterResource(R.drawable.test),
        contentDescription = null,
        modifier = Modifier.size(100.dp),
    )
}
```

## Row

行

```kt
//边距，对齐方式水平，对齐方式垂直
Row(modifier = Modifier.padding(start = 0.5.dp), horizontalArrangement = Arrangement.SpaceBetween, verticalAlignment = Alignment.CenterVertically) {
    Text("张三")
    Text("李四")
}
```

## Spacer

空白间距

```kt
//水平间距
Spacer(modifier = Modifier.width(8.dp))
//垂直间距
Spacer(modifier = Modifier.height(4.dp))
```

## TextField

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

## Surface

在容器表面覆盖一层,也可用来占位

```kt
//遮罩大小，形状（MaterialTheme.shapes.small是Shape.kt预先定义好的圆角的..[指定圆角：RoundedCornerShape(10.dp)；指定形状：CircleShape]）阴影深度，背景颜色
Surface(
    modifier = Modifier.size(40.dp),
    shape = MaterialTheme.shapes.small,
    shadowElevation = 4.dp,
    color = MaterialTheme.colorScheme.primary
) {}
```

## TopAppBar

顶部导航栏 

CenterAlignedTopAppBar 一般用这个就可以了

SmallTopAppBar

等


```kt
CenterAlignedTopAppBar(
    title = {
        Text(text = "Page title", maxLines = 2)
    },
    navigationIcon = {
        IconButton(onClick = {}) {
            Text(text = "图标")
        }
    },
    colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
        containerColor = MaterialTheme.colorScheme.primary,
        titleContentColor = MaterialTheme.colorScheme.surface,
        navigationIconContentColor = MaterialTheme.colorScheme.surface,
        actionIconContentColor = MaterialTheme.colorScheme.surface,
        scrolledContainerColor = MaterialTheme.colorScheme.surface,
    )
)
```

## NavigationBar

底部路由导航栏

这个导航栏高度固定死了，不太好用，如果要自定义高度，最好是自己搞个Row

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

    NavigationBar(
        containerColor = MaterialTheme.colorScheme.primary,
        contentColor = MaterialTheme.colorScheme.surface,
        modifier = Modifier
            .height(70.dp)
    ) {
        navigationItems.forEachIndexed { index, item ->
            NavigationBarItem(
                icon = {
                    Icon(
                        item.icon,
                        contentDescription = null,
                        modifier = Modifier
                            .padding(bottom = 40.dp)
                            .size(30.dp)
                    )
                },
                colors = NavigationBarItemDefaults.colors(
                    //隐藏指示器颜色
                    indicatorColor = MaterialTheme.colorScheme.primary,
                    selectedIconColor = Color.White,
                    selectedTextColor = Color.White,
                    unselectedIconColor = Color.LightGray,
                    unselectedTextColor = Color.LightGray
                ),
                label = {
                    Text(
                        item.label,
                        modifier = Modifier
                            .padding(top = 10.dp)
                    )
                },
                selected = selectedItem == index,
                onClick = {
                    selectedItem = index
                })
        }
    }
}
```

## scaffold

脚手架

```kt
@ExperimentalMaterial3Api
@Composable
fun DemoComponent() {
    Scaffold(
        topBar = {
            CenterAlignedTopAppBar(
                title = {
                    Text(text = "Page title", maxLines = 2)
                },
                navigationIcon = {
                    IconButton(onClick = {}) {
                        Text(text = "图标")
                    }
                },
                colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primary,
                    titleContentColor = MaterialTheme.colorScheme.surface,
                    navigationIconContentColor = MaterialTheme.colorScheme.surface,
                    actionIconContentColor = MaterialTheme.colorScheme.surface,
                    scrolledContainerColor = MaterialTheme.colorScheme.surface,
                ),
                //右侧操作栏
                actions = {
                    IconButton(onClick = {}) {
                        Icon(Icons.Filled.Done, contentDescription = null)
                    }
                }
            )
        },
        bottomBar = {
            NavigationBar(
                containerColor = MaterialTheme.colorScheme.primary,
                contentColor = MaterialTheme.colorScheme.surface,
            ) {
                NavigationBarItem(
                    icon = { Icon(Icons.Filled.Home, contentDescription = null) },
                    colors = NavigationBarItemDefaults.colors(
                        //隐藏指示器颜色
                        indicatorColor = MaterialTheme.colorScheme.primary,
                        selectedIconColor = Color.White,
                        selectedTextColor = Color.White,
                        unselectedIconColor = Color.LightGray,
                        unselectedTextColor = Color.LightGray
                    ),
                    selected = true,
                    onClick = {})
                NavigationBarItem(
                    icon = { Icon(Icons.Filled.Person, contentDescription = null) },
                    colors = NavigationBarItemDefaults.colors(
                        //隐藏指示器颜色
                        indicatorColor = MaterialTheme.colorScheme.primary,
                        selectedIconColor = Color.White,
                        selectedTextColor = Color.White,
                        unselectedIconColor = Color.LightGray,
                        unselectedTextColor = Color.LightGray
                    ),
                    selected = false,
                    onClick = {})
            }
        }
    ) { innerPadding ->
        //innerPadding 内容边距 PaddingValues(start=0.0.dp, top=0.0.dp, end=0.0.dp, bottom=0.0.dp
        BodyContent(Modifier.padding(innerPadding))
    }
}
```

## LazyColumn

长列表,性能更好

[案例来自](https://developer.android.google.cn/jetpack/compose/tutorial) LazyColumn 包含一个 items 子项。它接受 List 作为参数，并且其 lambda 会收到我们命名为 message 的参数（可以随意为其命名），它是 Message 的实例。简而言之，系统会针对提供的 List 的每个项调用此 lambda

```kt
data class Message(var name: String, var age: Int)

@Composable
fun CardList(messages: MutableList<Message>) {
    LazyColumn {
        items(messages.size) { index ->
            Text(messages[index].name + messages[index].age)
        }
    }
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    val list = mutableListOf<Message>()
    val message = Message("张三4", 18)
    list.add(message)
    list.add(message)
    CardList(list)
}
```

## stickyHeader

粘性头部,滚动到顶部时，吸附到顶部

```kt
@OptIn(ExperimentalFoundationApi::class)
@Composable
fun DemoComponent() {
    var tabIndexState by remember { mutableStateOf(0) }
    val titles = listOf("选项1", "选项2", "选项3")
    LazyColumn(
        modifier = Modifier
            .fillMaxWidth()
    ) {
        item {
            Surface(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(50.dp), color = Color.Red
            ) {
            }
        }
        stickyHeader() {
            TabRow(selectedTabIndex = tabIndexState) {
                titles.forEachIndexed { index, title ->
                    Tab(
                        text = { Text(title) },
                        selected = tabIndexState == index,
                        onClick = { tabIndexState = index }
                    )
                }
            }
        }
        when (tabIndexState) {
            0 ->
                items(100) {
                    Text(text = "列表1")
                }
            1 -> items(100) {
                Text(text = "列表2")
            }
            2 -> items(100) {
                Text(text = "列表3")
            }
        }
    }
}
```

## scroll

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

这里滚动的像素还没计算出来

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

## click

组件添加点击事件

```kt
@Composable
fun DemoComponent() {
    //如果要点击到内边距，则click需要加在内边距前面
    Text("张三", Modifier.clickable(onClick = {}).padding(20.dp))
}
```

## flex布局

使用Row Cloumn即可实现，另外需要注意的是flex:1使用Modifier.weight(1f)实现即可

```kt
Row(horizontalArrangement = Arrangement.SpaceBetween) {
    Text(text = "left")
    Surface(
        modifier = Modifier
            .weight(1f)
            .height(40.dp),
        color = Color.Red
    ) {
    }
    Text(text = "right")
}
```

## 自定义插槽

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

## 简单的状态管理

案例，控制一个列表项展开与合并

```kt
data class Message(var name: String, var age: Int)

@Composable
fun CardList(messages: MutableList<Message>) {
    LazyColumn {
        items(messages.size) { index ->
            Card(messages[index])
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
            color = MaterialTheme.colorScheme.primary,
            style = MaterialTheme.typography.titleSmall,
        )
        Image(
            painter = painterResource(R.drawable.test),
            contentDescription = "Contact profile picture",
            modifier = Modifier
                .size(40.dp)
                .clip(RectangleShape)
                .border(1.5.dp, MaterialTheme.colorScheme.secondary, RectangleShape)
        )
        Spacer(modifier = Modifier.width(8.dp))

        //创建一个状态 需要通过remember
        var isExpanded by remember { mutableStateOf(false) }
        //状态控制颜色变化并添加动画
        val surfaceColor by animateColorAsState(
            if (isExpanded) MaterialTheme.colorScheme.primary else MaterialTheme.colorScheme.surface,
        )

        Column(verticalArrangement = Arrangement.SpaceBetween, modifier = Modifier.clickable {
            isExpanded = !isExpanded
        }) {

            Text(text = "Hello ${msg.name}!")
            Spacer(modifier = Modifier.height(4.dp))
            if (isExpanded) {
                Surface(
                    shape = MaterialTheme.shapes.small,
                    shadowElevation = 4.dp,
                    color = surfaceColor
                ) {
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
    list.add(message)
    list.add(message)
    CardList(list)
}
```

## MutableState

局部状态管理的几种方式 一般使用第三种即可 带set和get

remember在屏幕旋转时会丢失状态,使用rememberSaveable代替即可

tips:遇到```ype 'TypeVariable(T)' has no method 'getValue(Nothing?, KProperty<*>)' and thus it cannot serve as a delegate```

导入```import androidx.compose.runtime.*```即可

```kt
val state = rememberSaveable { mutableStateOf(1) }
val (str, setStr) = rememberSaveable { mutableStateOf("二") }
val bool by rememberSaveable { mutableStateOf(false) }
val list = mutableListOf<String>()
val map = mutableMapOf<String,Int>()
val set = mutableSetOf<String>()
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

tips:=号方式的set get需要加.value获取  而by就已经帮你封装好set/get了

## ViewModel全局状态管理

以一个文章列表添加删除显示为例

[需要的依赖用于状态管理](https://developer.android.google.cn/jetpack/compose/state#viewmodels-source-of-truth)

app的build.gradle

```
implementation 'androidx.lifecycle:lifecycle-runtime-ktx:2.4.1'
implementation 'androidx.lifecycle:lifecycle-viewmodel-compose:2.4.1'
implementation 'androidx.activity:activity-compose:1.4.0'
```

```kt
@ExperimentalMaterial3Api
class MainActivity : ComponentActivity() {
    //获取一个store实例 一般不需要这么获取了
    //直接使用 articleViewModel: ArticleViewModel = viewModel()
    private val articleViewModel by viewModels<ArticleViewModel>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Scaffold(
                topBar = {
                    CenterAlignedTopAppBar(
                        title = {
                            Text(text = articleViewModel.title.value, maxLines = 2)
                        }
                    )
                },
            ) { innerPadding ->
                Surface(modifier = Modifier.padding(innerPadding)) {
                    DemoComponent(articleViewModel)
                }
            }
        }
    }
}

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

//主结构
@Composable
fun DemoComponent(articleViewModel: ArticleViewModel = viewModel()) {
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
        LazyColumn(modifier = Modifier
            .padding(bottom = 60.dp)
            .fillMaxSize()) {
            items(items.size) {
                val (id, title, content) = items[it]
                //创建一个状态 key是id 类似于vue对循环中的当前记录添加一个是否显示的状态
                var isExpanded by remember(id) { mutableStateOf(false) }
                Column {
                    Row(modifier = Modifier
                        .fillMaxWidth()
                        .clickable {
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
    Button(modifier = Modifier
        .height(60.dp)
        .background(Color.Transparent), onClick = {
        val id = (Math.random() * 100).toInt()
        onAddItem(
            Article(
                id, "标题文字", "文章详细内容文章详细内容文章详细内容文章详细内容文章详细内容文章详细内容文章详细内容" +
                        "文章详细内容文章详细内容文章详细内容文章详细内容文章详细内容"
            )
        )
    }) { Text("添加新闻") }
}


@Composable
@Preview
fun DefaultPreview() {
    DemoComponent()
}
```

**路由导航配合ViewModel**

有两种方法

第一种使用CompositionLocalProvider定义全局LocalViewModelStoreOwner,看viewModel()源码就知道他用的就是这个LocalViewModelStoreOwner.current，相当于把拿到了外面覆盖了参数上的

在路由导航组件里加上
```kt
@OptIn(ExperimentalMaterial3Api::class, ExperimentalAnimationApi::class)
@Composable
fun NavigationComponent() {
    ......
    //全局viewModel所有者  
    //LocalViewModelStoreOwner.current这个是系统内置的
    val viewModelStoreOwner = checkNotNull(LocalViewModelStoreOwner.current) {
        "No ViewModelStoreOwner was provided via LocalViewModelStoreOwner"
    }
    ......
    Scaffold { paddingValues ->
        Surface(
            modifier = Modifier.padding(paddingValues)
        ) {
            AnimatedNavHost(navController = navController,
                startDestination = Screen.HomeScreen.route,
                .....
               ) {
                composable(Screen.HomeScreen.route) {
                    //提供一个全局变量  使用共同的ViewModel所有者
                    CompositionLocalProvider(
                        LocalViewModelStoreOwner provides viewModelStoreOwner
                    ) {
                        HomeScreen(navController)
                    }
                }
                composable(Screen.UserScreen.route) {
                    //提供一个全局变量  使用共同的ViewModel所有者
                    CompositionLocalProvider(
                        LocalViewModelStoreOwner provides viewModelStoreOwner
                    ) {
                        UserScreen(navController)
                    }
                }

            }

        }
    }
}
```

第二种方法,这个直接用的当前Activity的Context作为key了，但是麻烦，每个都要传

```kt
vm: XXXViewModel = viewModel(viewModelStoreOwner = LocalContext.current as ComponentActivity)
```


## 组件内使用协程

rememberCoroutineScope.launch

用于 网络请求\IO读写\弹窗\滚动控制\动画\触摸 所有可能阻塞主线程的情况

这个协程方法，不用导入kotlin协程包...

```kt
@Composable
fun DemoComponent() {
    val scope = rememberCoroutineScope()

    //scope.launch必须在类似LaunchedEffect 附带效应里启动
    LaunchedEffect(Unit){
        scope.launch {
            println("组件内启动一个协程")
        }
    }
}
```

## 生命周期

[案例](https://developer.android.google.cn/topic/libraries/architecture/lifecycle)

## 副作用

其实就是任何状态的变更，都会触发一些列的函数和vue生命周期钩子差不多的意思

**LaunchedEffect**

组件首次加载的时候调用的协程，类似mounted，一些http请求就可以放在这里执行咯

**DisposableEffect**

如果需要在退出的时候进行处理，如计时器取消，关闭链接等，可以在这个协程域中进行

**SideEffect**

不需要添加参数，且不需要处理关闭连接等操作时使用，compose函数每次执行都会调用该方法（加载完成后），相当于update，如果需要与外部共享状态State的时候使用

```kt
@Composable
fun DemoComponent() {
    var show by remember {
        mutableStateOf(true)
    }
    var size by remember {
        mutableStateOf(20.0f)
    }
    Column {
        Button(onClick = {
            show = !show
        }) {
            Text(text = if (show) "隐藏" else "显示")
        }
        Button(onClick = {
            size = (Math.random() * 50).toFloat()
        }) {
            Text(text = "修改大小，触发组件重组，调用SideEffect")
        }
        if (show) {
            DemoEffect(size)
        }
    }
}

@Composable
fun DemoEffect(size: Float) {
    var name by remember {
        mutableStateOf("张三")
    }
    val scope = rememberCoroutineScope()

    Button(onClick = {
        scope.launch {
            println("点击中的网络api请求")
        }
        //我在这里改变person为了SideEffect监听到组件更新
        name = "张" + (Math.random() * 999).toInt()
    }) {
        Text(text = name, fontSize = size.sp)
    }

    LaunchedEffect(Unit) {
        scope.launch {
            println("调用LaunchedEffect网络api请求")
        }
    }

    DisposableEffect(Unit) {
        var loop = true
        scope.launch {
            while (loop) {
                delay(2000)
            }
        }
        onDispose {
            loop = false
            println("调用onDispose")
        }
    }

    SideEffect {
        println("调用SideEffect")
    }
}
```

## 局部状态作用域

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

## 单值动画

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

## 尺寸大小变化动画

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

## 显示隐藏动画

直接用AnimatedVisibility 代替if即可

实验性api 都要加上@ExperimentalAnimationApi注解（最新版不需要了）

```kt
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
```

## 多值动画

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
        when (it) {
            0 -> MaterialTheme.colorScheme.primary
            1 -> MaterialTheme.colorScheme.secondary
            2 -> MaterialTheme.colorScheme.background
            else -> {
                MaterialTheme.colorScheme.background
            }
        }
    }
    Column {
        TabRow(
            selectedTabIndex = tabIndexState,
            containerColor = backgroundColor,
            contentColor = Color.Black,
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
                    tabPosition[it].left
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
                    tabPosition[it].right
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

## 无限循环动画

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

## 滑动删除动画

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

## 路由导航

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
    NavigationBar(
        containerColor = MaterialTheme.colorScheme.primary,
        contentColor = MaterialTheme.colorScheme.surface,
    ) {
        run {
            navigationItems.forEach { screen ->
                NavigationBarItem(
                    icon = { Icon(screen.icon!!, contentDescription = null) },
                    label = { Text(screen.label) },
                    colors = NavigationBarItemDefaults.colors(
                        //隐藏指示器颜色
                        indicatorColor = MaterialTheme.colorScheme.primary,
                        selectedIconColor = Color.White,
                        selectedTextColor = Color.White,
                        unselectedIconColor = Color.LightGray,
                        unselectedTextColor = Color.LightGray
                    ),
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

//自定义AppBar组件 标题是居中的 可以直接使用现成的CenterAlignedTopAppBar组件
@Composable
fun AppBar(
    navController: NavHostController,
    title: String = "",
    showBackIcon: Boolean = false,
    rowScope: @Composable (() -> Unit)? = null
) {
    CenterAlignedTopAppBar(
        colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
            containerColor = MaterialTheme.colorScheme.primary,
            titleContentColor = MaterialTheme.colorScheme.surface,
            navigationIconContentColor = MaterialTheme.colorScheme.surface,
            actionIconContentColor = MaterialTheme.colorScheme.surface,
            scrolledContainerColor = MaterialTheme.colorScheme.surface,
        ),
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
            Text(
                text = title,
                maxLines = 2,
                overflow = TextOverflow.Ellipsis
            )
        },
        //右侧操作栏
        actions = {
            if (rowScope != null) {
                rowScope()
            }
        }
    )
}

@ExperimentalMaterial3Api
@Composable
fun DemoComponent() {
    //导航控制器状态对象
    val navController = rememberNavController()
    Scaffold {
        Surface(modifier = Modifier.padding(it)) {
            //根据导航控制器，返回不同的导航
            NavHost(navController = navController, startDestination = Screen.HomeComponent.route) {
                composable(Screen.HomeComponent.route) { HomeComponent(navController) }
                composable(Screen.UserComponent.route) { UserComponent(navController) }
                composable(Screen.UserDetailComponent.route) { UserDetailComponent(navController) }
                composable(Screen.ArticleListComponent.route) { ArticleListComponent(navController) }
                composable(Screen.ArticleDetailComponent.route) {
                    ArticleDetailComponent(
                        navController
                    )
                }
            }
        }
    }
}

//主页  navController 参数保证每个屏幕都能使用导航
@ExperimentalMaterial3Api
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
        Surface(modifier = Modifier.padding(it)) {
            Column {
                Text("主页")
                Text("跳转到文章列表", modifier = Modifier.clickable {
                    //跳转到文章列表
                    navController.navigate(Screen.ArticleListComponent.route)
                })
            }
        }
    }
}

//个人中心
@ExperimentalMaterial3Api
@Composable
fun UserComponent(navController: NavHostController) {
    Scaffold(
        //底部导航
        bottomBar = {
            //创建导航栏列表
            BottomBar(navController)
        }
    ) {
        Surface(modifier = Modifier.padding(it)) {
            Column {
                Text("个人中心")
                Text("对象传参跳转", modifier = Modifier.clickable {
                    //跳转到用户详情
                    navController.navigate(
                        "User/UserDetail/" + Json.encodeToString(
                            Person(
                                "张三",
                                18
                            )
                        )
                    )
                })
            }
        }
    }
}

//用户详情
@ExperimentalMaterial3Api
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
        Surface(modifier = Modifier.padding(it)) {
            Column {
                Text("用户详情" + userObj?.name)
            }
        }
    }
}

//文章列表
@ExperimentalMaterial3Api
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
        Surface(modifier = Modifier.padding(it)) {
            Column {
                Text("文字列表")
                Text("跳转到文章详情", modifier = Modifier.clickable {
                    //跳转到文章列表
                    navController.navigate("Article/ArticleDetail/333")
                })
            }
        }
    }
}

//文章详情
@ExperimentalMaterial3Api
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
        Surface(modifier = Modifier.padding(it)) {
            Column {
                Text("文章详情id：$id")
                Text("跳到指定页面,并移除堆栈指定路由之上的所有路由(用的不多)", modifier = Modifier.clickable {
                    //跳转到首页
                    navController.navigate(Screen.HomeComponent.route) {
                        //移除文章列表上的所有路由，这里移除了文章详情，跳到到首页返回后只能返回到文章列表
                        popUpTo(Screen.ArticleListComponent.route)
                    }
                })
                Text(
                    "跳到指定页面,并移除堆栈指定路由之上的所有路由,包括指定路由(可用，相当于移除所有路由，只保留一个路由)",
                    modifier = Modifier.clickable {
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
}
```

**路由动画**

依赖

```
implementation 'com.google.accompanist:accompanist-navigation-animation:0.24.10-beta'
```

```kt
//下面这三个替换的类
import com.google.accompanist.navigation.animation.AnimatedNavHost
import com.google.accompanist.navigation.animation.composable
import com.google.accompanist.navigation.animation.rememberAnimatedNavController

@ExperimentalAnimationApi
@ExperimentalMaterial3Api
@Composable
fun DemoComponent() {
    //动画导航替换rememberAnimatedNavController
    val navController = rememberAnimatedNavController()
    Scaffold {
        Surface(modifier = Modifier.padding(it)) {
            //动画导航替换AnimatedNavHost
            AnimatedNavHost(navController = navController, startDestination = Screen.HomeComponent.route,
                //下面的是水平滑动的写法 这里是定义的全局的
                //当前页面进入动画
                enterTransition = {
                    //initialState targetState 是作用域的内置属性
                    when (initialState.destination.route + "To" + targetState.destination.route) {
                        //这里是某些不需要动画的导航处理
                        "HomeToUser" -> EnterTransition.None
                        "UserToHome" -> EnterTransition.None
                        //下面这个是全局的
                        else -> slideInHorizontally(
                            //目的偏移量
                            initialOffsetX = { fullWidth -> fullWidth },
                            //动画规则
                            animationSpec = tween(500)
                        )
                    }
                },
                //当前页面退出动画
                exitTransition = {
                    when (initialState.destination.route + "To" + targetState.destination.route) {
                        "HomeToUser" -> ExitTransition.None
                        "UserToHome" -> ExitTransition.None
                        else ->
                            slideOutHorizontally(
                                targetOffsetX = { fullWidth -> -fullWidth },
                                animationSpec = tween(500)
                            )
                    }
                },
                //目标页面进入
                popEnterTransition = {
                    when (initialState.destination.route + "To" + targetState.destination.route) {
                        "HomeToUser" -> EnterTransition.None
                        "UserToHome" -> EnterTransition.None
                        else ->
                            slideInHorizontally(
                                initialOffsetX = { fullWidth -> -fullWidth },
                                animationSpec = tween(500)
                            )
                    }
                },
                //目标页面退出
                popExitTransition = {
                    when (initialState.destination.route + "To" + targetState.destination.route) {
                        "HomeToUser" -> ExitTransition.None
                        "UserToHome" -> ExitTransition.None
                        else ->
                            slideOutHorizontally(
                                targetOffsetX = { fullWidth -> fullWidth },
                                animationSpec = tween(500)
                            )
                    }
                }) {
                //动画导航替换composable
                composable(Screen.HomeComponent.route) { HomeComponent(navController) }
                composable(Screen.UserComponent.route) { UserComponent(navController) }
                composable(Screen.UserDetailComponent.route) { UserDetailComponent(navController) }
                composable(Screen.ArticleListComponent.route) { ArticleListComponent(navController) }
                composable(Screen.ArticleDetailComponent.route) {
                    ArticleDetailComponent(
                        navController
                    )
                }
            }
        }
    }
}
```

## 状态栏

依赖

```
//用于设置状态栏
implementation 'com.google.accompanist:accompanist-systemuicontroller:0.24.10-beta'
//用于获取状态栏高度 沉浸式状态栏需要(废弃啊，已经支持了，不需要下面这个了)
implementation 'com.google.accompanist:accompanist-insets:0.23.1'
```

**设置导航栏颜色**

```kt
@ExperimentalMaterial3Api
@Composable
fun DemoComponent() {
    //系统UI设置控制器
    val systemUiController = rememberSystemUiController()
    //使用白色图标
    val useDarkIcons = false
    //背景颜色
    val backgroundColor = MaterialTheme.colorScheme.secondary
    //设置效果
    SideEffect {
        systemUiController.setSystemBarsColor(
            color = backgroundColor,
            darkIcons = useDarkIcons
        )
    }
    Scaffold(
        topBar = {
            CenterAlignedTopAppBar(
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
        Surface(modifier = Modifier.padding(it)) {
            LazyColumn(modifier = Modifier.fillMaxSize()) {
                items(100) {
                    Text("item $it")
                }
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
@ExperimentalMaterial3Api
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
    Scaffold(bottomBar = {
        NavigationBar(
            containerColor = MaterialTheme.colorScheme.primary,
            contentColor = MaterialTheme.colorScheme.surface,
        ) {
            NavigationBarItem(
                icon = { Icon(Icons.Filled.Home, contentDescription = null) },
                colors = NavigationBarItemDefaults.colors(
                    //隐藏指示器颜色
                    indicatorColor = MaterialTheme.colorScheme.primary,
                    selectedIconColor = Color.White,
                    selectedTextColor = Color.White,
                    unselectedIconColor = Color.LightGray,
                    unselectedTextColor = Color.LightGray
                ),
                label = { Text("底部导航栏") },
                selected = true,
                onClick = {})
        }
    }) { paddingValues ->
        val scrollState = rememberScrollState()
        //.padding(paddingValues) 即可解决底部挡住问题
        Column(
            modifier = Modifier
                .verticalScroll(scrollState)
                .fillMaxWidth()
                .padding(paddingValues)
        ) {
            Spacer(
                //设置状态栏高度
                modifier = Modifier
                    .windowInsetsTopHeight(WindowInsets.statusBars)
                    .fillMaxWidth()
                    .background(
                        //渐变色
                        brush = Brush.linearGradient(
                            colors = listOf(
                                MaterialTheme.colorScheme.primary,
                                MaterialTheme.colorScheme.secondary,
                            )
                        )
                    )
            )
            CenterAlignedTopAppBar(
                title = {
                    Text(text = "Page title", maxLines = 2)
                },
                navigationIcon = {
                    IconButton(onClick = {}) {
                        Text(text = "图标")
                    }
                },
                colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
                    containerColor = Color.Transparent,
                    titleContentColor = MaterialTheme.colorScheme.surface,
                    navigationIconContentColor = MaterialTheme.colorScheme.surface,
                    actionIconContentColor = MaterialTheme.colorScheme.surface,
                    scrolledContainerColor = MaterialTheme.colorScheme.surface,
                ),
                //渐变色
                modifier = Modifier
                    .background(
                        brush = Brush.linearGradient(
                            colors = listOf(
                                MaterialTheme.colorScheme.primary,
                                MaterialTheme.colorScheme.secondary,
                            )
                        )
                    ),
            )
            repeat(100) {
                Text("列表内容 $it")
            }
        }
    }
}
```

## 轮播图

依赖

```
implementation 'com.google.accompanist:accompanist-pager:0.24.10-beta'
```

```kt
@ExperimentalPagerApi
@Composable
fun DemoComponent() {
    val pagerState1 = rememberPagerState()
    val coroutineScope = rememberCoroutineScope()

    val list = listOf("1", "2", "3", "4")
    val pagerState2 = rememberPagerState()

    Column {
        HorizontalPager(count = list.size, state = pagerState1) { page ->
            Text(
                text = "Page:" + list[page],
                modifier = Modifier.fillMaxWidth()
            )
        }
        HorizontalPager(count = list.size, state = pagerState2) { page ->
            Text(
                text = "Page:" + list[page],
                modifier = Modifier.fillMaxWidth()
            )
        }
    }


    //计时器实现
    DisposableEffect(Unit) {
        //一个定时器类
        val timer = Timer()
        timer.schedule(object : TimerTask() {
            override fun run() {
                coroutineScope.launch {
                    pagerState1.animateScrollToPage(if (pagerState1.currentPage + 1 == list.size) 0 else pagerState1.currentPage + 1)
                }
            }
        }, 2000, 2000)
        onDispose {
            timer.cancel()
        }
    }

    //死循环实现
    DisposableEffect(Unit){
        var loop = true
        coroutineScope.launch {
            while (loop) {
                delay(2000)
                pagerState2.animateScrollToPage(if (pagerState2.currentPage + 1 == list.size) 0 else pagerState2.currentPage + 1)
            }
        }

        onDispose {
            loop = false
        }
    }
}
```

## 主题

主题色定义在ui.theme下的Color.kt文件下

onPrimary就是一个颜色，意思就是 背景色紫色，在紫色上显示就会是白色

onSurface 意思就是背景色白色，在白色上显示就会色黑色

```kt
@Composable
fun DemoComponent() {
    Column() {
        Surface(
            modifier = Modifier.fillMaxWidth(),
            color = MaterialTheme.colorScheme.primaryContainer //淡紫色
        ) {
            Column() {
                Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字")//文字黑色 背景淡紫色
                Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字", color = MaterialTheme.colorScheme.surface)//文字白色
                Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字", color = MaterialTheme.colorScheme.onPrimary)//文字白色
                Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字", color = MaterialTheme.colorScheme.onPrimaryContainer)//文字紫色
                Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字", color = MaterialTheme.colorScheme.onSurface)//文字黑色
                Button(onClick = { /*TODO*/ }) {
                    Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字") //文字白色 背景紫色
                }
            }
        }
        Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字")//文字黑色
        Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字", color = MaterialTheme.colorScheme.surface)//文字白色
        Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字", color = MaterialTheme.colorScheme.onPrimary)//文字白色
        Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字", color = MaterialTheme.colorScheme.onPrimaryContainer)//文字紫色
        Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字", color = MaterialTheme.colorScheme.onSurface)//文字黑色
        Button(onClick = { /*TODO*/ }) {
            Text(text = "示例文字示例文字示例文字示例文字示例文字示例文字示例文字") //文字白色 背景紫色
        }
    }
}
```

## 本地存储

[DataStore](https://developer.android.google.cn/topic/libraries/architecture/datastore#kotlin)

```
implementation "androidx.datastore:datastore-preferences:1.0.0"
```

```kt
class LocalStorage(private val context: Context) {
    //伴生对象 就是静态的意思
    companion object {
        //对Context类做扩展
        val Context.localStorage by preferencesDataStore("localStorage")

        //定义键值常量 比如我要存储一个歌曲列表
        val SONG_LIST = stringPreferencesKey("User")
    }

    //设置值
    suspend fun setLocalStorage(key: Preferences.Key<String>, str: String) =
        context.localStorage.edit {
            it[key] = str
        }

    //获取值
    suspend fun getLocalStorage(key: Preferences.Key<String>) = context.localStorage.data.map {
        it[key] ?: ""
    }.first()
}

@ExperimentalPagerApi
@ExperimentalAnimationApi
@Composable
fun DemoComponent() {

    val scope = rememberCoroutineScope()
    //获取上下文对象
    val context = LocalContext.current

    Column {
        Button(onClick = {
            val list = listOf("歌曲1", "歌曲2", "歌曲3")
            scope.launch {
                LocalStorage(context).setLocalStorage(
                    LocalStorage.SONG_LIST,
                    Json.encodeToString(list)
                )
            }
        }) {
            Text(text = "设置")
        }
        Button(onClick = {
            scope.launch {
                val list = Json.decodeFromString<List<String>>(
                    LocalStorage(context).getLocalStorage(
                        LocalStorage.SONG_LIST
                    )
                )
                println(list.toString())//[歌曲1, 歌曲2, 歌曲3]
            }
        }) {
            Text(text = "获取值")
        }
    }
}
```

## 网络

[Retrofit](https://square.github.io/retrofit/)

依赖

```
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
implementation 'com.squareup.retrofit2:converter-gson:2.9.0' // 必要依赖，解析json字符
```

权限
```
<uses-permission android:name="android.permission.INTERNET"/>
```

工具类HttpUtils.kt

```kt
//不需要双重锁机制，这样多简单
object HttpUtils {
    //初始化
    private val retrofit: Retrofit by lazy {
        Retrofit.Builder()
            .baseUrl("http://noval.o8o8o8.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }

    //创建实例方法
    fun <T> createHttp(clazz: Class<T>): T {
        return retrofit.create(clazz)
    }

    //协程扩展函数增加全局处理请求
    fun <T> request(
        getResponse: suspend CoroutineScope.() -> Response<T>,//方法执行作用域 加上suspend才是一个协程方法 CoroutineScope.就是协程作用域
        success: (T?) -> Unit, //成功响应
        error: (String) -> Unit = {} //失败响应 赋默认值 可以不传
    ) {
        //使用IO协程来进行网络通信
        CoroutineScope(Dispatchers.IO).launch {
            //设置超时时间5秒 自动取消协程 这个函数好啊
            withTimeoutOrNull(5000) {
                try {
                    val response = getResponse()
                    if (response.code != 20000) {
                        error(response.message)
                    } else {
                        success(response.data)
                    }
                } catch (e: RuntimeException) {
                    error("网络请求异常" + e.message)
                } finally {
                    this.cancel()
                }
            }
        }
    }
}
```

接口类BookApi.kt

```kt
//http接口请求，实际单独放一个文件
interface BookApi {

    //GET请求格式就是这样子
    @GET("Book/getBookListByBook")
    suspend fun getBookListByBook(
        @Query("book_name") book_name: String,
        @Query("page") page: Int,
        @Query("pageSize") pageSize: Int,
    ): Response<MutableList<Book>>

    //拿到实例
    companion object {
        val instance: BookApi by lazy {
            HttpUtils.createHttp(BookApi::class.java)
        }
    }
}
```

数据类，通过插件JsonToDataClass kotlin生成  拆分生成，不要合并

Response.kt

```kt
data class Response<T>(
    @SerializedName("code")
    val code: Int,
    @SerializedName("data")
    val `data`: T,
    @SerializedName("message")
    val message: String
)
```

Book.kt

```kt
data class Book(
    @SerializedName("author")
    val author: String,
    @SerializedName("book_name")
    val bookName: String,
    @SerializedName("category_name")
    val categoryName: String,
    @SerializedName("chapter_count")
    val chapterCount: Int,
    @SerializedName("chapter_detail")
    val chapterDetail: String,
    @SerializedName("chapter_detail_count")
    val chapterDetailCount: Int,
    @SerializedName("id")
    val id: Int,
    @SerializedName("image_url")
    val imageUrl: String,
    @SerializedName("update_time")
    val updateTime: Int
)
```

使用

```kt
@Composable
fun DemoComponent() {
    val data = remember {
        mutableStateListOf<Book>()
    }

    LaunchedEffect(Unit) {
        //发送请求
        HttpUtils.request(
            getResponse = {
                BookApi.instance.getBookListByBook("1", 1, 10)
            },
            success = {
                //回调数据
                it?.let {
                    data.addAll(it)
                }
            },
            error = {
                println(it)
            },
        )
    }

    LazyColumn {
        items(data.size) { index ->
            Text(data[index].bookName)
        }
    }
}
```


## 全局Context和Toast

建一个类继承Application以获取全局Context

BaseApplication.kt

```kt
class BaseApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        context = this
    }

    companion object {
        lateinit var context: BaseApplication
            private set
    }
}
```

然后在AndroidManifest.xml里添加

```
...
<application
        android:name="com.xxx.xxx.BaseApplication"
        ....
...
```

**封装Toast**

目前会报警告，还未解决

需要kotlin协程库

```
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.6.2"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.2"
```

```kt
//任意位置显示Toast弹窗
fun showToast(msg: String) {
    MainScope().launch {
        var myLooper = Looper.myLooper()
        if (myLooper == null) {
            Looper.prepare()//多加的两行是为了在协程中调用，不然无法使用
            myLooper = Looper.myLooper()
        }
        val toast = Toast.makeText(BaseApplication.context, msg, Toast.LENGTH_LONG)
        toast.setGravity(Gravity.CENTER, 0, 0)
        toast.show()
        if (myLooper != null) {
            Looper.loop()//以及这行
            myLooper.quit()
        }
    }
}
```

## 下拉刷新和上拉加载

下拉刷新的依赖

```
implementation "com.google.accompanist:accompanist-swiperefresh:0.24.10-beta"
```

使用

实际这些都放到ViewModel里去控制，这里只是演示

```kt
//有状态组件
@Composable
fun DemoComponent() {
    //数据
    val data = remember {
        mutableStateListOf<Book>()
    }

    //是否正在刷新
    var isRefreshing by remember {
        mutableStateOf(false)
    }

    //当前页
    var page by remember {
        mutableStateOf(0)
    }

    //加载状态
    val loadState = rememberLazyListState(1, 2)
    //加载状态派生状态 实际可以把这个写到LazyState状态的扩展函数中，这样就不用每次写下面这二段了，这里只是记录一下写在一起的写法
    val onReachBottom by remember {
        derivedStateOf {
            //到底了
            loadState.firstVisibleItemIndex + loadState.layoutInfo.visibleItemsInfo.size == loadState.layoutInfo.totalItemsCount
        }
    }
    //监听这个是否到底部的状态
    LaunchedEffect(onReachBottom) {
        if(onReachBottom){
            //下一页
            page += 1
            //发送请求
            getBookList(data, page) {}
        }
    }

    //抽成无状态组件
    StateLessWidget(isRefreshing, data, loadState) {
        //这里是刷新完的回调函数作用域
        isRefreshing = true
        page = 1
        getBookList(data, page) {
            isRefreshing = false
        }
    }
}

@Composable
private fun StateLessWidget(
    isRefreshing: Boolean,
    data: SnapshotStateList<Book>,
    loadState: LazyListState,
    callback: () -> Unit
) {
    //下拉刷新组件
    SwipeRefresh(
        state = rememberSwipeRefreshState(isRefreshing),
        onRefresh = {
            callback()
        },
        //自定义指示器
        indicator = { state, trigger ->
            SwipeRefreshIndicator(
                state = state,
                refreshTriggerDistance = trigger,
                scale = true,
                backgroundColor = MaterialTheme.colorScheme.primary,
                contentColor = MaterialTheme.colorScheme.surface,
                shape = CutCornerShape(4.dp)
            )
        }
    ) {
        LazyColumn(
            state = loadState,
            modifier = Modifier.fillMaxWidth()
        ) {
            items(data.size) { index ->
                Text(data[index].bookName, modifier = Modifier.height(150.dp))
            }
        }
    }
}

//获取列表数据
private fun getBookList(data: SnapshotStateList<Book>, page: Int, callback: () -> Unit) {
    HttpUtils.request(
        getResponse = {
            //模拟一下延时
            delay(1000)
            BookApi.instance.getBookListByBook("1", page, 10)
        },
        success = {
            if (page == 1) {
                //如果是首次加载，则清空数据
                data.clear()
            }
            //解构
            it?.let {
                data.addAll(it)
            }
            println(it)
            //回调表示请求完毕
            callback()
        },
        error = {
            println(it)
            //回调表示请求完毕
            callback()
        },
    )
}

@Composable
@Preview
fun DefaultPreview() {
    DemoComponent()
}
```

**封装成扩展函数**

```kt
@Composable
fun LazyListState.OnReachBottom(callback: () -> Unit) {
    //加载状态派生状态 实际可以把这个写到LazyState状态的扩展函数中，这样就不用每次写下面这二段了，这里只是记录一下写在一起的写法
    val onReachBottom by remember {
        derivedStateOf {
            //初始如果为0，则直接返回true
            if (this.layoutInfo.totalItemsCount == 0) {
                return@derivedStateOf true
            }
            //到底了
            this.firstVisibleItemIndex + this.layoutInfo.visibleItemsInfo.size == this.layoutInfo.totalItemsCount
        }
    }
    //监听这个是否到底部的状态
    LaunchedEffect(onReachBottom) {
        println(onReachBottom)
        if (onReachBottom) {
            callback()
        }
    }
}
```

使用

```kt
loadState.OnReachBottom {
    //下一页
    page += 1
    //发送请求
    getBookList(data, page) {}
}
```

## 折叠AppBar

这里直接用现成的，不要加太多特效，会卡的

[依赖](https://github.com/onebone/compose-collapsing-toolbar)

```
implementation "me.onebone:toolbar-compose:2.3.3"
```

使用

```kt
@Composable
fun DemoComponent() {
    val state = rememberCollapsingToolbarScaffoldState()
    CollapsingToolbarScaffold(
        modifier = Modifier.fillMaxSize(),
        scrollStrategy = ScrollStrategy.ExitUntilCollapsed,
        state = state,
        enabled = true,
        toolbarModifier = Modifier.background(MaterialTheme.colorScheme.primary),
        toolbar = {
            Box(contentAlignment = Alignment.TopStart) {
                Image(
                    painter = painterResource(id = R.drawable.test),
                    modifier = Modifier
                        .parallax(0.5f)
                        .height(300.dp)
                        .graphicsLayer {
                            //动态改变透明度
                            alpha = state.toolbarState.progress
                        },
                    contentScale = ContentScale.Crop,
                    contentDescription = null
                )
            }
            CenterAlignedTopAppBar(
                title = {
                    Text(text = "Page title", maxLines = 2)
                },
                navigationIcon = {
                    IconButton(onClick = {}) {
                        Text(text = "图标")
                    }
                },
                colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
                    containerColor = Color.Transparent,
                    titleContentColor = MaterialTheme.colorScheme.surface,
                    navigationIconContentColor = MaterialTheme.colorScheme.surface,
                    actionIconContentColor = MaterialTheme.colorScheme.surface,
                    scrolledContainerColor = MaterialTheme.colorScheme.surface,
                )
            )
        }
    ) {
        LazyColumn(Modifier.fillMaxWidth()) {
            items(100) {
                Text(it.toString())
            }
        }
    }
}
```

## placeholder

占位符 实验性修饰符

visible 绑定占位符显示状态

依赖

```
implementation "com.google.accompanist:accompanist-placeholder-material:0.24.10-beta"
```

```kt
@OptIn(ExperimentalFoundationApi::class)
@Composable
fun DemoComponent() {
    val list = remember {
        mutableStateListOf<String>()
    }
    var isLoadFinish by remember {
        mutableStateOf(false)
    }

    val scope = rememberCoroutineScope()
    LaunchedEffect(Unit) {
        scope.launch {
            delay(2000)
            for (i in 1..5) {
                list.add("网络请求内容$i")
            }
            isLoadFinish = true
        }
    }

    LazyColumn {
        val count = if (isLoadFinish) list.size else 5
        items(count) { index ->
            Text(
                text = if (isLoadFinish) list[index] else "加载中",
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(16.dp)
                    .placeholder(
                        visible = list.size == 0,
                        highlight = PlaceholderHighlight.shimmer()
                    )
            )
        }
    }
}
```

## 日期选择插件

[第三方插件](https://github.com/boguszpawlowski/ComposeCalendar)

```
//日期选择插件
implementation "io.github.boguszpawlowski.composecalendar:composecalendar:0.5.1"
```

基本使用,实际使用放到dialog中显示即可

```kt
@Composable
fun MainScreen() {
    val calendarState = rememberSelectableCalendarState()

    Column(
        Modifier.verticalScroll(rememberScrollState())
    ) {
        SelectableCalendar(calendarState = calendarState)
        Button(onClick = {
            println(calendarState.selectionState.selection[0])
        }) {
            Text(text = "确定")
        }
    }
}
```

## 依赖总结

```
//compose版本
compose_version = '1.2.0-beta03'
//kotlin版本
id 'org.jetbrains.kotlin.android' version '1.6.21' apply false
//material3
implementation 'androidx.compose.material3:material3:1.0.0-alpha11'
//图标
implementation "androidx.compose.material:material-icons-extended:$compose_version"
//图片加载
implementation "io.coil-kt:coil-compose:2.1.0"
//ViewModel状态管理
implementation 'androidx.lifecycle:lifecycle-runtime-ktx:2.4.1'
implementation 'androidx.lifecycle:lifecycle-viewmodel-compose:2.4.1'
implementation 'androidx.activity:activity-compose:1.4.0'
//路由导航
implementation 'androidx.navigation:navigation-compose:2.4.0'
//json格式化 project下的build     
id 'org.jetbrains.kotlin.plugin.serialization' version '1.6.21' ///json格式化 app下build 
plugins{id 'org.jetbrains.kotlin.plugin.serialization'}
implementation "org.jetbrains.kotlinx:kotlinx-serialization-json:1.3.3"
//路由动画
implementation 'com.google.accompanist:accompanist-navigation-animation:0.24.10-beta'
//状态栏
implementation 'com.google.accompanist:accompanist-systemuicontroller:0.24.10-beta'
//轮播图ViewPager
implementation 'com.google.accompanist:accompanist-pager:0.24.10-beta'
//本地存储
implementation "androidx.datastore:datastore-preferences:1.0.0"
//网络
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
//解析json字符串
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
//下拉刷新
implementation "com.google.accompanist:accompanist-swiperefresh:0.24.10-beta"
//折叠AppBar
implementation "me.onebone:toolbar-compose:2.3.3"
//占位符placeholder
implementation "com.google.accompanist:accompanist-placeholder-material:0.24.10-beta"
//日期选择插件
implementation "io.github.boguszpawlowski.composecalendar:composecalendar:0.5.1"
```