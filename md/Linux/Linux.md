# Linux

# 工具

## putty

## FinalShell

推荐

# 常用命令
## 切换超级管理员
su

## 修改密码
passwd root 输入两次密码

## 查看操作系统版本

cat /proc/version 查看内核

lsb_release -a 查看内核版本

## 查看服务器访问量

netstat -pnt | grep :80 | wc -l 查看80端口访问量

## 安装命令

### Centos

1. yum install xxx
2. yum remove xxx
3. yum update xxx
4. yum list installed

### Ubuntu

1. apt-get install xxx
2. apt-get remove xxx
3. apt-get update xxx

### Alpine

1. apk search xxx #查询xxx相关的软件包
2. apk add xxx #安装一个软件包
3. apk del xxx #删除已安装的xxx软件包
4. apk update #更新软件包索引文件
5. apk info #查看已安装软件

    例如安装ssh([参考](https://www.cnblogs.com/zpcdbky/p/15568702.html#ref03))：
    ```
    apk add --no-cache openssh-server
    apk add vim
    vim /etc/ssh/sshd_config
    apk add --no-cache openrc
    rc-update add sshd
    ssh-keygen -A
    rc-status
    touch /run/openrc/softlevel
    #需要先停止
    /etc/init.d/sshd stop
    /etc/init.d/sshd status
    /etc/init.d/sshd restart
    /etc/init.d/sshd status

    具体的sshd_config 配置
    Port 22
    ListenAddress 0.0.0.0
    ListenAddress ::
    PermitRootLogin yes
    PasswordAuthentication
    # 不设置下面这个，无法查看目录
    Subsystem sftp internal-sftp
    ```

镜像 vi /etc/apk/repositories
https://mirrors.aliyun.com/alpine/v3.15/main
https://mirrors.aliyun.com/alpine/v3.15/community


## 下载工具

wget 下载地址

## 创建多级目录
mkdir -p

## 清除
clear

## 上传文件到当前目录

没有命令则先安装工具 yum install lrzsz -y

rz -be

-b：binary 用binary的方式上传下载，不解释字符为ASCII

-e：强制escape 所有控制字符，比如Ctrl+x，DEL等

## 删除文件

rm -rf xxxx

## 修改文件权限

1. chown

-R 表示递归

chown -R 777/755 /xxx/xxx

chown -R git:1004 /xxx/xxx 递归修改文件用户和用户组(git/1004)

2. chmod

-r 可读

-w 可写

-x 可执行

chmod 777 所有权限
chmod 644 当前用户可写可读 用户组可读 其它用户可读

## 列出当前目录

pwd

## 下载命令
wget `http://download.com` 
一般下载路径放在`/usr/local/src/`
应用程序放在`/usr/local/`  

## 移动目录或文件
mv xxx /usr/local/

## 拷贝文件

cp xxx /usr/local

cp -r 递归复制目录下的所有子目录和文件到指定目录

cp -r /home/packageA/* /home/cp/packageB/

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

lsof -i:8080 lsof(list open files)是一个列出当前系统打开文件的工具

## 查看进程端口号

ps -ef|grep redis

ps -e|grep php

grep就是过滤名称 -e : 显示所有进程 -f : 全格式 

## 关闭FIN_WAIT1

sysctl -a |grep tcp_max_orph   记下net.ipv4.tcp_max_orphans的值赋给orig_orphans

sysctl -w net.ipv4.tcp_max_orphans=0

sysctl -w net.ipv4.tcp_max_orphans=$orig_orphans

FIN_WAIT1状态就是强制关闭端口号出现的 tcp_max_orph应该就是tcp连接的回收时间

## 查找文件

whereis php whereis命令用于查找文件。该指令会在特定目录中查找符合条件的文件

which php which指令会在环境变量$PATH设置的目录里查找符合条件的文件

find / -name php*  模糊查找

## 杀死进程

kill PID

kill -9 PID 强制杀死

## 开启防火墙

centos7使用firewalld 旧版本使用iptables

- 永久开放443端口  
    firewall-cmd --zone=public --add-port=443/tcp --permanent
- 重启防火墙  
    firewall-cmd --reload
- 查看
    firewall-cmd --state 
    firewall-cmd --list-all
    firewall-cmd --list-services
- 启动
    systemctl start firewalld
- 停止
    systemctl stop firewalld
- 查看状态
    systemctl status firewalld

## 环境变量

```
vim /etc/profile
export PATH=$PATH:/usr/local/php8/bin
source /etc/profile
echo $PATH
```

## 查看文件内容

cat xxx
tail -f xxx 实时监控

## 查看系统信息

df -h 查看分区容量

## 个性化命令配置

vim ~/.bashrc

例如加上ll命令

```shell
alias ll='ls -l'
```

source ~/.bashrc

# vim命令
创建文件 vim 新建文件名  
回到命令行状态 esc  
保存 wq  
不保存 q!  
插入 i  
搜索 esc + / + search_string  
移动到末尾 G  
移动到开始 g
删除一行 dd
gdG删除光标以下所有内容

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


# 宝塔

## 一键部署

```
docker run -d --name centos8-4-1 -v E:/sxc/code/docker:/www/wwwroot -p 80:80 -p 22:22 -it --privileged -u root --entrypoint /usr/sbin/init centos:8.4.2105

docker exec -it --user root 容器ID /bin/bash
#yum解决下载问题 修改镜像源
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
#建立搜索缓存
yum makecache
#-y跳过询问
yum update -y
#安装宝塔
yum install -y wget && wget -O install.sh https://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
#提交镜像删除旧容器
docker commit centos8-4-1 centos8-4-1:v1
#重新创建新容器80 http 443 https 22 ssh 6379 redis 3306数据库 22071 宝塔
docker run -d --name centos8-4-1 -v E:/sxc/code/docker:/www/wwwroot -p 80:80 -p 22:22 -p 443:443 -p 6379:6379 -p 3306:3306 -p 22071:22071 -it --privileged -u root --entrypoint /usr/sbin/init centos8-4-1:v1
#进入容器
docker exec -it --user root 容器ID /bin/bash
#修改宝塔配置
bt
13
bt
5 123456
bt
6 btadmin

外网面板地址: http://10.10.11.43:22071/2dacf2ec
内网面板地址: http://127.0.0.1:22071/2dacf2ec
username: btadmin
password: 123456


#安装ssh
yum install -y openssh* 失败的话看提示信息 加上跳过参数--skip-broken
vim /etc/ssh/sshd_config
Port 22
ListenAddress 0.0.0.0
ListenAddress ::
PermitRootLogin yes
PasswordAuthentication yes

#然后执行
mkdir -p /var/run/sshd
ssh-keygen -q -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key -N '' 
ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''
ssh-keygen -t dsa -f /etc/ssh/ssh_host_ed25519_key -N ''
#启动
/usr/sbin/sshd -D & 
#或者使用systemctl
systemctl start sshd.service
#设置为开机启动
systemctl enable sshd.service

#修改root密码
yum install -y passwd
passwd root

#设置时区
timedatectl set-timezone Asia/Shanghai
```