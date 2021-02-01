# Php
## 环境搭建
### wnmp搭建
<p align="left" style="color:#777777;">发布日期：2020-05-08</p>

!> 安装根目录这里都设为F:\Ul\  替换为自己的安装目录即可

1. 分别下载nginx,mysql,php
    - [nginx 1.12.2](http://nginx.org/en/download.html)
    - [mysql 5.6.43](https://downloads.mysql.com/archives/community/)
        > mysql可以安装更高版本，性能更好，但取决于你服务器的内存
    - [php 5.6.0](https://windows.php.net/downloads/releases/archives/)
2. 配置php
    - 复制php.ini-development 改名为php.ini
    - 找到extension_dir 随便一个改为 extension_dir = './ext (设置php扩展目录)
    - 设置时区date.timezone = Asia/Shanghai
    - 开启以下扩展或配置
        ```ini
        ;always_populate_raw_post_data = -1 //开启post_data
        ;cgi.fix_pathinfo=1 //开启phpcgi 这个又漏洞
        ;extension=php_curl.dll //curl远程抓取
        ;extension=php_fileinfo.dll//文件类处理函数
        ;extension=php_gd2.dll//gd库
        ;extension=php_mbstring.dll//处理多字节字符串
        ;extension=php_mysql.dll   //php支持mysql
        ;extension=php_mysqli.dll //php支持mysqli（i代表优化过的）
        ;extension=php_openssl.dll//openssl 加密解密用的
        ;extension=php_pdo_mysql.dll //mysql pdo 连接
        ;extension=php_sockets.dll//Socket 函数库
        #差一个redis
        ```
3. 配置nginx.conf
    找到conf
    配置localhost如下
    > 以下F:/Ul/html 可以切换成你自己的服务器代码根目录
    ```nginx
    #user  nobody;
    worker_processes  1;

    events {
        worker_connections  1024;
    }

    http {
        #这里要注意域名的长度过长会报错，所以要配置这个
        server_names_hash_bucket_size 64;
        include       mime.types;
        default_type  application/octet-stream;
        sendfile        on;
        keepalive_timeout  65;

        server {
            listen       80;
            server_name  localhost;
            root   F:\UI\html;
            #以下是上传文件
            client_max_body_size 2m;

            location / {
                index  index.php index.html index.htm;
                if (!-e $request_filename) {
                    rewrite  ^(.*)$  /index.php?s=/$1  last;
                    break;
                }
            }

            error_page   500 502 503 504  /50x.html;
            location = /50x.html {
                root   html;
            }

            location ~ \.php {
                fastcgi_index   index.php;
                fastcgi_pass    127.0.0.1:9000;

                fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
                fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;

                #以下是Pathinfo
                fastcgi_split_path_info ^(.+\.php)(.*)$;
                fastcgi_param   PATH_INFO $fastcgi_path_info;
                fastcgi_param   PATH_TRANSLATED $document_root$fastcgi_path_info;
                include         fastcgi_params;

                #以下是上传文件
                client_max_body_size 2m;
            }

            #配置图片跨域访问
            location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
                #允许跨域请求
                add_header Access-Control-Allow-Origin '*';
                add_header Access-Control-Allow-Headers X-Requested-With;
                add_header Access-Control-Allow-Methods GET,POST,OPTIONS;

                expires 30d;
                access_log off;
            }
        }
    }
    ```
    > 检查配置是否正确nginx.exe -t -c ./conf/nginx.conf
4. 安装mysql为系统服务
    找到mysqld
    执行mysqld --install（需管理员权限的cmd）
5. 编写wnmp启动脚本bat
    !> 以下脚本需管理员权限启动
    - 编写server_start.bat
        ```bash
        @echo off

        F:
        cd F:\UI\wnmp\nginx-1.12.2
        start nginx
        echo Start nginx success

        net start mysql
        echo Start mysql success

        cd F:\UI\wnmp\php-5.6.0
        php-cgi.exe -b 127.0.0.1:9000 -c php.ini
        echo Start php-cgi success
        ```
    - 编写server_stop.bat
        ```bash
        @echo off
        taskkill /F /IM nginx.exe > nul
        echo Stop nginx success

        taskkill /F /IM php-cgi.exe > nul
        echo Stop PHP FastCGI success

        net stop mysql
        echo Stop mysql success
        pause
        ```
6. 查看进程
    ```bash
    tasklist /fi "imagename eq nginx.exe"
    tasklist /fi "imagename eq php-cgi.exe"
    netstat -ano | findstr "3306"
    ```

7. 为php安装redis扩展
    - 用phpinfo(） 查看php extension build 是ts版还是nts版 （线程安全不安全版）
    - 下载redis [下载地址](http://pecl.php.net/package/redis/2.2.7/windows)
        - 选择对应的tsORnts
        - 解压后将php_redis.dll放入php\ext文件夹中
        - 为php.ini 加上扩展extension=php_redis.dll
    - 下载redis客户端[下载地址](https://github.com/MicrosoftArchive/redis/releases)
8. PHP7\PHP8的搭建方式一样
    > 有一些配置不同了而已
    - php7.4.13 [下载地址](https://windows.php.net/downloads/releases/php-7.4.13-nts-Win32-vc15-x64.zip)
    - php8.0.1 [下载地址](https://windows.php.net/downloads/releases/php-8.0.1-nts-Win32-vs16-x64.zip)

!>windows下项目目录命名为tp前缀会打不开

