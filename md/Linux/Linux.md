# Linux

# 工具

## putty

## FinalShell

推荐

# 常用命令
## 切换超级管理员
su
## 创建多级目录
mkdir -p

## 清除
clear

## 上传文件到当前目录

rz -b

## 删除文件

rm -rf xxxx

## 下载命令
wget `http://download.com` 
一般下载路径放在`/usr/local/src/`
应用程序放在`/usr/local/`  

## 移动目录或文件
mv xxx /usr/local/

## 拷贝文件

cp xxx /usr/local

cp -r 递归复制目录下的所有子目录和文件到指定目录

## 修改文件夹权限

-R 表示递归

chmod -R 777/755 /xxx/xxx

## 解压 文件
- *.tar 用 tar –xvf 解压
- *.gz 用 gzip -d或者gunzip 解压
- *.tar.gz和*.tgz 用 tar -zxvf 解压
- *.bz2 用 bzip2 -d或者用bunzip2 解压
- *.tar.bz2用tar -xjvf 解压
- *.rar 用 unrar e解压
- *.zip 用 unzip 解压

## 列出所有tpc监听端口

netstat -ntpl

## 查看端口号

netstat -antp|grep 6379

## 查看进程端口号

ps -ef|grep redis

## 杀死进程

kill 端口号

kill -9 端口号 强制杀死

## 开启防火墙

- 永久开放443端口  
    firewall-cmd --zone=public --add-port=443/tcp --permanent
- 重启防火墙  
    firewall-cmd --reload

# vim命令
创建文件 vim 新建文件名  
回到命令行状态 esc  
保存 wq  
不保存 q!  
插入 i  
搜索 esc + / + search_string  
移动到末尾 G  
移动到开始 g

# 直播或监控解决方案
1. [nginx-http-flv-module](https://github.com/winshining/nginx-http-flv-module)+[flv.js](https://github.com/bilibili/flv.js)+ffmpeg或obs推流
2. [rtsp直接转webrtc播放,以组件形式](https://github.com/mpromonet/webrtc-streamer)

# 服务器迁移
1. 原服务器关机
2. 创建原服务器镜像
3. 跨区域复制镜像
4. 新服务器选择重装系统，选择自定义镜像
5. 安装完毕后 启动相应Web服务
6. 域名解析全部替换为新服务器的ip地址