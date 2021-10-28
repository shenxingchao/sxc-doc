# Python
## 文档地址
[地址](https://docs.python.org/zh-cn/3/)
[在线调试地址](http://pythontutor.com/)

## 环境搭建
### windows10 python3.9.4环境搭建
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

1. Python官网下载安装文件 [下载地址](https://www.python.org/downloads/windows/)
2. 选择自定义安装 选择好安装路径 不能有中文
3. 能安装的插件全部勾上，选中Add to Path添加到环境变量
4. win+r 打开cmd 输入python -V  命令行输出Python 3.9.4 解释器安装成功
5. vscode python 用户片段
```json
{	
    "function": {        
		"prefix": "function",        
		"body": [            
			"\"\"\"",            
			"@description ",                    
			"@param ",             
			"@return ",          
			"\"\"\"",                 
		],	
	},
	"python": {        
		"prefix": "python",        
		"body": [            
			"def main():",            
			"if __name__ == \"__main__\":",
			"    main()",           
		],	
	}
}
```
6. vscide python 设置
```json
{
    "python.pythonPath": "D:\\sxc\\python3\\python.exe",//python.exe绝对路径
    "python.formatting.provider": "black",//python格式化工具
    "editor.formatOnSave": true, //保存时自动格式化
    "python.formatting.blackArgs": ["--line-length", "120"],
    "less.compile": {
        "compress": true, // true => remove surplus whitespace
        "sourceMap": false, // true => generate source maps (.css.map files)
        "out": true, // false => DON'T output .css files (overridable per-file, see below)
        "outExt": "qss"//less转qss样式 用于pyqt，pyside
    }
}
``` 

## python100天入门笔记

### 第一个python脚本
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

编写如下代码，保存为hello.py
```py
print("hello,world!")
```
命令行运行 
```powershell
python hello.py
```
或 
```powershell
py hello.py
```
输出 hello,world！

### 注释
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

#### 行注释 用#
```py
#我是行注释
```
#### 块注释 用""" """或''' '''包含起来
```py
"""
print("双引号块注释")
"""
'''
print("单引号块注释")
'''
```

### 变量定义
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

1. 整型
```py
a = 123
b = 345
print(a, b)
```
2. 浮点数
```py
a = 3.1415926
print(a)
```
3. 字符串
```py
str = "hello world!"
print(str)
```
4. 布尔值
```py
bool_true = True
bool_false = False
print(bool_true, bool_false)
```
!> 这里就出错了，Boolean值在python里是大写的True和False 否则会报变量未定义的错误

5. 一行定义多个
```py
a, b = 123, 345
print(a, b)
```
6. 使用技巧交换变量
```py
a, b = 123, 345
a, b = b, a
print(a, b)
```
7. 类型转换
!> python没有隐式类型转换，所以不同类型之间的运算必须要转换之后再运算
```py
a = int("3")
b = 4
print(a + b)
```
[类型转换](#类型转换)

### 运算符
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

| 运算符                                                         | 描述                           |
| -------------------------------------------------------------- | ------------------------------ |
| `[]` `[:]`                                                     | 下标，切片                     |
| `**`                                                           | 指数                           |
| `~` `+` `-`                                                    | 按位取反, 正负号               |
| `*` `/` `%` `//`                                               | 乘，除，模，整除               |
| `+` `-`                                                        | 加，减                         |
| `>>` `<<`                                                      | 右移，左移                     |
| `&`                                                            | 按位与                         |
| `^` `\|`                                                       | 按位异或，按位或               |
| `<=` `<` `>` `>=`                                              | 小于等于，小于，大于，大于等于 |
| `==` `!=`                                                      | 等于，不等于                   |
| `is`  `is not`                                                 | 身份运算符                     |
| `in` `not in`                                                  | 成员运算符                     |
| `not` `or` `and`                                               | 逻辑运算符                     |
| `=` `+=` `-=` `*=` `/=` `%=` `//=` `**=` `&=` `^=` `>>=` `<<=` | （复合）赋值运算符             |

| &操作
```py
a = True
b = False
print(a | b, a & b) # 输出True False
```
in操作
```py
str = "hello world"
if "llo" in str:
    print(True)
```

### 代码折行
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

一行代码太长？用\来让代码折行
```py
a = True
b = True
if a & \
b:
    print("代码折行用\ ")
```

### 条件语句
<p align="left" style="color:#777777;">发布日期：2021-04-16</p>

```py
a = True
b = False
if a:
    print("a")
elif not b:
    print("因为a满足了不会输出,a满足了就跳出这个if elif else代码块了")
else:
    print("因为a满足了不会输出,else不会输出的")
```
?> 非! 在python用not表示

###  循环
<p align="left" style="color:#777777;">发布日期：2021-04-16</p>

[这里](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#for-statements)
1. for循环计算0-100的和,循环100次用range
```py
sum = 0
for i in range(1, 101):#也可以写做for i in range(101):
    sum += i
print(sum)
```
2. for循环输出列表
```py
arr = [1, 2, 3, 4]
for i in arr:
    print(i)
```
3. while循环最好是用在不确定循环次数的时,也可计算0-100的和
```py
sum = 0
i = 1
while True:
    if i <= 100:
        sum += i
        i += 1
    else:
        break
print(sum)
```
4. 循环斐波那契数列

第一种可测试语言速度
```py
import time

a = 0  # 数1
b = 0  # 数2
c = 0  # 数3
arr = []  # list
t1 = time.time()
print("开始计时", t1)
while len(arr) < 42:
    if c == 0:
        a = 1
        arr.append(1)
    elif c == 1:
        b = 1
        arr.append(1)
    elif (a + b) == c:
        # 如果前2个数之后等于第三个数
        a = b
        b = c
        arr.append(c)
    c += 1
print(arr)
t2 = time.time()
print("计时结束", t2)
print("耗时", t2 - t1)
```

第二种方法
```py
def fbnq(n):
    """
    @description 取斐波那契数列的前n项
    @param n 项数
    @return arr 存储数据的列表
    """
    arr = [1, 1]
    if n == 1 or n == 2:
        return 1
    else:
        for i in range(2, n):
            s = arr[i - 2] + arr[i - 1]  # i-2项 和 i-1项的和等于当前项，再把当前项记录下来
            arr.append(s)
        return arr


def main():
    arr = fbnq(100)
    print(arr)


if __name__ == "__main__":
    main()
```

### 函数
<p align="left" style="color:#777777;">发布日期：2021-04-19</p>

#### 最基础的函数定义 参数和返回值
```py
def sum(a, b):
    """
    @description 求输入的两个数之和
    @param a 数1
    @param b 数2
    @return int 两个数之和
    """
    return a + b

def main():
    a = int(input("请输入第一个数："))
    b = int(input("请输入第二个数："))
    print(sum(a, b))
if __name__ == "__main__":
    main()
```

#### 可选参数
不确定参数个数的情况下可以使用
```py
def sum(*args):
    """
    @description 求输入的两个数之和
    @param a 数1
    @param b 数2
    @return int 两个数之和
    """
    sum = 0
    for item in args:
        sum += item
    return sum


def main():
    a = int(input("请输入第一个数："))
    b = int(input("请输入第二个数："))
    c = int(input("请输入第三个数："))
    print(sum(a, b))
    print(sum(a, b, c))


if __name__ == "__main__":
    main()
```

理解位置参数*args 和**kwargs  kw=>keywords 这两个的参数名字可以随便起,不固定
```py
def function(arg, *args, **kwargs):
    """
    @description 定义一个函数有三个参数，function(正常参数，将7,8,9打包成元组给函数使用,将a=1,b=2,c=3打包成字典给函数使用)
    @param arg 正常参数
    @param *args 位置参数，不确定个数的参数打包成的元组
    @param **kwargs 关键字参数，不确定个数的参数打包成的字典
    @return
    """
    print(arg, args, kwargs)  # 输出6 (7, 8, 9) {'a': 1, 'b': 2, 'c': 3}


def main():
    # 按顺序传入参数
    function(6, 7, 8, 9, a=1, b=2, c=3)


if __name__ == "__main__":
    main()
```

#### 正确的main函数书写方法
```py
def main():
    # Todo: Add your code here
    pass


# python里当前执行的模块名字是__main__ 如果当前文件被导入到其他文件时，下面代码不会执行
if __name__ == "__main__":
    main()
```

#### 装饰器函数（高级特性）
```py
# 导入装饰器模块
from functools import wraps


def a_new_decorator_with_parma(name):
    """
    @description 最外面的包裹是带参数的装饰器函数，不需要参数可以去掉
    @param
    @return
    """
    print(name)

    def a_new_decorator(fn):
        """
        @description 创建装饰器函数
        @param
        @return
        """
        param_default = fn.__defaults__  # 这个是原函数参数的默认值元组,如果需要从这里拿,可以拿到 (你好我是原函数参数,)
        # wraps接受一个函数来进行装饰 这可以让我们在装饰器里面访问在装饰之前的函数的属性。
        @wraps(fn)
        def wrapTheFunction(*args, **kwargs):
            """
            @description 装饰函数，实际在这里进行装饰（进行别的操作，调用原函数并返回，最里层返不返回随便你）
            @param
            @return
            """
            print("装饰后输出")  # 输出装饰后输出
            print(*args, **kwargs)  # 没有输出 原函数的默认值不会被传过来
            return fn(*args, **kwargs)  # 输出我是原函数输出

        # 把装饰后的函数返回回去，它还是一个函数
        return wrapTheFunction

    return a_new_decorator


@a_new_decorator_with_parma("你好我是带参数的装饰器函数的参数")
def a_function_requiring_decoration(name="你好我是原函数参数"):
    print("我是原函数输出")
    print(f"原函数参数输出:{name}")


# 调用装饰后的函数
a_function_requiring_decoration()
# 输出装饰后函数的名称
print(a_function_requiring_decoration.__name__)  # a_function_requiring_decoration
```

#### 类装饰器
```py
from functools import wraps
from time import time


class Record:
    """通过定义类的方式定义装饰器"""

    def __init__(self, name):
        print(name)  # 输出你好我是带参数的类装饰器的参数

    def __call__(self, fn):
        """
        __call__允许类用
        record = Record()
        record(fn)  类的实例去调用 相当于record.__call__(fn)
        """

        @wraps(fn)
        def wrapper(*args, **kwargs):
            print("装饰后输出")  # 输出装饰后输出
            return fn(*args, **kwargs)  # 输出我是原函数输出 原函数参数输出:你好我是原函数参数

        return wrapper


@Record("你好我是带参数的类装饰器的参数")  # 相当于 record = Record("你好我是带参数的类装饰器的参数") ; record(fn)
def a_function_requiring_decoration(name="你好我是原函数参数"):
    print("我是原函数输出")
    print(f"原函数参数输出:{name}")


# 调用装饰后的函数
a_function_requiring_decoration()
```

### 模块
<p align="left" style="color:#777777;">发布日期：2021-04-20</p>

假如有一个函数库fn.py
```py
def sum(*args):
    """
    @description 求输入的两个数之和
    @param a 数1
    @param b 数2
    @return int 两个数之和"""
    sum = 0
    for item in args:
        sum += item
    return sum


# 下面是演示导入的模块代码不会被执行的方法
print("我会被执行")
# python里当前执行的模块名字是__main__
if __name__ == "__main__":
    print("我不会被执行")
```

导入方法
```py
from fn import sum  # 从fn模块导入sum函数
import fn  # 直接导入整个模块 也可以重命名模块 import fn as f


def main():
    a = int(input("请输入第一个数："))
    b = int(input("请输入第二个数："))
    c = int(input("请输入第三个数："))
    print(sum(a, b))
    print(sum(a, b, c))
    print(fn.sum(a, c))


if __name__ == "__main__":
    main()
```

### 字符串
<p align="left" style="color:#777777;">发布日期：2021-04-21</p>

#### 字符串切片操作
!> 切片操作索引都是从0开始的,切片操作基本表达式：object[start_index : end_index : step]  
step的正负号决定方向,start_index开始位置,end_index结束位置

```py
str = "hello world"

print(str[1])  # 输出e
# 截取指定区间的字符串 不包含最后一个 从前面开始
print(str[0:2])  # 输出he
# 指定字符位置后面的字符串 包含第一个
print(str[1:])  # 输出ello world
# 指定字符位置之前的字符串 不包含自身
print(str[:10])  # 输出hello worl
# 截取偶数索引位的数
print(str[0::2])  # 输出hlowrd
# 截取奇数索引位的数
print(str[1::2])  # 输出el ol
# 截取最后2位
print(str[-2:])  # 输出ld
# 去除最后2位
print(str[:-2])  # 输出hello wor
# 字符串反向输出
print(str[::-1])  # 输出dlrow olleh
# 截取指定区间 从倒数第三个开始
print(str[-3::1])  # 输出rld
```


#### 字符串函数操作
```py
str = "hello world"
# 通过内置函数len计算字符串的长度
print(len(str))  # 11
# 获得字符串首字母大写的拷贝
print(str.capitalize())  # Hello world
# 获得字符串每个单词首字母大写的拷贝
print(str.title())  # Hello World
# 获得字符串变大写后的拷贝
print(str.upper())  # HELLO WORLD
# 从字符串中查找子串所在位置
print(str.find("or"))  # 7
print(str.find("shit"))  # 没找到返回-1
# 查找指定字符串最后出现的位置
print(str.rfind("o"))  # 7
# 查找指定字符串第一次出现的位置
print(str.find("o"))  # 4
# 检查字符串是否以指定的字符串开头
print(str.startswith("He"))  # False
print(str.startswith("hel"))  # True
# 检查字符串是否以指定的字符串结尾
print(str.endswith("d"))  # True
# 将字符串以指定的宽度居中并在两侧填充指定的字符
print(str.center(50, "*"))
# 将字符串以指定的宽度靠右放置左侧填充指定的字符
print(str.rjust(50, "*"))
str = "abc123456"
# 检查字符串是否由数字构成
print(str.isdigit())  # False
# 检查字符串是否以字母构成
print(str.isalpha())  # False
# 检查字符串是否以数字和字母构成
print(str.isalnum())  # True
# 获得字符串修剪左右两侧空格之后的拷贝
str = " hello world "
print(str.strip())  # 输出hello world
```

### 列表

#### 列表基本操作
<p align="left" style="color:#777777;">发布日期：2021-04-22</p>

```py
# 定义一个列表
arr = [1, 2, 3]
arr = list(range(3))
# 打印整个列表
print(arr)  # [1,2,3]
# 打印列表第一个索引的值
print(arr[0])  # 输出1
# 列表元素重复次数输出
print(arr * 3)  # 输出[1,2,3,1,2,3,1,2,3]
# 列表长度
print(len(arr))  # 输出3
# for循环通过下标遍历列表
for i in range(len(arr)):
    print(arr[i])  # 输出 1 2 3
# for循环直接遍历列表
for item in arr:
    print(item)  # 输出 1 2 3
# 通过enumerate处理后遍历输出
for index, item in enumerate(arr):
    print(item, index)  # 输出1 0 2 1 3 2
```
!> 一般用emunerate遍历就可以了

#### 列表操作方法
```py
# 定义一个列表
arr = [1, 2, 3]
# 向列表末尾添加一个元素
arr.append(4)
print(arr)  # 输出[1,2,3,4]
# 向列表指定索引位置插入一个元素
arr.insert(0, 0)  # insert(索引位置，插入的数)
print(arr)  # 输出[0,1,2,3,4]
# 判断元素是否在列表中，在就删除
if (0 and 4) in arr:
    arr.remove(0)
    arr.remove(4)
print(arr)  # 输出[1,2,3]
# 从指定位置删除一个元素
arr.pop(0)  # 删除索引位置为0的元素
print(arr)  # 输出[2,3]
# 清空列表1
arr = [] #重新初始化
print(arr)  # 输出[]
# 清空列表2
arr = [1, 2, 3]
arr.clear()  # clear()会清空引用内存占用空间，导致列表不能引用
print(arr)  # 输出[]
# 清空列表3
arr = [1, 2, 3]
arr *= 0 #不明觉厉
print(arr)
# 清空列表4
arr = [1, 2, 3]
del arr[:]  # del可以清楚范内的列表元素 如果不给定索引值，就会清除所有元素 这种方法和clear一样
print(arr)
# 列表排序
arr = [1, 3, 2]
print(arr.sort(reverse=True))  # 它会直接修改原列表 如果不需要原列表性能更好 返回none 输出 none，reverse参数从大到小
print(arr)  # 输出[3,2,1]
arr = [1, 3, 2]
print(sorted(arr, reverse=True))  # sorted返回排序后的列表，不会改变原列表， 输出[3,2,1]
arr = ["1", "12", "123"]
print(sorted(arr, key=len, reverse=True))  # key =len表示根据列表元素的长度进行排序,reverse参数从大到小

```
!> clear()会清空内存占用空间，导致列表不能复用,所以用重新初始化即可 = []
```py
arr = [1, 2, 3]
arr2 = arr
arr.clear()
print(arr2)  # 输出[] arr2的引用内存空间没被清空

arr = [1, 2, 3]
arr2 = arr
arr = []
print(arr2)  # 输出[1, 2, 3] arr2的引用内存空间没被清空
```

#### 列表切片和拷贝
```py
arr = [1, 2, 3]
# 输出指定范围内的元素 不包含最后一个，和字符串切片相同
print(arr[0:1])  # 输出[1]
# 列表连接用+
print(arr + [4])  # 输出[1,2,3,4]
# 列表倒转
print(arr[::-1])  # 输出[3,2,1]
# 通过切片复制列表，实现第一层深拷贝
copy = arr[:]
print(copy)  # 输出[1,2,3]
# 通过直接赋值复制，2个列表的内存地址是一样的，改变一个都会影响另一个
copy = arr
print(copy)  # 输出[1,2,3]
# 通过copy()，实现第一层深拷贝
copy = arr.copy()
print(copy)  # 输出[1,2,3]
# 通过列表循环生成，实现第一层深拷贝
copy = []
for index, item in enumerate(arr):
    copy.append(item)
print(copy)  # 输出[1,2,3]

# 通过deepcopy()深拷贝列表
import copy

copys = copy.deepcopy(arr)
print(copys)
```

#### 浅拷贝和深拷贝实战
```py
import copy


def main():
    """ 浅拷贝 """
    # 创建一个二维列表
    list = [[1, 2, 3], [4, 5, 6]]
    list_copy = list.copy()  # 也可以 list_copy = copy.copy(list)
    # 分别改变第一层和第二层的内容
    list_copy[0] = [11, 22, 33]
    list_copy[1][0] = [44]
    # 输出拷贝后的列表
    print(list_copy)  # 输出[[11, 22, 33], [[44], 5, 6]]
    # 输出原列表，发现原列表的深层列表值被改变，浅层的未发生变化
    print(list)  # 输出[[1, 2, 3], [[44], 5, 6]]

    """ 深拷贝 """
    # 创建一个二维列表
    list = [[1, 2, 3], [4, 5, 6]]
    list_copy = copy.deepcopy(list)  # 只能这种方式
    # 分别改变第一层和第二层的内容
    list_copy[0] = [11, 22, 33]
    list_copy[1][0] = [44]
    # 输出拷贝后的列表
    print(list_copy)  # 输出[[11, 22, 33], [[44], 5, 6]]
    # 输出原列表，发现原列表的深层和浅层的均未发生变化，实现了深拷贝
    print(list)  # 输出[[1, 2, 3], [4, 5, 6]]


if __name__ == "__main__":
    main()
```

### 元组
<p align="left" style="color:#777777;">发布日期：2021-04-22</p>

#### 基本操作
```py
tup = ("hello", 1, True)
# 打印元组
print(tup)  # 输出('hello', 1, True)
# 打印元组第一个元素
print(tup[0])  # 输出 hello
# 遍历元组
for item in tup:
    print(item)  # 输出hello 1 True
# 元组元素不能重新赋值可以重新赋值整个元祖
tup = ("hello tuple", 1, True)
print(tup)  # 输出('hello tuple', 1, True)
# 元组转换成列表
arr = list(tup)
print(arr)  # 输出['hello tuple', 1, True]
# 列表转回元组
tup = tuple(arr)
print(tup)  # 输出('hello tuple', 1, True)
```

### 集合
<p align="left" style="color:#777777;">发布日期：2021-04-22</p>

#### 集合创建
```py
sets = {1, 2, 3, 1, 2, 3}
# 利用集合去重
print(sets)  # 输出{1,2,3}
# 利用set构造器创建
sets = set((1, 2, 3, 1, 2))
print(sets)  # 输出{1,2,3}
# python推导式语法
sets = {(i + 1) for i in range(0, 3)}
print(sets)  # 输出{1,2,3}
```
!>推导式语法，{返回的实际数据处理 + for循环 }

#### 基本操作
```py
sets = {1, 2, 3}
# 集合末尾添加一项
sets.add(4)
print(sets)  # 输出{1, 2, 3, 4}
# 集合指定位置插入一项
sets.update([0, 1])
print(sets)  # 输出{0, 1, 2, 3, 4}
# 删除指定元素
sets.remove(0)
sets.remove(4)
print(sets)  # 输出{1, 2, 3}
# 删除第一个相当于队列出去一个
sets.pop()
print(sets)  # 输出{2, 3}

# 交集，差集，并集运算
set1, set2 = {1, 2, 3}, {2, 3, 4}
# 交集
print(set1 & set2)  # 输出{2, 3}
# 并集
print(set1 | set2)  # 输出{1, 2, 3, 4}
# 差集
print(set1 - set2)  # 输出{1}
# 两者都没有的
print((set1 - set2) | (set2 - set1))  # 输出{1,4}

# 判断是否是子集
set1, set2 = {1, 2, 3}, {1, 2, 3, 4}
print(set1 <= set2)  # 输出True
```

### 字典
<p align="left" style="color:#777777;">发布日期：2021-04-22</p>

```py
# 字典和js里的对象无异 比较常用吧 由键值对组成
dictionary = {"age": 18, "sex": "男"}
print(dictionary)  # 输出 {'age': 18, 'sex': '男'}
# 使用构造器创建1
dictionary = dict(age=18, sex="男")
print(dictionary)  # 输出 {'age': 18, 'sex': '男'}
# 使用构造器创建2
dictionary = dict([("age", 18), ("sex", "男")])
print(dictionary)  # 输出 {'age': 18, 'sex': '男'}
# 取值
print(dictionary["age"])  # 输出18
# 遍历字典
for key in dictionary:
    item = dictionary[key]
    print(key, item)
# 通过方法取值
print(dictionary.get("age"))  # 输出18
# 通过方法取值没有这个值并设置默认值,不会改变原字典数据
print(dictionary.get("name", "张三"))  # 输出张三
# 更新一个元素
dictionary["age"] = 20
print(dictionary)  # 输出{'age': 20, 'sex': '男'}
# 更新多个元素
dictionary.update(age=3, sex="女")
print(dictionary)  # 输出{'age': 3, 'sex': '女'}
# 删除最后一项
dictionary.popitem()
print(dictionary)  # 输出{'age': 3}
# 删除指定索引的值 删除age，若删除失败，则返回False,否则返回age对应的值3
dictionary.pop("age", False)
print(dictionary)  # 输出{}
# 现在是{}的没有age元素所以下面的输出False
print(dictionary.pop("age", False))  # 输出False
# 创建两个列表
arr1 = [1, 2, 3]
arr2 = [4, 5, 6]
dictionary = dict(zip(arr1, arr2))  # 以arr1值为键名 arr2值为键值创建字典
print(dictionary)  # 输出{1: 4, 2: 5, 3: 6}
```

### 类

#### 创建类
<p align="left" style="color:#777777;">发布日期：2021-04-23</p>

```py
# 定义一个Person类
class Person:
    def __init__(self, name):
        """
        @description 构造方法
        @param name姓名
        @return
        """
        self.name = name

    def sayName(self):
        """
        @description 介绍自己的姓名
        @param
        @return
        """
        print("my name is " + self.name)

    def say(self, content):
        """
        @description 说话的方法
        @param content 说话的内容
        @return
        """
        print("I say " + content)

    def __saySex(self):
        """
        @description 私有方法类外部直接通过方法名不能访问，可以通过_Person__saySex()访问
        @param
        @return
        """
        print("I am a boy")


# 定义入口函数
def main():
    # 创建一个Person对象
    person = Person("绘梦")
    # 调用介绍自己的姓名的方法
    person.sayName()
    # 调用说话的方法
    person.say("hello")
    # 调用私有方法介绍性别
    person._Person__saySex()


if __name__ == "__main__":
    main()
```

#### 私有属性 get set装饰器
<p align="left" style="color:#777777;">发布日期：2021-04-23</p>

```py
class Person:
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    @property
    def name(self):
        """
        @description  定义一个get装饰器 用于获取类内部私有属性
        @param
        @return name
        """
        return self.__name

    @name.setter
    def name(self, name):
        """
        @description 定义一个set装饰器，用于设置类内部私有属性值
        @param
        @return
        """
        self.__name = name

    @property
    def age(self):
        """
        @description  定义一个get装饰器 用于获取类内部私有属性
        @param
        @return age
        """
        return self.__age


def main():
    person = Person("绘梦", 18)
    person.name = "豆豆"
    print(person.name, person.age)  # 输出 豆豆 18


if __name__ == "__main__":
    main()
```
!> 注意get装饰器必须在set装饰器之前,不然会出错

#### 静态方法装饰器
```py
class Person:
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    @staticmethod
    def staticFn(content):
        """
        @description 静态方法装饰器 不需要创建对象即可调用 ,即第一个参数不需要传self
        @param
        @return
        """
        print(content)


def main():
    # 直接调用
    Person.staticFn("直接调用")
    # 创建对象调用
    person = Person("绘梦", 18)
    person.staticFn("创建对象调用")


if __name__ == "__main__":
    main()
```
!>定义：使用装饰器@staticmethod。参数随意，没有“self”和“cls”参数，但是方法体中不能使用类或实例的任何属性和方法  
  调用：类和实例对象都可以调用


#### 类方法装饰器
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

```py
# 定义一个公司类
class Company:
    __num = 0  # 公司总人数

    # 初始化类时调用统计公司人数方法,不用实例化公司类就能统计人数
    def __new__(cls):
        Company.addNum()  # 类名直接调用类方法
        return super().__new__(cls)  # 必须返回，这里super()就是调用父类object的方法或数学，直接返回父类的__new__方法

    # 定义类方法  直接用类名去调用 和静态方法一样
    @classmethod
    def addNum(cls):
        cls.__num += 1

    # 定义类方法  直接用类名去调用 和静态方法一样
    @classmethod
    def getNum(cls):
        return cls.__num


# 定义个员工类
class Person(Company):
    # 实例化方法
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    # 初始化方法 这里必须要定义自己的__new__,不然会去继承父类的__new__,直接调用Company类的__new__,因为父类的__new__只有一个参数，这样就报错误了
    # 这里定义之后，会先调用这里的__new__,在调用父类的__new__
    def __new__(cls, name, age):
        return super().__new__(cls)


def main():
    # 创建2个员工实例
    a = Person("张三", 18)
    b = Person("李四", 20)
    print(Company.getNum())  # 类名调用类方法 输出2


if __name__ == "__main__":
    main()
```

!>定义：使用装饰器@classmethod。第一个参数必须是当前类对象，该参数名一般约定为“cls”，通过它来传递类的属性和方法（不能传实例的属性和方法）  
  调用：类和实例对象都可以调用

#### 继承和多态
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

```py
# 定义一个公司类
class Company:
    __address = "浙江省杭州市"  # 公司地址
    __company_name = "阿里巴巴"  # 公司名称

    @property
    def address(self):
        return self.__address

    @address.setter
    def address(self, address):
        """
        @description 设置公司地址
        @param
        @return
        """
        self.__address = address

    def getCompanyName(self):
        """
        @description 获取公司名称
        @param
        @return
        """
        return self.__company_name


# 定义个员工类  继承公司类
class Person(Company):
    # 实例化方法
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    # 重写父类的方法 称之为多态。。。
    def getCompanyName(self):
        """
        @description 获取公司名称
        @param
        @return
        """
        return "某东"


def main():
    # 创建2个员工实例
    person = Person("张三", 18)
    # 继承后可调用父类的属性
    person.address = "上海市金融大厦"
    print(person.address)  # 输出上海市金融大厦
    # 继承后可调用父类的方法
    print(person.getCompanyName())  # 输出某东


if __name__ == "__main__":
    main()
```

#### 多继承
```py
class D:
    def printA(self):
        print("D")


class A:
    def printA(self):
        print("A")
        x = super(A, self).printA()
        print("A2")
        return x


class B(A, D):
    def printB(self):
        print("B")


class C(B):
    def printA(self):
        print("C")
        n = super(C, self).printA()
        return n


c = C()
c.printA()

# 多继承关系  C继承B  B继承A和D
# super()方法调用父类的方法，所以父类的方法都可以调用
# 重写 重写需要子类的函数名和父类的函数名一样 然后在子类的函数里重写后再加上调用super().函数名 就实现了重写
# 这段代码的执行流程
# 实例化C对象
# ->调用C.printA() 打印C
# ->由于C里的printA调用了super()方法，所以去B类里找pintA方法没找到
# ->继续去A和D里找
# ->先找A，找到了，调用A.printA() 打印A
# ->由于A里的printA调用了super()方法，所以继续去D类里面找printA方法
# ->找D，找到了，调用D.printA() 打印D
# ->最后打印A2
```

#### 抽象类和抽象方法
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

```py
# 从abc模块 抽象类需要的模块
from abc import ABCMeta, abstractmethod

# 定义一个动物抽象类
class Animal(metaclass=ABCMeta):
    @abstractmethod
    def say(self):
        """
        @description 定义一个抽象方法 不需要实现 也不强制为空
        @param
        @return
        """
        pass


# 定义一个狗类 去实现动物类的所有方法
class Dog(Animal):
    # 实现了Animal类的say方法
    def say(self, content):
        print(content)


# 定义一个猫类 去实现动物类的所有方法
class Cat(Animal):
    # 实现了Animal类的say方法
    def say(self, content):
        print(content)


def main():
    dog = Dog()
    dog.say("汪汪汪")  # 输出汪汪汪
    cat = Cat()
    cat.say("我们一起喵喵喵")  # 输出我们一起喵喵喵


if __name__ == "__main__":
    main()
```

#### 接口类
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

接口类和抽象类大致相同
1. 接口类支持多继承，抽象类尽量避免多继承
2. 接口类只有方法，抽象类可以有方法和属性
3. 接口类的方法实现为空，具体有子类去实现，抽象类可以写一些方法去做基础实现，供子类参考

#### 常见魔术方法
```py
# 这是一个装饰器，你只需要实现 __eq__ 方法和其他任意一个方法如__lt__就可以推导出所有的 __ge__, __le__, __gt__
from functools import total_ordering


@total_ordering
class Person:
    def __new__(cls, age):
        """
        @description 创建方法在__init__之前被调用
        @param
        @return
        """
        print("__new__被调用")
        return super().__new__(cls)  # 必须返回，这里super()就是调用父类object的方法或属性，直接返回父类的__new__方法

    def __init__(self, age):
        self.age = age
        """
        @description 初始化方法
        @param
        @return
        """
        print("__init__被调用")

    def __del__(self):
        """
        @description 实例销毁实输出 ，一般是程序运行结束后 简单的调用方法 : del obj
        @param
        @return
        """
        print("__del__被调用")

    def __call__(self):
        """
        @description 定义这个方法 就可以用实例() 去调用  p() ，在类装饰器中有用到
        @param
        @return
        """
        print("__call__被调用")

    def __str__(self):
        """
        @description return 值在打印对象时会被输出，一般用来描写这个对象  简单的调用方法 : str(obj)
        @param
        @return
        """
        print(self.age)
        return "__str__被调用"

    def __repr__(self):
        """
        @description  return 值在打印对象时会被输出 用来代替__str__输出，若不存在__str__方法，就会输出这个方法 简单的调用方法 : repr(obj)
        @param
        @return
        """
        return "__repr__被调用"

    def __eq__(self, s):
        """
        @description __eq__用实例自身self和传入的实例 s 进行比较，返回相等的比较
        @param self 自身实例
        @param s 传入的实例
        @return
        """
        print("__eq__被调用")
        return self.age == s.age

    def __lt__(self, s):
        """
        @description __lt__用实例自身self和传入的实例 s 进行比较，返回小于的比较
        @param self 自身实例
        @param s 传入的实例
        @return
        """
        print("__lt__被调用")
        return self.age < s.age


def main():
    # 类实例化对象
    p = Person(18)  # 输出__new__被调用 __init__被调用
    # 实例调用__call__方法调用
    p()  # 相当于p.__call() 输出__call__被调用
    # 打印实例
    print(p)  # 输出__str__被调用
    print("%s" % p)  # 输出__str__被调用
    print("%r" % p)  # 输出__repr__被调用
    print("程序运行结束")  # 输出__repr__被调用
    # 结束后输出__del__被调用

    # # 为了避免和上面的输出混淆，要测试__lt__单独测试这块
    # list = sorted([Person(20), Person(10), Person(30)])
    # for item in list:
    #     print(item)
    # # 依次输出 __lt__被调用 __lt__被调用 __lt__被调用 10 __str__被调用 20 __str__被调用 30 __str__被调用
    # print(Person(20) < Person(25))  # 输出True


if __name__ == "__main__":
    main()
```

### 文件和异常处理
#### 打开文件并读取内容，并处理打开文件的异常
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

```py
def main():
    f = None  # 定义空变量，不然不能在finally代码块使用
    try:
        # 以r只读方式打开file.txt,没有文件会报错，使用utf-8编码打开
        f = open("file.txt", "r", encoding="utf-8")
        # 读取文件内容
        print(f.read())
    # 处理异常，异常类型可以在运行的结果中找到如：FileNotFoundError: [Errno 2] No such file or directory: 'file.txt'
    except FileNotFoundError:
        print("文件找不到")
    # LookupError: unknown encoding: utf-8xxx
    except LookupError:
        print("指定了未知的编码")
    #未知的异常
    except Exception as result:
        print(result)
    # finally代码块是不管有没有异常都会执行的
    finally:
        if f:
            # 关闭文件
            f.close()


if __name__ == "__main__":
    main()
```
!> 一般还是用with···as···方便

#### 读写文件操作
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

```py
def main():
    # 写入文件 覆盖写入 文件不存在会创建
    with open("file.txt", "w", encoding="utf-8") as f:
        f.write("text write line 1" + "\n")
        f.write("text write line 2" + "\n")
    # 追加方式写入 文件不存在会创建
    with open("file.txt", "a", encoding="utf-8") as f:
        f.write("text write line 3" + "\n")
    # 追加方式写入并读取
    with open("file.txt", "a+", encoding="utf-8") as f:
        f.write("text write line 4" + "\n")
        f.seek(0, 0)  # 移动光标到最前面 不然追加写完光标再最后 输出就为空了
        print(f.read())
    # 文件内容读取到列表
    with open("file.txt", "r", encoding="utf-8") as f:
        lines = f.readlines()
        print(lines)


if __name__ == "__main__":
    main()
```

#### 读写json文件及序列化和反序列化
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

```py
import json


def main():
    # 字典数据（这里暂时称之为json）
    obj = {
        "name": "张三",
        "age": 18,
        "address": "浙江杭州阿里巴巴",
    }

    # json序列化 输出{"name": "\u5f20\u4e09", "age": 18, "address": "\u6d59\u6c5f\u676d\u5dde\u963f\u91cc\u5df4\u5df4"}
    obj = json.dumps(obj)
    print(obj)
    # json 反序列化 输出{'name': '张三', 'age': 18, 'address': '浙江杭州阿里巴巴'}
    obj = json.loads(obj)
    print(obj)

    with open("file.json", "w+", encoding="utf-8") as f:
        # 将json数据序列化写入文件
        json.dump(obj, f)
        # 光标移到最前面
        f.seek(0, 0)
        # 直接读取文件内容输出 {"name": "\u5f20\u4e09", "age": 18, "address": "\u6d59\u6c5f\u676d\u5dde\u963f\u91cc\u5df4\u5df4"}
        print(f.read())
        # 光标移到最前面
        f.seek(0, 0)
        # 处理成json格式的字符串并输出 输出 "{\"name\": \"\\u5f20\\u4e09\", \"age\": 18, \"address\": \"\\u6d59\\u6c5f\\u676d\\u5dde\\u963f\\u91cc\\u5df4\\u5df4\"}"
        print(json.dumps(f.read()))
        # 光标移到最前面
        f.seek(0, 0)
        # 将上面输出的json格式字符串反序列化成python对象 输出 {"name": "\u5f20\u4e09", "age": 18, "address": "\u6d59\u6c5f\u676d\u5dde\u963f\u91cc\u5df4\u5df4"}
        print(json.loads(json.dumps(f.read())))
        f.seek(0, 0)
        # 将文件中的内容反序列化成对象 输出{'name': '张三', 'age': 18, 'address': '浙江杭州阿里巴巴'}
        print(json.load(f))


if __name__ == "__main__":
    main()
```

### 上下文对象
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

**用法**
```py
class Demo:
    def __enter__(self):
        print("In __enter__()")
        return self

    def __exit__(self, type, value, trace):
        print("In__exit__()")
        # 输出<class 'Exception'> 异常抛出 <traceback object at 0x0000021C61A70D80>
        print(f"{type}, {value}, {trace}")
        # 处理异常
        return True  # 异常处理后返回True 后面代码才会被执行

    @staticmethod
    def getObject():
        return Demo()


def main():
    with Demo.getObject() as object:
        raise Exception("异常抛出")  # 手动抛出异常
    print("我会被执行")


if __name__ == "__main__":
    main()
```
**实例一，文件自动关闭,使用with ··· as ···代码块结束后会自动调用f.close()关闭文件流**
```py
def main():
    try:
        # 以r只读方式打开file.txt,没有文件会报错，使用utf-8编码打开
        with  open("file.txt", "r", encoding="utf-8") as f:
            # 读取文件内容
            print(f.read())
    # 处理异常，异常类型可以在运行的结果中找到如：FileNotFoundError: [Errno 2] No such file or directory: 'file.txt'
    except FileNotFoundError:
        print("文件找不到")
    # LookupError: unknown encoding: utf-8xxx
    except LookupError:
        print("指定了未知的编码")

if __name__ == "__main__":
    main()
```
**实例二,数据自动关闭——[转自](https://zhuanlan.zhihu.com/p/164457246)**
```py
class DBCM: 
   # 负责对数据库进行初始化，也就是将主机名、接口（这里是 localhost 和 8080）分别赋予变量 hostname 和 port；
    def __init__(self, hostname, port): 
        self.hostname = hostname 
        self.port = port 
        self.connection = None
   # 连接数据库，并且返回对象 DBCM；
    def __enter__(self): 
        self.connection = DBClient(self.hostname, self.port) 
        return self
   # 负责关闭数据库的连接
    def __exit__(self, exc_type, exc_val, exc_tb): 
        self.connection.close() 
  
with DBCM('localhost', '8080') as db_client: 
    ....
```

### 正则表达式
#### 常用表达式符号
<p align="left" style="color:#777777;">发布日期：2021-04-26</p>

| 符号               | 解释                                   | 示例                | 说明                                                                            |
| ------------------ | -------------------------------------- | ------------------- | ------------------------------------------------------------------------------- |
| .                  | 匹配任意字符                           | b.t                 | 可以匹配bat / but / b#t / b1t等                                                 |
| \\.                | 匹配.                                  | 3\\.14              | 可以匹配3.14                                                                    |
| \\w                | 匹配字母/数字/下划线                   | b\\wt               | 可以匹配bat / b1t / b_t等<br>但不能匹配b#t                                      |
| \\s                | 匹配空白字符（包括\r、\n、\t等）       | love\\syou          | 可以匹配love you                                                                |
| \\d                | 匹配数字                               | \\d\\d              | 可以匹配01 / 23 / 99等                                                          |
| \\b                | 匹配单词的边界                         | \\bThe\\b           |                                                                                 |
| ^                  | 匹配字符串的开始或在[]里表示否定        | ^The                | 可以匹配The开头的字符串                                                         |
| $                  | 匹配字符串的结束                       | .exe$               | 可以匹配.exe结尾的字符串                                                        |
| \\W                | 匹配非字母/数字/下划线                 | b\\Wt               | 可以匹配b#t / b@t等<br>但不能匹配but / b1t / b_t等                              |
| \\S                | 匹配非空白字符                         | love\\Syou          | 可以匹配love#you等<br>但不能匹配love you                                        |
| \\D                | 匹配非数字                             | \\d\\D              | 可以匹配9a / 3# / 0F等                                                          |
| \\B                | 匹配非单词边界                         | \\Bio\\B            |                                                                                 |
| []                 | 匹配来自字符集的任意单一字符           | [aeiou]             | 可以匹配任一元音字母字符                                                        |
| [^]                | 匹配不在字符集中的任意单一字符         | [^aeiou]            | 可以匹配任一非元音字母字符                                                      |
| [M-N]              | 匹配M到N字符或数字范围内的任意单一字符 | [a-z] [0-9]         | 可以匹配a-z任意字母字符或者匹配0-9任意数字字符                                  |
| *                  | 匹配0次或多次                          | \\w*                |                                                                                 |
| +                  | 匹配1次或多次                          | \\w+                |                                                                                 |
| ?                  | 匹配0次或1次                           | \\w?                |                                                                                 |
| {N}                | 匹配N次                                | \\w{3}              |                                                                                 |
| {M,}               | 匹配至少M次                            | \\w{3,}             |                                                                                 |
| {M,N}              | 匹配至少M次至多N次                     | \\w{3,6}            |                                                                                 |
| \|                 | 分支                                   | foo\|bar            | 可以匹配foo或者bar                                                              |
| (?#)               | 注释                                   |                     |                                                                                 |
| (exp)              | 匹配exp并捕获到自动命名的组中          |                     |                                                                                 |
| (?&lt;name&gt;exp) | 匹配exp并捕获到名为name的组中          |                     |                                                                                 |
| (?:exp)            | 匹配exp但是不捕获匹配的文本            |                     |                                                                                 |
| (?=exp)            | 匹配exp前面的位置                      | \\b\\w+(?=ing)      | 可以匹配I'm dancing中的danc                                                     |
| (?<=exp)           | 匹配exp后面的位置                      | (?<=\\bdanc)\\w+\\b | 可以匹配I love dancing and reading中的第一个ing                                 |
| (?!exp)            | 匹配后面不是exp的位置                  |                     |                                                                                 |
| (?<!exp)           | 匹配前面不是exp的位置                  |                     |                                                                                 |
| *?                 | 重复任意次，但尽可能少重复             | a.\*b<br>a.\*?b     | 将正则表达式应用于aabab，前者会匹配整个字符串aabab，后者会匹配aab和ab两个字符串 |
| +?                 | 重复1次或多次，但尽可能少重复          |                     |                                                                                 |
| ??                 | 重复0次或1次，但尽可能少重复           |                     |                                                                                 |
| {M,N}?             | 重复M到N次，但尽可能少重复             |                     |                                                                                 |
| {M,}?              | 重复M次以上，但尽可能少重复            |                     |                                                                                 |

#### re函数使用方法
<p align="left" style="color:#777777;">发布日期：2021-04-26</p>

```py
import re

"""
@description 测试所有正则表达式方法 
@param 
@return 
"""


def main():
    # 定义我们要处理的字符串
    str = "The \. apple is a big apple!"
    """
    re.match(pattern, string, flags) 返回匹配的对象
    从字符串起始位置匹配，如果起始位置没匹配到，返回None
    r'' 匹配\的时候直接用\就行 不用\\去匹配了
    re.I 大小写不敏感 re.M 多行匹配 re.S 空格也匹配
    """
    res = re.match(r"the\s\\.", str, re.I | re.M)  # \s匹配空白字符串 \匹配\   \.匹配.
    """
    res.gorup(num) res为匹配的对象 num默认为0  输出一组结果，这里没加括号所以输出第一个结果
    """
    if res:
        print(res.group())  # 输出The \. 或者res.group(0)

    res = re.match(r"(the)\s(\\.)", str, re.I | re.M)
    """
    res.gorups(num) res为匹配的对象 num不传返回所有结果组成的元组
    """
    if res:
        print(res.groups())  # 输出('The', '\\.')

    """
    re.search(pattern, string, flags) 返回匹配的对象
    搜索整个字符串  返回第一个匹配的结果
    """
    res = re.search(r"apple", str, re.I | re.M)
    if res:
        print(res.group())  # 输出apple
        # 返回起始位置
        print(res.start())  # 输出7
        # 返回重点位置
        print(res.end())  # 输出12
        # 以元组形式返回位置
        print(res.span())  # 输出(7, 12)

    """
    re.sub(pattern, repl, string, count=0, flags) 表达式，替换的字符串，搜索的字符串，替换次数，模式
    正则指定次数替换字符串 返回替换后的字符串
    """
    res = re.sub(r"apple", "oppo", str, 1, re.I | re.M)
    if res:
        print(res)  # 输出 The \. oppo is a big apple!

    """
    re.compile(pattern,flags) 表达式 模式
    返回一个正则表达式对象(Pattern) 可调用其他的re函数
    """
    pattern = re.compile(r"the", re.I | re.M)
    res = pattern.match(str)
    if res:
        print(res.group())  # 输出 The

    """
    findall(pattern,string,start,end,flags) 表达式 字符串 开始位置默认为0 结束位置默认为最后 模式
    查找指定返回内的字符 并返回到列表 没找到返回[]
    """
    res = re.findall(r"apple", str, re.I | re.M)
    if len(res) > 0:
        print(res)  # 输出 ['apple', 'apple']

    """
    re.finditer(pattern, string, flags) 表达式 字符串 模式
    匹配指定字符返回迭代器 迭代器可以直接for循环遍历
    """
    res = re.finditer(r"apple", str, re.I | re.M)
    for item in res:
        print(item.group())  # 依次输出apple apple

    """
    re.split(pattern,string) 拆分表达式 字符串
    按指定字符拆分字符串，返回拆分的列表
    """
    str2 = "窗前明月光，疑是地上霜。举头望明月，低头思故乡。"
    res = re.split(r"['，'，'。']", str2)  # 按逗号和句号拆分
    print(res)  # 输出 ['窗前明月光', '疑是地上霜', '举头望明月', '低头思故乡', '']
    # 这段代码可以删除空格
    while "" in res:
        res.remove("")
    print(res)  # 输出 ['窗前明月光', '疑是地上霜', '举头望明月', '低头思故乡']


if __name__ == "__main__":
    main()
```

### 多进程和多线程

#### 多进程
<p align="left" style="color:#777777;">发布日期：2021-04-26</p>

**基本案例**
```py
# 导入多进程模块
from multiprocessing import Process
from time import sleep, time
from os import getpid

"""
@description 计算2个数的和 
@param  a 数a
@param  b 数b
@return 
"""


def fn(a, b):
    # 假如要计算5秒钟
    print(getpid())
    sleep(5)
    return a + b


def main():
    # 开始计时
    start_time = time()
    """
    Process(target = 执行的函数,args =  函数的参数,元组类型)
    """
    p1 = Process(target=fn, args=(1, 2))
    p2 = Process(target=fn, args=(3, 4))
    # 开启进程p1
    p1.start()
    # 开启进程p2
    p2.start()
    # 等待p1进程执行完毕
    p1.join()
    # 等待p2进程执行结束
    p2.join()
    # 结束计时
    end_time = time()
    # 花费的总时间
    print(f"总耗时{(end_time - start_time):.3f}s")  # 2个进程同时执行，最后的执行时间是5.132s


if __name__ == "__main__":
    main()
```

**使用类的方式创建进程**
```py
# 导入多进程模块
from multiprocessing import Process
from time import sleep, time
from os import getpid


class Sum(Process):
    # 重写父类的初始化方法
    def __init__(self, a, b):
        super().__init__()  # 调用父类的初始化方法
        self.__a = a
        self.__b = b

    # 重写Process的启动函数
    def run(self):
        # 假如要计算5秒钟
        print(getpid())
        sleep(5)
        print(self.__a + self.__b)


def main():
    # 开始计时
    start_time = time()
    # 使用类创建p1,p2进程
    p1 = Sum(1, 2)
    p2 = Sum(3, 4)
    # 开启进程p1
    p1.start()
    # 开启进程p2
    p2.start()
    # 等待p1进程执行完毕
    p1.join()
    # 等待p2进程执行结束
    p2.join()
    # 结束计时
    end_time = time()
    # 花费的总时间
    print(f"总耗时{(end_time - start_time):.3f}s")  # 2个进程同时执行，最后的执行时间是5.132s


if __name__ == "__main__":
    main()
```
!> 类方式创建进程和创建线程是相同的

**多进程共享队列模块**
```py
# 导入多进程模块,进程队列模块
from multiprocessing import Process, Queue


def fn(q, a, b):
    """
    @description 计算2个数的和
    @param  q 队列
    @param  a 数1
    @param  b 数2
    @return"""
    # 把a+b的结果放入队列
    q.put(a + b)


def readRes(q):
    """
    @description 读取计算的和
    @param  q 队列
    @return"""
    while True:
        print(q.get())  # 依次输出 7 9


def main():
    # 开启一个队列
    q = Queue()

    # 开启计算和进程
    p1 = Process(target=fn, args=(q, 3, 4))
    p2 = Process(target=fn, args=(q, 4, 5))

    # 读取队列进程
    p3 = Process(target=readRes, args=(q,))
    # 开启进程p1
    p1.start()
    # 开启进程p2
    p2.start()
    # 开启进程p3
    p3.start()
    # 等待p1，p2进程结束
    p1.join()
    p2.join()
    # 强行结束p3进程
    p3.terminate()


if __name__ == "__main__":
    main()
```

#### 多线程
<p align="left" style="color:#777777;">发布日期：2021-04-26</p>

**基本案例**
```py
from threading import Thread
from time import sleep, time


def fn(a, b):
    """
    @description 计算2个数的和
    @param  a 数a
    @param  b 数b
    @return"""
    # 假如要计算5秒钟
    sleep(5)
    return a + b


def main():
    # 开始计时
    start_time = time()
    """
    Thread(target = 执行的函数,args =  函数的参数,元组类型)
    """
    t1 = Thread(target=fn, args=(1, 2))
    t2 = Thread(target=fn, args=(3, 4))
    # 开启线程t1
    t1.start()
    # 开启线程t2
    t2.start()
    # 等待t1线程执行完毕
    t1.join()
    # 等待t2线程执行结束
    t2.join()
    # 结束计时
    end_time = time()
    # 花费的总时间
    print(f"总耗时{(end_time - start_time):.3f}s")  # 2个线程同时执行，总耗时5.002s


if __name__ == "__main__":
    main()
```

**多线程加锁**
```py
from threading import Thread, Lock, RLock
from time import sleep

lock = Lock()  # 一个线程里多次使用lock的话用RLock，防止阻塞


def fn(List, num):
    """
    @description 把num放入List
    @param List 列表
    @param num 数
    @return"""
    # 获取锁
    lock.acquire()
    try:
        sleep(0.01)  # 假如放入一个数的时间需要0.01秒
        List.append(num)
    finally:
        # 释放锁
        lock.release()

    """
    #推荐用这种写法
    with lock:  # 会自动调用release是否锁
        sleep(0.01)  # 假如放入一个数的时间需要0.01秒
        List.append(num)
    """


def main():
    # 初始化列表
    List = []
    Threads = []
    # 循环创建100个数
    for i in range(100):
        # 创建线程
        t = Thread(target=fn, args=(List, i))
        # 线程保存到列表
        Threads.append(t)
        # 启动线程
        t.start()
    # 等待所有线程结束
    for t in Threads:
        t.join()
    print(List)  # 如果不加锁 那么顺序是乱的，因为同时操作一个List


if __name__ == "__main__":
    main()
```
!> 这里注意 多线程和多进程最大的不同在于，多进程中，同一个变量，各自有一份拷贝存在于每个进程中，互不影响，而多线程中，所有变量都由所有线程共享，所以，任何一个变量都可以被任何一个线程修改，因此，线程之间共享数据最大的危险在于多个线程同时改一个变量，把内容给改乱了

### 网络请求

#### requests库
<p align="left" style="color:#777777;">发布日期：2021-04-26</p>

**基本案例**
```py
# 导入网络请求库
import requests

# 导入正则表达库
import re

# 导入多线程
from threading import Thread

# 保存图片线程类
class SaveImage(Thread):
    def __init__(self, url, filename):
        super().__init__()
        self.__url = url
        self.__filename = filename

    def run(self):
        with open(self.__filename, "wb") as f:
            res = requests.get(self.__url)
            # .content是二进制数据 比如图片，音频，视频
            f.write(res.content)


def main():
    # 请求当前地址内容
    res = requests.get("http://www.netbian.com/quanping/")
    # 匹配所有输出（）里的内容 .text是文本数据
    match = re.findall(r"src=\"(.*?.[jpg|png])\"", res.text, re.I | re.M)
    # 输出所有匹配到的图片地址列表 ['http://img.netbian.com/file/2020/0908/e02f00702a7cb32024f71ecce443e78d.jpg',···]
    # print(match)
    # 循环保存图片
    i = 1
    for url in match:
        t = SaveImage(url, str(i) + ".jpg")
        t.start()
        i += 1
        if i == 100:
            break


if __name__ == "__main__":
    main()
```

!> 若出现ValueError: check_hostname requires server_hostname把系统设置里的代理关掉就行了

**常用方法**
```py
import requests

# 定义header头
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.212 Safari/537.36",
}
# 定义数据
data = {"key": "value"}
# 发送post请求
post_response = requests.post("https://httpbin.org/post", headers=headers, data=None, json=data)
"""
request.post(url,header,data,json)
@description requests post请求方法
@param url 请求地址
@param headers 请求头
@param data application/x-www-form-urlencoded 表单数据提交方式
@param json application/json json数据提交方式
@return response对象
"""
print(post_response.status_code)  # 状态码 200 404 503等
print(post_response.encoding)  # 编码 utf-8 等
print(post_response.headers)  # 响应头 response header
print(post_response.text)  # 文档格式返回
print(post_response.content)  # 二进制类型 例如获取视频音频图片就可以用这个
print(post_response.json())  # json格式返回
# 发送get请求
cookies = {"c_key": "c_value"}
# 发送get请求
get_response = requests.get("https://httpbin.org/cookies", params=data, cookies=cookies)
"""
requests.get(url,params)
@description  gets请求方法
@param url 请求地址
@param params 请求get参数
@param cookies cookies参数
@return 
"""
print(get_response.text)  # 文本格式返回 返回的是html源代码


get_stream = requests.get("https://api.github.com/events", stream=True)
"""
@description 获取流相应内容
@param stream bool 是否获取流内容
@return 
"""
print(get_stream.content)
```

**应用**
抓取m3u8  ts类型的视频
```py
import requests

with requests.get(
    "https://apd-vliveachy.apdcdn.tc.qq.com/vqq/moviets.tc.qq.com/AJYii2wcis67IqvazEtSo4i8eeh3hpdbGPkpGroBNOEU/uwMROfz2r5xgoaQXGdGnC2df64gVTKzl5C_X6A3JOVT0QIb-/TpCabJQXXHCtRpDp-mq7mZCOmwwko_kxe3rMOrnioOdPj3kg9M_YqrTBeUzH6lpLJzhmCQsA0hurm48QCAmsCdBozEdlWybosPhwqaID8VbsMrrpzJPCY4JaWAyPTffTTt3-SUlNpBkcxE5BZ1b78Q/03_n0036mf5isl.321004.1.ts?index=3&start=30400&end=42400&brs=12516100&bre=18324923&ver=4",
    stream=True,
) as r:
    with open("1.ts", "wb") as f:#后缀名.mp4也可以
        for chunk in r.iter_content(chunk_size=1024):
            f.write(chunk)
```

### 异步IO
#### asyncio 和 async/await

同步
```py
import time


def http_request():
    """
    @description 模拟一个http请求
    @param
    @return
    """
    # 睡眠1秒
    time.sleep(1)
    print("返回数据了")


def main():
    for _ in range(5):
        http_request()


if __name__ == "__main__":
    main()
""" 
请求同步执行（顺序执行）
"""
```

异步 async/await 创建任务
```py
import asyncio


async def http_request():
    """
    @description 模拟一个http请求
    @param
    @return
    """
    # 睡眠1秒
    await asyncio.sleep(1)
    print("返回数据了")


async def main():
    # asyncio.create_task 创建一个任务
    tasks = [asyncio.create_task(http_request()) for _i in range(5)]
    for item in tasks:
        await item


if __name__ == "__main__":
    asyncio.run(main())
"""
请求并发执行（一起执行）
"""
```

等待任务，然后一起执行
```py
import asyncio


async def http_request():
    """
    @description 模拟一个http请求
    @param
    @return
    """
    # 睡眠1秒
    await asyncio.sleep(1)
    print("返回数据了")


def main():
    tasks = [http_request() for _ in range(5)]
    asyncio.run(asyncio.wait(tasks))


if __name__ == "__main__":
    main()
"""
请求并发执行（一起执行）
"""
```

并发运行任务
```py
import asyncio


async def http_request():
    """
    @description 模拟一个http请求
    @param
    @return
    """
    # 睡眠1秒
    await asyncio.sleep(1)
    print("返回数据了")


async def main():
    tasks = [http_request() for _ in range(5)]
    # 并发运行任务，也可叫做协程
    await asyncio.gather(*tasks)
    # 上面的等同于 await asyncio.gather(http_request(), http_request(), http_request(), http_request(), http_request())


if __name__ == "__main__":
    asyncio.run(main())
"""
请求并发执行（一起执行）
"""
```

实例 异步请求 aiohttp
[文档地址](https://docs.aiohttp.org/en/stable/)
```py
import asyncio
import aiohttp


async def http_request():
    # 创建了一个异步的网络请求的上线文管理具柄  用了with 会自动释放调用close的
    async with aiohttp.ClientSession() as session:
        # 异步请求数据
        async with session.get("http://www.baidu.com") as resp:
            # 输出请求成功状态码
            print(resp.status)
            # text = await resp.text()
            # html = await response.read()


async def main():
    tasks = [http_request() for _ in range(5)]
    # 并发运行任务
    await asyncio.gather(*tasks)


if __name__ == "__main__":
    asyncio.run(main())
```

### 图片处理

#### 常用操作
<p align="left" style="color:#777777;">发布日期：2021-04-26</p>

```py
from PIL import Image, ImageFilter


def main():
    image = Image.open("./1.png")
    # 获取图片真实格式 假如是jpg的图片命名为png  也会获取到jpg的
    print(image.format)  # 输出png
    # 获取图片的分辨率
    print(image.size)  # 输出(1024, 1024)
    # 获取图片的颜色模式
    print(image.mode)  # 输出RGBA
    # 用默认模式打开图片
    image.show()
    # 裁剪 crop((x1,y1,x2,y2)) 传元组
    image.crop((300, 200, 600, 800)).show()
    # 生成缩略图thumbnail((width,height))
    image.thumbnail((100, 100))
    image.show()
    # 缩放图片resize((width,height))
    image.resize((10, 10)).show()
    # 合并图像paste(src,(x,y))
    image.paste(image.resize((100, 100)), (100, 100))
    image.show()
    # 旋转180度
    image.rotate(180).show()
    # 左右翻转
    image.transpose(Image.FLIP_LEFT_RIGHT).show()
    # 滤镜 随便搞个模糊滤镜
    image.filter(ImageFilter.BLUR).show()


if __name__ == "__main__":
    main()
```

### excel基础操作
<p align="left" style="color:#777777;">发布日期：2021-04-27</p>

```py
# 导入excel处理库
from openpyxl import Workbook, load_workbook

# 导入日期处理库
from datetime import datetime as dt

# 这些都是样式处理库 见 https://openpyxl.readthedocs.io/en/stable/styles.html
from openpyxl.styles import (
    PatternFill,
    Border,
    Side,
    Alignment,
    Protection,
    Font,
    Color,
)


def main():
    # 初始化工作簿对象
    wb = Workbook()
    """下面是工作表worksheet操作"""
    # 在最末尾插入一个worksheet
    wb.create_sheet("最后一个sheet")
    # 在指定位置插入worksheet
    wb.create_sheet("我是插入的sheet", 1)
    # 获取当前激活的worksheet
    ws = wb.active
    # 更改默认的sheet名字
    ws.title = "我是默认的sheet,标题被修改了"
    # 查看所有的表格
    print(wb.sheetnames)  # 输出['我是默认的sheet,标题被修改了', '我是插入的sheet', '最后一个sheet']
    # 更改sheet的选项卡下划线颜色
    ws.sheet_properties.tabColor = "f60f60"
    # 激活指定的worksheet
    ws = wb["最后一个sheet"]  # 知道名字可以这么切换sheet
    ws.title = "最后一个sheet标题也被改啦"
    print(wb.sheetnames)  # 输出['我是默认的sheet,标题被修改了', '我是插入的sheet', '最后一个sheet']
    ws = wb.worksheets[0]  # 使用索引激活回到第一个sheet

    """下面是单元格操作"""
    # 改变指定单元格内容
    ws["A3"] = "我是A3单元格内容"
    # 根据行列创建数据
    ws.cell(3, 2, "我是第三行第二列内容")
    # 访问指定单元格内容
    print(ws["A3"].value)  # 输出我是A3单元格内容
    print(ws["B3"].value)  # 输出我是第三行第二列内容
    # Cell对象.value可以直接赋值 为了避免混淆。这种方式不要用  用这种 ws["A3"] = "我是A3单元格内容"
    ws["A3"].value = "我是A3单元格内容新赋值的"
    print(ws["A3"].value)  # 输出我是A3单元格内容新赋值的
    # 当问多个单元格
    # 切片方式 访问指定范围, 访问一列用ws[A], 访问一行用ws[3]
    for row in ws["A1":"B3"]:
        for cell in row:
            print(cell.value)  # 分别输出None None None None 我是A3单元格内容新赋值的 我是第三行第二列内容
    # 行列方式
    for cell in ws["A"]:
        print(cell.value)  # 分别输出 None None 我是A3单元格内容新赋值的  没有内容会输出None
    # 如果需要按一行一行访问 参考 https://openpyxl.readthedocs.io/en/stable/tutorial.html#accessing-many-cells  iter_rows方法
    for row in ws.iter_rows(min_row=1, max_col=2, max_row=3):
        for cell in row:
            print(cell)
    # 输出
    # <Cell '我是默认的sheet,标题被修改了'.A1>
    # <Cell '我是默认的sheet,标题被修改了'.B1>
    # <Cell '我是默认的sheet,标题被修改了'.A2>
    # <Cell '我是默认的sheet,标题被修改了'.B2>
    # <Cell '我是默认的sheet,标题被修改了'.A3>
    # <Cell '我是默认的sheet,标题被修改了'.B3>
    # 如果需要按一列一列访问
    for col in ws.iter_cols(min_row=1, max_col=2, max_row=3):
        for cell in col:
            print(cell)
    # 输出
    # <Cell '我是默认的sheet,标题被修改了'.A1>
    # <Cell '我是默认的sheet,标题被修改了'.A2>
    # <Cell '我是默认的sheet,标题被修改了'.A3>
    # <Cell '我是默认的sheet,标题被修改了'.B1>
    # <Cell '我是默认的sheet,标题被修改了'.B2>
    # <Cell '我是默认的sheet,标题被修改了'.B3>
    # 只遍历输出值
    for row in ws.values:
        for value in row:
            print(value)
    # 获取每一行 .rows 每一列 columns
    for row in ws.rows:
        for cell in row:
            print(cell)
    # 输出
    # <Cell '我是默认的sheet,标题被修改了'.A1>
    # <Cell '我是默认的sheet,标题被修改了'.B1>
    # <Cell '我是默认的sheet,标题被修改了'.A2>
    # <Cell '我是默认的sheet,标题被修改了'.B2>
    # <Cell '我是默认的sheet,标题被修改了'.A3>
    # <Cell '我是默认的sheet,标题被修改了'.B3>
    # excel覆盖保存文件到当前文件夹
    wb.save("第一个excel.xlsx")
    # 打开已有文档
    wb = load_workbook("第一个excel.xlsx")
    ws = wb.active
    # 插入到第3行
    ws.insert_rows(3)
    # 插入到第二列
    ws.insert_cols(2)
    # 删除行
    ws.delete_rows(3)
    # 删除列
    ws.delete_cols(2)
    # 删除多行 删除第4行到第六行
    ws.delete_rows(4, 3)
    # 删除多列 删除C到E列
    ws.delete_cols(3, 3)  # 3 => C ,3 => 从C开始删除3列
    # 创建一下数据
    for row in ws["A1":"B2"]:
        for cell in row:
            cell.value = 1  # 这里直接赋值给cell会失败，注意
    # 使用函数 求和
    ws["C1"] = "=SUM(A1:B1)"
    # 日期格式化
    ws["D1"] = dt.now().strftime("%Y-%m-%d %H:%M:%S")  # strftime 格式化日期

    # 样式什么的，我觉得导出来再处理会比较好，毕竟毕竟简单。
    # 可以设个隔行变色
    i = 0
    ft = Font(name="微软雅黑", size=18, color="f60f60", bold=True, italic=True)
    for row in ws.rows:
        for cell in row:
            if i % 2 == 0:
                cell.font = ft
            # 上下左右居中 还可以使用right、left等等参数
            cell.alignment = Alignment(horizontal="center", vertical="center")
        i += 1
    # 第2行行高
    ws.row_dimensions[3].height = 100
    # C列列宽
    ws.column_dimensions["C"].width = 150
    # 合并单元格 合并的值以左上角为例
    ws.merge_cells("A2:B2")
    # 拆分单元格 拆分后只有左上角的单元格有值
    ws.unmerge_cells("A2:B2")
    # excel覆盖保存文件到当前文件夹
    wb.save("第一个excel.xlsx")


if __name__ == "__main__":
    main()
```

### 推导式语法
<p align="left" style="color:#777777;">发布日期：2021-04-27</p>

```py
def main():
    # 用推导式语法创建一个列表
    list = [item for item in range(1, 9)]
    print(list)  # 输出[1, 2, 3, 4, 5, 6, 7, 8]
    # 用推导式语法取出大于2的数据加上2,组成新的列表
    list = [(item + 2) for item in list if item > 2]
    print(list)  # 输出[5, 6, 7, 8, 9, 10]
    # 用推导式语法生成相邻两数之和，并存入列表
    list = [
        item + list[index + 1]
        for index, item in enumerate(list)
        if index < (len(list) - 1)
    ]
    print(list)  # 输出[11, 13, 15, 17, 19]
    # 定义一个学生列表
    student = [
        {"name": "小红", "age": 18},
        {"name": "小明", "age": 17},
        {"name": "小张", "age": 19},
    ]
    # 用推导式语法创建满18岁的学生列表
    student = [item for item in student if item["age"] >= 18]
    print(student)  # 输出 [{'name': '小红', 'age': 18}, {'name': '小张', 'age': 19}]
    # 用推导式语法创建一个集合
    sets = {item for item in range(1, 9)}
    print(sets)  # 输出{1, 2, 3, 4, 5, 6, 7, 8}


if __name__ == "__main__":
    main()
```

### 生成器
<p align="left" style="color:#777777;">发布日期：2021-04-27</p>

#### 生成器基本执行流程
```py
def createGenerator():
    """
    @description 创建一个生成器
    @param
    @return
    """
    list = range(3)
    for i in list:
        res = yield i * i
        print(res)


def main():
    # 创建一个生成器 创建方法和列表类似
    list = (item for item in range(10))  # generator object
    for item in list:
        print(item)  # 输出0~9的数字
    # 迭代完一次后再迭代发现没有输出
    for item in list:
        print(item)
    # 创建生成器
    g = createGenerator()
    # 通过next()可以打印下一个yield返回的结果
    print(next(g))  # 输出0
    print(next(g))  # 输出None和1 这里的None是函数里Print(res)输出的
    # g.send(10) 把10返回给了res，所以res有值
    print(g.send(10))  # 输出10和4 这里的10是函数里Print(res)输出的
    # 创建生成器
    g = createGenerator()
    for item in g:
        print(item)  # 依次输出0 None 1 None 4 None 这里的None是函数里Print(res)输出的


if __name__ == "__main__":
    main()
```
!>生成器用完一次后即被释放,大列表都可以用生成器去创建

#### 用生成器生成无限序列
```py
def createGenerator():
    """
    @description 创建生成器
    @param
    @return
    """
    num = 0
    while True:
        yield num
        num = num + 1
        # 下面给个条件，让生成器终止
        if num > 1000000:
            break


def main():
    # 这将是一个无限序列的生成器
    g = createGenerator()
    # 除非手动停止，不然无限输出下去
    for item in range(10000):
        print(item)


if __name__ == "__main__":
    main()
```

#### 生成器深入理解与应用
```py
from time import time


def createGenerator():
    """
    @description 创建生成器
    @param
    @return
    """
    list = []
    for item in range(100000):
        list = [item] * 2000  # 在函数内部处理时间很长的时候，这个时候，用生成器速度快，且不占内存
        yield list


def main():
    # 列表推导式开始计时
    start_time1 = time()
    # 列表推导式生成list
    list = [[item] * 2000 for item in range(100000)]
    # 迭代
    for _ in list:
        continue
    #  列表推导式计时结束
    end_time1 = time()

    # 函数生成器开始计时
    start_time2 = time()
    # 创建函数生成器
    g = createGenerator()
    for _ in g:
        continue
    # 函数生成器计时结束
    end_time2 = time()

    # 列表推导式生成器开始计时
    start_time3 = time()
    # 列表推导式生成器生成list
    g = ([item] * 2000 for item in range(100000))
    for _ in g:
        continue
    # 列表推导式生成器计时结束
    end_time3 = time()

    print(f"列表推导式时间{(end_time1-start_time1):.8f}s")  # 输出2.15891385s
    print(f"函数生成器时间{(end_time2-start_time2):.8f}s")  # 输出0.60804820s
    print(f"列表推导式生成器时间{(end_time3-start_time3):.8f}s")  # 输出0.61485124s


if __name__ == "__main__":
    main()
```

### 数据库
#### pymysql
```py
import pymysql

# 数据库配置 也可写在配置文件里，然后去读取
database = {
    "host": "127.0.0.1",
    "port": 3306,
    "db": "test",
    "user": "root",
    "passwd": "",
    "charset": "utf8",
}


class Db:
    def __init__(self):
        """
        @description 初始化Db类
        @param
        @return
        """
        self.conn = pymysql.connect(
            host=database["host"],
            port=database["port"],
            db=database["db"],
            user=database["user"],
            passwd=database["passwd"],
            charset=database["charset"],
        )
        # 创建游标，操作设置为字典类型
        self.cur = self.conn.cursor(cursor=pymysql.cursors.DictCursor)
        # 执行操作的表名
        self.table_name = ""
        # 分页字符串
        self.limit_sql = ""
        # 保存的最后一次执行sql语句
        self.sql = ""
        # 保存查询的排序方式
        self.order_sql = ""
        # 保存的where条件
        self.where_sql = ""
        self.values = []
        # 保存的查询field字段
        self.field_sql = "*"
        # 是否自动提交 否的时候用在执行事务流程上
        self.auto_commit = True

    def __enter__(self):
        """
        @description with语法入口 返回实例化对象
        @param
        @return self 数据库操作对象 Class
        """
        return self

    def __exit__(self, type, value, trace):
        """
        @description with语法出口 每次执行完sql关闭数据库连接 释放资源
        @param
        @return
        """
        # 关闭游标
        self.cur.close()
        # 关闭数据库连接
        self.conn.close()

    def __execute(self, sql, values=None, return_last_row_id=False):
        """
        @description 执行sql语句，并返回影响的行数
        @param sql sql语句 string
        @param values 执行条件值元组 tuple | list
        @param return_last_row_id 是否返回当前游标所在的行id boolean
        @return 影响的行数或游标所在的行id int
        """
        try:
            effect_num = self.cur.execute(sql, values)
            if self.auto_commit:
                self.conn.commit()
            if return_last_row_id:
                return self.cur.lastrowid
            else:
                return effect_num
        except Exception as result:
            raise result
        finally:
            self.__setLastSql(sql)
            self.__clearSql()

    def queryOne(self, sql, values=None):
        """
        @description 执行sql语句，并返回单个结果集
        @param sql sql语句 string
        @param values 查询条件值元组 tuple | list
        @return result 结果集 dict
        """
        try:
            self.cur.execute(sql, values)
            if self.auto_commit:
                self.conn.commit()
            result = self.cur.fetchone()
            return result
        except Exception as result:
            raise result
        finally:
            self.__setLastSql(sql)
            self.__clearSql()

    def query(self, sql, values=None):
        """
        @description 执行sql语句，并返回所有结果集
        @param sql sql语句 string
        @param values 查询条件值元组 tuple | list
        @return result 结果集 dict
        """
        try:
            self.cur.execute(sql, values)
            if self.auto_commit:
                self.conn.commit()
            result = self.cur.fetchall()
            return result
        except Exception as result:
            raise result
        finally:
            self.__setLastSql(sql)
            self.__clearSql()

    def table(self, table_name):
        self.table_name = table_name
        return self

    def where(self, *wheres):
        """
        @description 获取需要的查询条件 和 query等方法中，execute执行需要的传参values
        @param wheres 查询条件 tuple(dict | list)
        @return where_sql 拼接完成的查询条件 string
        @return self db对象
        """
        self.values = []
        self.where_sql = ""
        if wheres:
            self.where_sql = " WHERE "
            for where in wheres:
                if len(self.values) != 0:
                    self.where_sql += "or "
                if where and isinstance(where, dict):
                    # 如果where不为空且是字典传参 等值查找条件
                    for key in where:
                        if len(self.values) != 0 and self.where_sql[-3:-1:1] != "or":
                            self.where_sql += "and "
                        self.where_sql += "`" + key + "`"
                        self.where_sql += "="
                        self.where_sql += "%s "
                        self.values.append(where[key])
                elif where and isinstance(where, list):
                    # 如果where不为空且是嵌套列表传参 高级查询
                    for item in where:
                        if len(self.values) != 0 and self.where_sql[-3:-1:1] != "or":
                            self.where_sql += "and "
                        self.where_sql += "`" + item[0] + "`"
                        if item[1] == "between" or item[1] == "BETWEEN":
                            self.where_sql += " BETWEEN %s AND %s "
                            self.values.append(item[2])
                            self.values.append(item[3])
                        elif item[1] == "in" or item[1] == "IN":
                            sub_where_sql = ""
                            for item_value in item[2]:
                                if sub_where_sql:
                                    sub_where_sql += ","
                                sub_where_sql += "%s"
                                self.values.append(item_value)
                            self.where_sql += " IN (" + sub_where_sql + ")"
                        elif item[1] == "not in" or item[1] == "NOT IN":
                            sub_where_sql = ""
                            for item_value in item[2]:
                                if sub_where_sql:
                                    sub_where_sql += ","
                                sub_where_sql += "%s"
                                self.values.append(item_value)
                            self.where_sql += " NOT IN (" + sub_where_sql + ")"
                        else:
                            self.where_sql += " " + item[1] + " "
                            self.where_sql += "%s "
                            self.values.append(item[2])
                else:
                    continue
        return self

    def field(self, field):
        """
        @description  根据传入的field组装成带``的field
        @param field 逗号分隔的字段字符串 string
        @return self db对象
        """
        if field != "*":
            # 按英文逗号拆分后，用引号包裹组装
            list = field.split(",")
            field_list = []
            for item in list:
                field_list.append("`" + item + "`")
            self.field_sql = ",".join(field_list)
        return self

    def __getFieldValues(self, data=None):
        """
        @description 根据原始数据组装要插入的数据Sql语句
        @param data 插入的数据 dict | list
        @return value_sql 拼接完成的insert into value字符串 string
        @return values execute所需参数 list
        """
        field_str = ""  # 字段字符串
        value_str = ""  # %s拼接的字符串
        value_sql = ""  # 拼接完成的insert into value字符串
        values = []  # execute所需参数
        if data:
            if isinstance(data, dict):
                # 插入一条记录
                for key in data:
                    if len(values) != 0:
                        field_str += ","
                        value_str += ","
                    field_str += "`" + key + "`"
                    value_str += "%s"
                    values.append(data[key])
                value_sql = "(" + field_str + ") VALUES " + "(" + value_str + ")"
            if isinstance(data, list):
                # 插入多条记录
                # 只组装一遍字段名称
                for key in data[0]:
                    if field_str:
                        field_str += ","
                    field_str += "`" + key + "`"
                for item in data:
                    # 临时变量 单个Value (%s,%s,%s···)
                    value_str_sub = ""
                    for key in item:
                        if len(value_str_sub) != 0:
                            value_str_sub += ","
                        value_str_sub += "%s"
                        values.append(item[key])
                    if value_str:
                        value_str += ","
                    value_str += "(" + value_str_sub + ")"
                value_sql = "(" + field_str + ") VALUES " + value_str
        return value_sql, values

    def __getSetField(self, data):
        """
        @description 根据原始数据组装要更新的update set table 【field】部分的Sql语句
        @param data 更新的数据 dict
        @return field 更新设置field部分的sql语句  string
        @return values execute所需参数 list
        """
        field = ""
        values = []
        if data:
            for key in data:
                if len(values) != 0:
                    field += ","
                field += "`" + key + "`=%s"
                values.append(data[key])
        return field, values

    def find(self):
        """
        @description 查询符合条件的一条记录
        @return 结果集 dict
        """
        sql = (
            "SELECT "
            + self.field_sql
            + " FROM `"
            + self.table_name
            + "`"
            + self.where_sql
            + self.limit_sql
            + self.order_sql
        )
        return self.queryOne(sql, self.values)

    def select(self):
        """
        @description 查询符合条件的所有记录
        @return 结果集 dict
        """
        sql = (
            "SELECT "
            + self.field_sql
            + " FROM `"
            + self.table_name
            + "`"
            + self.where_sql
            + self.limit_sql
            + self.order_sql
        )
        return self.query(sql, self.values)

    def insert(self, data, return_last_row_id=False):
        """
        @description 插入一条记录
        @param data 插入的数据 dict | list
        @param return_last_row_id 是否返回当前游标所在的行id boolean
        @return 影响行数或游标所在id int
        """
        value_sql, values = self.__getFieldValues(data)
        sql = "INSERT INTO `" + self.table_name + "` " + value_sql
        return self.__execute(sql, values, return_last_row_id)

    def update(self, data):
        """
        @description 更新符合条件的所有记录的多个字段
        @param data 更新的数据 dict
        @return 影响行数 int
        """
        field, field_values = self.__getSetField(data)
        # 注意field在前 where在后
        values = field_values + self.values
        sql = "UPDATE `" + self.table_name + "` SET " + field + self.where_sql
        return self.__execute(sql, values)

    def delete(self):
        """
        @description 删除符合条件的所有记录
        @return 影响行数 int
        """
        sql = "DELETE FROM `" + self.table_name + "`" + self.where_sql
        return self.__execute(sql, self.values)

    def getLastSql(self):
        """
        @description 获取最后一次执行保存的sql语句
        @param
        @return sql sql语句 string
        """
        return self.sql

    def __setLastSql(self, sql):
        """
        @description 设置最后一次执行保存的sql语句
        @param sql sql语句 string
        @return
        """
        self.sql = sql

    def limit(self, page=1, page_size=10):
        """
        @description 设置实例对象的分页字符串
        @param page 当前页 int
        @param page_size 分页页数 int
        @return
        """
        self.limit_sql = " LIMIT " + str((page - 1) * page_size) + "," + str(page_size)
        return self

    def order(self, order=""):
        """
        @description  设置查询的排序方式
        @param order 排序字符串 格式 id desc,name asc
        @return
        """
        self.order_sql = " ORDER BY " + order
        return self

    def beginTransaction(self):
        """
        @description 开启事务
        @param
        @return
        """
        self.auto_commit = False

    def commit(self):
        """
        @description 提交事务
        @param
        @return
        """
        # 提交数据库并执行
        self.conn.commit()
        self.auto_commit = True

    def rollback(self):
        """
        @description 事务回滚
        @param
        @return
        """
        self.conn.rollback()
        self.auto_commit = True

    def __clearSql(self):
        """
        @description  清除全局拼接的sql字符串
        @param
        @return
        """
        self.table_name = ""  # 清除执行操作的表名
        self.limit_sql = ""  # 清除分页字符串
        self.order_sql = ""  # 清除排序字符串
        self.where_sql = ""  # 清除查询条件字符串
        self.values = []  # 清除查询条件对应的值
        self.field_sql = "*"  # 清除查询field字段


def main():
    """
    假如有表格user数据如下
    SET FOREIGN_KEY_CHECKS=0;

    -- ----------------------------
    -- Table structure for user
    -- ----------------------------
    DROP TABLE IF EXISTS `user`;
    CREATE TABLE `user` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `name` varchar(255) NOT NULL DEFAULT '',
    PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

    -- ----------------------------
    -- Records of user
    -- ----------------------------
    INSERT INTO `user` VALUES ('1', 'a');
    INSERT INTO `user` VALUES ('2', 'b');
    INSERT INTO `user` VALUES ('3', 'c');
    INSERT INTO `user` VALUES ('4', 'd');
    INSERT INTO `user` VALUES ('5', 'hack');

    下面是使用案例:
    """
    with Db() as db:
        """
        查询方法
        原生语句 SELECT `field1`,`field2` FROM `table_name` WHERE where1 AND where2  OR where3 AND where4
        """
        # 查询一条记录
        res = db.table("user").where({"id": 3, "name": "c"}).field("id,name").find()
        print(res)  # 输出{'id': 3, 'name': 'c'}
        # 简单查询
        res = db.table("user").where({"id": 3, "name": "c"}).field("id,name").select()
        print(res)  # 输出[{'id': 3, 'name': 'c'}]
        # 高级查询
        res = db.table("user").where([["id", ">", 2], ["name", "like", "%c%"]]).field("id,name").select()
        print(res)  # 输出[{'id': 3, 'name': 'c'}, {'id': 5, 'name': 'hack'}]
        # 或查询
        res = db.table("user").where([["id", ">", 2], ["name", "like", "%c%"]], {"name": "b"}).field("id,name").select()
        print(res)  # 输出[{'id': 2, 'name': 'b'}, {'id': 3, 'name': 'c'}, {'id': 5, 'name': 'hack'}]
        # between
        res = db.table("user").where([["id", "between", 3, 5]]).field("id,name").select()
        print(res)  # 输出[{'id': 3, 'name': 'c'}, {'id': 4, 'name': 'd'}, {'id': 5, 'name': 'hack'}]
        # in | not in
        res = db.table("user").where([["id", "in", [3, 4, 5]]]).field("id,name").select()
        print(res)  # 输出[{'id': 3, 'name': 'c'}, {'id': 4, 'name': 'd'}, {'id': 5, 'name': 'hack'}]
        # 分页查询
        res = db.table("user").limit(2, 2).select()
        print(res)  # 输出[{'id': 3, 'name': 'c'}, {'id': 4, 'name': 'd'}]
        # 排序查询
        res = db.table("user").order("id desc,name asc").select()
        print(res)
        # 输出 [{'id': 5, 'name': 'hack'}, {'id': 4, 'name': 'd'}, {'id': 3, 'name': 'c'}, {'id': 2, 'name': 'b'}, {'id': 1, 'name': 'a'}]
        # 原生查询
        res = db.query("SELECT * FROM `user` WHERE `id` = %s AND `name` = %s", [3, "c"])
        print(res)  # 输出[{'id': 3, 'name': 'c'}]
        """
        添加方法
        原生语句  INSERT INTO `table_name` (`field1`,`field2`) VALUES (value1,value2),(value3,value4)
        """
        # 插入一条记录
        res = db.table("user").insert({"name": "e"})  # 返回影响行数
        print(res)  # 输出1
        res = db.table("user").insert({"name": "f"}, True)  # 返回插入的Id值
        print(res)  # 输出7
        # 插入多条记录
        res = db.table("user").insert([{"name": "新增多条"}, {"name": "新增多条"}])  # 返回影响的行数
        print(res)  # 输出2
        """
        修改方法
        原生语句 UPDATE `table_name` SET `field1`=value1,`field2`=value2 WHERE where1 AND where2  OR where3 AND where4
        """
        res = db.table("user").where({"id": 5}).update({"name": "hack"})  # 返回影响的行数
        print(res)  # 输出1 输出0 说明没更新，或更新失败
        """ 
        删除方法
        原生语句 DELETE FROM `table_name` WHERE where1 AND where2  OR where3 AND where4
        """
        res = db.table("user").where([["id", ">", 5]]).delete()  # 返回影响的行数
        print(res)  # 输出4
        """
        事务
        """
        db.beginTransaction()
        try:
            db.table("不存在的表名").where({"id": 1}).update({"name": "事务更新"})  # 返回影响的行数
            db.commit()
        except Exception as result:
            print(result)  # 输出(1146, "Table 'test.不存在的表名' doesn't exist")
            db.rollback()


if __name__ == "__main__":
    main()
```

#### sqlite3
测试的表格如下  
![calc](../images/sqlite3_table_user.png)  

```py
# 导入sqlite3
import sqlite3


class SqliteDb:
    def __init__(self, db_name) -> None:
        """
        @description 初始化Db类
        @param db_name 数据库名称
        @return
        """
        # 创建连接
        self.conn = sqlite3.connect(db_name)
        # 创建游标，操作设置为字典类型
        self.cur = self.conn.cursor()
        # 执行操作的表名
        self.table_name = ""
        # 分页字符串
        self.limit_sql = ""
        # 保存的最后一次执行sql语句
        self.sql = ""
        # 保存查询的排序方式
        self.order_sql = ""
        # 保存的where条件
        self.where_sql = ""
        self.values = []
        # 保存的查询field字段
        self.field_sql = "*"
        # 是否自动提交 否的时候用在执行事务流程上
        self.auto_commit = True

    def __enter__(self):
        """
        @description with语法入口 返回实例化对象
        @param
        @return self 数据库操作对象 Class
        """
        return self

    def __exit__(self, type, value, trace):
        """
        @description with语法出口 每次执行完sql关闭数据库连接 释放资源
        @param
        @return
        """
        # 关闭游标
        self.cur.close()
        # 关闭数据库连接
        self.conn.close()

    def __execute(self, sql, values=None, return_last_row_id=False):
        """
        @description 执行sql语句，并返回影响的行数
        @param sql sql语句 string
        @param values 执行条件值元组 tuple | list
        @param return_last_row_id 是否返回当前游标所在的行id boolean
        @return 影响的行数或游标所在的行id int
        """
        try:
            cursor = self.cur.execute(sql, values)
            if self.auto_commit:
                self.conn.commit()
            if return_last_row_id:
                return self.cur.lastrowid
            else:
                return cursor.rowcount
        except Exception as result:
            raise result
        finally:
            self.__setLastSql(sql)
            self.__clearSql()

    def queryOne(self, sql, values=None):
        """
        @description 执行sql语句，并返回单个结果集
        @param sql sql语句 string
        @param values 查询条件值元组 tuple | list
        @return result 结果集 dict
        """
        try:
            self.cur.execute(sql, values)
            if self.auto_commit:
                self.conn.commit()
            result = self.cur.fetchone()
            return result
        except Exception as result:
            raise result
        finally:
            self.__setLastSql(sql)
            self.__clearSql()

    def query(self, sql, values=None):
        """
        @description 执行sql语句，并返回所有结果集
        @param sql sql语句 string
        @param values 查询条件值元组 tuple | list
        @return result 结果集 dict
        """
        try:
            self.cur.execute(sql, values)
            if self.auto_commit:
                self.conn.commit()
            result = self.cur.fetchall()
            return result
        except Exception as result:
            raise result
        finally:
            self.__setLastSql(sql)
            self.__clearSql()

    def table(self, table_name):
        self.table_name = table_name
        return self

    def where(self, *wheres):
        """
        @description 获取需要的查询条件 和 query等方法中，execute执行需要的传参values
        @param wheres 查询条件 tuple(dict | list)
        @return where_sql 拼接完成的查询条件 string
        @return self db对象
        """
        self.values = []
        self.where_sql = ""
        if wheres:
            self.where_sql = " WHERE "
            for where in wheres:
                if len(self.values) != 0:
                    self.where_sql += "or "
                if where and isinstance(where, dict):
                    # 如果where不为空且是字典传参 等值查找条件
                    for key in where:
                        if len(self.values) != 0 and self.where_sql[-3:-1:1] != "or":
                            self.where_sql += "and "
                        self.where_sql += "`" + key + "`"
                        self.where_sql += "="
                        self.where_sql += "? "
                        self.values.append(where[key])
                elif where and isinstance(where, list):
                    # 如果where不为空且是嵌套列表传参 高级查询
                    for item in where:
                        if len(self.values) != 0 and self.where_sql[-3:-1:1] != "or":
                            self.where_sql += "and "
                        self.where_sql += "`" + item[0] + "`"
                        if item[1] == "between" or item[1] == "BETWEEN":
                            self.where_sql += " BETWEEN ? AND ? "
                            self.values.append(item[2])
                            self.values.append(item[3])
                        elif item[1] == "in" or item[1] == "IN":
                            sub_where_sql = ""
                            for item_value in item[2]:
                                if sub_where_sql:
                                    sub_where_sql += ","
                                sub_where_sql += "?"
                                self.values.append(item_value)
                            self.where_sql += " IN (" + sub_where_sql + ")"
                        elif item[1] == "not in" or item[1] == "NOT IN":
                            sub_where_sql = ""
                            for item_value in item[2]:
                                if sub_where_sql:
                                    sub_where_sql += ","
                                sub_where_sql += "?"
                                self.values.append(item_value)
                            self.where_sql += " NOT IN (" + sub_where_sql + ")"
                        else:
                            self.where_sql += " " + item[1] + " "
                            self.where_sql += "? "
                            self.values.append(item[2])
                else:
                    continue
        return self

    def field(self, field):
        """
        @description  根据传入的field组装成带``的field
        @param field 逗号分隔的字段字符串 string
        @return self db对象
        """
        if field != "*":
            # 按英文逗号拆分后，用引号包裹组装
            list = field.split(",")
            field_list = []
            for item in list:
                field_list.append("`" + item + "`")
            self.field_sql = ",".join(field_list)
        return self

    def __getFieldValues(self, data=None):
        """
        @description 根据原始数据组装要插入的数据Sql语句
        @param data 插入的数据 dict | list
        @return value_sql 拼接完成的insert into value字符串 string
        @return values execute所需参数 list
        """
        field_str = ""  # 字段字符串
        value_str = ""  # ?拼接的字符串
        value_sql = ""  # 拼接完成的insert into value字符串
        values = []  # execute所需参数
        if data:
            if isinstance(data, dict):
                # 插入一条记录
                for key in data:
                    if len(values) != 0:
                        field_str += ","
                        value_str += ","
                    field_str += "`" + key + "`"
                    value_str += "?"
                    values.append(data[key])
                value_sql = "(" + field_str + ") VALUES " + "(" + value_str + ")"
            if isinstance(data, list):
                # 插入多条记录
                # 只组装一遍字段名称
                for key in data[0]:
                    if field_str:
                        field_str += ","
                    field_str += "`" + key + "`"
                for item in data:
                    # 临时变量 单个Value (?,?,?···)
                    value_str_sub = ""
                    for key in item:
                        if len(value_str_sub) != 0:
                            value_str_sub += ","
                        value_str_sub += "?"
                        values.append(item[key])
                    if value_str:
                        value_str += ","
                    value_str += "(" + value_str_sub + ")"
                value_sql = "(" + field_str + ") VALUES " + value_str
        return value_sql, values

    def __getSetField(self, data):
        """
        @description 根据原始数据组装要更新的update set table 【field】部分的Sql语句
        @param data 更新的数据 dict
        @return field 更新设置field部分的sql语句  string
        @return values execute所需参数 list
        """
        field = ""
        values = []
        if data:
            for key in data:
                if len(values) != 0:
                    field += ","
                field += "`" + key + "`=?"
                values.append(data[key])
        return field, values

    def find(self):
        """
        @description 查询符合条件的一条记录
        @return 结果集 dict
        """
        sql = (
            "SELECT "
            + self.field_sql
            + " FROM `"
            + self.table_name
            + "`"
            + self.where_sql
            + self.limit_sql
            + self.order_sql
        )
        return self.queryOne(sql, self.values)

    def select(self):
        """
        @description 查询符合条件的所有记录
        @return 结果集 dict
        """
        sql = (
            "SELECT "
            + self.field_sql
            + " FROM `"
            + self.table_name
            + "`"
            + self.where_sql
            + self.limit_sql
            + self.order_sql
        )
        return self.query(sql, self.values)

    def insert(self, data, return_last_row_id=False):
        """
        @description 插入一条记录
        @param data 插入的数据 dict | list
        @param return_last_row_id 是否返回当前游标所在的行id boolean
        @return 影响行数或游标所在id int
        """
        value_sql, values = self.__getFieldValues(data)
        sql = "INSERT INTO `" + self.table_name + "` " + value_sql
        return self.__execute(sql, values, return_last_row_id)

    def update(self, data):
        """
        @description 更新符合条件的所有记录的多个字段
        @param data 更新的数据 dict
        @return 影响行数 int
        """
        field, field_values = self.__getSetField(data)
        # 注意field在前 where在后
        values = field_values + self.values
        sql = "UPDATE `" + self.table_name + "` SET " + field + self.where_sql
        return self.__execute(sql, values)

    def delete(self):
        """
        @description 删除符合条件的所有记录
        @return 影响行数 int
        """
        sql = "DELETE FROM `" + self.table_name + "`" + self.where_sql
        return self.__execute(sql, self.values)

    def getLastSql(self):
        """
        @description 获取最后一次执行保存的sql语句
        @param
        @return sql sql语句 string
        """
        return self.sql

    def __setLastSql(self, sql):
        """
        @description 设置最后一次执行保存的sql语句
        @param sql sql语句 string
        @return
        """
        self.sql = sql

    def limit(self, page=1, page_size=10):
        """
        @description 设置实例对象的分页字符串
        @param page 当前页 int
        @param page_size 分页页数 int
        @return
        """
        self.limit_sql = " LIMIT " + str((page - 1) * page_size) + "," + str(page_size)
        return self

    def order(self, order=""):
        """
        @description  设置查询的排序方式
        @param order 排序字符串 格式 id desc,name asc
        @return
        """
        self.order_sql = " ORDER BY " + order
        return self

    def beginTransaction(self):
        """
        @description 开启事务
        @param
        @return
        """
        self.auto_commit = False

    def commit(self):
        """
        @description 提交事务
        @param
        @return
        """
        # 提交数据库并执行
        self.conn.commit()
        self.auto_commit = True

    def rollback(self):
        """
        @description 事务回滚
        @param
        @return
        """
        self.conn.rollback()
        self.auto_commit = True

    def __clearSql(self):
        """
        @description  清除全局拼接的sql字符串
        @param
        @return
        """
        self.table_name = ""  # 清除执行操作的表名
        self.limit_sql = ""  # 清除分页字符串
        self.order_sql = ""  # 清除排序字符串
        self.where_sql = ""  # 清除查询条件字符串
        self.values = []  # 清除查询条件对应的值
        self.field_sql = "*"  # 清除查询field字段


def main():
    """
    用法  手动在当前路径下db文件夹创建database.db文件
    """
    with SqliteDb("./db/database.db") as db:
        """
        查询方法
        原生语句 SELECT `field1`,`field2` FROM `table_name` WHERE where1 AND where2  OR where3 AND where4
        """
        # 查询一条记录
        res = db.table("user").where({"id": 3, "name": "c"}).field("id,name").find()
        print(res)  # 输出(3, 'c')
        # 简单查询
        res = db.table("user").where({"id": 3, "name": "c"}).field("id,name").select()
        print(res)  # 输出[(3, 'c')]
        # 高级查询
        res = db.table("user").where([["id", ">", 2], ["name", "like", "%c%"]]).field("id,name").select()
        print(res)  # 输出[(3, 'c'), (5, 'hack')]
        # 或查询
        res = db.table("user").where([["id", ">", 2], ["name", "like", "%c%"]], {"name": "b"}).field("id,name").select()
        print(res)  # 输出[(2, 'b'), (3, 'c'), (5, 'hack')]
        # between
        res = db.table("user").where([["id", "between", 3, 5]]).field("id,name").select()
        print(res)  # 输出[(3, 'c'), (4, 'd'), (5, 'hack')]
        # in | not in
        res = db.table("user").where([["id", "in", [3, 4, 5]]]).field("id,name").select()
        print(res)  # 输出[(3, 'c'), (4, 'd'), (5, 'hack')]
        # 分页查询
        res = db.table("user").limit(2, 2).select()
        print(res)  # 输出[(3, 'c'), (4, 'd')]
        # 排序查询
        res = db.table("user").order("id desc,name asc").select()
        print(res)
        # 输出 [(5, 'hack'), (4, 'd'), (3, 'c'), (2, 'b'), (1, 'a')]
        # 原生查询
        res = db.query("SELECT * FROM `user` WHERE `id` = ? AND `name` = ?", [3, "c"])
        print(res)  # 输出[(3, 'c')]
        """
        添加方法
        原生语句  INSERT INTO `table_name` (`field1`,`field2`) VALUES (value1,value2),(value3,value4)
        """
        # 插入一条记录   self.cur.execute返回Cursor对象 里面有好多属性的 例如 lastrowid row_factory  rowcount 其中rowcount就是影响的行数
        res = db.table("user").insert({"name": "e"})
        print(res)  # 输出1
        res = db.table("user").insert({"name": "f"}, True)  # 返回插入的Id值
        print(res)  # 输出7
        # 插入多条记录
        res = db.table("user").insert([{"name": "新增多条"}, {"name": "新增多条"}])  # 返回影响的行数
        print(res)  # 输出2
        """
        修改方法
        原生语句 UPDATE `table_name` SET `field1`=value1,`field2`=value2 WHERE where1 AND where2  OR where3 AND where4
        """
        res = db.table("user").where({"id": 5}).update({"name": "hack"})  # 返回影响的行数
        print(res)  # 输出1 其他就是更新失败，这里不输出0  不更新也输出1
        """
        删除方法
        原生语句 DELETE FROM `table_name` WHERE where1 AND where2  OR where3 AND where4
        """
        res = db.table("user").where([["id", ">", 5]]).delete()  # 返回影响的行数
        print(res)  # 输出4
        """
        事务
        """
        db.beginTransaction()
        try:
            db.table("不存在的表名").where({"id": 1}).update({"name": "事务更新"})  # 返回影响的行数
            db.commit()
        except Exception as result:
            print(result)  # 输出no such table: 不存在的表名
            db.rollback()


if __name__ == "__main__":
    main()
```

### 内置模块

#### random随机数
```py
import random


def main():
    # 随机浮点数 0<=n<=100
    print(random.random() * 100)
    # 范围内的随机浮点数 0<=n<=100
    print(random.uniform(0, 100))
    # 随机整数 0<=n<=100
    print(random.randint(0, 100))  # 输出0~100中随机的一个数
    # 范围内的随机整数 0<=n<100
    print(random.randrange(0, 101, 50))  # 只能输出 0 50 100 三个数中的一个
    # 定义一个列表
    list = ["a", "b", "c", "d", "e"]
    # 随机取一个数
    print(random.choice(list))  # 输出是随机的 如c
    # 将列表随机打乱
    random.shuffle(list)
    print(list)  # 输出是随机的 如['d', 'c', 'e', 'a', 'b']
    # 取指定个数的随机列表
    print(random.sample(list, 3))  # 输出是随机的 如['c', 'b', 'd']


if __name__ == "__main__":
    main()
```
!> randint产生的随机数包含左右边界，randrange只包含左边界，且randrange可以设定步长  
shuffle会改变原数据源 sample不会

#### Math数学方法
数学方法很多,没有的参照[这里](https://docs.python.org/zh-cn/3/library/math.html)
```py
import math


def main():
    # 绝对值
    print(abs(-10))  # 输出10
    # 最大值
    print(max([1, 2, 3, 4, 5]))  # 输出5
    # 最小值
    print(min([1, 2, 3, 4, 5]))  # 输出1
    # n的n次方
    print(pow(2, 2))  # 输出4
    # n的n次方
    print(3 ** 3)  # 输出27
    # 四舍五入 并保留2位小数
    print(round(3.1415, 3))  # 输出3.142
    # 向上取整
    print(math.ceil(3.4))  # 输出4
    # 向下取整
    print(math.floor(3.4))  # 输出3
    # 平方根
    print(int(math.sqrt(4)))  # 输出2
    # sin函数
    print(math.sin(90))  # 输出0.8939966636005579


if __name__ == "__main__":
    main()
```

#### os操作系统
基本是操作文件和执行shell命令有用
```py
import os


def main():
    # 获取操作系统平台
    print(os.name)  # 输出nt
    # 获取当前文件所在目录
    print(os.getcwd())  # 输出D:\···
    # 列出指定目录下的所有文件名和目录名 默认输出当前目录下的所有文件名和目录名
    print(os.listdir())  # 输出D盘下的所有文件名和目录名
    # 运行shell命令
    os.system("cd.>1.txt")  # os.system("cls") 清理屏幕输出
    # 删除文件
    try:
        os.remove("1.txt")
    except FileNotFoundError:
        print("文件找不到")
    # 检查目录是否存在
    print(os.path.exists(os.getcwd()))  # 输出True
    # 判断是否是文件
    print(os.path.isfile("1.txt"))  # 输出False
    # 判断是否是文件夹
    print(os.path.isdir(os.getcwd()))  # 输出True
    # 获取绝对路径
    print(os.path.abspath("1.txt"))  # 输出以传入的文件名和当前绝对路径组合的完整路径
    # 输出D:\···\1.txt
    # 分离文件名与扩展名
    print(os.path.splitext("1.txt")[1])  # 输出.txt   完整返回('1', '.txt')
    print("." + "1.txt".split(".")[1])  # 输出.txt
    # 拆分路径为 目录+文件名
    print(os.path.split("D:\\test\\1.txt"))  # 输出('D:\\test', '1.txt')
    # 连接目录和文件名
    print(os.path.join("D:\\test", "1.txt"))  # 输出D:\test\1.txt
    print("D:\\test" + "\\" + "1.txt")  # 输出D:\test\1.txt
    # 获得文件名
    print(os.path.basename("D:\\test\\1.txt"))  # 输出1.txt
    # 获得文件路径
    print(os.path.dirname("D:\\test\\1.txt"))  # 输出D:\test


if __name__ == "__main__":
    main()
```

#### 日期和时间
```py
# 导入日期处理库
from datetime import datetime as dt

# 导入时间处理库
import time


def main():
    """ datetime """
    # 输出当前日期时间并格式化 推荐
    print(dt.now().strftime("%Y-%m-%d %H:%M:%S"))  # 输出2021-05-06 08:52:19
    # 日期转换为时间戳 不推荐
    print(dt(2020, 5, 6, 8, 52, 19).timestamp())  # 输出 1588726339.0
    # 时间戳转换为日期 推荐
    print(dt.fromtimestamp(1588726339.0))  # 输出2020-05-06 08:52:19
    # 日期转换成datetime类型
    print(type(dt.strptime("2020-05-06 08:52:19", "%Y-%m-%d %H:%M:%S")))  # 输出<class 'datetime.datetime'>
    """ time """
    # 输出当前时间戳 推荐
    print(time.time())  # 输出1620263243.0934012
    # 输出当前日期时间并格式化 不推荐
    print(time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()))  # 输出2021-05-06 09:39:09
    # 日期转换成时间戳 推荐
    print(time.mktime(time.strptime("2020-05-06 08:52:19", "%Y-%m-%d %H:%M:%S")))  # 输出1588726339.0
    # 时间戳转换成日期 不推荐
    print(time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(1588726339.0)))  # 输出2020-05-06 08:52:19
    # 睡眠1秒
    time.sleep(1)


if __name__ == "__main__":
    main()
```

#### 加密模块
```py
# 各种hash算法 如md5 sha1
import hashlib

# key+md5的算法
import hmac


def main():
    # md5加密
    print(hashlib.md5("123456".encode("utf-8")).hexdigest())  # 输出e10adc3949ba59abbe56e057f20f883e
    # sha1加密
    print(hashlib.sha1("123456".encode("utf-8")).hexdigest())  # 输出7c4a8d09ca3762af61e59520943dc26494f8941b
    # sha256加密
    print(hashlib.sha256("123456".encode("utf-8")).hexdigest())
    # 输出8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
    # hmac算法
    print(hmac.new("keystr".encode("utf-8"), "123456".encode("utf-8"), digestmod="MD5").hexdigest())
    # 输出185a0a1af7ec0be8a71b6aba67f687ba
    print(hmac.new(b"keystr", b"123456", digestmod="MD5").hexdigest())  # 输出185a0a1af7ec0be8a71b6aba67f687ba
    # b转换成bytes和encode编码转换成bytes效果等同,传参都是需要二进制的


if __name__ == "__main__":
    main()
```

#### 堆模块排序

**堆的定义 第i个数总是大于第i/2处的元素**

```py
# 导入堆模块
import heapq


def main():
    # 定义一个乱序列表
    list = [2, 3, 4, 5, 1, 6, 7, 9, 8]
    # 找到堆里最大的几个数 不转换成堆调用也正确
    print(heapq.nlargest(2, list))  # 输出[9,8]
    # 找到堆里最小的几个数 不转换成堆调用也正确
    print(heapq.nsmallest(2, list))  # 输出[1,2]
    # 首先要转换为堆
    heapq.heapify(list)
    # 入堆
    heapq.heappush(list, 10)
    # 出堆并生成列表看排序结果  输出[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    print([heapq.heappop(list) for _ in range(len(list))])

    # 将多个排序号的列表，从小到大合并成一个排序 没啥用的
    list1 = [32, 3, 5, 34, 54, 23, 132]
    list2 = [23, 2, 12, 656, 324, 23, 54]
    list1 = sorted(list1)
    list2 = sorted(list2)

    list = heapq.merge(list1, list2)  # 返回生成器  generator object merge
    # 输出 [2, 3, 5, 12, 23, 23, 23, 32, 34, 54, 54, 132, 324, 656]
    print([item for item in list])

    # 更简单的
    list = sorted(list1 + list2)
    print(list)  # 输出 [2, 3, 5, 12, 23, 23, 23, 32, 34, 54, 54, 132, 324, 656]

    p = lambda x, y: x + y
    print(p(4, 6))

    # 定义一个列表嵌套字典的
    list = [
        {"name": "IBM", "shares": 100, "price": 91.1},
        {"name": "AAPL", "shares": 50, "price": 543.22},
        {"name": "FB", "shares": 200, "price": 21.09},
        {"name": "HPQ", "shares": 35, "price": 31.75},
        {"name": "YHOO", "shares": 45, "price": 16.35},
        {"name": "ACME", "shares": 75, "price": 115.65},
    ]
    # 取出价格最大的2项
    print(heapq.nlargest(2, list, key=lambda x: x["price"]))
    # 输出 [{'name': 'AAPL', 'shares': 50, 'price': 543.22}, {'name': 'ACME', 'shares': 75, 'price': 115.65}]
    # 等同于下面的方式
    """  
    def fn(x):
        return x["price"]

    print(heapq.nlargest(2, list, key=fn))
    """
    # 取出分享最少的两项
    print(heapq.nlargest(2, list, key=lambda x: x["shares"]))
    # 输出[{'name': 'FB', 'shares': 200, 'price': 21.09}, {'name': 'IBM', 'shares': 100, 'price': 91.1}]


if __name__ == "__main__":
    main()
```

#### 迭代工具
```py
# 导入迭代工具模块
import itertools


def main():
    # count(num) 是无限序列数的迭代器,从num开始无限加1
    num = itertools.count(0)

    for item in num:
        print(item)  # 一直不停的输出下去 0,1,2,3,···
        # 手动让它终止
        if item == 100:
            break

    # cycle 无限重复输出序列 一下几种等效
    # str = itertools.cycle(["a", "b", "c"])  # 也可以直接传其他的字典对象字符串都可以算序列
    # str = itertools.cycle({"a", "b", "c"})
    # str = itertools.cycle(("a", "b", "c"))
    # str = itertools.cycle({"a": 1, "b": 2, "c": 3})
    str = itertools.cycle("abc")
    i = 0
    for item in str:
        print(item)  # 一直不停的输出a b c a b c a ···
        i += 1
        # 手动让它终止
        if i == 100:
            break

    # repeat 无限重复输出指定元素，但可以指定次数
    str = itertools.repeat("abc", 3)
    for item in str:
        print(item)  # 输出abc  abc  abc

    # combinations产生3选2组合 相当于C-3-2
    str = itertools.combinations("abc", 2)
    for item in str:
        print(item)  # 输出 ('a', 'b') ('a', 'c') ('b', 'c')

    # permutations 输出全排列 相当于A-3-3
    str = itertools.permutations("abc", 3)
    for item in str:
        print(item)
    # 输出
    # ('a', 'b', 'c')
    # ('a', 'c', 'b')
    # ('b', 'a', 'c')
    # ('b', 'c', 'a')
    # ('c', 'a', 'b')
    # ('c', 'b', 'a')


if __name__ == "__main__":
    main()
```

**分组**
```py
from itertools import groupby


def main():
    # 定义一个字典列表
    lists = [
        {"name": "老二", "age": 18},
        {"name": "老三", "age": 18},
        {"name": "老大", "age": 50},
        {"name": "老四", "age": 15},
        {"name": "老五", "age": 15},
    ]
    # 先进行排序，sort()会改变原列表
    lists.sort(key=lambda x: x["age"])
    # 再进行分租
    for key, group in groupby(lists, lambda x: x["age"]):
        for g in group:
            print(key, g)
    # 输出
    # 15 {'name': '老四', 'age': 15}
    # 15 {'name': '老五', 'age': 15}
    # 18 {'name': '老二', 'age': 18}
    # 18 {'name': '老三', 'age': 18}
    # 50 {'name': '老大', 'age': 50}


if __name__ == "__main__":
    main()
```

#### 集合

```py
from collections import deque, Counter


def main():
    """ 高效率操作列表 """
    # deque底层是双向链表，因此当你需要在头尾添加和删除元素是，deque会表现出更好的性能
    list = [item for item in range(1, 10)]
    # 创建链表
    dq = deque(list)
    # 往链表最后添加一个值
    dq.append(10)
    # 往链表最左边添加一个值
    dq.appendleft(0)
    print(dq)  # 输出deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

    """ 统计字符出现的个数 这个有用"""
    # Counter
    list = ["小明", "小红", "小张", "小明", "小红", "小明"]
    counter = Counter(list)
    # 找出元素出现个数最多的2个
    print(counter.most_common(2))  # 输出[('小明', 3), ('小红', 2)]
    # 统计每个元素出现的次数
    print(counter)  # 输出Counter({'小明': 3, '小红': 2, '小张': 1})
    print(counter["小明"])  # 输出3


if __name__ == "__main__":
    main()
```

#### 定时器功能实现
```py
from threading import Timer

# 定时器函数
def fn(text, i=0):
    i+=1
    t = Timer(1, fn, (text,i,))
    t.start()
    print(text)
    if i == 10:
        t.cancel()


def main():
    fn("1秒输出1次")


if __name__ == "__main__":
    main()
```

### 内置函数
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

#### type()
检查变量
```py
a = 3
print(type(a))
```
输出<class 'int'>

#### 类型转换
- int()：将一个数值或字符串转换成整数，可以指定进制。
- float()：将一个字符串转换成浮点数。
- str()：将指定的对象转换成字符串形式，可以指定编码。
- chr()：将整数转换成该编码对应的字符串（一个字符）。
- ord()：将字符串（一个字符）转换成对应的编码（整数）。

#### 输入和输出函数
输入input("请输入：")
输出print("输出")
```py
a = int(input("a="))
b = int(input("b="))
c = input("c=")
d = float(input("d="))
print("您输入了：a=%d,b=%d,c=%s,d=%f,a+b=%d" % (a, b, c, d, a + b))  # 占位符替换
print(
    f"您输入了：a={a},b={b},c={c},d={d:.3f},a+b={a+b}"
)  ## print(f"")里的f是格式化format的意思   :.3f表示保留三位
```
上面的例子包含了input和print的用法，还有占位符%,输出字符f的用法,推荐f用法，简单高效
输出
```powershell
您输入了：a=1,b=2,c=3,d=4.000000,a+b=3
a=1,b=2,c=3,d=4.000,a+b=3
```

#### 计算长度
```py
str = "hello world"
arr = [1, 2, 3, 4]
print(len(str))  # 输出11
print(len(arr))  # 输出4
```

#### map函数
```py
from functools import reduce


def main():
    lists = [item for item in range(1, 10)]
    # map函数 对每个列表元素进行处理 并返回新的列表
    # map(fn,list) 第一个参数是个函数 第二个参数是迭代器
    lists = list(map(lambda x: x * x, lists))  # 输出[1, 4, 9, 16, 25, 36, 49, 64, 81]
    print(lists)
    # 推导式可以等效于map 推导式更简洁
    # 输出[1, 4, 9, 16, 25, 36, 49, 64, 81]
    lists = [item * item for item in range(1, 10)]
    print(lists)
    """ 
    lambda x: x * x 等效于
    def fn(x):
        return x * x
    """

    # map实例 将英文单词转换为驼峰式
    lists = ["adam", "LISA", "barT"]
    lists = list(map(lambda x: x[0:1:1].upper() + x[1::1].lower(), lists))
    print(lists)  # 输出['Adam', 'Lisa', 'Bart']


if __name__ == "__main__":
    main()
```

#### reduce累加器,累乘器
```py
from functools import reduce


def main():
    lists = [item for item in range(1, 11)]
    # 利用reduce 求1-10的和 这里做一下示例，其实可以直接用sum
    # reduce就是把结果继续和序列的下一个元素做累积计算 相当于 fn(fn(fn(x1, x2), x3), x4)
    print(reduce(lambda x, y: x + y, lists))  # 输出55
    print(sum(lists))  # 输出55

    # reduce实例 求1~10的积
    lists = [item for item in range(1, 11)]
    print(reduce(lambda x, y: x * y, lists))  # 输出3628800


if __name__ == "__main__":
    main()
```

#### filter过滤器
```py
def main():
    lists = [item for item in range(100)]
    # 找出所有偶数
    lists = list(filter(lambda x: x % 2 == 0, lists))
    print(lists)
    # 输出[0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36,
    # 38, 40, 42, 44, 46, 48, 50, 52, 54, 56, 58, 60, 62, 64, 66, 68, 70, 72, 74,
    #  76, 78, 80, 82, 84, 86, 88, 90, 92, 94, 96, 98]


if __name__ == "__main__":
    main()
```

#### sorted排序
```py
import random


def main():
    # 定义一个随机数列表
    lists = [random.randint(0, 10) for item in range(10)]
    # 按值从小到大排序
    print(sorted(lists))  # 输出类似[0, 3, 3, 3, 5, 5, 5, 8, 8, 9]

    # 定义一个列表
    lists = [1, -2, -4, 3, 5]
    # 按绝对值从大到小排序
    print(sorted(lists, key=lambda x: abs(x), reverse=True))  # 输出[5, -4, 3, -2, 1]
    # 简写
    print(sorted(lists, key=abs, reverse=True))  # 输出[5, -4, 3, -2, 1]

    # 定义一个字典列表
    lists = [
        {"name": "老二", "age": 18},
        {"name": "老三", "age": 18},
        {"name": "老大", "age": 50},
        {"name": "老四", "age": 15},
        {"name": "老五", "age": 15},
    ]
    # 按年龄从小到大排序
    print(sorted(lists, key=lambda x: x["age"]))
    # 输出[{'name': '老四', 'age': 15}, {'name': '老五', 'age': 15}, {'name': '老二', 'age': 18}, {'name': '老三', 'age': 18}, {'name': '老大', 'age': 50}]


if __name__ == "__main__":
    main()
```

### pip
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

***pip*** python工具库安装工具
#### 查看版本号
```
pip -V
```

#### 查看已经安装的第三方库
```
pip list
```

#### 查看可以更新的库
```
pip list --outdated --trusted-host mirrors.aliyun.com
```

#### 更新库命令
```
pip install --upgrade 库名称
```

#### 切换安装源
echo %APPDATA% 路径下创建pip文件夹
创建pip.ini文件
内容
```ini
[global]
index-url=http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com
```

#### pip升级
```powershell
pip install --upgrade pip
```
!> 如果升级出现错误ModuleNotFoundError: No module named 'pip'
```powershell
python -m ensurepip
python -m pip install --upgrade pip
```
[找到其来源](https://docs.python.org/3/library/ensurepip.html#command-line-interface)

#### 代码格式化库black
```powershell
pip install black
```
vscode格式化配置
```json
{
    "python.pythonPath": "D:\\sxc\\python3\\python.exe",//python.exe绝对路径
    "python.formatting.provider": "black",//python格式化工具
    "editor.formatOnSave": true, //保存时自动格式化
    "python.formatting.blackArgs": ["--line-length", "120"]
}
```

### 别人遇到的坑

#### 嵌套列表的坑
<p align="left" style="color:#777777;">发布日期：2021-04-27</p>

```py
from random import randint


def main():
    # 简单错误案例
    list = [0, 0, 0]
    list = [list] * 2  # 这个过程种的list指向的内存地址是同一个
    list[0][0] = 1  # 只改了一个索引
    print(list)  # 输出[[1, 0, 0], [1, 0, 0]]   只改了一个索引 另外一个也跟着变了
    # 正确写法
    list = [[0] * 3 for _ in range(2)]
    list[0][0] = 1
    print(list)  # 输出 [[1, 0, 0], [0, 0, 0]]

    # 复杂错误案例
    list = [[0] * 3] * 5
    for index, item in enumerate(list):
        for key, value in enumerate(item):
            list[index][key] = randint(0, 100)
    # 会输出类似的[[64, 39, 44], [64, 39, 44], [64, 39, 44], [64, 39, 44], [64, 39, 44]]，发现每个数都是一样的
    print(list)
    # 正确写法
    list = [[0] * 3 for _ in range(5)]
    for index, item in enumerate(list):
        for key, value in enumerate(item):
            list[index][key] = randint(0, 100)
    # 会输出[[12, 51, 68], [83, 83, 55], [0, 81, 29], [75, 33, 25], [60, 80, 60]] 发现这个是正确的
    print(list)


if __name__ == "__main__":
    main()
```

### 爬虫

#### beautifulsoup4
- 安装
    ```powershell
    pip install beautifulsoup4
    pip install lxml
    ```
- 使用
    ```py
    # 导入BeautifulSoup类库
    from bs4 import BeautifulSoup

    # 导入requests
    import requests
    
    def main():
        request_url = "http://bang.dangdang.com/books/fivestars/01.00.00.00.00.00-recent30-0-0-1-1"
        res = requests.get(request_url)
        # 创建BeautifulSoup对象 使用lxml解析器
        soup = BeautifulSoup(res.text, "lxml")
        # 获取页面标题
        print(soup.select_one("title"))  # 输出<title>【5星图书】畅销榜-近30日5星图书榜-当当5星图书排行榜</title>
        print(soup.select_one("title").name)  # 输出title
        print(soup.select_one("title").text)  # 输出 【5星图书】畅销榜-近30日5星图书榜-当当5星图书排行榜
        # 获取属性
        for item in soup.select(".bang_list .name a"):
            print(item["title"])  # 循环输出书名 也可用item.get("title")
        # 用属性获取内容
        for item in soup.select("div[class='list_num red']"):
            print(item.text)  # 输出了前三名的排行数字

    if __name__ == "__main__":
        main()
    ```

#### 爬取当当网top500书籍
```py
# 导入网络请求库
import requests

# 导入时间库
import time

# 导入正则表达库
import re

# 导入多线程
from threading import Thread, Lock

# 导入excel处理库
from openpyxl import Workbook, load_workbook

lock = Lock()  # 初始化锁

# 请求线程类
class Request(Thread):
    def __init__(self, url_list, response_list, index):
        """
        @description 初始化方法
        @param url_list list 请求的url列表
        @param response_list list 返回的数据列表
        @param index 返回列表的的起始索引
        @return
        """
        super().__init__()
        self.__url_list = url_list
        self.__response_list = response_list
        self.__index = index

    def run(self):
        # 多线程同时请求url
        print(f"线程{self.__index + 1}启动")
        for key, value in enumerate(self.__url_list):
            print(f"请求{self.__index + 1}-{key}发出")
            res = requests.get(value)
            # 正则匹配每个网站的标题
            match = re.findall(
                r"<div class=\"list_num.*?\">(\d+).</div>.*?<div class=\"name\">.*?title=\"(.*?)\">.*?</a></div>.*?<span class=\"price_n\">&yen;(.*?)</span>",
                res.text,
                re.S | re.I | re.M,
            )
            # 会自动调用release是否锁
            with lock:
                # 将获取的内容存入list.
                data = []
                for item in match:
                    data.append(
                        {
                            "rank": item[0],
                            "title": item[1],
                            "price": item[2],
                        }
                    )
                self.__response_list[self.__index + key] = data
            # 设置请求间隔
            time.sleep(0.01)


def main():
    # 请求当前地址内容
    base_url = "http://bang.dangdang.com/books/fivestars/01.00.00.00.00.00-recent30-0-0-1-"
    # 生成器生成url列表
    base_url_list = [base_url + str(item) for item in range(1, 26)]
    # url分组 10个为1组,分成3组  每组用一个线程去处理
    request_url_list = []
    for i in range(0, len(base_url_list), 10):
        request_url_list.append(base_url_list[i : i + 10])

    # 最终内容存入list，初始化一个26长度的list，用于后面赋值到指定位置
    response_list = [i for i in range(1, 26)]
    Threads = []
    for key, value in enumerate(request_url_list):
        t = Request(value, response_list, key * 10)
        Threads.append(t)
        t.start()
    # 等待所有线程结束
    for t in Threads:
        t.join()
    # 输出结果
    # for item in response_list:
    #     for book in item:
    #         print(book["rank"] + "." + book["title"] + " ￥" + book["price"] + "\n")
    # 写入excel
    # 初始化工作簿对象
    wb = Workbook()
    # 获取当前激活的worksheet
    ws = wb.active
    ws["A1"] = "排名"
    ws["B1"] = "书名"
    ws["C1"] = "折后价格"
    i = 2
    for item in response_list:
        for book in item:
            ws.cell(i, 1, book["rank"])
            ws.cell(i, 2, book["title"])
            ws.cell(i, 3, book["price"])
            i += 1
    # excel覆盖保存文件到当前文件夹
    wb.save("当当网500本书抓取记录.xlsx")


if __name__ == "__main__":
    main()
```

#### 爬取人才网所有职位
```py
# 导入网络请求库
import requests

# 导入时间库
import time

# 导入正则表达库
import re

# 导入多线程
from threading import Thread, Lock

# 导入excel处理库
from openpyxl import Workbook, load_workbook

# 导入BeautifulSoup类库
from bs4 import BeautifulSoup

# 随机数
import random

lock = Lock()  # 初始化锁

# 代理IP池 免费的代理IP没用
proxies = [
    "http://27.191.60.25:3256",
    "http://182.84.145.191:3256",
    "http://58.255.6.214:9999",
    "http://60.168.80.91:1133",
    "http://70.37.165.170:3218",
]

# 请求线程类
class Request(Thread):
    def __init__(self, url_list, response_list, index):
        """
        @description 初始化方法
        @param url_list list 请求的url列表
        @param response_list list 返回的数据列表
        @param index 返回列表的的起始索引
        @return
        """
        super().__init__()
        self.__url_list = url_list
        self.__response_list = response_list
        self.__index = index

    def run(self):
        # 多线程同时请求url
        print(f"线程{self.__index + 1}启动")
        for key, value in enumerate(self.__url_list):
            print(f"请求{self.__index + 1}-{key}发出")
            res = requests.get(value, proxies={"http": random.choice(proxies)})

            # 创建BeautifulSoup对象 使用lxml解析器
            soup = BeautifulSoup(res.text, "lxml")

            # 会自动调用release是否锁
            with lock:
                # 将获取的内容存入list.
                data = []
                # 循环获取需要的内容
                for item in soup.select(".search_job_list"):
                    child_soup = BeautifulSoup(str(item), "lxml")
                    address = ""
                    experience = ""
                    education = ""
                    if len(child_soup.select(".com_search_job_em")) > 0:
                        address = child_soup.select(".com_search_job_em")[0].get_text()
                    if len(child_soup.select(".com_search_job_em")) > 1:
                        if child_soup.select(".com_search_job_em")[1].get_text().find("经验") != -1:
                            experience = child_soup.select(".com_search_job_em")[1].get_text()
                        if child_soup.select(".com_search_job_em")[1].get_text().find("学历") != -1:
                            education = child_soup.select(".com_search_job_em")[1].get_text()
                    if len(child_soup.select(".com_search_job_em")) > 2:
                        education = child_soup.select(".com_search_job_em")[2].get_text()
                    data.append(
                        {
                            "title": child_soup.select_one(".yunjoblist_newname a")["title"],
                            "money": re.sub(
                                r"-\d+元", "", child_soup.select_one(".yunjoblist_newxz").text, 1, re.I | re.M
                            ),
                            "company": child_soup.select_one(".search_job_com_name")["title"],
                            "address": address,
                            "experience": experience,
                            "education": education,
                            "time": child_soup.select_one(".yunjoblist_new_time span").text
                            if child_soup.select_one(".yunjoblist_new_time span")
                            else "置顶",
                        }
                    )
                self.__response_list[self.__index + key] = data
            # 设置请求间隔
            time.sleep(0.5)


def main():
    # 请求当前地址内容
    base_url = "https://www.hnrcrs.com/job/index.php?city=830000&all=0_0_0_0_0_0_0_0&tp=0&page="
    # 生成器生成url列表
    base_url_list = [base_url + str(item) for item in range(1, 251)]
    # url分组 10个为1组,分成 len/10 组  每组用一个线程去处理
    request_url_list = []
    for i in range(0, len(base_url_list), 10):
        request_url_list.append(base_url_list[i : i + 10])

    # 最终内容存入list，初始化一个len长度的list，用于后面赋值到指定位置
    response_list = [i for i in range(1, 251)]
    Threads = []
    for key, value in enumerate(request_url_list):
        t = Request(value, response_list, key * 10)
        Threads.append(t)
        t.start()
    # 等待所有线程结束
    for t in Threads:
        t.join()
    # 输出结果
    # for items in response_list:
    #     for item in items:
    #         print(item["title"])
    # # 写入excel
    # # 初始化工作簿对象
    wb = Workbook()
    # 获取当前激活的worksheet
    ws = wb.active
    ws["A1"] = "职位"
    ws["B1"] = "薪水"
    ws["C1"] = "公司"
    ws["D1"] = "地址"
    ws["E1"] = "经验"
    ws["F1"] = "学历"
    ws["G1"] = "发布时间"
    i = 2
    for item in response_list:
        for book in item:
            ws.cell(i, 1, book["title"])
            ws.cell(i, 2, book["money"])
            ws.cell(i, 3, book["company"])
            ws.cell(i, 4, book["address"])
            ws.cell(i, 5, book["experience"])
            ws.cell(i, 6, book["education"])
            ws.cell(i, 7, book["time"])
            i += 1
    # excel覆盖保存文件到当前文件夹
    wb.save("海宁人才网所有职位.xlsx")


if __name__ == "__main__":
    main()
```

#### 爬取小说网站所有小说  并存入数据库 同步请求方式
实际请求地址为https://quanxiaoshuo.com
```py
# 导入网络请求库
import requests

# 导入时间库
import time

# 导入正则表达库
import re

# 导入多进程
from multiprocessing import Process

# 导入BeautifulSoup类库
from bs4 import BeautifulSoup

# 随机数
import random

# 数据库
from Db import Db


BaseUrl = "http://www.test.com"


# 定义header头
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.212 Safari/537.36",
}

# 请求进程类
class Request(Process):
    def __init__(self, url_list, index):
        """
        @description 初始化方法
        @param url_list list 请求的url列表
        @param index 返回列表的的起始索引
        @return
        """
        super().__init__()
        self.__url_list = url_list
        self.__index = index

    def run(self):
        # 多进程同时请求url
        print(f"进程{self.__index + 1}启动")
        for key, value in enumerate(self.__url_list):
            print(f"请求{self.__index + 1}-{key}发出")
            # 请求书籍列表
            res = requests.get(value, headers=headers)
            print(f"请求{self.__index + 1}-{key}收到")
            # 创建BeautifulSoup对象 使用lxml解析器
            soup = BeautifulSoup(res.text, "lxml")
            # 循环获取小说列表
            for item in soup.select(".list_content "):
                # 章节soup
                chapter_soup = BeautifulSoup(str(item), "lxml")
                # 找到书名
                book_name = chapter_soup.select(".cc2 a")[0].text
                # 作者
                author = chapter_soup.select(".cc4 a")[0].text
                # 最后一次更新时间
                update_time = int(time.mktime(time.strptime(chapter_soup.select(".cc5")[0].text, "%Y-%m-%d %H:%M")))
                # 找到书籍id
                book_id = chapter_soup.select(".cc2 a")[0]["href"]

                with Db() as db:
                    # 因为他网站定时会更新排序 所以判断下重复插入
                    res = db.table("book").where({"book_name": book_name}).field("id").find()
                    if res:
                        continue
                    # 请求目录
                    res = requests.get(BaseUrl + book_id, headers=headers)
                    # 章节目录soup
                    chapter_content_soup = BeautifulSoup(res.text, "lxml")
                    # 大于1000章的直接死掉
                    if len(chapter_content_soup.select(".chapter a")) > 1000:
                        continue
                    # 写入书籍表
                    insert = {
                        "book_name": book_name,
                        "author": author,
                        "book_introduction": "",
                        "update_time": update_time,
                    }
                    # 返回数据库书籍id
                    id = db.table("book").insert(insert, True)
                    print("录入书名：" + book_name + ",录入成功")
                    # 分类名称
                    category_name = (
                        chapter_content_soup.select_one(".w3 font").text
                        if chapter_content_soup.select_one(".w3 font")
                        else "其他"
                    )
                    # 写入分类表
                    category = {
                        "category_name": category_name,
                        "book_id": id,
                    }
                    db.table("book_category").insert(category)
                    # 设置请求间隔
                    time.sleep(random.uniform(0, 1) + 2)
                    # 章节序号
                    chapter_index = 1
                    # 循环获取章节目录
                    for chapter_item in chapter_content_soup.select(".chapter a"):
                        # 章节名称
                        chapter_name = re.sub(r"第.*?章\s+", "", str(chapter_item.text), 1, re.S | re.I | re.M)
                        # 章节详情id
                        chapter_id = chapter_item["href"]
                        # 请求详情
                        res = requests.get(BaseUrl + chapter_id, headers=headers)
                        # 详情soup
                        book_detail_soup = BeautifulSoup(res.text, "lxml")
                        # 详情内容
                        chapter_detail = re.sub(
                            r"<div.*?>.*?</div>.*?</div>",
                            "",
                            re.sub(
                                r"<div id=\"content\">\s+<div style=\".*?\">.*?</div>",
                                "",
                                str(book_detail_soup.select_one("#content")),
                                1,
                                re.S | re.I | re.M,
                            ),
                            1,
                            re.S | re.I | re.M,
                        )
                        # 写入章节表
                        chapter = {
                            "chapter_name": chapter_name,
                            "chapter_detail": chapter_detail,
                            "book_id": id,
                            "chapter_index": chapter_index,
                        }
                        db.table("book_chapter").insert(chapter)
                        chapter_index = chapter_index + 1
                        # 设置请求间隔
                        time.sleep(random.uniform(0, 1) + 2)
                        print(chapter_item.text)
            # 设置请求间隔
            time.sleep(random.uniform(0, 1) + 2)


def main():
    # 请求当前地址内容
    base_url = "http://www.test.com/top/"
    page_ext = "/"
    # 生成器生成url列表  一共有4500页
    base_url_list = [base_url + str(item) + page_ext for item in range(1, 4)]
    # url分组 650个为1组,分成 len/650 组  每组用一个进程去处理 开7个进程
    request_url_list = []
    for i in range(0, len(base_url_list), 1):
        request_url_list.append(base_url_list[i : i + 1])
    Process = []
    for key, value in enumerate(request_url_list):
        p = Request(value, key * 1)
        Process.append(p)
    # 同时开启进程
    for p in Process:
        p.start()
    # 等待所有进程结束
    for p in Process:
        p.join()
    print("爬取完成")


if __name__ == "__main__":
    main()
```

####  爬取指定名称的小说封面图
```py
# 导入网络请求库
import requests

# 导入时间库
import time

# 导入正则表达库
import re

# 导入多进程
from multiprocessing import Process

# 导入BeautifulSoup类库
from bs4 import BeautifulSoup

# 随机数
import random

# 数据库
from Db import Db

# 为什么是test.com 因为访问的是nginx反向代理服务器 具体配置看nginx反向代理服务器那块
BaseUrl = "http://www.qidian.com"


# 定义header头
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.212 Safari/537.36",
}

# 请求进程类
class Request(Process):
    def __init__(self, url_list, response_list, index):
        """
        @description 初始化方法
        @param url_list list 请求的url列表
        @param response_list list 返回的数据列表
        @param index 返回列表的的起始索引
        @return
        """
        super().__init__()
        self.__url_list = url_list
        self.__response_list = response_list
        self.__index = index

    def run(self):
        # 多进程同时请求url
        print(f"进程{self.__index + 1}启动")
        for key, value in enumerate(self.__url_list):
            print(f"请求{self.__index + 1}-{key}发出")
            # 请求书籍列表
            res = requests.get(value, headers=headers)
            print(f"请求{self.__index + 1}-{key}收到")
            # 创建BeautifulSoup对象 使用lxml解析器
            soup = BeautifulSoup(res.text, "lxml")
            # 找到图片容器
            image_text = soup.select(".res-book-item")[0]
            # 找到书籍名称
            book_soup = BeautifulSoup(str(image_text), "lxml")
            # 如果书名相同，则找到图片地址
            image_url = "https://bookcover.yuewen.com/qdbimg/349573/1026722127/400"
            if book_soup.select_one("cite") and book_soup.select_one("cite").text == str.replace(
                value, "https://www.qidian.com/search?kw=", ""
            ):
                image_url = "https:" + book_soup.select_one(".book-img-box img")["src"]
            id = self.__response_list[self.__index + key]
            # 保存图片
            if image_url != "":
                with open("./images/" + str(id) + ".png", "wb") as f:
                    res = requests.get(image_url)
                    # .content是二进制数据 比如图片，音频，视频
                    f.write(res.content)
            # 设置请求间隔
            time.sleep(random.uniform(0, 1) + 0.25)


def main():
    # 请求当前地址内容
    base_url = "https://www.qidian.com/search?kw="
    with Db() as db:
        res = db.table("book").where({"image_url": ""}).field("id,book_name").select()
        # 生成器生成url列表  一共有540
        base_url_list = [base_url + item["book_name"] for item in res]

        # url分组 650个为1组,分成 len/650 组  每组用一个进程去处理 开7个进程
        request_url_list = []
        for i in range(0, len(base_url_list), 200):
            request_url_list.append(base_url_list[i : i + 200])

        # id list用于保存图片的名称
        response_list = [item["id"] for item in res]

        Process = []
        for key, value in enumerate(request_url_list):
            p = Request(value, response_list, key * 200)
            Process.append(p)
        # 同时开启进程
        for p in Process:
            p.start()
        # 等待所有进程结束
        for p in Process:
            p.join()
        print("爬取完成")


if __name__ == "__main__":
    main()
```

!> 下载完图片后上传到服务器，然后数据库执行批量设置图片地址 设置字数
```sql
UPDATE `book` SET image_url = CONCAT('http://noval.o8o8o8.com/static/images/',id,'.png'); 
SELECT sum(chapter_detail_count) AS chapter_detail_count_all,book_id FROM (
SELECT CHAR_LENGTH(chapter_detail) AS chapter_detail_count,book_id FROM book_chapter WHERE book_id BETWEEN 642 AND 646) AS bc GROUP BY book_id
```

#### 爬取小说的描述
```py
# 导入网络请求库
import requests

# 导入时间库
import time

# 导入正则表达库
import re

# 导入多进程
from multiprocessing import Process

# 导入BeautifulSoup类库
from bs4 import BeautifulSoup

# 随机数
import random

# 数据库
from Db import Db


# 定义header头
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.212 Safari/537.36",
}

# 请求进程类
class Request(Process):
    def __init__(self, url_list, response_list, index):
        """
        @description 初始化方法
        @param url_list list 请求的url列表
        @param response_list list 返回的数据列表
        @param index 返回列表的的起始索引
        @return
        """
        super().__init__()
        self.__url_list = url_list
        self.__response_list = response_list
        self.__index = index

    def run(self):
        # 多进程同时请求url
        print(f"进程{self.__index + 1}启动")
        for key, value in enumerate(self.__url_list):
            print(f"请求{self.__index + 1}-{key}发出")
            # 请求书籍列表
            res = requests.get(value, headers=headers)
            print(f"请求{self.__index + 1}-{key}收到")
            # 创建BeautifulSoup对象 使用lxml解析器
            soup = BeautifulSoup(res.text, "lxml")
            # 找到图片容器
            image_text = soup.select(".res-book-item")[0]
            # 找到书籍名称
            book_soup = BeautifulSoup(str(image_text), "lxml")
            if book_soup.select_one("cite") and book_soup.select_one("cite").text == str.replace(
                value, "https://www.qidian.com/search?kw=", ""
            ):
                # 书籍详情链接
                book_introduction_href = "https:" + book_soup.select_one(".book-img-box  a")["href"]
                # 请求书籍详情
                res = requests.get(book_introduction_href, headers=headers)
                # 创建BeautifulSoup对象 使用lxml解析器
                book_introduction_soup = BeautifulSoup(res.text, "lxml")
                # 找到描述
                book_introduction = str(book_introduction_soup.select_one(".book-intro p"))
                # id
                id = self.__response_list[self.__index + key]
                # 写入数据库
                with Db() as db:
                    res = db.table("book").where({"id": id}).update({"book_introduction": book_introduction})
                    # 返回影响的行数
                    print(res)
                    if res == 1:
                        print(id, "更新成功")
            # 设置请求间隔
            time.sleep(random.uniform(0, 1) + 0.25)


def main():
    # 请求当前地址内容
    base_url = "https://www.qidian.com/search?kw="
    with Db() as db:
        res = db.table("book").where({"book_introduction": ""}).field("id,book_name").select()
        # 生成器生成url列表  一共有540
        base_url_list = [base_url + item["book_name"] for item in res]

        # url分组 650个为1组,分成 len/650 组  每组用一个进程去处理 开7个进程
        request_url_list = []
        for i in range(0, len(base_url_list), 200):
            request_url_list.append(base_url_list[i : i + 200])

        # id list用于保存对应记录的描述
        response_list = [item["id"] for item in res]

        Process = []
        for key, value in enumerate(request_url_list):
            p = Request(value, response_list, key * 200)
            Process.append(p)
        # 同时开启进程
        for p in Process:
            p.start()
        # 等待所有进程结束
        for p in Process:
            p.join()
        print("爬取完成")


if __name__ == "__main__":
    main()
```

#### 爬取小说网站所有小说 异步请求方式
这种方式太快了，直接搞崩了
```py
# 异步io
import asyncio

# 异步请求
import aiohttp

# 导入时间库
import time

# 导入正则表达库
import re

# 导入多进程
from multiprocessing import Process

# 导入BeautifulSoup类库
from bs4 import BeautifulSoup

# 随机数
import random

# 数据库
from Db import Db

# 为什么是test.com 因为访问的是nginx反向代理服务器 具体配置看nginx反向代理服务器那块
BaseUrl = "http://www.test.com"

# 定义header头
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.212 Safari/537.36",
}

# 请求进程类
class Request(Process):
    def __init__(self, url_list, index):
        """
        @description 初始化方法
        @param url_list list 请求的url列表
        @param index 返回列表的的起始索引
        @return
        """
        super().__init__()
        self.__url_list = url_list
        self.__index = index

    def run(self):
        # 多进程同时请求url
        print(f"进程{self.__index + 1}启动")
        # 异步请求方法
        async def http_request(key, value):
            print(f"请求{self.__index + 1}-{key}发出")
            # 创建了一个异步的网络请求的上线文管理具柄  用了with 会自动释放调用close的
            async with aiohttp.ClientSession() as session:
                # 请求书籍列表
                async with session.get(value, headers=headers) as res:
                    if res.status == 200:  # 判断请求码
                        print(f"请求{self.__index + 1}-{key}收到")
                        source = await res.text()  # 使用await关键字获取返回结果
                        # 创建BeautifulSoup对象 使用lxml解析器
                        soup = BeautifulSoup(source, "lxml")
                        # 循环获取小说列表
                        for item in soup.select(".list_content "):
                            # 章节soup
                            chapter_soup = BeautifulSoup(str(item), "lxml")
                            # 找到书名
                            book_name = chapter_soup.select(".cc2 a")[0].text
                            # 作者
                            author = chapter_soup.select(".cc4 a")[0].text
                            # 最后一次更新时间
                            update_time = int(
                                time.mktime(time.strptime(chapter_soup.select(".cc5")[0].text, "%Y-%m-%d %H:%M"))
                            )
                            # 找到书籍id
                            book_id = chapter_soup.select(".cc2 a")[0]["href"]

                            with Db() as db:
                                # 写入书籍表
                                insert = {
                                    "book_name": book_name,
                                    "author": author,
                                    "update_time": update_time,
                                }
                                # 因为他网站定时会更新排序 所以判断下重复插入
                                res = db.table("book").where({"book_name": book_name}).field("id").find()
                                if res:
                                    continue
                                # 返回数据库书籍id
                                id = db.table("book").insert(insert, True)
                                print("录入书名：" + book_name + ",录入成功")
                                # 请求目录
                                async with aiohttp.ClientSession() as session:
                                    async with session.get(BaseUrl + book_id, headers=headers) as res:
                                        if res.status == 200:  # 判断请求码
                                            source = await res.text()  # 使用await关键字获取返回结果
                                            # 章节目录soup
                                            chapter_content_soup = BeautifulSoup(source, "lxml")
                                            # 分类名称
                                            category_name = (
                                                chapter_content_soup.select_one(".w3 font").text
                                                if chapter_content_soup.select_one(".w3 font")
                                                else "其他"
                                            )
                                            with Db() as db:
                                                # 写入分类表
                                                category = {
                                                    "category_name": category_name,
                                                    "book_id": id,
                                                }
                                                db.table("book_category").insert(category)
                                                # 设置请求间隔
                                                time.sleep(random.uniform(0, 1) + 0.5)
                                                # 章节使用事务统一提交  不然太慢了
                                                db.beginTransaction()
                                                try:
                                                    # 章节序号
                                                    chapter_index = 1
                                                    # 循环获取章节目录
                                                    for chapter_item in chapter_content_soup.select(".chapter a"):
                                                        # 章节名称
                                                        chapter_name = re.sub(
                                                            r"第.*?章\s+",
                                                            "",
                                                            str(chapter_item.text),
                                                            1,
                                                            re.S | re.I | re.M,
                                                        )
                                                        # 章节详情id
                                                        chapter_id = chapter_item["href"]
                                                        # 请求详情
                                                        async with aiohttp.ClientSession() as session:
                                                            async with session.get(
                                                                BaseUrl + chapter_id,
                                                                headers=headers,
                                                            ) as res:
                                                                if res.status == 200:  # 判断请求码
                                                                    source = await res.text()  # 使用await关键字获取返回结果
                                                                    # 详情soup
                                                                    book_detail_soup = BeautifulSoup(source, "lxml")
                                                                    # 详情内容
                                                                    chapter_detail = re.sub(
                                                                        r"<div.*?>.*?</div>.*?</div>",
                                                                        "",
                                                                        re.sub(
                                                                            r"<div id=\"content\">\s+<div style=\".*?\">.*?</div>",
                                                                            "",
                                                                            str(
                                                                                book_detail_soup.select_one("#content")
                                                                            ),
                                                                            1,
                                                                            re.S | re.I | re.M,
                                                                        ),
                                                                        1,
                                                                        re.S | re.I | re.M,
                                                                    )
                                                                    # 写入章节表
                                                                    chapter = {
                                                                        "chapter_name": chapter_name,
                                                                        "chapter_detail": chapter_detail,
                                                                        "book_id": id,
                                                                        "chapter_index": chapter_index,
                                                                    }
                                                                    db.table("book_chapter").insert(chapter)
                                                                    chapter_index = chapter_index + 1
                                                                    # 设置请求间隔
                                                                    time.sleep(random.uniform(0, 1) + 0.5)
                                                                    print(chapter_item.text)
                                                    db.commit()
                                                except Exception as result:
                                                    print(result)  # 输出事务错误
                                                    db.rollback()
                            # 设置请求间隔
                            time.sleep(random.uniform(0, 1) + 0.5)

        # 异步运行任务方法
        async def run_http():
            tasks = []
            for key, value in enumerate(self.__url_list):
                tasks.append(asyncio.create_task(http_request(key, value)))
            await asyncio.gather(*tasks)

        # 异步运行任务
        asyncio.run(run_http())


def main():
    # 请求当前地址内容
    base_url = "http://www.test.com/last"
    page_ext = ".htm"
    # 生成器生成url列表  一共有4页
    base_url_list = [base_url + str(item) + page_ext for item in range(1, 4)]
    # url分组 2个为1组,分成 len/2 组  每组用一个进程去处理 开2个进程
    request_url_list = []
    for i in range(0, len(base_url_list), 2):
        request_url_list.append(base_url_list[i : i + 2])

    Process = []
    for key, value in enumerate(request_url_list):
        p = Request(value, key * 2)
        Process.append(p)
    # 同时开启进程
    for p in Process:
        p.start()
    # 等待所有进程结束
    for p in Process:
        p.join()
    print("爬取完成")


if __name__ == "__main__":
    main()
```

#### 爬取m3u8视频，批量合并爬取的视频为mp4
```py
# 导入网络请求库
import requests

# 导入时间库
import time

# 导入正则表达库
import re

# 导入多线程
from threading import Thread

# 导入操作系统模块
import os

# 请求线程类
class Request(Thread):
    def __init__(self, url_list, index):
        """
        @description 初始化方法
        @param url_list list 请求的url列表
        @param index 每集的索引
        @return
        """
        super().__init__()
        self.__url_list = url_list
        self.__index = index

    def run(self):
        # 多线程同时请求url
        print(f"线程{self.__index + 1}启动")
        for key, value in enumerate(self.__url_list):
            print(f"请求{self.__index + 1}-{key}发出")
            res = requests.get(value)
            # 正则匹配每个网站的标题
            match = re.findall(
                r"(http.*?)\n#EXT",
                res.text,
                re.S | re.I | re.M,
            )
            dir_name = "./97kpw/sscf/第" + str(self.__index + 1) + "集"
            if not os.path.exists(dir_name):
                os.makedirs(dir_name)
            # ts片段序号最大长度 假如是399 那么长度是3
            max_length = len(str(len(match)))
            for index, item in enumerate(match):
                with requests.get(
                    item,
                    stream=True,
                ) as r:
                    # 当前ts索引数长度
                    cur_length = len(str(index + 1))
                    # 根据长度差计算补0数
                    list_num = "0" * (max_length - cur_length) + str(index + 1)
                    # 保存
                    with open(dir_name + "/" + list_num + ".ts", "wb") as f:  # 后缀名.mp4也可以
                        for chunk in r.iter_content(chunk_size=1024):
                            f.write(chunk)
                # 设置请求间隔
                time.sleep(0.02)
            self.__index = self.__index + 1
            # 设置请求间隔
            time.sleep(0.02)


def main():
    # 请求当前地址内容  一个url为1集
    base_url_list = [
        "https://n1.szjal.cn/20210601/WtIJbUjL/index.m3u8",
    ]

    # 生成器生成url列表
    # url分组 3个为1组,分成3组  每组用一个线程去处理
    request_url_list = []
    for i in range(0, len(base_url_list), 3):
        request_url_list.append(base_url_list[i : i + 3])

    Threads = []
    for key, value in enumerate(request_url_list):
        t = Request(value, key * 3)
        Threads.append(t)
        t.start()
    # 等待所有线程结束
    for t in Threads:
        t.join()
    for key, value in enumerate(base_url_list):
        # 执行合并 r防止转义
        path1 = r".\97kpw\sscf\第" + str(key + 1) + "集\*.ts"
        path2 = r".\97kpw\sscf\第" + str(key + 1) + "集.mp4"
        os.system("copy /b " + path1 + " " + path2)

    print("合并完成")


if __name__ == "__main__":
    main()
```
!>这里的base_url_list 需要自己去找或者利用各种ajax拦截获取,我这里破解不了97看片网的链接sign，所以是一个一个找的

### numpy
```py
import numpy as np


def main():
    """
    一维数组
    """
    # 1.创建一维数组 1-10
    array = np.array([item for item in range(1, 11)])
    print(array)  # 输出 [ 1  2  3  4  5  6  7  8  9 10]
    # 2.使用np的arange创建 0到10的偶数 第三个参数是步长
    array = np.arange(0, 11, 2)
    print(array)  # 输出 [ 0  2  4  6  8 10]
    # 3.创建均匀步长生成数字序列 用在图表里面x轴数据生成很有用
    array = np.linspace(0, 10, 5)
    print(array)  # 输出 [ 0.   2.5  5.   7.5 10. ]
    # 4.创建0-10内指定个数的随机数 rand(指定个数) 生成的是0~1范围内的随机数，再*10才能生成0-10
    array = np.random.rand(10) * 10
    print(array)
    # 输出[7.15715951 4.95430276 4.77331489 1.95758046 6.96759422 5.8898343 7.78075441 2.70991707 6.67044971 6.73632015]
    # 5.创建指定范围内指定个数的随机整数
    array = np.random.randint(0, 100, 10)
    print(array)  # 输出 [ 3 11 98 66 76 47 38 38  5 98]
    """
    二维数组
    """
    # 1.通过list创建
    array = np.array([[1, 2, 3], [4, 5, 6]])
    print(array)  # 输出[[1 2 3][4 5 6]]
    # 2.通过内置函数创建 np.ones((2,2)) -> 创建2行2列为1的二维数组 np.zeros((2,2)) -> 创建2行2列为0的二维数组
    # 3.通过full创建指定行指定列的，指定初始化值为10的二维数组
    array = np.full((2, 2), 10)
    print(array)  # 输出[[10 10][10 10]]
    # 4.将一维数组拆分 组装成二维数组
    # 下面将[ 0  1  2  3  4  5  6  7  8  9] 拆分成长度为2 每组个数为5的二维数组
    array = np.arange(0, 10).reshape(2, 5)
    print(array)  # 输出[[0 1 2 3 4][5 6 7 8 9]]
    # 5.二维数组随机小数
    array = np.random.rand(2, 2) * 10
    print(array)  # 输出0-10的随机小数 2 * 2的数组 [[9.03732201 6.56019959][3.53644986 5.90690465]]
    # 6.二维数组随机整数 指定范围内的随机整数二维数组
    array = np.random.randint(0, 100, (2, 2))
    print(array)  # 输出[[72  6][94 80]]

    """
    多维数组
    注意reshape(x1,x2,x3) 参数从前到后分别代表从外到里数组元素的个数 
    """
    # 1.将一维数组拆分 组装成多维数组
    # 下面将[ 0  1  2  3  4  5 ] 拆分成 3*2*1 的三维数组
    array = np.arange(0, 6).reshape((3, 2, 1))
    print(array)  # 输出[[[0][1]][[2][3]][[4][5]]]

    """
    数组属性
    """
    # 1.size 输出元素个数
    print(array.size, len(array))  # 输出6,3 注意len()方法这里只能计算最外层的数组长度
    # 2.输出数组维度详情
    print(array.shape)  # 输出 (3, 2, 1)
    # 3.输出数组元素的数据类型
    print(array.dtype)  # 输出int32
    # 4，输出数组维度
    print(array.ndim)  # ss输出3维
    """
    索引和切片
    切片语法 [开始索引:结束索引:步长]
    """
    array = np.array([[1, 2, 3], [4, 5, 6]])
    # 1.输出指定索引的数据
    print(array[0], array[0][array[0].size - 1])  # 输出[1 2 3] 和 3
    # 2.一维数组切片和列表相似
    # 3.二维数组切片 0:2:1 代表取最外层从索引0到2(不包含2) 步长为1取元素  2:3:1表示从第二层取索引2:3（不包含3）步长为1取元素 所以取到的是3和6 他们的索引是2
    print(array[0:2:1, 2:3:1])  # 输出[[3][6]]
    # 4.利用数组索引取值array[[第几行][对应的第几列]]
    print(array[[0, 1, 1], [2, 1, 2]])  # 取二维数组的最外层索引为0和1和1的数组 内层索引分别对应为2 和 1,2的元素 输出[3 5 6]
    """
    数组方法
    """
    # 1.求和
    print(array.sum())  # 输出21
    # 2.求平均值mean 平均值的意思
    print(array.mean())  # 输出3.5
    # 3.求最大值
    print(array.max())  # 输出6
    # 4.求最小值
    print(array.min())  # 输出1
    # 5.数组排序 影响原数组
    array = np.array([[3, 1, 2], [5, 4, 6]])
    array.sort()
    print(array)  # 输出[[1 2 3][4 5 6]]
    # 6.里层逆序输出 外层不逆序  这里::1表示取最外层且不逆序  ::-1表示第二次逆序
    print(array[::1, ::-1])  # 输出[[3 2 1][6 5 4]]
    # 6.转成list
    print(array.tolist())  # 输出[[1, 2, 3], [4, 5, 6]]

    """
    数组运算
    """
    # 1.作用于每个元素的+-*/
    print(array + 1)  # 输出[[2 3 4][5 6 7]]
    # 2.两个数组相加
    array2 = np.array([[1, 2, 3], [4, 5, 6]])
    print(array + array2)  # [[ 2  4  6][ 8 10 12]]
    """
    其他函数
    """
    # 1.去重 是对于所有元素来说的
    array = np.array([[1, 2, 2], [4, 4, 6], [4, 4, 6]])
    print(np.unique(array))  # 输出一维[1 2 4 6]
    # 2.拆分成列表
    array = np.array([1, 2, 3])
    print(np.split(array, 3))  # 输出[array([1]), array([2]), array([3])]
    # 3.追加元素
    print(np.append(array, 4))  # 输出[1 2 3 4]
    # 4.插入元素
    print(np.insert(array, 2, 4))  # 输出[1 2 4 3]
    # 5.重复数组元素到指定个数
    array = np.array([1, 2, 2, 3])
    print(np.resize(array, 10))  # 输出[1 2 2 3 1 2 2 3 1 2]
    # 6.补0
    array = np.array([1, 2, 2, 3])
    array.resize(5)
    print(array)  # 输出 [1 2 2 3 0]


if __name__ == "__main__":
    main()
```

### pandas
#### pandas两种对象及图表结合
```py
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

"""
matplotlib 图表中文支持
"""
# 配置支持中文的非衬线字体（默认的字体无法显示中文）
plt.rcParams["font.sans-serif"] = [
    "SimHei",
    "SimSun",
]
# 使用指定的中文字体时需要下面的配置来避免负号无法显示
plt.rcParams["axes.unicode_minus"] = False


"""
二维表格数据类型 类似于数据库表  有字段名和行号
"""
data = [{"年龄": 18, "分数": 90}, {"年龄": 16, "分数": 80}]
# 下面是直接根据字典创建
# dtfame = pd.DataFrame(data)
# 下面是自定义行列索引 列索引可不填
dtfame = pd.DataFrame(data, index=["小明", "小红"], columns=["年龄", "分数"])
"""
输出
  name  age
0   张三   18
1   李四   16
"""
figure = dtfame.plot(kind="bar", color=["#f60f60", "g"])
plt.xlabel("姓名")
plt.ylabel("年龄/分数")
# x轴的坐标旋转到0度（中文水平显示）
plt.xticks(rotation=0)
# 下面2个很重要  设置坐标刻度为索引
figure.set_xticks(range(len(dtfame.index)))
figure.set_xticklabels(dtfame.index)
# 设置柱状图宽度
x1, x2 = figure.get_xlim()
figure.set_xlim(x1 - 0.5, x2 + 0.25)
# 在柱状图的柱子上绘制数字
for index, item in enumerate(data):
    plt.text(index - 0.15, data[index]["年龄"] + 1, data[index]["年龄"])
    plt.text(index + 0.1, data[index]["分数"] + 1, data[index]["分数"])
plt.show()


"""
创建series数据对象
"""
series = pd.Series({"一季度": 400, "二季度": 540, "三季度": 180, "四季度": 380})
series = pd.Series(
    [400, 500, 180, 380],
    index=[
        "一季度",
        "二季度",
        "三季度",
        "四季度",
    ],
)

"""
统计
"""
# 求和
print(series.sum())  # 1500
# 求均值
print(series.mean())  # 375.0
# 求最大
print(series.max())  # 540
# 求最小
print(series.min())  # 180
# 计数
print(series.count())  # 4

"""
柱状图
"""
# 通过Series对象的plot方法绘图（kind='bar'表示绘制柱状图）
series.plot(kind="bar", color=["#f60f60", "g", "b", "y"])
# x轴的坐标旋转到0度（中文水平显示）
plt.xticks(rotation=0)
# 在柱状图的柱子上绘制数字
for i in range(4):
    plt.text(i, series[i] + 5, series[i], ha="center")
# 显示图像
plt.show()


"""
画饼图
"""
# autopct参数可以配置在饼图上显示每块饼的占比
series.plot(kind="pie", autopct="%.1f%%")
# 设置y轴的标签（显示在饼图左侧的文字）
plt.ylabel("各季度占比")
plt.show()


"""
画折线图
"""
series.plot()
plt.show()
```

#### pandas和excel交互
```py
import matplotlib.pyplot as plt
import pandas as pd

"""
matplotlib 图表中文支持
"""
# 配置支持中文的非衬线字体（默认的字体无法显示中文）
plt.rcParams["font.sans-serif"] = [
    "SimHei",
    "SimSun",
]
# 使用指定的中文字体时需要下面的配置来避免负号无法显示
plt.rcParams["axes.unicode_minus"] = False


"""
二维表格数据类型 类似于数据库表  有字段名和行号
"""
data = pd.read_excel("./1.xlsx")
"""
excel数据如下
姓名	第一季度	第二季度	第三季度	第四季度
张三	1	           2	       3	    4
李四	4	           5	       6	    7
王五	8	           9	       10	    11
"""
dtfame = pd.DataFrame(data, index=data.index, columns=data.columns)
figure = dtfame.plot(kind="bar", color=["red", "blue", "green", "black"])
plt.xlabel("姓名")
plt.ylabel("分数")
# x轴的坐标旋转到0度（中文水平显示）
plt.xticks(rotation=0)
# 下面2个很重要  设置坐标刻度为索引
figure.set_xticks(range(len(dtfame.index)))
figure.set_xticklabels(dtfame["姓名"])
# # 在柱状图的柱子上绘制数字
for index, item in enumerate(data):
    for i, value in enumerate(data[item]):
        if index != 0:
            plt.text((i - 0.36) + 0.13 * index, int(value) + 0.1, value)
plt.show()
# 要将dtfame输出excel用下面的方式 csv等其他格式类似
dtfame.to_excel("./2.xlsx")
```

### matplotlib
```py
"""
所有案例参考自https://matplotlib.org/stable/gallery/index.html
"""
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

"""
matplotlib 图表中文支持
"""
# 配置支持中文的非衬线字体（默认的字体无法显示中文）
plt.rcParams["font.sans-serif"] = [
    "SimHei",
    "SimSun",
]
# 使用指定的中文字体时需要下面的配置来避免负号无法显示
plt.rcParams["axes.unicode_minus"] = False


data = {"x": ["第一季度", "第二季度", "第三季度", "第四季度"], "y": [10, 20, 30, 40], "z": [20, 10, 60, 80]}
"""
基本折线图
"""
dataframe = pd.DataFrame(data)
# 获取图表对象和轴对象
figure, axes = plt.subplots()
# 设置轴数据
axes.plot(dataframe["x"], dataframe["y"], label="张三")
axes.plot(dataframe["x"], dataframe["z"], label="李四")
# 显示图例
axes.legend()
# 设置图表标题
axes.set_title("年度业绩表")
plt.show()

"""
基本柱形图
"""
dataframe = pd.DataFrame(data)
# 获取图表对象和轴对象
figure, axes = plt.subplots()
# 设置轴数据
axes.bar(dataframe["x"], dataframe["y"], width=0.2)
# 设置图表标题
axes.set_title("年度业绩表")
plt.show()

"""
基本散点图
"""
dataframe = pd.DataFrame(data)
# 获取图表对象和轴对象
figure, axes = plt.subplots()
# 设置轴数据
axes.scatter(dataframe["x"], dataframe["y"], label="张三")
axes.scatter(dataframe["x"], dataframe["z"], label="李四")
# 显示图例
axes.legend()
# 设置图表标题
axes.set_title("年度业绩表")
plt.show()

"""
基本饼图
"""
data = {"x": ["第一季度", "第二季度", "第三季度", "第四季度"], "y": [10, 20, 30, 40], "z": [20, 10, 60, 80]}
# only "explode" the 2nd slice (i.e. 'Hogs')  意思就是第二个凸出0.1
explode = (0, 0.1, 0, 0)
# 获取图表对象和轴对象
figure, axes = plt.subplots()
# 画饼图
axes.pie(
    data["y"],
    explode=explode,
    labels=data["x"],
    autopct="%1.1f%%",
    shadow=False,
    startangle=90,
    textprops=dict(color="#ffffff"),
)
# 确保饼图被画成一个圆
axes.axis("equal")
# 设置图表标题
axes.set_title("年度业绩表")
plt.show()

"""
簇状柱形图
"""
data = {"x": ["第一季度", "第二季度", "第三季度", "第四季度"], "y": [10, 20, 30, 40], "z": [20, 10, 60, 80]}
dataframe = pd.DataFrame(data)
# 获取图表对象和轴对象
figure, axes = plt.subplots()
# darray的长度
df_range = list(range(1, len(dataframe) + 1))
# 每个柱的宽度
width = 0.2
# 每个柱形图的偏移量
x1 = [i - width for i in df_range]
x2 = [i + width for i in df_range]

# 分别画2个柱形
rects1 = axes.bar(x1, dataframe["y"], width * 2, label="张三", color="#1EAFAE")
rects2 = axes.bar(x2, dataframe["z"], width * 2, label="李四", color="#69FFFF")
# 设置横坐标刻度
axes.set_xticks(df_range)
# 设置横坐标刻度标签
axes.set_xticklabels(dataframe["x"])
# 显示图例
axes.legend()
# 设置y轴刻度范围
axes.set_ylim(0, 150)
# 调粗坐标轴刻度 那根刻度线的粗细。。 colors刻度的颜色 gridOn 添加网格线
axes.tick_params(axis="y", width=2, colors="#f60f60", gridOn=True, grid_color="#ccc", grid_alpha=1, grid_linewidth=0.5)
# 设置y轴label
axes.set_ylabel("业绩")
# 设置x轴label
axes.set_xlabel("季度")
# 在坐标(x,y)处添加文字 text(x,y)
for i in df_range:
    axes.text(i - width, data["y"][i - 1] + 2, data["y"][i - 1], fontsize=14)
    axes.text(i + width, data["z"][i - 1] + 2, data["z"][i - 1], fontsize=12)
# 设置图表标题
axes.set_title("年度业绩表")
plt.show()

"""
带标签和值的饼图
"""
data = {"x": ["第一季度", "第二季度", "第三季度", "第四季度"], "y": [375, 75, 250, 300], "z": [20, 10, 60, 80]}

# 获取图表对象和轴对象 equal确保饼图被画成一个圆
figure, axes = plt.subplots(subplot_kw=dict(aspect="equal"))


def func(pct, data):
    """
    @description 自定义内部文字显示格式
    @param pct 每项所占百分比值
    @param value 每项的值
    @return 显示格式
    """
    value = int(round(pct / 100 * np.sum(data)))
    return f"{pct:.1f}%\n({value}元)"


# 画饼图 textprops 文字颜色
wedges, texts, autotexts = axes.pie(
    data["y"],
    labels=data["x"],
    autopct=lambda pct: func(pct, data["y"]),
    shadow=False,
    startangle=90,
    textprops=dict(color="#ffffff"),
)

# 设置图例
axes.legend(wedges, data["x"], title="各季度", loc="center left", bbox_to_anchor=(1, 0, 0.5, 1))
# 设置字体
plt.setp(autotexts, size=12, weight="bold")
# 设置图表标题
axes.set_title("年度业绩表")
plt.show()
```

### 自动化测试工具selenium
#### 安装
```powershell
pip install selenium
```

#### 安装谷歌驱动
找到自己的谷歌浏览器版本
浏览器地址栏输入
```
chrome://version/
```
[下载地址](https://npm.taobao.org/mirrors/chromedriver)
找到对应版本的驱动，解压后配置.exe路径的环境变量
如
```
D:\chromedriver
```

#### 基本用法
```py
# 导入了 web 驱动模块
from selenium import webdriver

# 创建了一个 Chrome 驱动
driver = webdriver.Chrome() 
# 打开百度
driver.get("https://www.baidu.com")

# 找到输入框元素
input = driver.find_element_by_css_selector("#kw")
# 输入哈哈
input.send_keys("哈哈")

# 找到搜索按钮
button = driver.find_element_by_css_selector("#su")
# 模拟点击
button.click()

""" 
# 其他获取元素的方法
driver.find_element_by_id
driver.find_element_by_name
driver.find_element_by_xpath
driver.find_element_by_link_text
driver.find_element_by_partial_link_text
driver.find_element_by_tag_name
driver.find_element_by_class_name
driver.find_element_by_css_selector
#获取多个
driver.find_elements_by_name
driver.find_elements_by_xpath
driver.find_elements_by_link_text
driver.find_elements_by_partial_link_text
driver.find_elements_by_tag_name
driver.find_elements_by_class_name
driver.find_elements_by_css_selector

#获取源代码
driver.page_source
"""
```

> 创建浏览器扩展
```py
driver = webdriver.Chrome(options=chrome_options, executable_path="./chromedriver.exe")
```

#### 官方文档
[地址](https://www.selenium.dev/documentation/en/)

#### 利用selenium爬取b站数据
```py
# 导入了 web 驱动模块
from selenium import webdriver

# 导入浏览器配置
from selenium.webdriver.chrome.options import Options

# 导入等待模块显性等待类
from selenium.webdriver.support.wait import WebDriverWait

# 导入等待模块条件类
from selenium.webdriver.support import expected_conditions as EC

# 定位方式类
from selenium.webdriver.common.by import By

# 导入动作类
from selenium.webdriver.common.action_chains import ActionChains as AC

# 导入BeautifulSoup类库
from bs4 import BeautifulSoup

# 导入时间模块
import time


def recursive_click_next_page(driver, wait):
    """
    @description  递归点击下一页 并抓取页面数据
    @param
    @return
    """
    page = 1  # 抓取页数
    # 这里抓取前3页的数据
    while page < 4:
        # 获取所有窗口句柄
        all_h = driver.window_handles
        # 切换到刚刚打开的新窗口，这里其实会自己切换到新窗口打开,如果没有，可以手动切换一下
        driver.switch_to.window(all_h[1])
        # 解析源代码  获取所有标题
        # 创建BeautifulSoup对象 使用lxml解析器
        soup = BeautifulSoup(driver.page_source, "lxml")
        # 循环输出标题
        for item in soup.select(".video-list .video-item .img-anchor"):
            print(item["title"] + "\n")
        # 解析完当前页 点击下一页
        button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".pages .next")))
        button.click()
        page += 1
        # 休眠1秒 不然页面没有渲染完 数据没有
        time.sleep(1)


