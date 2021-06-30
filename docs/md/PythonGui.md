# PythonGUI
## 安装
1. 安装PySide2
```shell
pip install PySide2
```
!> 这里不用pyqt5是因为比较旧，且收费，两者的接口大都相同，但是PySide2开发的软件可以闭源商用，果断选他

2. designer
安装完PySide2 库里自带designer
路径大概是
```
Lib\site-packages\PySide2
```

## 基本案例
```py
import sys

# 这里我们提供必要的引用。基本控件位于PySide2.qtwidgets模块中。
from PySide2.QtWidgets import QApplication, QLabel, QPushButton, QWidget


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = QWidget()
    # 设置窗口标题
    window.setWindowTitle("hello PySide2!")
    # 设置窗口大小
    window.resize(500, 500)
    # 设置窗口背景颜色
    window.setStyleSheet("background:#fafafa;")

    # 在窗口上放置一个按钮
    btn = QPushButton(window)
    # 设置按钮文字
    btn.setText("第一个按钮")
    # 设置按钮宽高
    btn.resize(120, 40)
    # 移动按钮
    btn.move(190, 230)
    # 设置样式
    btn.setStyleSheet("background:blue;color:#ffffff;font-size:16px;border-radius:3px;")

    # 在窗口上放置一个bi标签
    label = QLabel(window)
    label.setText("我是标签名")
    label.setStyleSheet("font-size:20px")
    label.move(200, 200)

    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 类模板
未完善，不确定
```py
"""
类模板
"""
from PySide2.QtWidgets import QApplication, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 放置一个按钮
        self.addBtn("第一个按钮", 190, 230, 120, 40)
        self.addBtn("第二个按钮", 0, 230, 120, 40)
        self.addBtn("第三个按钮", 380, 230, 120, 40)

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        DEFAULT_STYLE = "width:120px;height:40px;background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 在窗口上放置一个按钮
        btn = QPushButton(self)
        # 设置按钮文字
        btn.setText(text)
        # 设置按钮宽高
        btn.resize(width, height)
        # 移动按钮
        btn.move(x, y)
        # 设置样式
        btn.setStyleSheet(DEFAULT_STYLE + style)


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 父子元素操作元素删除
```py
""" 
父子元素操作
"""
from PySide2.QtCore import QObject
from PySide2.QtWidgets import QApplication, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 放置一个按钮
        btn = self.addBtn("父按钮", 100, 200, 300, 100)
        # 相当于js 设置按钮的ID
        btn.setObjectName("btn")
        # 打印按钮的ID
        print(btn.objectName())  # 输出 btn
        # 设置按钮的附加属性
        btn.setProperty("attr", "属性值1")
        btn.setProperty("name", "属性值2")
        # 打印属性
        print(btn.property("attr"))  # 输出 属性值1
        # 获取所有属性对象列表
        print(btn.dynamicPropertyNames())  # 输出[PySide2.QtCore.QByteArray(b'attr'), PySide2.QtCore.QByteArray(b'name')]
        # 创建一个按钮 这里注意的是子元素是相对于父元素定位的
        btn_son1 = self.addBtn("子按钮1", 0, 0, 120, 40, "background:red;")
        # 设置为子按钮
        btn_son1.setParent(btn)
        # 创建一个按钮
        btn_son2 = self.addBtn("子按钮2", 0, 50, 120, 40, "background:green;")
        # 设置为子按钮
        btn_son2.setParent(btn)
        # 打印父按钮的属性
        print(btn_son1.parent().property("name"))  # 输出 属性值2
        # 打印子元素列表
        print(btn.children())  # 输出 2个子按钮的列表
        # 打印子元素对象
        print(btn.findChild(QObject).text())  # 输出 子按钮1
        # 打印子元素列表
        print(btn.findChildren(QObject))  # 输出2个子按钮的列表
        # 判断控件类型
        print(btn.inherits("QPushButton"))  # 输出True
        # 删除一个按钮
        # btn_son2.deleteLater()
        # 查看指定控件有没有子元素 有则返回控件 没有则返回None
        print(btn.childAt(10, 10))  # 输出btn_son1对象
        # 查看父控件
        print(btn_son1.parentWidget())  # 输出btn对象
        # 查看子控件的范围
        print(btn.childrenRect())  # 输出PySide2.QtCore.QRect(0, 0, 120, 90)

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        DEFAULT_STYLE = "width:120px;height:40px;background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 在窗口上放置一个按钮
        btn = QPushButton(self)
        # 设置按钮文字
        btn.setText(text)
        # 设置按钮宽高
        btn.resize(width, height)
        # 移动按钮
        btn.move(x, y)
        # 设置样式
        btn.setStyleSheet(DEFAULT_STYLE + style)
        return btn


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 事件处理机制
```py
""" 
事件处理机制
"""
from PySide2.QtCore import QObject
from PySide2.QtWidgets import QApplication, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 放置一个按钮
        btn = self.addBtn("按钮", 100, 200, 300, 100)

        def handleClickBtn():
            """
            @description 点击事件
            @param
            @return
            """
            print("点击了")

        fn = lambda: print("按下了")
        # 绑定事件
        btn.clicked.connect(handleClickBtn)
        btn.pressed.connect(fn)
        # 最终先输出按下，松开鼠标后在输出点击

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        DEFAULT_STYLE = "width:120px;height:40px;background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 在窗口上放置一个按钮
        btn = QPushButton(self)
        # 设置按钮文字
        btn.setText(text)
        # 设置按钮宽高
        btn.resize(width, height)
        # 移动按钮
        btn.move(x, y)
        # 设置样式
        btn.setStyleSheet(DEFAULT_STYLE + style)
        return btn


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 类封装进阶和gui里的定时器
```py
""" 
类封装和定时器
"""
from PySide2.QtWidgets import QApplication, QPushButton, QWidget
import sys
from threading import Timer


class Window(QWidget):
    def __init__(self, *args, **kwargs):
        # 调用父类的方法
        super().__init__(*args, **kwargs)
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 放置一个按钮
        btn = Btn("10", 100, 200, 300, 100, "", self)
        # 调用按钮定时器方法
        btn.setInterval2(1)


class Btn(QPushButton):
    def __init__(self, text="", x=0, y=0, width=0, height=0, style="", *args, **kwargs):
        DEFAULT_STYLE = "background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 调用父类的方法
        super().__init__(*args, **kwargs)
        # 设置按钮文字
        self.setText(text)
        # 设置按钮宽高
        self.resize(width, height)
        # 移动按钮
        self.move(x, y)
        # 设置样式
        self.setStyleSheet(DEFAULT_STYLE if not style else style)

    def setInterval(self, ms=1000):
        """
        @description 用内置的定时器
        @param ms 毫秒
        @return
        """
        self.interval = self.startTimer(ms)

    def timerEvent(self, *args, **kwargs):
        # 获取text中的数据
        sec = int(self.text())
        # 倒计时
        sec -= 1
        self.setText(str(sec))
        # 停止计时
        if sec == 0:
            self.killTimer(self.interval)

    def setInterval2(self, s=1):
        """
        @description 用其他的定时器
        @param s秒
        @return
        """
        self.t = Timer(s, self.setInterval2, (s,))
        self.t.start()
        self.fn()

    def fn(self):
        # 获取text中的数据
        sec = int(self.text())
        # 倒计时
        sec -= 1
        self.setText(str(sec))
        # 停止计时
        if sec == 0:
            self.t.cancel()


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 自定义按钮事件含传参
```py
""" 
自定义按钮事件(右键事件)含传参
"""
from PySide2 import QtGui
from PySide2.QtCore import Qt, Signal
from PySide2.QtWidgets import QApplication, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 放置一个按钮
        btn = self.addBtn("按钮", 100, 200, 300, 100)

        def handleClickBtn():
            """
            @description 点击事件
            @param
            @return
            """
            print("点击了")

        fn = lambda: print("按下了")
        # 绑定事件
        btn.clicked.connect(handleClickBtn)
        btn.pressed.connect(fn)
        # 最终先输出按下，松开鼠标后在输出点击
        # 绑定自定义事件 并传参，我感觉这个传参没什么鸟用
        btn.rightClicked[str, int].connect(lambda x, y: print("右键按下" + x + "，宽度是：" + str(y)))  # 输出右键按下按钮300

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        DEFAULT_STYLE = "width:120px;height:40px;background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 在窗口上放置一个按钮
        btn = Btn(self)
        # 设置按钮文字
        btn.setText(text)
        # 设置按钮宽高
        btn.resize(width, height)
        # 移动按钮
        btn.move(x, y)
        # 设置样式
        btn.setStyleSheet(DEFAULT_STYLE + style)
        return btn


