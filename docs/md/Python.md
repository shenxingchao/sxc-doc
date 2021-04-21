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

| 运算符                                                              | 描述                           |
| ------------------------------------------------------------------- | ------------------------------ |
| `[]` `[:]`                                                          | 下标，切片                     |
| `**`                                                                | 指数                           |
| `~` `+` `-`                                                         | 按位取反, 正负号               |
| `*` `/` `%` `//`                                                    | 乘，除，模，整除               |
| `+` `-`                                                             | 加，减                         |
| `>>` `<<`                                                           | 右移，左移                     |
| `&`                                                                 | 按位与                         |
| `^` `\|`                                                            | 按位异或，按位或               |
| `<=` `<` `>` `>=`                                                   | 小于等于，小于，大于，大于等于 |
| `==` `!=`                                                           | 等于，不等于                   |
| `is`  `is not`                                                      | 身份运算符                     |
| `in` `not in`                                                       | 成员运算符                     |
| `not` `or` `and`                                                    | 逻辑运算符                     |
| `=` `+=` `-=` `*=` `/=` `%=` `//=` `**=` `&=` `|=` `^=` `>>=` `<<=` | （复合）赋值运算符             |
```py
a = True
b = False
print(a | b, a & b)
```
输出True False

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
2. for循环输出数组
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