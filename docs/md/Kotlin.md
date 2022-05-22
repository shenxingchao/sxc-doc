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

### 解构赋值

```kt
package com.org.kotlin

fun main() {
    //初始化数组方法
    val list =  intArrayOf(1,2,3)
    //解构赋值
    val (a,b,c) = list
    println("$a $b $c");//1 2 3
}
```

### 循环

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
    for (item in list){
        println(item) //1 2 3
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

### 函数

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

### 特殊类

**Nothing**

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