def main():
    # 下面是设置无痕浏览器的配置
    chrome_options = Options()
    chrome_options.add_argument("window-size=1920x3000")  # 指定浏览器分辨率 必须指定 不然元素不存在
    chrome_options.add_argument("--disable-gpu")  # 谷歌文档提到需要加上这个属性来规避bug
    chrome_options.add_argument("--hide-scrollbars")  # 隐藏滚动条, 应对一些特殊页面
    chrome_options.add_argument("blink-settings=imagesEnabled=false")  # 不加载图片, 提升速度
    chrome_options.add_argument("--headless")  # 浏览器不提供可视化页面. linux下如果系统不支持可视化不加这条会启动失败
    chrome_options.add_argument("log-level=3")  # 关闭控制台输出 INFO = 0 WARNING = 1 LOG_ERROR = 2 LOG_FATAL = 3 default is 0
    # 创建了一个无痕Chrome浏览器
    driver = webdriver.Chrome(options=chrome_options)
    # 最大化 防止一些元素被遮挡不能交互
    driver.maximize_window()
    # 打开B站
    driver.get("https://www.bilibili.com/")
    # 获取header头
    # agent = driver.execute_script("return navigator.userAgent")
    # print(agent)
    # 第二个参数30，表示等待的最长时间，超过30秒则抛出TimeoutException
    # 第三个参数0.2，表示0.2秒去检查一次
    wait = WebDriverWait(driver, 30, 0.2)
    # EC.presence_of_element_located 验证元素是否出现
    input = wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, ".unlogin-avatar")))
    # 移动鼠标到指定元素上
    AC(driver).move_to_element(input).perform()
    # 移动鼠标到指定偏移位置
    AC(driver).move_by_offset(-200, 0).perform()
    # EC.visibility_of_element_located 验证元素是否可见-可以在页面被看见
    input = wait.until(EC.visibility_of_element_located((By.CSS_SELECTOR, "#nav_searchform input")))
    # input输入字符串 随便你输入啥
    input.send_keys("三国演义")
    # EC.element_to_be_clickable 验证元素是否可以点击
    button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".nav-search-btn")))
    button.click()  # 如果点击被其他元素遮挡，用下面的方法，否则会报错selenium.common.exceptions.ElementClickInterceptedException: Message: element click intercepted
    # driver.execute_script("arguments[0].click();", button)
    # 刷新窗口
    # driver.refresh()
    # 递归点击下一页 抓取数据
    recursive_click_next_page(driver, wait)
    # 停留10秒
    time.sleep(10)
    # 关闭驱动
    driver.quit()


