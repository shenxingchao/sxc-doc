
# Linux
## 常用命令
### 切换超级管理员
su
### 创建多级目录
mkdir -p

### 解压 文件
- *.tar 用 tar –xvf 解压
- *.gz 用 gzip -d或者gunzip 解压
- *.tar.gz和*.tgz 用 tar –xzf 解压
- *.bz2 用 bzip2 -d或者用bunzip2 解压
- *.tar.bz2用tar –xjf 解压
- *.Z 用 uncompress 解压
- *.tar.Z 用tar –xZf 解压
- *.rar 用 unrar e解压
- *.zip 用 unzip 解压

## vim命令
创建文件 vim 新建文件名  
回到命令行状态 esc  
保存 wq  
不保存 q!  
插入 i  
搜索 esc + / + search_string  
移动到末尾 G  
移动到开始 g


### 直播或监控解决方案
1. [nginx-http-flv-module](https://github.com/winshining/nginx-http-flv-module)+[flv.js](https://github.com/bilibili/flv.js)+ffmpeg或obs推流
2. [rtsp直接转webrtc播放,以组件形式](https://github.com/mpromonet/webrtc-streamer)