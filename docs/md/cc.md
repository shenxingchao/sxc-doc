## 标签
```py
"""
标签控件
"""
from PySide2.QtCore import Qt
from PySide2.QtGui import QPixmap
from PySide2.QtWidgets import QApplication, QLabel, QWidget
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
        # 创建标签
        label = QLabel("hello PySde2！hello PySde2！hello PySde2！hello PySde2！hello PySde2！", self)
        # 设置样式
        label.resize(100, 300)
        label.move(120, 120)
        label.setStyleSheet("background:green;")
        label.setMargin(20)
        # 设置对齐方式
        label.setAlignment(Qt.AlignRight | Qt.AlignTop)
        # 设置文本可鼠标选中 且可编辑
        label.setTextInteractionFlags(Qt.TextSelectableByMouse | Qt.TextEditable)
        # 设置尺寸根据内容调整，换行失效
        # label.adjustSize()
        # 设置单词换行
        label.setWordWrap(True)
        # 设置文本
        label.setText("哈哈哈")

        # 创建图片标签
        label2 = QLabel(self)
        label2.resize(50, 50)
        label2.move(120, 300)
        # 切记这里的路径不能设置错误，否则无法显示
        label2.setPixmap(QPixmap("./1.jpg"))
        # 根据图片缩放 显示原始大小
        # label2.adjustSize()
        # 根据标签大小显示图片
        label2.setScaledContents(True)


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

## 进度条
```py
"""

"""
from PySide2.QtCore import QTimer, Qt
from PySide2.QtWidgets import QApplication, QProgressBar, QWidget
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
        # 进度条
        progress_bar = QProgressBar(self)
        progress_bar.resize(300, 20)
        progress_bar.move(100, 200)
        # 设置范围
        progress_bar.setMinimum(0)
        progress_bar.setMaximum(100)
        # progress_bar.setRange(0,100)
        # 设置当前进度
        progress_bar.setValue(50)
        # 获取进度
        progress_bar.value()
        # 设置文字水平居中
        progress_bar.setAlignment(Qt.AlignHCenter)
        # 设置方向
        # progress_bar.setOrientation(Qt.Vertical)
        progress_bar.setOrientation(Qt.Horizontal)
        # 设置显示格式 %p:百分比   %v:当前值   %m:总值
        progress_bar.setFormat("当前下载%v%")
        # 创建定时器对象
        timer = QTimer(progress_bar)

        def progressBarStart():
            """
            @description  进度条值定时更新方法
            @param
            @return
            """
            if progress_bar.value() == progress_bar.maximum():
                timer.stop()
            progress_bar.setValue(progress_bar.value() + 10)

        # 时间定时信号
        timer.timeout.connect(progressBarStart)
        # 设置定时时间
        timer.start(1000)
        # 信号
        progress_bar.valueChanged.connect(lambda val: print("当前下载：", val))


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