if __name__ == "__main__":
    main()

""" 
selenium鼠标键盘事件 https://www.selenium.dev/documentation/en/support_packages/mouse_and_keyboard_actions_in_detail/
"""
```

#### 爬取ajax数据
- 安装

```
pip install browsermob-proxy
```
下载browsermob-proxy 并放到项目目录下
[下载地址](https://github.com/lightbody/browsermob-proxy/releases) 再在releases包即可

- 使用

```py
# 导入了 web 驱动模块
from selenium import webdriver

# 导入browsermobproxy
from browsermobproxy import Server

# 导入浏览器配置
from selenium.webdriver.chrome.options import Options

# 导入时间模块
import time


def main():
    # 启动代理
    server = Server(r"D:\\browsermob-proxy-2.1.4\\bin\\browsermob-proxy.bat", options={"port": 8090})
    server.start()
    proxy = server.create_proxy(params={"trustAllServers": "true"})

    # 启动浏览器
    chrome_options = Options()
    chrome_options.add_argument("--ignore-certificate-errors")  # 忽略https证书错误
    chrome_options.add_argument(f"--proxy-server={proxy.proxy}")  # 启动时设置代理的地址

    # 创建了一个Chrome浏览器
    driver = webdriver.Chrome(options=chrome_options)
    # 最大化 防止一些元素被遮挡不能交互
    driver.maximize_window()
    # 调用 new_har 方法，同时指定捕获 Resopnse Body 和 Headers 信息
    proxy.new_har(options={"captureContent": True, "captureHeaders": True})
    # 打开97看片网
    driver.get("https://dynamic2.scrape.center/page/1")
    # 休眠3秒 不然页面没有渲染完 数据没有
    time.sleep(3)
    # 代理拦截对象
    result = proxy.har
    # print(result)
    for entry in result["log"]["entries"]:
        _url = entry["request"]["url"]
        # 获取异步请求的url
        # print(_url)
        #  根据URL找到数据接口,
        if "https://dynamic2.scrape.center/api/movie" in _url:
            _response = entry["response"]
            _content = _response["content"]
            # 获取接口返回内容
            print(_response)
    # 记得关闭服务器了
    server.stop()
    # 关闭浏览器
    driver.quit()


