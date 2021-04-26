# Python
## 文档地址
[地址](https://docs.python.org/zh-cn/3/)
## 环境搭建
### windows10 python3.9.4环境搭建
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

1. Python官网下载安装文件 [下载地址](https://www.python.org/downloads/windows/)
2. 选择自定义安装 选择好安装路径 不能有中文
3. 能安装的插件全部勾上，选中Add to Path添加到环境变量
4. win+r 打开cmd 输入python -V  命令行输出Python 3.9.4 解释器安装成功

## python100天入门笔记

### 第一个python脚本
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

编写如下代码，保存为hello.py
```py
print("hello,world!")
```
命令行运行 
```bash
python hello.py
```
或 
```bash
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
# 取斐波那契数列的前n项
"""
@description 取斐波那契数列的前n项
@param n 项数
@return arr 存储数据的列表
"""


def fbnq(n):
    arr = [1, 1]
    if n == 1 or n == 2:
        return 1
    else:
        for i in range(2, n):
            s = arr[i - 2] + arr[i - 1]  # i-2项 和 i-1项的和等于当前项，再把当前项记录下来
            arr.append(s)
        return arr


arr = fbnq(100)
print(arr)
```

### 函数
<p align="left" style="color:#777777;">发布日期：2021-04-19</p>

#### 最基础的函数定义 参数和返回值
```py
"""
@description 求输入的两个数之和 
@param a 数1
@param b 数2
@return int 两个数之和
"""


def sum(a, b):
    return a + b


a = int(input("请输入第一个数："))
b = int(input("请输入第二个数："))
print(sum(a, b))
```

#### 可选参数
不确定参数个数的情况下可以使用
```py
"""
@description 求输入的两个数之和 
@param a 数1
@param b 数2
@return int 两个数之和
"""


def sum(*args):
    sum = 0
    for item in args:
        sum += item
    return sum


a = int(input("请输入第一个数："))
b = int(input("请输入第二个数："))
c = int(input("请输入第三个数："))
print(sum(a, b))
print(sum(a, b, c))
```

理解位置参数*args 和**kwargs  kw=>keywords 这两个的参数名字可以随便起,不固定
```py
"""
@description 定义一个函数有三个参数，function(正常参数，将7,8,9打包成元组给函数使用,将a=1,b=2,c=3打包成字典给函数使用)
@param arg 正常参数
@param *args 位置参数，不确定个数的参数打包成的元组
@param **kwargs 关键字参数，不确定个数的参数打包成的字典
@return 
"""


def function(arg, *args, **kwargs):
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

#python里当前执行的模块名字是__main__ 如果当前文件被导入到其他文件时，下面代码不会执行
if __name__ == '__main__':
    main()
```

### 模块
<p align="left" style="color:#777777;">发布日期：2021-04-20</p>

假如有一个函数库fn.py
```py
"""
@description 求输入的两个数之和 
@param a 数1
@param b 数2
@return int 两个数之和
"""


def sum(*args):
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

a = int(input("请输入第一个数："))
b = int(input("请输入第二个数："))
c = int(input("请输入第三个数："))
print(sum(a, b))
print(sum(a, b, c))
print(fn.sum(a, c))
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
    """
    @description 构造方法
    @param name姓名
    @return
    """

    def __init__(self, name):
        self.name = name

    """
    @description 介绍自己的姓名
    @param 
    @return 
    """

    def sayName(self):
        print("my name is " + self.name)

    """
    @description 说话的方法 
    @param content 说话的内容
    @return 
    """

    def say(self, content):
        print("I say " + content)

    """
    @description 私有方法类外部直接通过方法名不能访问，可以通过_Person__saySex()访问
    @param 
    @return 
    """

    def __saySex(self):
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

    """
    @description  定义一个get装饰器 用于获取类内部私有属性
    @param
    @return name
    """

    @property
    def name(self):
        return self.__name

    """
    @description 定义一个set装饰器，用于设置类内部私有属性值
    @param 
    @return 
    """

    @name.setter
    def name(self, name):
        self.__name = name

    """
    @description  定义一个get装饰器 用于获取类内部私有属性
    @param
    @return age
    """

    @property
    def age(self):
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

    """
    @description 静态方法装饰器 不需要创建对象即可调用 ,即第一个参数不需要传self
    @param 
    @return 
    """

    @staticmethod
    def staticFn(content):
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

    """
    @description 设置公司地址 
    @param 
    @return 
    """

    @address.setter
    def address(self, address):
        self.__address = address

    """
    @description 获取公司名称 
    @param 
    @return 
    """

    def getCompanyName(self):
        return self.__company_name


# 定义个员工类  继承公司类
class Person(Company):
    # 实例化方法
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    # 重写父类的方法 称之为多态。。。
    """
    @description 获取公司名称 
    @param 
    @return 
    """

    def getCompanyName(self):
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

#### 抽象类和抽象方法
<p align="left" style="color:#777777;">发布日期：2021-04-25</p>

```py
# 从abc模块 抽象类需要的模块
from abc import ABCMeta, abstractmethod

# 定义一个动物抽象类
class Animal(metaclass=ABCMeta):

    """
    @description 定义一个抽象方法 不需要实现 也不强制为空
    @param
    @return
    """

    @abstractmethod
    def say(self):
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

| 符号               | 解释                             | 示例                | 说明                                                                            |
| ------------------ | -------------------------------- | ------------------- | ------------------------------------------------------------------------------- |
| .                  | 匹配任意字符                     | b.t                 | 可以匹配bat / but / b#t / b1t等                                                 |
| \\w                | 匹配字母/数字/下划线             | b\\wt               | 可以匹配bat / b1t / b_t等<br>但不能匹配b#t                                      |
| \\s                | 匹配空白字符（包括\r、\n、\t等） | love\\syou          | 可以匹配love you                                                                |
| \\d                | 匹配数字                         | \\d\\d              | 可以匹配01 / 23 / 99等                                                          |
| \\b                | 匹配单词的边界                   | \\bThe\\b           |                                                                                 |
| ^                  | 匹配字符串的开始                 | ^The                | 可以匹配The开头的字符串                                                         |
| $                  | 匹配字符串的结束                 | .exe$               | 可以匹配.exe结尾的字符串                                                        |
| \\W                | 匹配非字母/数字/下划线           | b\\Wt               | 可以匹配b#t / b@t等<br>但不能匹配but / b1t / b_t等                              |
| \\S                | 匹配非空白字符                   | love\\Syou          | 可以匹配love#you等<br>但不能匹配love you                                        |
| \\D                | 匹配非数字                       | \\d\\D              | 可以匹配9a / 3# / 0F等                                                          |
| \\B                | 匹配非单词边界                   | \\Bio\\B            |                                                                                 |
| []                 | 匹配来自字符集的任意单一字符     | [aeiou]             | 可以匹配任一元音字母字符                                                        |
| [^]                | 匹配不在字符集中的任意单一字符   | [^aeiou]            | 可以匹配任一非元音字母字符                                                      |
| *                  | 匹配0次或多次                    | \\w*                |                                                                                 |
| +                  | 匹配1次或多次                    | \\w+                |                                                                                 |
| ?                  | 匹配0次或1次                     | \\w?                |                                                                                 |
| {N}                | 匹配N次                          | \\w{3}              |                                                                                 |
| {M,}               | 匹配至少M次                      | \\w{3,}             |                                                                                 |
| {M,N}              | 匹配至少M次至多N次               | \\w{3,6}            |                                                                                 |
| \|                 | 分支                             | foo\|bar            | 可以匹配foo或者bar                                                              |
| (?#)               | 注释                             |                     |                                                                                 |
| (exp)              | 匹配exp并捕获到自动命名的组中    |                     |                                                                                 |
| (?&lt;name&gt;exp) | 匹配exp并捕获到名为name的组中    |                     |                                                                                 |
| (?:exp)            | 匹配exp但是不捕获匹配的文本      |                     |                                                                                 |
| (?=exp)            | 匹配exp前面的位置                | \\b\\w+(?=ing)      | 可以匹配I'm dancing中的danc                                                     |
| (?<=exp)           | 匹配exp后面的位置                | (?<=\\bdanc)\\w+\\b | 可以匹配I love dancing and reading中的第一个ing                                 |
| (?!exp)            | 匹配后面不是exp的位置            |                     |                                                                                 |
| (?<!exp)           | 匹配前面不是exp的位置            |                     |                                                                                 |
| *?                 | 重复任意次，但尽可能少重复       | a.\*b<br>a.\*?b     | 将正则表达式应用于aabab，前者会匹配整个字符串aabab，后者会匹配aab和ab两个字符串 |
| +?                 | 重复1次或多次，但尽可能少重复    |                     |                                                                                 |
| ??                 | 重复0次或1次，但尽可能少重复     |                     |                                                                                 |
| {M,N}?             | 重复M到N次，但尽可能少重复       |                     |                                                                                 |
| {M,}?              | 重复M次以上，但尽可能少重复      |                     |                                                                                 |

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
    re.I 大小写不敏感 re.M 多行匹配
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


"""
@description 计算2个数的和 
@param  q 队列
@param  a 数1
@param  b 数2
@return 
"""


def fn(q, a, b):
    # 把a+b的结果放入队列
    q.put(a + b)


"""
@description 读取计算的和
@param  q 队列
@return 
"""


def readRes(q):
    while True:
        print(q.get())  # 一次输出 7 9


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

"""
@description 计算2个数的和 
@param  a 数a
@param  b 数b
@return 
"""


def fn(a, b):
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
from threading import Thread, Lock
from time import sleep

lock = Lock()

"""
@description 把num放入List 
@param List 列表
@param num 数
@return 
"""


def fn(List, num):
    # 获取锁
    lock.acquire()
    try:
        sleep(0.01)  # 假如放入一个数的时间需要0.01秒
        List.append(num)
    finally:
        # 释放锁
        lock.release()


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
```bash
您输入了：a=1,b=2,c=3,d=4.000000,a+b=3
a=1,b=2,c=3,d=4.000,a+b=3
```

#### 随机数
```py
import random

# 随机整数
print(random.randint(0, 100))  # 输出0~100中随机的一个数
# 一个范围内的随机数
print(random.randrange(0, 101, 50))  # 只能输出 0 50 100 三个数中的一个
```
!> randint产生的随机数包含左右边界，randrange只包含左边界，且randrange可以设定步长

#### 数学方法
数学方法很多,参照[这里](https://docs.python.org/zh-cn/3/library/math.html)
```py
# 导入函数
import math

# 平方根
print(math.sqrt(4))
# sin函数
print(math.sin(90))
```

#### 计算长度
```py
str = "hello world"
arr = [1, 2, 3, 4]
print(len(str))  # 输出11
print(len(arr))  # 输出4
```

#### os模块函数
**清除输出**
```py
import os

print("hello")
os.system("cls")
print("world")
# 只会看到输出world
```

#### time模块
**睡眠**
```py
import time

i = 0
while True:
    print(i)
    time.sleep(1)  # 睡眠1秒
    i += 1
```

### pip
<p align="left" style="color:#777777;">发布日期：2021-04-15</p>

***pip*** python工具库安装工具
#### 查看版本号
```
pip -V
```

####查看已经安装的第三方库
```
pip list
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
```bash
pyp install --upgrade pip
```
!> 如果升级出现错误ModuleNotFoundError: No module named 'pip'
```bash
python -m ensurepip
python -m pip install --upgrade pip
```
[找到其来源](https://docs.python.org/3/library/ensurepip.html#command-line-interface)

#### 代码格式化库black
```bash
pip install black
```