class Btn(QPushButton):
    """
    @description 按钮类继承自QPushButton
    @param
    @return
    """

    # 定义类属性
    # 定义了一个信号 且传递按钮的参数出去
    rightClicked = Signal([str, int])

    def mousePressEvent(self, e: QtGui.QMouseEvent) -> None:
        """
        @description 自定义鼠标按下事件
        @param
        @return
        """
        if e.button() == Qt.RightButton:
            # e.button()的值为 1左键 2右键 比较用枚举值或1,2来比较
            # emit 触发绑定的事件
            self.rightClicked[str, int].emit(self.text(), self.width())
        return super().mousePressEvent(e)


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 使用装饰器自动连接绑定的事件
```py
""" 
使用装饰器自动连接绑定的事件
"""
from PySide2.QtCore import QMetaObject, Slot
from PySide2.QtWidgets import QApplication, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 放置一个按钮
        btn = self.addBtn("按钮", 100, 200, 300, 100)
        # 设置按钮的唯一Name值
        btn.setObjectName("btn")
        # 使用自动绑定事件通过唯一Name值
        QMetaObject.connectSlotsByName(self)

    # 加装饰器 避免打印一次
    @Slot()
    def on_btn_clicked(self):
        """
        @description 自动绑定的点击事件 必须这样写
        @param
        @return
        """
        print("点击了")

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        DEFAULT_STYLE = "width:120px;height:40px;background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 在窗口上放置一个按钮
        btn = QPushButton(self)
        # 设置按钮文字
        btn.setText(text)
        # 设置按钮宽高
        btn.resize(width, height)
        # 移动按钮
        btn.move(x, y)
        # 设置样式
        btn.setStyleSheet(DEFAULT_STYLE + style)
        return btn


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 类之间信号和槽的用法
解释一下：信号就是去触发事件的方法，槽就是要触发的事件
```py
from PySide2.QtCore import QObject, Signal