if __name__ == "__main__":
    main()
```

#### 利用selenium爬取视频
有些地址是需要破解的，这里举出不需要破解的
```py
# 导入了 web 驱动模块
from selenium import webdriver

# 导入browsermobproxy
from browsermobproxy import Server

# 导入浏览器配置
from selenium.webdriver.chrome.options import Options

# 导入网络请求库
import requests

# 导入时间库
import time

# 导入正则表达库
import re

# 导入多线程
from threading import Thread

# 导入操作系统模块
import os

# 导入加密模块
from Crypto.Cipher import AES


# 请求线程类
class Request(Thread):
    def __init__(self, url_list, index):
        """
        @description 初始化方法
        @param url_list list 请求的url列表
        @param index 每集的索引
        @return
        """
        super().__init__()
        self.__url_list = url_list
        self.__index = index

    def run(self):
        # 多线程同时请求url
        print(f"线程{self.__index + 1}启动")
        for key, value in enumerate(self.__url_list):
            print(f"请求{self.__index + 1}-{key}发出")
            res = requests.get(value)
            # 获取加密秘钥地址
            match = re.search(
                r"https.*?key.key",
                res.text,
                re.S | re.I | re.M,
            )
            # 获取秘钥内容
            key = requests.get(match.group()).text
            key = bytes(key, encoding="utf8")

            # 解密
            cipher = AES.new(key, AES.MODE_CBC)  # 第三个参数传啥不清楚  好像是iv b"0000000000000000" 也好像是key

            # 正则匹配m3u8 地址列表
            match = re.findall(
                r"(http.*?.ts)\n#EXT",
                res.text,
                re.S | re.I | re.M,
            )
            dir_name = "./video/第" + str(self.__index + 1) + "集"
            if not os.path.exists(dir_name):
                os.makedirs(dir_name)
            # ts片段序号最大长度 假如是399 那么长度是3
            max_length = len(str(len(match)))
            for index, item in enumerate(match):
                with requests.get(item) as r:
                    if r.status_code == 200:
                        # 当前ts索引数长度
                        cur_length = len(str(index + 1))
                        # 根据长度差计算补0数
                        list_num = "0" * (max_length - cur_length) + str(index + 1)
                        # 保存
                        with open(dir_name + "/" + list_num + ".ts", "wb") as f:  # 后缀名.mp4也可以
                            # 也可以 f.write(cipher.decrypt(r.content)) 推荐下面的 迭代的方式读取 边下载边存盘
                            for chunk in r.iter_content(chunk_size=1024):
                                if len(chunk) % 16 == 0:
                                    # 解密是数据必须是16的倍数
                                    f.write(cipher.decrypt(chunk))

                # 设置请求间隔
                time.sleep(0.02)
            self.__index = self.__index + 1
            # 设置请求间隔
            time.sleep(0.02)


