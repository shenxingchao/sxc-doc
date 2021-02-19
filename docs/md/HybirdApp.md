# HybirdApp
## uniapp
### Native.js示例汇总
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

工作时用到了低功耗蓝牙开发BLE蓝牙  
[示例地址](https://ask.dcloud.net.cn/article/114)


* * *

## react native
### 环境搭建
#### 一、java jdk安装和环境变量配置
[jdk1.8.0下载地址](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html#license-lightbox)  
环境变量配置

| 系统变量名称 |                                                  值 |
| :----------- | --------------------------------------------------: |
| JAVA_HOME    |                                    D:\tool\jdk1.8.0 |
| Path         |                                     %JAVA_HOME%\bin |
| Path         |                                 %JAVA_HOME%\jre\bin |
| CLASSPATH    | .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar; |

检查是否配置成功
```
java -version
```
输出java version "1.8.0_191"
    

#### 二、安装android studio
[下载地址](https://developer.android.google.cn/studio/)  
- 选择Custom选项安装
- 确保选中
  - Android SDK
  - Android SDK Platform
-  SDK Manager - SDK Platforms - Show Package Details 
   -  Android SDK Platform 29
-  SDK Manager - SDK Tools - Show Package Details
   -  Android SDK Build-Tools
      -  29.0.2版本
   -  NDK (Side by side) - Show Package Details
      -  20.1.5948944版本

?> 安装sdk和编译工具，模拟器不需要安装，用第三方

环境变量配置

| 系统变量名称 |                            值 |
| :----------- | ----------------------------: |
| ANDROID_HOME |            D:\tool\androidsdk |
| Path         | %ANDROID_HOME%\platform-tools |
| Path         |          %ANDROID_HOME%\tools |
| Path         |      %ANDROID_HOME%\tools\bin |

#### 三、安装夜神模拟器
[下载地址](https://www.yeshen.com/)

!> 备份原有模拟器的nox_adb.exe 为nox_adb_old.exe，需要将androidsdk\platform-tools 下的adb.exe 改名为nox_adb.exe复制到夜神模拟器的adb文件夹，因为sdk版本不一样会跑不起来。 

#### 四、cli创建应用
npm install -g  react-native-cli  
react-native init

#### 五、运行
- 开启模拟器
- abd连接模拟器
  - adb connect 127.0.0.1:62001
- 查看设备连接状态
  - adb devices
    - 显示：127.0.0.1:62001 device 表示已连接

运行前先下载安装gradle，手动下载那个连接，然后配置url
- 打开 android\gradle\wrapper\gradle-wrapper.properties
  - 找到 distributionUrl 下载到本地
- 替换distributionUrl 地址为本地
  - distributionUrl=file\:///D\:/tool/gradle/gradle-6.2-all.zip

```npm
npm run android
```

#### 六、调试工具
```npm
npm config set electron_mirror https://cdn.npm.taobao.org/dist/electron/  
npm install -g electron
npm install -g react-devtools
npx react-devtools
```

```
adb reverse tcp:8097 tcp:8097
```

!> 安装调式工具react-devtools 时 一定要先安装electron 再安装 react-devtools

!> 启动不成功 adb reverse tcp:8097 tcp:8097

?> 打开开发菜单 在cli.js start窗口按d就可以了 或者 adb shell input keyevent 82