class Signal(QObject):
    """
    @description 创建一个信号类
    @param
    @return
    """

    # 定义一个信号
    clicked = Signal(str)

    def trigger(self):
        """
        @description 去触发事件的方法
        @param
        @return
        """
        self.clicked.emit("我是触发事件的参数")


class Solt(QObject):
    """
    @description他们叫槽，其实就是上面信号触发的事件
    @param
    @return
    """

    def handleClicked(self, str):
        print(str)


def main():
    # 创建信号
    signal = Signal()
    # 创建槽
    solt = Solt()
    # 绑定信号和槽  就是绑定事件
    signal.clicked.connect(solt.handleClicked)
    # 触发
    signal.trigger()  # 输出我是触发事件的参数


if __name__ == "__main__":
    main()
```

## 设置控件尺寸/边距/层级关系
```py
"""
设置控件尺寸,边距,层级关系
"""
from PySide2.QtWidgets import QApplication, QLabel, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 设置最小尺寸和最大尺寸
        self.setMaximumSize(800, 800)
        self.setMinimumSize(400, 400)
        # 放置一个按钮
        btn = self.addBtn("按钮", 190, 230, 120, 40)
        # 设置内容自适应
        btn.adjustSize()
        # 设置固定尺寸后 其他设置宽高就没用了
        btn.setFixedSize(100, 100)
        # 放置一个标签
        label = self.addLabel("标签", 10, 10, 120, 40)
        # 设置标签边距
        label.setContentsMargins(50, 0, 0, 0)
        # 打印边距
        print(label.getContentsMargins())
        # 打印内容区域
        print(label.contentsRect())
        # 放置一个标签
        label2 = self.addLabel("标签2", 20, 20, 120, 40, "background:green;")
        # 在放置一个标签叠加起来
        label3 = self.addLabel("标签3", 30, 30, 120, 40, "background:red;")
        # 让标签2在最上面 提升层级
        label2.raise_()
        # 让标签3在最小面 降低层级
        label3.lower()
        # 将标签2放到标签1的下面
        label2.stackUnder(label)

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        DEfAULT_STYLE = "width:120px;height:40px;background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 在窗口上放置一个按钮
        btn = QPushButton(self)
        # 设置按钮文字
        btn.setText(text)
        # # 设置按钮宽高
        btn.resize(width, height)
        # # 移动按钮
        btn.move(x, y)
        # 上面2行代码可以用下面的代替
        btn.setGeometry(x, y, width, height)
        # 设置样式
        btn.setStyleSheet(DEfAULT_STYLE + style)
        return btn

    def addLabel(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description  添加标签
        @param
        @return
        """
        DEfAULT_STYLE = "width:120px;height:40px;background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 窗口上放置一个标签
        label = QLabel(self)
        # 设置文字
        label.setText(text)
        # 设置位置
        label.setGeometry(x, y, width, height)
        # 设置样式
        label.setStyleSheet(DEfAULT_STYLE + style)
        return label


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 设置鼠标状态和样式
```py
"""
设置鼠标状态和样式
"""

from PySide2.QtCore import Qt
from PySide2.QtGui import QCursor, QPixmap
from PySide2.QtWidgets import QApplication, QLabel, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 设置最小尺寸和最大尺寸
        self.setMaximumSize(800, 800)
        self.setMinimumSize(400, 400)
        # 放置一个按钮
        btn = self.addBtn("按钮", 190, 230, 120, 40, "cursor:pointer;")
        # 设置鼠标移入状态为手型 https://www.riverbankcomputing.com/static/Docs/PySide2/api/qtgui/qcursor.html
        btn.setCursor(Qt.CursorShape.PointingHandCursor)
        # 设置鼠标样式
        # 导入一张图片
        image = QPixmap("1.png")
        # 图片缩放
        new_image = image.scaled(10, 10)
        # 创建鼠标对象 bitmap,offsetx,offsety 鼠标中心焦点相对于图片的偏移位置
        cursor = QCursor(new_image, 0, 0)
        # 给当前窗口设置鼠标样式 可给任意控件设置鼠标样式
        self.setCursor(cursor)

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        DEfAULT_STYLE = "width:120px;height:40px;background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 在窗口上放置一个按钮
        btn = QPushButton(self)
        # 设置按钮文字
        btn.setText(text)
        # # 设置按钮宽高
        btn.resize(width, height)
        # # 移动按钮
        btn.move(x, y)
        # 上面2行代码可以用下面的代替
        btn.setGeometry(x, y, width, height)
        # 设置样式
        btn.setStyleSheet(DEfAULT_STYLE + style)
        return btn


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```


## 鼠标事件
```py
""" 
鼠标事件
"""
from PySide2 import QtGui
from PySide2 import QtCore
from PySide2.QtWidgets import QApplication, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 设置为True则mouseMoveEvent事件不需要按下也能触发,不然要按着鼠标左键或右键才能触发
        self.setMouseTracking(True)

    def mouseMoveEvent(self, a0: QtGui.QMouseEvent) -> None:
        """
        @description 鼠标移动事件
        @param
        @return
        """
        print("鼠标移动 x:" + str(a0.x()) + ",y:" + str(a0.y()))
        return super().mouseMoveEvent(a0)

    def mousePressEvent(self, a0: QtGui.QMouseEvent) -> None:
        """
        @description 鼠标按下事件
        @param
        @return
        """
        print("鼠标按下")
        # 鼠标相对于系统桌面的位置
        print(a0.globalPos())
        # 鼠标相对于主界面的位置
        print(a0.localPos())
        return super().mousePressEvent(a0)

    def mouseDoubleClickEvent(self, a0: QtGui.QMouseEvent) -> None:
        """
        @description 鼠标双击事件
        @param
        @return
        """
        print("鼠标双击")
        return super().mouseDoubleClickEvent(a0)

    def mouseReleaseEvent(self, a0: QtGui.QMouseEvent) -> None:
        """
        @description 鼠标松开事件
        @param
        @return
        """
        print("鼠标松开")
        return super().mouseReleaseEvent(a0)

    def enterEvent(self, a0: QtCore.QEvent) -> None:
        """
        @description 鼠标进入事件
        @param
        @return
        """
        print("鼠标进入")
        return super().enterEvent(a0)

    def leaveEvent(self, a0: QtCore.QEvent) -> None:
        """
        @description 鼠标离开事件
        @param
        @return
        """
        print("鼠标离开")
        return super().leaveEvent(a0)


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 窗口事件
```py
""" 
窗口事件
"""
from PySide2 import QtGui
from PySide2.QtWidgets import QApplication, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")

    def showEvent(self, a0: QtGui.QShowEvent) -> None:
        """
        @description  窗口显示事件
        @param
        @return
        """
        print("窗口显示")
        return super().showEvent(a0)

    def hideEvent(self, a0: QtGui.QHideEvent) -> None:
        """
        @description  窗口隐藏事件
        @param
        @return
        """
        print("窗口隐藏")
        return super().hideEvent(a0)

    def closeEvent(self, a0: QtGui.QCloseEvent) -> None:
        """
        @description  窗口关闭事件
        @param
        @return
        """
        print("窗口关闭")
        return super().closeEvent(a0)

    def moveEvent(self, a0: QtGui.QMoveEvent) -> None:
        """
        @description  窗口移动事件
        @param
        @return
        """
        print("窗口移动事件")
        return super().moveEvent(a0)

    def resizeEvent(self, a0: QtGui.QResizeEvent) -> None:
        """
        @description  窗口缩放事件
        @param
        @return
        """
        print("窗口缩放事件")
        return super().resizeEvent(a0)


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 自定义顶部工具条 窗口可拖动
```py
""" 
自定义顶部工具条 窗口可拖动
"""
from PySide2 import QtGui
from PySide2.QtCore import Qt
from PySide2.QtWidgets import QApplication, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()
        # 定义鼠标是否按下，鼠标按下才能拖动
        self.is_pressed = False
        # 记录按下时窗口坐标， 这个用于窗口移动
        self.win_x = 0
        self.win_y = 0
        # 记录按下时鼠标坐标，这个用于计算鼠标移动的距离
        self.mouse_x = 0
        self.mouse_y = 0

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 设置窗口透明度
        # self.setWindowOpacity(0.9)
        # 设置没有工具条的无边框窗口
        self.setWindowFlags(Qt.WindowType.FramelessWindowHint)
        # 添加关闭按钮
        self.close_btn = self.addBtn("×", self.width() - 40, 2, 40, 40)
        # 添加最大化按钮
        self.max_btn = self.addBtn("□", self.width() - 82, 2, 40, 40)
        # 添加最小化按钮
        self.min_btn = self.addBtn("-", self.width() - 124, 2, 40, 40)
        # 关闭按钮点击绑定窗口关闭事件
        self.close_btn.pressed.connect(self.close)
        # 最大化按钮绑定窗口最大化事件和事件
        def max_event():
            if self.isMaximized():
                self.showNormal()
                self.max_btn.setText("□")
            else:
                self.showMaximized()
                self.max_btn.setText("恢复")

        self.max_btn.pressed.connect(max_event)
        # 最小化按钮绑定窗口最小化事件
        self.min_btn.pressed.connect(self.showMinimized)
        # 放置一个工具条 这里这个工具条拖动还未实现，到时候看用什么控件比较合适，然后用 # 自定义按钮事件含传参的方法，去给这个控件加拖动事件监听
        self.tool_bar = self.addBtn("", 0, 0, self.width(), 44, "background:#cccccc;")
        # 置于底层
        self.tool_bar.lower()

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
        style="",
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        DEfAULT_STYLE = "background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 在窗口上放置一个按钮
        btn = QPushButton(self)
        # 设置按钮文字
        btn.setText(text)
        # 设置宽高
        btn.setGeometry(x, y, width, height)
        # 设置样式
        btn.setStyleSheet(DEfAULT_STYLE + style)
        return btn

    def resizeEvent(self, a0: QtGui.QResizeEvent) -> None:
        """
        @description  窗口缩放事件
        @param
        @return
        """
        # 最大化最小化的时候，需要去改变按钮组位置
        self.close_btn.move(self.width() - 40, 2)
        self.max_btn.move(self.width() - 82, 2)
        self.min_btn.move(self.width() - 124, 2)
        self.tool_bar.resize(self.width(), 44)
        return super().resizeEvent(a0)

    def mousePressEvent(self, a0: QtGui.QMouseEvent) -> None:
        """
        @description 鼠标按下事件
        @param
        @return
        """
        # 如果按下的是鼠标左键
        if a0.button() == Qt.MouseButton.LeftButton:
            # 设置为按下
            self.is_pressed = True
            # 记录按下时窗口坐标， 这个用于窗口移动
            self.win_x = self.x()
            self.win_y = self.y()
            # 记录按下时鼠标坐标，这个用于计算鼠标移动的距离
            self.mouse_x = a0.globalX()
            self.mouse_y = a0.globalY()
        return super().mousePressEvent(a0)

    def mouseMoveEvent(self, a0: QtGui.QMouseEvent) -> None:
        """
        @description 鼠标移动事件
        @param
        @return
        """
        # 如果按下才能移动
        if self.is_pressed:
            # 获取移动后鼠标的位置
            mouse_move_x = a0.globalX()
            mouse_move_y = a0.globalY()
            # 计算移动的距离
            offset_x = mouse_move_x - self.mouse_x
            offset_y = mouse_move_y - self.mouse_y
            # 设置窗口移动的距离
            self.move(self.win_x + offset_x, self.win_y + offset_y)
        return super().mouseMoveEvent(a0)

    def mouseReleaseEvent(self, a0: QtGui.QMouseEvent) -> None:
        """
        @description 鼠标松开事件
        @param
        @return
        """
        self.is_pressed = False
        return super().mouseReleaseEvent(a0)


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 控件交互事件
```py
"""
控件交互事件
"""
from PySide2.QtWidgets import QApplication, QLabel, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("[*]hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 放置一个按钮
        btn = self.addBtn("按钮", 190, 230, 120, 40)
        # 设置是否可用
        btn.setEnabled(True)
        # 判断是否可用
        print(btn.isEnabled())
        # 设置是否显示
        btn.setVisible(False)
        # 判断是否显示
        print(btn.isVisible())
        # 设置是否隐藏
        btn.setHidden(False)
        # 判断是是否隐藏
        print(btn.isHidden())
        # 关闭按钮 效果和隐藏一样
        btn.close()
        # 直接隐藏
        btn.hide()
        # 直接显示
        btn.show()
        # 设置窗口是否编辑 需要在设置标题里setWindowTitle 加上[*]
        self.setWindowModified(True)  # 设置完后处理编辑的窗口标题会显示*号
        # 判断窗口是否处理编辑状态
        print(self.isWindowModified())

    def addBtn(
        self,
        text="",
        x=0,
        y=0,
        width=0,
        height=0,
    ):
        """
        @description 添加按钮
        @param
        @return
        """
        # 在窗口上放置一个按钮
        btn = QPushButton(self)
        # 设置按钮文字
        btn.setText(text)
        # # 设置按钮宽高
        btn.resize(width, height)
        # # 移动按钮
        btn.move(x, y)
        return btn


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # 创建窗口2
    window2 = Window()
    # 显示窗口
    window2.show()
    # 判断当前窗口是否是激活的窗口
    print(window.isActiveWindow())  # 输出False
    print(window2.isActiveWindow())  # 输出True
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 键盘事件
```py
"""
键盘事件
"""
from PySide2 import QtGui
from PySide2.QtCore import Qt
from PySide2.QtWidgets import QApplication, QLabel, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()

    def keyPressEvent(self, a0: QtGui.QKeyEvent) -> None:
        """
        @description 键盘按下
        @param
        @return
        """
        if a0.key() == Qt.Key.Key_A:
            print("按下了A键")
        if a0.modifiers() == Qt.KeyboardModifier.ControlModifier and a0.key() == Qt.Key.Key_C:
            print("按下了组合键Ctrl+C")
        if (
            a0.modifiers() == Qt.KeyboardModifier.ControlModifier | Qt.KeyboardModifier.ShiftModifier
            and a0.key() == Qt.Key.Key_C
        ):
            print("按下了组合键Ctrl+Shift+C")
        return super().keyPressEvent(a0)

    def keyReleaseEvent(self, a0: QtGui.QKeyEvent) -> None:
        """
        @description 键盘松开
        @param
        @return
        """
        return super().keyReleaseEvent(a0)


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 单行文本框焦点控制
```py
"""
单行文本框焦点控制
"""
from PySide2 import QtGui
from PySide2.QtCore import Qt
from PySide2.QtWidgets import QApplication, QLabel, QLineEdit, QPushButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 创建单行文本框
        text1 = QLineEdit(self)
        text1.move(100, 100)
        text2 = QLineEdit(self)
        text2.move(100, 200)
        text3 = QLineEdit(self)
        text3.move(100, 300)
        # 设置焦点
        text2.setFocus()
        # 清除焦点
        text2.clearFocus()
        # 切换焦点的方式 TabFocus tab切换  StrongFocus tab和click切换 NoFocus 不能切换 WheelFocus滚轮点击切换
        text3.setFocusPolicy(Qt.FocusPolicy.NoFocus)


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 按钮类事件单选多选复选框
```py
"""
按钮类事件单选多选复选框
"""
import PySide2
from PySide2.QtCore import QPoint, QSize
from PySide2.QtGui import QIcon
from PySide2.QtWidgets import QApplication, QCheckBox, QPushButton, QRadioButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("[*]hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 放置一个按钮
        btn = Btn("按钮", 100, 200, 300, 100, "", self)
        # 设置按钮图标
        qicon = QIcon("./1.png")
        btn.setIcon(qicon)
        # 设置图标大小
        qsize = QSize(50, 50)
        btn.setIconSize(qsize)
        # 加一个按钮按下事件
        btn.pressed.connect(lambda: print("快捷键按下了"))
        # 设置快捷键
        btn.setShortcut("Ctrl+Alt+S")
        # 设置点击按钮时自动重复点击事件 相当于点一下连点的功能
        btn.setAutoRepeat(True)
        # 设置自动重复的时间间隔2s
        btn.setAutoRepeatInterval(2000)
        # 设置首次重复延迟4s
        btn.setAutoRepeatDelay(5000)
        # 设置按钮状态为按下
        btn.setDown(True)
        # 设置模拟点击
        btn.click()
        # 有动画效果的点击 点击按住10秒
        btn.animateClick(10000)
        # 添加一个单选按钮
        radio1 = QRadioButton("单选按钮1", self)
        radio2 = QRadioButton("单选按钮2", self)
        radio2.move(0, 20)
        # 设置排他性 就是设置后可以同时选中单选按钮1和单选按钮2 否则不能
        # radio1.setAutoExclusive(False)
        # radio2.setAutoExclusive(False)
        # 设置选中
        radio1.setChecked(True)
        # 添加复选框按钮
        checkbox1 = QCheckBox("浙江", self)
        checkbox2 = QCheckBox("上海", self)
        checkbox3 = QCheckBox("北京", self)
        checkbox1.move(100, 0)
        checkbox2.move(100, 20)
        checkbox3.move(100, 40)
        # 设置排他性 就是设置后变成单选啦
        # checkbox1.setAutoExclusive(True)
        # checkbox2.setAutoExclusive(True)
        # checkbox3.setAutoExclusive(True)


class Btn(QPushButton):
    def __init__(self, text="", x=0, y=0, width=0, height=0, style="", *args, **kwargs):
        DEFAULT_STYLE = "background:blue;color:#ffffff;font-size:16px;border-radius:3px;"
        # 调用父类的方法
        super().__init__(*args, **kwargs)
        # 设置按钮文字
        self.setText(text)
        # 设置按钮宽高
        self.resize(width, height)
        # 移动按钮
        self.move(x, y)
        # 设置样式
        self.setStyleSheet(DEFAULT_STYLE if not style else style)

    def hitButton(self, pos: PySide2.QtCore.QPoint) -> bool:
        """
        @description 获取按钮点击位置pos(x,y)
        @param
        @return
        """
        # 设置只能点右半边有效果
        if pos.x() > self.width() / 2:
            return super().hitButton(pos)
        return False


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```

## 单选按钮切换事件
```py
"""
单选按钮切换事件
"""
from PySide2.QtWidgets import QApplication, QRadioButton, QWidget
import sys


class Window(QWidget):
    def __init__(self):
        # 调用父类的方法
        super().__init__()
        # 初始化UI
        self.initUI()

    def initUI(self):
        """
        @description  初始化UI
        @param
        @return
        """
        # 设置窗口标题
        self.setWindowTitle("[*]hello PySide2!")
        # 设置窗口大小
        self.resize(500, 500)
        # 设置窗口背景颜色
        self.setStyleSheet("background:#fafafa;")
        # 添加一个单选按钮
        radio1 = QRadioButton("单选按钮1", self)
        radio2 = QRadioButton("单选按钮2", self)
        radio3 = QRadioButton("单选按钮2", self)
        radio1.move(0, 0)
        radio2.move(0, 20)
        radio3.move(0, 40)
        # 设置选中
        radio1.setChecked(True)
        # 要绑定所有的才行 因为是切换 所以切换前和切换后的单选按钮的切换事件都触发了
        radio1.toggled.connect(lambda: print("切换了1"))
        radio2.toggled.connect(lambda: print("切换了2"))
        radio3.toggled.connect(lambda: print("切换了3"))


def main():
    # 创建应用程序对象  argv是命令行输入参数列表
    app = QApplication(sys.argv)
    # 创建窗口对象
    window = Window()
    # 显示窗口
    window.show()
    # app.exec_()程序一直循环运行直到主窗口被关闭终止进程  sys.exit返回退出时的状态码
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```