def main():
    # 启动代理
    server = Server(r"D:\\browsermob-proxy-2.1.4\\bin\\browsermob-proxy.bat", options={"port": 8090})
    server.start()
    proxy = server.create_proxy(params={"trustAllServers": "true"})

    # 启动浏览器
    chrome_options = Options()
    chrome_options.add_argument("--ignore-certificate-errors")  # 忽略https证书错误
    chrome_options.add_argument(f"--proxy-server={proxy.proxy}")  # 启动时设置代理的地址
    # 去除自动化弹窗提示
    chrome_options.add_experimental_option("excludeSwitches", ["enable-automation"])
    chrome_options.add_experimental_option("useAutomationExtension", False)

    # 创建了一个Chrome浏览器
    driver = webdriver.Chrome(options=chrome_options)
    # 最大化 防止一些元素被遮挡不能交互
    driver.maximize_window()
    # 设定页面加载timeout时长，需要的元素能加载出来就行
    driver.set_page_load_timeout(10)
    driver.set_script_timeout(10)
    # 调用 new_har 方法，同时指定捕获 Resopnse Body 和 Headers 信息
    proxy.new_har(options={"captureContent": True, "captureHeaders": True})
    # 当前页数
    page = 10
    # 请求当前地址内容  一个url为1集
    base_url_list = []

    for _ in range(2):
        # 打开看片网
        try:
            driver.get("https://www.kpkuang.com/vodplay/421945-5-" + str(page) + ".html")
        except:
            print("加载页面太慢，停止加载，继续下一步操作")
        # 代理拦截对象
        result = proxy.har
        # print(result)
        _url = ""
        for entry in result["log"]["entries"]:
            # 获取异步请求的url
            _url = entry["request"]["url"]
            if "https://qimihe.com/?url" in _url:
                break
        try:
            driver.get(_url)
        except:
            print("加载页面太慢，停止加载，继续下一步操作")
        # 代理拦截对象
        result = proxy.har
        for entry in result["log"]["entries"]:
            # 获取异步请求的url
            _url = entry["request"]["url"]
            if "hls/index.m3u8" in _url:
                break
        # 请求当前地址内容  一个url为1集
        base_url_list.append(_url)
        page = page + 1
    # 记得关闭服务器了
    # 关闭浏览器
    server.stop()
    driver.quit()
    # 生成器生成url列表
    # url分组 3个为1组,分成3组  每组用一个线程去处理
    request_url_list = []
    for i in range(0, len(base_url_list), 3):
        request_url_list.append(base_url_list[i : i + 3])

    Threads = []
    for key, value in enumerate(request_url_list):
        t = Request(value, key * 3)
        Threads.append(t)
        t.start()
    # 等待所有线程结束
    for t in Threads:
        t.join()
    for key, value in enumerate(base_url_list):
        # 执行合并
        path1 = r".\video\第" + str(key + 1) + "集\*.ts"
        path2 = r".\video\第" + str(key + 1) + "集.mp4"
        os.system("copy /b " + path1 + " " + path2)
    print("合并完成")


