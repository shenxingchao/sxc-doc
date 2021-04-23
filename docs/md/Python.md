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

#### 可变参数
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

#### 正确的函数书写方法
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
#随机整数
print(random.randint(1, 100))
#一个范围内的随机数
print(random.randrange(1, 100))
```

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
