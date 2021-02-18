# HybirdApp
## uniapp
### Native.js示例汇总
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

工作时用到了低功耗蓝牙开发BLE蓝牙  
[示例地址](https://ask.dcloud.net.cn/article/114)


* * *

## react native
### 环境搭建
#### java jdk
#### android studio
>! 安装gardle时  手动下载那个连接  然后配置url

#### 夜神模拟器
>! 需要将android sdk androidsdk\platform-tools 下的adb 改名为nox_adb复制到夜神模拟器的adb文件夹 因为sdk版本不一样 会跑不起来

#### 调式
npx react-devtools
>! 安装调式工具react-devtools 时 一定要先安装electron 在安装 react-devtools
npm config set electron_mirror https://cdn.npm.taobao.org/dist/electron/  
npm install -g electron
npm install -g react-devtools
启动不成功adb reverse tcp:8097 tcp:8097