if __name__ == "__main__":
    main()
```
!>  安装AES解密模块 [参考地址](https://www.pycryptodome.org/en/latest/src/installation.html#windows-from-sources-python-3-5-and-newer)  x不要即可 需要vs2019环境
```powershell
pip install pycryptodome --no-binary :all: 
```

#### selenium京东秒杀
```py
# 导入了 web 驱动模块
from selenium import webdriver

# 导入浏览器配置
from selenium.webdriver.chrome.options import Options

# 导入等待模块显性等待类
from selenium.webdriver.support.wait import WebDriverWait

# 导入等待模块条件类
from selenium.webdriver.support import expected_conditions as EC

# 定位方式类
from selenium.webdriver.common.by import By

# 导入动作类
from selenium.webdriver.common.action_chains import ActionChains as AC

# 导入BeautifulSoup类库
from bs4 import BeautifulSoup

# 导入时间模块
import time

# 抢购地址 尿不湿M22 片13块
BaseUrl = "https://item.jd.com/100014486277.html"


def login(driver, wait):
    # 登录 ReturnUrl  为抢购地址
    driver.get("https://passport.jd.com/uc/login?ltype=logout&ReturnUrl=" + BaseUrl)
    # 点击微信登录
    login_button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, "#kbCoagent ul li:nth-child(2)")))
    login_button.click()
    # 10秒扫描登录时间 登录后跳转到商品详情页
    time.sleep(10)


