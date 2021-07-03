## 标签控件
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

## 进度条控件
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