## 对话框
```py
"""
对话框
"""
from PySide2.QtGui import QPixmap
from PySide2.QtWidgets import QApplication, QCheckBox, QErrorMessage, QMessageBox, QPushButton, QWidget
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
        # 创建错误消息提示框
        error_message = QErrorMessage(self)
        # 设置标题
        error_message.setWindowTitle("提示")
        # 设置内容同时展示
        error_message.showMessage("失败")
        # 展示
        # error_message.show()

        # 1.创建消息提示框
        message = QMessageBox(self)
        # 2.设置非模态
        message.setModal(False)
        # 显示消息框
        message.show()

        # 1.创建
        message_box = QMessageBox(QMessageBox.Warning, "警告", "你好不对", QMessageBox.Ok, self)
        # 2.设置标题
        message_box.setWindowTitle("提示")
        # 3.设置图标
        message_box.setIcon(QMessageBox.Question)
        # 4.设置自定义图标
        message_box.setIconPixmap(QPixmap("./1.png"))
        # 5.设置内容
        message_box.setText("操作成功！")
        # 6.设置提示内容
        message_box.setInformativeText("提示内容")
        # 7.设置详细文本
        message_box.setDetailedText("详细内容")
        # 8.设置复选框
        message_box.setCheckBox(QCheckBox("同意", message_box))
        # 9.设置按钮
        message_box.setStandardButtons(QMessageBox.Ok | QMessageBox.Help | QMessageBox.Cancel)
        # 10.自定义按钮
        btn = QPushButton("确定", message_box)
        message_box.addButton(btn, QMessageBox.YesRole)
        # 11.删除按钮
        message_box.removeButton(btn)
        # 显示
        message_box.show()
        # 12.获取按钮
        ok_btn = message_box.button(QMessageBox.Ok)
        cancel_btn = message_box.button(QMessageBox.Cancel)
        # 13.判断按钮点击
        def buttonClick(btn):
            if btn == ok_btn:
                print("确定按钮按下")
            print("取消按钮按下")

        # 14.信号
        message_box.buttonClicked.connect(lambda btn: buttonClick(btn))
        message_box.show()

        # 1.创建消息提示框
        message_quick = QMessageBox()
        # 3.快捷方法
        message_quick.information(self, "信息", "内容", QMessageBox.Ok)
        # 显示消息框
        message_quick.show()


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

## 盒子布局
```py
"""
盒子布局
"""
from PySide2.QtCore import Qt
from PySide2.QtWidgets import QApplication, QBoxLayout, QLabel, QWidget
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
        label1 = QLabel("按钮1", self)
        label1.setStyleSheet("background-color:green")
        label2 = QLabel("按钮2", self)
        label2.setStyleSheet("background-color:red")
        label3 = QLabel("按钮3", self)
        label3.setStyleSheet("background-color:green")
        label4 = QLabel("按钮4", self)
        label4.setStyleSheet("background-color:green")

        # 1.创建布局
        # 垂直布局
        layout = QBoxLayout(QBoxLayout.LeftToRight, self)
        # 2.添加子控件到布局
        layout.addWidget(label1)
        # 添加子控件到布局
        layout.addWidget(label2)
        # 3.添加布局到父控件
        self.setLayout(layout)
        # 4.添加子控件
        layout.addWidget(label4)
        # 5.插入子控件
        layout.insertWidget(2, label3)
        # 6.删除子控件 需要隐藏控件
        label4.hide()
        layout.removeWidget(label4)
        # 7.添加空白块
        layout.addSpacing(100)
        # 添加2/3的剩余宽度空白
        # layout.addStretch(2)
        # 添加1/3的剩余宽度空白
        # layout.addStretch(1)

        # 8.添加子布局
        label5 = QLabel("按钮5", self)
        label5.setStyleSheet("background-color:green")
        label6 = QLabel("按钮6", self)
        label6.setStyleSheet("background-color:red")
        label7 = QLabel("按钮7", self)
        label7.setStyleSheet("background-color:green")
        v_layout = QBoxLayout(QBoxLayout.TopToBottom, self)
        # 9.添加子控件到布局
        v_layout.addWidget(label5)
        v_layout.addWidget(label6)
        v_layout.addWidget(label7)
        layout.addLayout(v_layout)


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

## 垂直或水平盒子布局
```py
"""
垂直或水平盒子布局
"""
from PySide2.QtCore import Qt
from PySide2.QtWidgets import QApplication, QHBoxLayout, QLabel, QVBoxLayout, QWidget
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
        label1 = QLabel("按钮1", self)
        label1.setStyleSheet("background-color:green")
        label2 = QLabel("按钮2", self)
        label2.setStyleSheet("background-color:red")
        label3 = QLabel("按钮3", self)
        label3.setStyleSheet("background-color:green")

        # 1.创建布局
        # 垂直布局
        # v_layout = QVBoxLayout(self)
        # 水平布局
        h_layout = QHBoxLayout(self)
        # 设置边距 左上右下
        h_layout.setContentsMargins(10, 30, 50, 70)
        # 设置内边距 row or col每块之间的间距
        h_layout.setSpacing(100)

        # 2.设置布局方向
        # 从右到左
        # self.setLayoutDirection(Qt.RightToLeft)

        # 3.添加子控件到布局
        h_layout.addWidget(label1)
        h_layout.addWidget(label2)
        h_layout.addWidget(label3)

        # 4.添加布局到父控件
        self.setLayout(h_layout)


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