#   该方法用来确认元素是否存在，如果存在返回flag=true，否则返回false
def isElementExist(driver, element):
    flag = True
    try:
        driver.find_element_by_css_selector(element)
        return flag
    except:
        flag = False
        return flag


def main():
    # 下面是设置无痕浏览器的配置
    chrome_options = Options()
    chrome_options.add_argument("window-size=1920x3000")  # 指定浏览器分辨率 必须指定 不然元素不存在
    chrome_options.add_argument("--disable-gpu")  # 谷歌文档提到需要加上这个属性来规避bug
    chrome_options.add_argument("--hide-scrollbars")  # 隐藏滚动条, 应对一些特殊页面
    # chrome_options.add_argument("blink-settings=imagesEnabled=false")  # 不加载图片, 提升速度
    # chrome_options.add_argument("--headless")  # 浏览器不提供可视化页面. linux下如果系统不支持可视化不加这条会启动失败
    chrome_options.add_argument("log-level=3")  # 关闭控制台输出 INFO = 0 WARNING = 1 LOG_ERROR = 2 LOG_FATAL = 3 default is 0
    # 去除自动化弹窗提示
    chrome_options.add_experimental_option("excludeSwitches", ["enable-automation"])
    chrome_options.add_experimental_option("useAutomationExtension", False)
    # 创建了一个无痕Chrome浏览器
    driver = webdriver.Chrome(options=chrome_options)
    # 最大化 防止一些元素被遮挡不能交互
    driver.maximize_window()
    # 初始化等待对象，第三个参数0.2，表示0.2秒去检查一次
    wait = WebDriverWait(driver, 30, 0.2)
    # 扫码登录登录 自己扫
    login(driver, wait)
    # 点击
    """ 
    下面是具体测试的功能
    """
    # 无限循环 查找是否有抢购按钮
    while True:
        # 是否有抢购按钮
        if isElementExist(driver, "#btn-reservation"):
            # 抢购按钮文字
            button_text = driver.find_element(By.ID, "btn-reservation").get_attribute("textContent")
            # 抢购按钮class
            button_class = driver.find_element(By.ID, "btn-reservation").get_attribute("class")
            if button_text != "抢购":
                print("没有抢购文字")
            elif button_class.find("btn-disable") != -1:
                print("不能抢购")
            else:
                print("可以抢购")
                driver.find_element(By.ID, "btn-reservation").click()
                break
        # 是否有加入购物车按钮
        elif isElementExist(driver, "#InitCartUrl"):
            # 加入购物车按钮文字
            button_text = driver.find_element(By.ID, "InitCartUrl").get_attribute("textContent")
            # 加入购物车按钮class
            button_class = driver.find_element(By.ID, "InitCartUrl").get_attribute("class")
            if button_text != "加入购物车":
                print("没有加入购物车文字")
            elif button_class.find("btn-disable") != -1:
                print("不能加入购物车")
            else:
                print("可以加入购物车")
                driver.find_element(By.ID, "InitCartUrl").click()
                break
        # 刷新浏览器 这个值看网速
        time.sleep(0.3)
        driver.refresh()
    # 点击去结算
    qujiesuan_button = wait.until(EC.element_to_be_clickable((By.ID, "GotoShoppingCart")))
    qujiesuan_button.click()
    # 点击结算
    qujiesuan_button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".common-submit-btn")))
    qujiesuan_button.click()
    # 点击提交订单
    qujiesuan_button = wait.until(EC.element_to_be_clickable((By.ID, "order-submit")))
    qujiesuan_button.click()
    # 点击微信支付
    wx_pay_button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".plate-form-item:nth-child(2)")))
    wx_pay_button.click()
    # 下面付款 抢购结束
    """ 
    上面是具体测试的功能
    """
    # 停留500秒
    time.sleep(500)
    # 关闭驱动
    driver.quit()


if __name__ == "__main__":
    main()
```

### 自动化测试工具playwright

1. 安装
   ```powershell
    pip install playwright
    playwright install
   ```
2. 打开谷歌浏览器，并开启录制
   ```powershell
   playwright codegen -b chromium http://rc.aiyunc.cn
   ```
3. 文档地址
   [使用](https://playwright.dev/python/docs/intro/) [api](https://playwright.dev/python/docs/api/class-page)

### 移动端自动化工具appium
#### 安装服务端
```powershell
npm install -g appium
```
#### 开启
```powershell
appium
```

#### 安装python客户端
```powershell
pip install Appium-Python-Client
```

!> 其他安卓环境参考App开发那块

#### 连接夜神模拟器
```powershell
abd connect 127.0.0.1:62001
```

#### 检测环境是否安装完成
```powershell
npm install -g appium-doctor
appium-doctor # 有警告无所谓不影响使用
```

#### 获取必要的参数
1. 获取设备名称
   ```powershell
   adb devices
   ```
2. 获取包名和启动页面
   先打开测试app，然后执行下面的命令
   ```powershell
   adb shell
   dumpsys activity | grep mFocusedActivity # 如 mFocusedActivity: ActivityRecord{a20305f u0 com.example.novalapp/.MainActivity t5}
   ```
3. 查看手机型号
   ```powershell
   adb shell getprop ro.build.version.release #7.1.2
   ```

#### 启动app
```py
from appium import webdriver

desired_caps = {
    "platformName": "Android",
    "deviceName": "device",
    "platformVersion": "7.1.2",
    "appPackage": "com.example.novalapp",
    "appActivity": "com.example.novalapp.MainActivity",
    "noReset": True,  # 防止一登录登出
    "fullReset": False,  # 防止一登录登出
}

driver = webdriver.Remote("http://localhost:4723/wd/hub", desired_caps)
```

#### 获取app内元素
使用安卓adk目录下的uiautomatorviewer
位置androidsdk\tools\bin\uiautomatorviewer.bat,双击打开  
出现错误，关闭bat下面命令后重新打开:  
```powershell
adb root
```
不行的话重启模拟器，真机的话打开手机开发者权限，将USB调试按钮重新启动就好了

#### 完整示例
```py
# appium python驱动
from appium import webdriver

from appium.webdriver.common.mobileby import MobileBy as By

from appium.webdriver.common.touch_action import TouchAction as Action

# 导入等待模块显性等待类
from selenium.webdriver.support.wait import WebDriverWait

# 导入等待模块条件类
from selenium.webdriver.support import expected_conditions as EC

import time

# 启动参数
desired_caps = {
    "platformName": "Android",
    "deviceName": "device",
    "platformVersion": "7.1.2",
    "appPackage": "com.example.novalapp",
    "appActivity": "com.example.novalapp.MainActivity",
    "noReset": True,  # 防止一登录登出
    "fullReset": False,  # 防止一登录登出
}

def isElement(driver, by, value):
    """
    @description 判断元素是否存在
    @param
    @return
    """
    flag = None
    try:
        if by == "id":
            driver.find_element_by_id(value)
        elif by == "xpath":
            driver.find_element_by_xpath(value)
        elif by == "class":
            driver.find_element_by_class_name(value)
        elif by == "text":
            driver.find_element_by_android_uiautomator(value)
        flag = True
    except Exception as e:
        print("不存在")
        flag = False
    finally:
        return flag

def main():
    # 启动
    driver = webdriver.Remote("http://localhost:4723/wd/hub", desired_caps)
    # selenium的wait
    wait = WebDriverWait(driver, 30, 0.2)
    # 获取当前界面activity
    ac = driver.current_activity
    # 等待加载完成
    driver.wait_activity(ac, 30)
    # 为了测试输出还需要等内容完全加载 不然下面的print都没有数据的 实际可注释掉
    time.sleep(2)

    # 通过resource-id属性获取
    print(driver.find_element_by_id("android:id/content"))

    # 通过class属性获取（不好用）
    print(driver.find_elements_by_class_name("android.view.View"))
    # 输入内容
    wait.until(EC.element_to_be_clickable((By.CLASS_NAME, "android.widget.EditText"))).click()
    driver.find_element_by_class_name("android.widget.EditText").send_keys("搜索内容")

    # 向下滑屏 从屏幕的1/3高度位置 滑到3/2高度位置 持续时间只能是100 或者 200
    screen_width = driver.get_window_size()["width"]
    screen_height = driver.get_window_size()["height"]
    driver.swipe(screen_width / 2, screen_height / 3, screen_width / 2, screen_height * 2 / 3, 100)
    time.sleep(3)

    # 通过content-desc获取 复制出来有换行的就是\n （好用推荐）
    print(driver.find_element_by_accessibility_id("分类\nTab 2 of 3"))
    # 等待元素可点击
    # driver.find_element_by_accessibility_id("分类\nTab 2 of 3").click()
    wait.until(EC.element_to_be_clickable((By.ACCESSIBILITY_ID, "分类\nTab 2 of 3"))).click()
    time.sleep(3)

    # 通过文本text属性来获取
    wait.until(EC.element_to_be_clickable((By.ACCESSIBILITY_ID, "主页\nTab 1 of 3"))).click()
    print(driver.find_element_by_android_uiautomator('text("全站小说搜索就看，惊喜等你来发现")'))
    wait.until(EC.element_to_be_clickable((By.ANDROID_UIAUTOMATOR, 'text("全站小说搜索就看，惊喜等你来发现")'))).click()
    time.sleep(3)

    # 使用xpath定位子元素 index ：按同一控件类型 然后找index
    # print(driver.find_element_by_xpath("//android.widget.ScrollView/android.widget.ImageView[1]"))
    print(driver.find_element_by_xpath('//*[@class="android.widget.ScrollView"]/android.widget.ImageView[1]'))
    wait.until(
        EC.element_to_be_clickable((By.XPATH, "//android.widget.ScrollView/android.widget.ImageView[1]"))
    ).click()
    time.sleep(3)

    # 模拟点击返回按钮 显示的边界是 [0,54][126,203] tap第一个参数是表示手指包含的区域
    driver.tap([(0, 54), (126, 203)], 10)
    time.sleep(3)

    # 模拟点击任意位置 长按,移动等等
    Action(driver).tap(x=540, y=930, count=1).release().perform()
    # Action(driver).press(x=540, y=930).release().perform()
    time.sleep(3)
    
    # 返回上一页
    driver.back()

if __name__ == "__main__":
    main()
```

#### 淘宝自动领喵币(真机，模拟器不行，有检测)
```py
# appium python驱动
from appium import webdriver

from appium.webdriver.common.mobileby import MobileBy as By

from appium.webdriver.common.touch_action import TouchAction as Action

# 导入等待模块显性等待类
from selenium.webdriver.support.wait import WebDriverWait

# 导入等待模块条件类
from selenium.webdriver.support import expected_conditions as EC

import time

# 启动参数
desired_caps = {
    "platformName": "Android",
    "deviceName": "3030140101005KU",
    "platformVersion": "11",
    "appPackage": "com.taobao.taobao",
    "appActivity": "com.taobao.tao.TBMainActivity",
    "noReset": True,  # 防止一登录登出
    "fullReset": False,  # 防止一登录登出
}


"""
@description 判断元素是否存在
@param 
@return 
"""


def isElement(driver, by, value):
    """
    @description 判断元素是否存在
    @param
    @return
    """
    flag = None
    try:
        if by == "id":
            driver.find_element_by_id(value)
        elif by == "xpath":
            driver.find_element_by_xpath(value)
        elif by == "class":
            driver.find_element_by_class_name(value)
        elif by == "text":
            driver.find_element_by_android_uiautomator(value)
        flag = True
    except Exception as e:
        print("不存在")
        flag = False
    finally:
        return flag


def main():
    # 启动
    driver = webdriver.Remote("http://localhost:4723/wd/hub", desired_caps)
    # selenium的wait
    wait = WebDriverWait(driver, 30, 0.2)
    # 获取当前界面activity
    ac = driver.current_activity
    # 等待加载完成
    driver.wait_activity(ac, 30)
    # 为了测试输出还需要等内容完全加载 不然下面的print都没有数据的 实际可注释掉
    time.sleep(3)
    # 点击双11
    driver.tap([(26, 1056), (540, 1321)], 10)
    time.sleep(8)

    # 点击做任务
    Action(driver).press(x=939, y=1808).release().perform()
    time.sleep(3)

    # 完成签到任务
    if isElement(driver, "text", 'text("完成签到")'):
        wait.until(EC.element_to_be_clickable((By.ANDROID_UIAUTOMATOR, 'text("完成签到")'))).click()
        print("完成签到任务")
        time.sleep(3)

    # 获取屏幕尺寸
    screen_width = driver.get_window_size()["width"]
    screen_height = driver.get_window_size()["height"]
    # 完成浏览任务
    while True:
        if isElement(driver, "text", 'text("浏览15秒得奖励")'):
            wait.until(EC.element_to_be_clickable((By.ANDROID_UIAUTOMATOR, 'text("浏览15秒得奖励")'))).click()
            # 浏览他喵的15秒
            time.sleep(20)
            print("完成浏览15秒得奖励任务")
            driver.back()
            # 返回上一页
            time.sleep(3)
        else:
            break

    while True:
        if isElement(driver, "text", 'text("浏览15s立得")'):
            wait.until(EC.element_to_be_clickable((By.ANDROID_UIAUTOMATOR, 'text("浏览15s立得")'))).click()
            # 浏览他喵的15秒
            time.sleep(20)
            print("完成浏览15s立得任务")
            driver.back()
            # 返回上一页
            time.sleep(3)
        else:
            break

    # 任务完成
    time.sleep(5)
    print("所有任务完成")


if __name__ == "__main__":
    main()
```

### 更牛逼的跨平台自动化测试工具airtest
下载 [airtestIDE](https://airtest.netease.com/)
1. 确保adb连接手机或模拟器
2. 然后打开IDE——>connect
3. 使用IDE编写脚本
4. IDE运行
5. 或使用脚本运行
   ```powershell
   "D:\AirtestIDE\AirtestIDE" runner "C:\xxx\taobao.air"  --device android://127.0.0.1:5037/设备名
   ```

### pyinstaller打包python脚本

#### 安装
```py
pip install pyinstaller
```

#### 使用
```powershell
pyinstaller -F filename.py
```

其他选项

| 选项       | 描述                       |
| ---------- | -------------------------- |
| -h         | 帮助                       |
| -F         | 产生单个可执行文件         |
| -D         | 产生一个目录作为可执行程序 |
| -w         | 不显示windows命令行窗口    |
| --distpath | 指定输出目录               |
| -n         | 指定项目名称               |

!>打包的python文件要有main()入口函数

### python图表pyecharts

#### 安装
```py
pip install pyecharts
```
#### 使用
[文档地址](https://pyecharts.org/#/zh-cn/quickstart)


### 桌面软件自动化
#### 安装
需要pywinauto、win32gui、pyautogui

#### 实例 自动发送文字 按ENTER键发送
例如输入
请输入进程号：22744
请输入窗口名：微信
请输入输入框坐标x:500
请输入输入框坐标y:550
请输入发送的弹幕文字：自动发送
请输入发送频率：3
```py
# 时间接口
from time import sleep

# 打开已有窗口或者启动窗口
from pywinauto.application import Application

# win32界面接口
import win32gui

# 鼠标键盘自动化操作基于窗口
import pyautogui

# 复制粘贴
import pyperclip

# 忽略64python  运行win32的警告
import warnings


def autoSend(process, name, x, y, text, second):
    # 忽略警告
    with warnings.catch_warnings():
        warnings.simplefilter("ignore")
        # backend 有2种 win32  uia
        app = Application(backend="win32").connect(process=int(process))

    # 找到当前窗口
    win = app[name]

    # 置顶当前窗口
    win.set_focus()

    # 获取窗口控件类名
    calss_name = win.element_info.class_name

    # 找到gui界面
    hwnd = win32gui.FindWindow(calss_name, u"" + name + "")

    # 获取窗口边界
    left, top, right, bottom = win32gui.GetWindowRect(hwnd)

    # 复制文字
    pyperclip.copy(text)

    while True:
        # 移动鼠标  这里不需要了
        # pyautogui.moveTo(x=left + 100, y=bottom - 30, duration=0, tween=pyautogui.linear)
        # 单击
        pyautogui.click(x=left + int(x), y=top + int(y))

        # 输入文本
        # pyautogui.typewrite(message=text, interval=0.25)
        # 这里可以直接ctrl+c复制,使用组合键
        # 模拟组合热键
        pyautogui.hotkey("ctrl", "v")

        # 发送
        pyautogui.press("enter")

        # 发送间隔
        sleep(float(second))


def main():
    print("欢迎使用自动发送脚本")
    process = input("请输入进程号：")
    name = input("请输入窗口名：")
    x = input("请输入输入框坐标x:")
    y = input("请输入输入框坐标y:")
    text = input("请输入发送的弹幕文字：")
    second = input("请输入发送频率：")
    autoSend(
        process,
        name,
        x,
        y,
        text,
        second,
    )


if __name__ == "__main__":
    main()

"""
# 最大化
win.maximize()
# 最小化
win.minimize()
# 恢复正常
win.restore()
# 关闭窗口
win.close()
# 连接窗口 https://www.kancloud.cn/gnefnuy/pywinauto_doc/1193049 搜索connect
Application(backend="win32").connect(handle =21564) #句柄通过ViewWizard2.8.0获取
Application(backend="win32").connect(process=21564)
#pywinauto 文档
#https://www.kancloud.cn/gnefnuy/pywinauto_doc/1193049
"""
```

### 键盘鼠标控制
1. autohotkey
```py
    """
    https://www.autohotkey.com/
    py -m ahkpy .\hello.py
    """
    import sys
    import ahkpy as ahk
    from ahkpy.flow import sleep
    @ahk.hotkey("F1")
    def bye():
        """
        @description  添加函数热键监听F1终止程序
        @param
        @return
        """
        ahk.message_box("Bye!")
        sys.exit()
    sleep(2)
    while True:
        # 发送按键 https://www.autohotkey.com/docs/commands/Send.htm#keynames 组合键 https://www.autohotkey.com/docs/commands/Send.htm#Parameters
        ahk.send_event("{Backspace}")
        sleep(1)
```


2. Virtualkey
```py
    from re import T
    from bdtime import tt  # 新版由 bd_time 改为 bdtime
    from VirtualKey import keybd_event, scancode_down_up, scancodes, down_up, vk
    # keybd_event
    def f(ch):
        ret = keybd_event(ch)
        return ret
        
    tt.sleep(3)
    while True:
        f(vk.a)
        tt.sleep(0.02)
```

3. win32api
```py
    import ctypes
    import win32con
    import time

    class GUIAuto:
        def press(self, key_code):
            """
            @description 按下按键
            @param key_code a=65
            @return
            """
            ctypes.windll.user32.keybd_event(key_code, key_code, 0, 0)
            time.sleep(0.2)
            ctypes.windll.user32.keybd_event(key_code, key_code, win32con.KEYEVENTF_KEYUP, 0)

        def pressLong(self, key_code, second, i=0):
            """
            @description 长按按键
            @param key_code a=65
            @return
            """
            i += 1
            ctypes.windll.user32.keybd_event(key_code, key_code, 0, 0)
            time.sleep(0.02)
            if i * 0.02 == second:
                ctypes.windll.user32.keybd_event(key_code, key_code, win32con.KEYEVENTF_KEYUP, 0)
            else:
                self.pressLong(key_code, second, i)

        def move(self, x, y):
            """
            @description 鼠标移动到某一位置
            @param x 坐标x
            @param y 坐标y
            @return
            """
            ctypes.windll.user32.SetCursorPos(x, y)

        def click(self, x, y):
            """
            @description 鼠标单机某一位置
            @param x 坐标x
            @param y 坐标y
            @return
            """
            ctypes.windll.user32.SetCursorPos(x, y)
            # 枚举值 2是按下 4是抬起
            ctypes.windll.user32.mouse_event(2, 0, 0, 0, 0)
            ctypes.windll.user32.mouse_event(4, 0, 0, 0, 0)

        def drag(self, x, y, x1, y1):
            """
            @description   鼠标拖拽
            @param x 坐标x
            @param y 坐标y
            @param x1 拖动后坐标x
            @param y1 拖动后坐标y
            @return
            """
            ctypes.windll.user32.SetCursorPos(x, y)
            ctypes.windll.user32.mouse_event(2, 0, 0, 0, 0)
            time.sleep(1)
            ctypes.windll.user32.SetCursorPos(x1, y1)
            time.sleep(1)
            ctypes.windll.user32.mouse_event(4, 0, 0, 0, 0)
    def main():
        gui_auto = GUIAuto()
        time.sleep(3)
        while True:
            gui_auto.pressLong(65, 0.5)
            gui_auto.pressLong(68, 0.5)
    if __name__ == "__main__":
        main()
```
4. [kmclass虚拟键鼠驱动](https://di1shuai.com/kmclass%E8%99%9A%E6%8B%9F%E9%94%AE%E9%BC%A0%E9%A9%B1%E5%8A%A8.html)
    第二次启动会有报错，是因为第一次结束没有关闭服务 [证书配置](https://blog.csdn.net/weixin_45875105/article/details/117739777)
    禁止驱动签名  开启测试模式 全部完成后再重启运行示例即可



