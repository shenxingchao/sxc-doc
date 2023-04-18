# Php

# 环境搭建

## wnmp 搭建

<p align="left" style="color:#777777;">发布日期：2020-05-08</p>

!>安装根目录这里都设为 F:\Ul\ 替换为自己的安装目录即可

1. 分别下载 nginx,mysql,php
   - [nginx 1.12.2](http://nginx.org/en/download.html)
   - [mysql 5.6.43](https://downloads.mysql.com/archives/community/)
     > mysql 可以安装更高版本，性能更好，但取决于你服务器的内存
   - [php 5.6.0](https://windows.php.net/downloads/releases/archives/)
2. 配置 php
   - 复制 php.ini-development 改名为 php.ini
   - 找到 extension_dir 随便一个改为 extension_dir = './ext (设置 php 扩展目录)
   - 设置时区 date.timezone = Asia/Shanghai
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
3. 配置 nginx.conf
   找到 conf
   配置 localhost 如下

   > 以下 F:/Ul/html 可以切换成你自己的服务器代码根目录

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

   > 检查配置是否正确 nginx.exe -t -c ./conf/nginx.conf

4. 安装 mysql 为系统服务
   找到 mysqld
   执行 mysqld --install（需管理员权限的 cmd）
5. 编写 wnmp 启动脚本 bat

   - 编写 server_start.bat

     ```shell
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

   - 编写 server_stop.bat

     ```shell
     @echo off
     taskkill /F /IM nginx.exe > nul
     echo Stop nginx success

     taskkill /F /IM php-cgi.exe > nul
     echo Stop PHP FastCGI success

     net stop mysql
     echo Stop mysql success
     pause
     ```

!>以上脚本需管理员权限启动

6. 查看进程

   ```shell
   tasklist /fi "imagename eq nginx.exe"
   tasklist /fi "imagename eq php-cgi.exe"
   netstat -ano | findstr "3306"
   ```

7. 为 php 安装 redis 扩展
   - 用 phpinfo(） 查看 php extension build 是 ts 版还是 nts 版 （线程安全不安全版）
   - 下载 redis [下载地址](http://pecl.php.net/package/redis/2.2.7/windows)
     - 选择对应的 tsORnts
     - 解压后将 php_redis.dll 放入 php\ext 文件夹中
     - 为 php.ini 加上扩展 extension=php_redis.dll
   - 下载 redis 客户端[下载地址](https://github.com/MicrosoftArchive/redis/releases)
8. PHP7\PHP8 的搭建方式一样
   > 有一些配置不同了而已
   - php7.4.13 [下载地址](https://windows.php.net/downloads/releases/php-7.4.13-nts-Win32-vc15-x64.zip)
   - php8.0.1 [下载地址](https://windows.php.net/downloads/releases/php-8.0.1-nts-Win32-vs16-x64.zip)

!>windows 下项目目录命名为 tp 前缀会打不开

## lnmp 搭建

<p align="left" style="color:#777777;">发布日期：2019-04-01 更新日期：2021-02-06</p>

### 准备

1. 下载 putty 工具
2. yum -y update（升级所有软件包）
3. df –lh（查看磁盘空间）
4. 查看是否已安装 wget  
   rpm -qa wget
   - 否则安装  
      yum install wget
5. 查看是否已安装编译器  
   rpm -qa gcc
   - 否则安装  
      yum install gcc gcc-c++

### 安装 nginx

1. 安装 nginx 依赖包
   - pcre 正则表达式语法
     yum -y install pcre pcre-devel
   - zip 压缩
     yum -y install zlib zlib-devel
   - ssl
     yum -y install openssl openssl-devel
2. 下载 nginx 安装包 并解压
   - cd /usr/local/src
   - wget http://nginx.org/download/nginx-1.12.2.tar.gz
   - tar –zxvf nginx-1.12.2.tar.gz
3. 编译安装
   - cd nginx-1.12.2
   - ./configure --prefix=/usr/local/nginx
   - ./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-http_gzip_static_module (要 openssl 模块的话)
   - make （重新编译只要执行这一个就可以了，不然会覆盖安装 cp ./objs/nginx /usr/local/nginx/sbin/nginx）
   - make install
4. 创建并设置 nginx 运行账号
   - groupadd nginx
   - useradd -M -g nginx -s /sbin/nologin nginx
   - cd /usr/local/nginx/conf
   - vim nginx.conf，设置 user 参数如下  
      user nginx nginx
5. 设置 nginx 为系统服务
   - vim /lib/systemd/system/nginx.service
   - 文件内容
     ```
     [Unit]
     Description=nginx
     After=network.target
     [Service]
     Type=forking
     ExecStart=/usr/local/nginx/sbin/nginx
     ExecReload=/usr/local/nginx/sbin/nginx -s reload
     ExecStop=/usr/local/nginx/sbin/nginx -s stop
     PrivateTmp=true
     [Install]
     WantedBy=multi-user.target
     ```
   - 解释
     - [unit]: 服务的说明
     - Description:描述服务
     - After:描述服务类别
     - [Service]服务运行参数的设置
     - Type=forking 是后台运行的形式
     - ExecStart 为服务的具体运行命令
     - ExecReload 为重启命令
     - ExecStop 为停止命令
     - PrivateTmp=True 表示给服务分配独立的临时空间
     - 注意：[Service]的启动、重启、停止命令全部要求使用绝对路径
     - [Install]运行级别下服务安装的相关设置，可设置为多用户，即系统运行级别为 3
   - 保存退出。
6. 设置 nginx 开机自启动  
   systemctl enable nginx.service
7. 开启 nginx 服务  
   systemctl start nginx.service
   - 查看 nginx 是否启动成功  
      ps aux | grep nginx
   - 访问服务器地址 welcome to nginx 至此安装完成
8. 开启防火墙  
   systemctl start firewalld
   防火墙开放 80 端口（nginx 默认使用 80 端口，可在 nginx.conf 中配置，若无需进行远程访问则不需要开放端口）
   - 永久开放 80 端口  
      firewall-cmd --zone=public --add-port=80/tcp --permanent
   - 重启防火墙  
      firewall-cmd --reload
   - 查看防火墙开启状态  
      systemctl status firewalld
   - 查看 80 端口是否开放成功  
      firewall-cmd --zone=public --query-port=80/tcp

### 安装 Mysql

1. 卸载已有 mysql
   - 查看是否已安装 mysql  
      rpm -qa mysql
   - 有则卸载  
      rpm -e --nodeps 文件名称
   - 是否存在与 mysql 相关的文件或目录  
      whereis mysql 或 find / -name mysql
     - 是则删除  
        rm -rf /usr/lib/mysql rm -rf /usr/share/mysql
   - 查看是否存在 mariadb  
      rpm -qa | grep mariadb
     - 存在则卸载  
        rpm -e --nodeps 文件名 //文件名是上一个命令查询结果
   - 存在/etc/my.cnf，则需要先删除  
      rm /etc/my.cnf
2. 安装编译 mysql 需要的依赖包  
   yum install libevent* libtool* autoconf* libstd* ncurse* bison* openssl\*
3. 安装 cmake（mysql5.5 之后需要用 cmake 支持编译安装）
   - 查看是否已安装 cmake
     rpm -qa cmake
     - 没有则下载编译安装  
        cd /usr/local/src  
        wget http://www.cmake.org/files/v2.8/cmake-2.8.12.1.tar.gz  
        tar -xf cmake-2.8.12.1.tar.gz  
        cd cmake-2.8.12.1  
        ./configure  
        make&&make install
   - 检查 cmake 是否安装成功  
      cmake --version
4. 下载 mysql 包并解压（到/usr/local/src 目录）  
   cd /usr/local/src  
   wget https://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.43.tar.gz
   tar -zxvf mysql-5.6.43.tar.gz
5. 编译安装（到/usr/local/mysql 目录）  
   cd mysql-5.6.43  
   cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci  
   make（此过程需花费大概 20-30 分钟）  
   make install
6. 配置 mysql  
   groupadd mysql  
   useradd -M -g mysql -s /sbin/nologin mysql  
   chown -R mysql:mysql /usr/local/mysql
7. 初始化配置  
   cd /usr/local/mysql/scripts  
   ./mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql
8. 设置 mysql 为系统服务  
   vim /lib/systemd/system/mysql.service
   - 文件内容
     ```
     [Unit]
     Description=mysql
     After=network.target
     [Service]
     Type=forking
     ExecStart=/usr/local/mysql/support-files/mysql.server start
     ExecStop=/usr/local/mysql/support-files/mysql.server stop
     ExecRestart=/usr/local/mysql/support-files/mysql.server restart
     ExecReload=/usr/local/mysql/support-files/mysql.server reload
     PrivateTmp=true
     [Install]
     WantedBy=multi-user.target
     ```
9. 设置 mysql 服务开机自启动  
   systemctl enable mysql.service
10. 启动 mysql  
    systemctl start mysql.service
    - 创建不存在的目录即可  
       mkdir /var/lib/mysql  
       chown -R mysql:mysql /var/lib/mysql
    - 再次启动  
       systemctl start mysql.service
    - 查看  
       ps aux | grep mysql

!>(启动失败使用/usr/local/mysql/support-files/mysql.server restart 启动可以看到详细错误原因)

11. 登录 mysql 并设置 root 密码
    /usr/local/mysql/bin/mysql -u root set password=password('123456');
12. 创建远程连接用户  
    GRANT ALL PRIVILEGES ON _._ TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;  
    flush privileges;

!>(%也可换成 ip)

13. 重启 mysql  
    service mysql restart
14. 查看防火墙对外开放了哪些端口（看 3306 有没有被开启）这里是 centos7 的命令
    firewall-cmd --zone=public --list-ports
15. 打开端口  
    firewall-cmd --zone=public --add-port=3306/tcp --permanent （--permanent 永久生效，没有此参数重启后失效）  
    firewall-cmd --reload  
    在查看一次端口即可  
    yum update 导致 mysql 服务不能启动，解决办法 mv /etc/my.cnf /etc/my.cnf.bak

### 安装 PHP

1. 安装 php 依赖包  
   yum install libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel gmp gmp-devel libmcrypt libmcrypt-devel readline readline-devel libxslt libxslt-devel
2. 下载 php 包并解压  
   cd /usr/local/src
   - 下载  
      wget http://cn2.php.net/distributions/php-5.6.0.tar.xz
   - 解压  
      tar -zxvf php-5.6.0.tar.gz
   - 进入  
      cd php-5.6.0
   - 配置  
      ./configure --prefix=/usr/local/php --disable-fileinfo --enable-fpm --with-config-file-path=/etc --with-config-file-scan-dir=/etc/php.d --with-openssl --with-zlib --with-curl --enable-ftp --with-gd --with-xmlrpc --with-jpeg-dir --with-png-dir --with-freetype-dir --enable-gd-native-ttf --enable-mbstring --with-mcrypt=/usr/local/libmcrypt --enable-zip --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-mysql-sock=/var/lib/mysql/mysql.sock --without-pear --enable-bcmath
   - 编译  
      make（此过程需花费大概 20 分钟）  
      make install

!>（注意：--with-mcrypt 参数指定的是 libmcrypt 的安装目录。Php7 不再使用 mysql 的库来支持 mysql 的连接，而是启用了 mysqlnd 来支持，所以 php7 的编译已经不再使用--with-mysql 参数指定 mysql 的安装位置了，若想支持 mysql，需要设置--enable-mysqlnd、--with-mysqli 和--with-pdo-mysql=mysqlnd 参数，--with-mysql-sock 指定的是编译 mysql 时-DMYSQL_UNIX_ADDR 参数指定的文件）

3. 将 php 包解压目录中的配置文件放置到正确位置  
   cp php.ini-development /etc/php.ini

!>configure 命令中的--with-config-file-path 设置的位置 这里的目录可以直接设为/usr/local/php/etc 没必要放外面的

4. 创建并设置 php-fpm 运行账号  
   groupadd www-data  
   useradd -M -g www-data -s /sbin/nologin www-data  
   cd /usr/local/php/etc  
   cp php-fpm.conf.default php-fpm.conf  
   vim php-fpm.conf  
   搜索 user/group (esc 输入/string 搜索)  
   设置  
   user=www-data  
   group=www-data
5. 配置 nginx 支持 php  
   vim /usr/local/nginx/conf/nginx.conf  
   ![calc](../../images/nginx_php.png)  
   重启 nigix  
   systemctl start nginx.service
6. 设置 php-fpm 为系统服务  
   vim /etc/systemd/system/php-fpm.service
7. 设置 php-fpm 服务开机自启动  
   systemctl enable php-fpm.service
8. 启动 php-fpm  
   systemctl start php-fpm.service  
   查看是否启动成功：  
   ps aux | grep php-fpm

### 安装 PHP7.4.13

**统一下载路径 cd /usr/local/src**

1. 前置 1 安装 re2c 不然编译会报错 PHP （语法分析器 re2c）  
   wget https://github.com/skvadrik/re2c/releases/download/1.0.2/re2c-1.0.2.tar.gz  
   解压：tar -zxvf re2c-1.0.2.tar.gz  
   进入 cd ./re2c-1.0.2  
   编译安装./configure && make && make install
2. 前置 2No package 'sqlite3' found  
   sudo yum install -y sqlite-devel.x86_64
3. 前置 3No package 'oniguruma' found  
   wget https://github.com/kkos/oniguruma/archive/v6.9.4.tar.gz -O oniguruma-6.9.4.tar.gz  
   tar -xvf oniguruma-6.9.4.tar.gz  
   cd oniguruma-6.9.4/  
   ./autogen.sh  
   ./configure --prefix=/usr --libdir=/lib64&&make && make install //64 位的系统一定要标识 --libdir=/lib64 否则还是不行
4. 前置 4 No package 'libzip' found  
   #卸载老版本的 libzip  
   yum remove libzip  
   #下载安装 libzip-1.2.0  
   wget https://libzip.org/download/libzip-1.2.0.tar.gz  
   tar -zxvf libzip-1.2.0.tar.gz  
   cd libzip-1.2.0  
   ./configure  
   make && make install  
   安装完成后，查看是否存在/usr/local/lib/pkgconfig 目录,如果存在，执行如下命令来设置 PKG_CONFIG_PATH：  
   Php 编译配置前执行  
   export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig/"
5. 安装 php 依赖包(此处 5.6 上面已经安装了可以跳过)  
   yum install libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel libjpeg libjpeg-devel libpng  
   libpng-devel freetype freetype-devel gmp gmp-devel libmcrypt libmcrypt-devel readline readline-devel libxslt libxslt-devel
6. 下载 php 包并解压  
   cd /usr/local/src
   - 下载  
      wget https://www.php.net/distributions/php-7.4.13.tar.gz  
     建议直接下载，然后再通过 ftp 扔到 src 目录，有时候网速不好,会缺少文件
   - 解压  
      tar -zxvf php-7.4.13.tar.gz
   - 进入  
      cd php-7.4.13
   - 复制配置文件  
      cp php.ini-production /usr/local/php7/etc/php.ini
   - 配置  
      先执行  
      export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig/"  
      再执行  
      ./configure --prefix=/usr/local/php7 --disable-fileinfo --enable-fpm --with-config-file-path=/usr/local/php7/etc  
      --with-config-file-scan-dir=/usr/local/php7/etc/php.d --with-openssl --with-zlib --with-curl --enable-ftp --enable-gd  
      --with-xmlrpc --with-jpeg --with-freetype --enable-mbstring --with-zip --enable-mysqlnd --with-mysqli=mysqlnd  
      --with-pdo-mysql=mysqlnd --with-mysql-sock=/var/lib/mysql/mysql.sock --enable-bcmath --without-pear --enable-opcache  
      (删除一些没用的，启用一些必要的)
7. 编译安装  
   make && make install  
   等待 20~30 分钟
8. 配置 fpm  
   cd /usr/local/php7/etc
   - 首先复制出一份 php-fpm.conf  
      #cp php-fpm.conf.default php-fpm.conf
   - 切换到/usr/local/php7/etc/php-fpm.d 复制配置文件  
      #cp www.conf.default www.conf
   - 修改 fpm 监听端口  
      vim /usr/local/php7/etc/php-fpm.d/www.conf  
      listen = 127.0.0.1:9000
   - 将其端口修改为  
      listen = 127.0.0.1:9001
   - :wq 保存编辑退出.  
     然后配置 nginx 或 apache 支持 php7 即可.
9. 配置 php7 的 fpm 自动启动
   - 复制 5.6 的自启文件  
      cp /etc/systemd/system/php-fpm.service /etc/systemd/system/php7-fpm.service
   - 编辑改为 7 的目录  
      vim /etc/systemd/system/php7-fpm.service
10. 设置 php7-fpm 服务开机自启动：  
    systemctl enable php7-fpm.service
11. 启动 php-fpm：  
     systemctl start php7-fpm.service
    - 查看是否启动成功  
       ps aux | grep php-fpm  
       就能看到 5.6 的和 7 的同时启动成功了  
      12.nginx 支持 php7  
       第四大点第 5 小点 nginx 配置 127.0.0.1:9000 改为 127.0.0.1:9001 即可  
      13.php7 开启 opcache  
       vim /usr/local/php7/etc/php.ini
    - 末尾加上下面配置  
      zend_extension=opcache.so  
      [opcache]  
      ;开启 opcache  
      opcache.enable=1  
      ;CLI 环境下，PHP 启用 OPcache  
      opcache.enable_cli=1  
      ;OPcache 共享内存存储大小,单位 MB  
      opcache.memory_consumption=128  
      ;PHP 使用了一种叫做字符串驻留（string interning）的技术来改善性能。例如，如果你在代码中使用了 1000 次字符串“foobar”，在 PHP 内部只会在第一  
      用这个字符串的时候分配一个不可变的内存区域来存储这个字符串，其他的 999 次使用都会直接指向这个内存区域。>这个选项则会把这个特性提升一个层次—  
      默认情况下这个不可变的内存区域只会存在于单个 php-fpm 的进程中，如果设置了这个选项，那么它将会在所有的 php-fpm 进程中共享。在比较大的应用中，  
      可以非常有效地节约内存，提高应用的性能。  
      ;这个选项的值是以兆字节（megabytes）作为单位，如果把它设置为 16，则表示 16MB，默认是 4MB  
      opcache.interned_strings_buffer=8  
      ;这个选项用于控制内存中最多可以缓存多少个 PHP 文件。这个选项必须得设置得足够大，大于你的项目中的所有 PHP 文件的总和。  
      ;设置值取值范围最小值是 200，最大值在 PHP 5.5.6 之前是 100000，PHP 5.5.6 及之后是 1000000。也就是说在 200 到 1000000 之间。  
      ;opcache.max_accelerated_files=4000  
      ;设置缓存的过期时间（单位是秒）,为 0 的话每次都要检查  
      opcache.revalidate_freq=60  
      ;从字面上理解就是“允许更快速关闭”。它的作用是在单个请求结束时提供一种更快速的机制来调用代码中的析构器，从而加快 PHP 的响应速度和 PHP 进程资  
      的回收速度，这样应用程序可以更快速地响应下一个请求。把它设置为 1 就可以使用这个机制了。  
      opcache.fast_shutdown=1  
      ;如果启用（设置为 1），OPcache 会在 opcache.revalidate_freq 设置的秒数去检测文件的时间戳（timestamp）检查脚本是否更新。  
      ;如果这个选项被禁用（设置为 0），opcache.revalidate_freq 会被忽略，PHP 文件永远不会被检查。这意味着如果你修改了你的代码，然后你把它更新到  
      务器上，再在浏览器上请求更新的代码对应的功能，你会看不到更新的效果  
      ;强烈建议你在生产环境中设置为 0，更新代码后，再平滑重启 PHP 和 web 服务器。  
      opcache.validate_timestamps=0  
      ;开启 Opcache File Cache(实验性), 通过开启这个, 我们可以让 Opcache 把 opcode 缓存缓存到外部文件中, 对于一些脚本, 会有很明显的性能提升.  
      ;这样 PHP 就会在/tmp 目录下 Cache 一些 Opcode 的二进制导出文件, 可以跨 PHP 生命周期存在.  
      opcache.file_cache=/tmp
    - 重启  
      systemctl restart php7-fpm

### 安装 PHP8.0.1

**统一下载路径 cd /usr/local/src**

1. 和 php7 一样的前置条件安装
2. 下载 php 包并解压  
   https://www.php.net/distributions/php-8.0.1.tar.gz  
   建议直接下载，然后扔到/usr/local/src 目录
   - 解压  
      tar -zxvf php-8.0.1.tar.gz
   - 进入  
      cd php-8.0.1
   - 先执行  
      export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig/"
   - 再执行（编译错误每个编译项之间只能有一个空格，编译项和 7 略微不同，如 Xml）  
      ./configure --prefix=/usr/local/php8 --disable-fileinfo --enable-fpm --with-config-file-path=/usr/local/php8/etc  
      --with-config-file-scan-dir=/usr/local/php8/etc/php.d --with-openssl --with-zlib --with-curl --enable-ftp --enable-gd  
      --enable-xml --with-jpeg --with-freetype --enable-mbstring --with-zip --enable-mysqlnd --with-mysqli=mysqlnd  
      --with-pdo-mysql=mysqlnd --with-mysql-sock=/var/lib/mysql/mysql.sock --enable-bcmath --without-pear --enable-opcache
   - 编译安装，这里会缺很多扩展，[参考](https://www.jianshu.com/p/3c721f4e9075)
     make && make install  
      等待 20~30 分钟
3. 复制配置文件  
   cp php.ini-production /usr/local/php8/etc/php.ini
4. 配置 fpm  
   cd /usr/local/php8/etc
   - 首先复制出一份 php-fpm.conf  
      #cp php-fpm.conf.default php-fpm.conf
   - 切换到  
      /usr/local/php8/etc/php-fpm.d
   - 复制配置文件  
      #cp www.conf.default www.conf
   - 修改 fpm 监听端口  
      vim /usr/local/php8/etc/php-fpm.d/www.conf  
      listen = 127.0.0.1:9000  
      将其端口修改为  
      listen = 127.0.0.1:9002  
      :wq 保存编辑退出.
   - 然后配置 nginx 或 apache 支持 php8 即可.
5. 配置 php8 的 fpm 自动启动
   - 复制 5.6 的自启文件  
      cp /etc/systemd/system/php-fpm.service /etc/systemd/system/php8-fpm.service
   - 编辑改为 8 的目录  
      vim /etc/systemd/system/php8-fpm.service
6. 设置 php8-fpm 服务开机自启动：  
   systemctl enable php7-fpm.service
7. 启动 php-fpm  
   systemctl start php8-fpm.service

   - 查看是否启动成功  
      ps aux | grep php-fpm  
      就能看到 5.6 的和 7 的同时启动成功了

8. nginx 支持 php8  
   第四大点第 5 小点 nginx 配置 127.0.0.1:9000 改为 127.0.0.1:9002 即可

9. 设置环境变量

   ```
   vim /etc/profile
   export PATH=$PATH:/usr/local/php8/bin
   source /etc/profile
   重启
   查看
   php -v
   ```

10. php8 开启 opcache 和 jit  
    vim /usr/local/php8/etc/php.ini
    - 末尾加上下面配置
      ```
      zend_extension=opcache
      [opcache]
      ;开启opcache
      opcache.enable=1
      opcache.enable_cli=1
      opcache.memory_consumption=128
      opcache.interned_strings_buffer=8
      ;opcache.max_accelerated_files=4000
      opcache.revalidate_freq=60
      opcache.fast_shutdown=1
      opcache.validate_timestamps=0
      opcache.file_cache=/tmp
      ;支持jit
      opcache.jit=1255
      opcache.jopcache.jit_buffer_size=32M
      ```
    - 重启  
       systemctl restart php8-fpm

!>jit_buffer_size 设置过大会报错

### 常用

- nginx  
   启动 systemctl start nginx.service  
   停止 systemctl stop nginx.service  
   重启 systemctl restart nginx.service  
   查看是否启动 ps aux | grep nginx  
   配置目录/usr/local/nginx/conf/nginx.conf
- Php  
   启动 systemctl start php-fpm.service  
   停止 systemctl stop php-fpm.service  
   重启 systemctl restart php-fpm.service  
   查看是否启动 ps aux | grep php-fpm  
   Php 配置目录 /etc/php.ini
- Mysql  
   启动 systemctl start mysql.service  
   停止 systemctl stop mysql.service  
   重启 systemctl start mysql.service  
   查看是否启动 ps aux | grep mysql  
   配置目录/usr/local/mysql/my.cnf
- php7  
   启动 systemctl start php7-fpm.service  
   停止 systemctl stop php7-fpm.service  
   重启 systemctl restart php7-fpm.service  
   查看是否启动 ps aux | grep php-fpm  
   Php 配置目录 /usr/local/php7/etc/php.ini
- Php8  
   启动 systemctl start php8-fpm.service  
   停止 systemctl stop php8-fpm.service  
   重启 systemctl restart php8-fpm.service  
   查看是否启动 ps aux | grep php-fpm  
   Php 配置目录 /usr/local/php8/etc/php.ini

!> 注意开启 php opcache 后 如果设置了缓存，那么请求的 php 脚本会被缓存，缓存时间内 php 脚本不会更新，如果要立即生效，需要重启 fpm，这也是开启 opcache 后性能提升的原因，因为不需要重新编译 php 脚本了

# PHPSTROM

## 快捷键

键盘映射:下载 vscode 键盘方案

shift+shift 搜索

shift+alt+o 优化导入

alt+7 结构

按住 shift+alt 并拖动鼠标左键上下移动 多行编辑

## 代码格式化

CODESTYLE->PHP->set from ->Drupal 并设置缩进为 4 个 space

保存时格式化 编辑->录制宏 操作一下格式化和保存，然后去设置->格式化配置里选用宏

# 扩展

## linux 为 php 添加 redis 扩展

<p align="left" style="color:#777777;">发布日期：2020-07-23</p>

1. cd /usr/local/src
2. 下载 wget http://pecl.php.net/get/redis-2.2.7.tgz
3. 解压 tar -zxvf redis-2.2.7.tgz
4. cd redis-2.2.7
5. find / -name phpize
6. /usr/local/php/bin/phpize
7. ./configure --with-php-config=/usr/local/php/bin/php-config
8. make && make install
9. 编辑 php.ini 加入 redis.so  
   /usr/local/php/lib/php/extensions/no-debug-non-zts-20131226/redis.so
10. systemctl restart php-fpm

# 算法

## PHP 之抽奖概率算法

<p align="left" style="color:#777777;">发布日期：2019-03-27</p>

无论是任何抽奖的小游戏，例如转盘，九宫格，砸金蛋，刮刮卡，都是基于抽奖算法来抽奖，都可以基于同一种抽奖算法。无非就是前端的展现形式不一样。目前只看到过一种，附上。

```php
function getRand($proArr){
    $data = '';
    $proSum = array_sum($proArr); //概率数组的总概率精度
    foreach ($proArr as $k => $v) { //概率数组循环
        $randNum = mt_rand(1, $proSum); //重点
        if($randNum <= $v){ //重点
            $data = $k;
            break;
        } else {
            $proSum -= $v;
        }
    }
    unset($proArr);
    return $data;
}
```

这个被人称之为经典概率算法，$proArr 就是概率数组。例如('1','2','3','4')这个概率数组，那么他的总概率为 10  
那么抽中 1 对应的概率就为 1/10 。那怎么计算这个概率呢？  
就是用 1 到 10 的随机数，随机一个整数，那么随机到小于等于 2 的概率不就是 2/10。这里牵涉到 mt_rand 这个函数，有兴趣的研究下。  
九宫格抽奖：http://www.cnblogs.com/starof/p/4933907.html  
转盘抽奖：http://www.thinkphp.cn/code/1153.html

# thinkphp6 后端框架

此步骤所有文件参考原项目即可 http://demo.o8o8o8.com/vue-admin-thinkphp6/#/

## composer

[composer 安装](https://www.kancloud.cn/manual/thinkphp6_0/1037481) [windows 直接下载安装](https://getcomposer.org/download/)

## 创建项目

切换到网站根目录运行

```shell
composer create-project topthink/think demo
```

## 更新框架

切换到应用根目录

```shell
composer update topthink/framework
```

!> 运行出现 No input file specified 可能是 nginx root 目录配置的有问题 最好用\\双斜杠去表示\就不会出问题了

## 配置域名绑定模块

配置/config/app.php(上线需要更改的一块)

```php
<?
return [
 'domain_bind'      => [
        'www.xxx.com' => 'admin',
    ],
]
```

## 配置路由

/config/route.php

```php
<?php
return [
    // URL普通方式参数 用于自动生成
    'url_common_param'      => true,
    // 是否开启路由延迟解析
    'url_lazy_route'        => true,
    // 是否强制使用路由
    'url_route_must'        => true,
    // 合并路由规则
    'route_rule_merge'      => false,
    // 路由是否完全匹配
    'route_complete_match'  => true,
]
```

## 新建模块

/app/admin

## 配置数据库连接

/app/admin/config/database.php 和/.env 配置文件(上线需要更改的一块)
!> 上线还要重启 php8 不然数据库连不上出现 localhsot no Password 错误,很坑

## 配置路由中间件

/app/admin/config/route.php 配置路由中间件设置跨域规则。如果要鉴权的也可以在这里设置 priority 优先中间件，设置方法参考以前的项目

```php
<?php
//路由
return [
    //路由中间件
    'middleware' => [
        app\admin\middleware\Cors::class, //跨域中间件 这里开了整个模块都跨域
    ],
];
```

app\admin\middleware\Cors.php

```php
<?php
declare (strict_types = 1);
namespace app\admin\middleware;

//跨域中间件
class Cors {
    public function handle($request, \Closure$next) {
        //构造方法 设置允许跨域请求
        header('Access-Control-Allow-Origin:*');
        header("Access-Control-Allow-Methods: POST, GET, OPTIONS, PUT, DELETE");
        header("Access-Control-Allow-Credentials: true");
        header("Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept,  X-Token");
        header("Access-Control-Expose-Headers: Token,Code");
        header('Access-Control-Max-Age: 3600');
        if (strtoupper($_SERVER['REQUEST_METHOD']) == 'OPTIONS') {
            exit;
        }
        return $next($request);
    }
}
```

## 多应用模式

需要安装 think-multi-app 不然无法访问，默认是单应用模式

```shell
composer require topthink/think-multi-app
```

## 新建路由

新建 app\admin\route\route.php
路由文件随便起怎么起，只要在这下面都会加载

# swoole

## 安装扩展

[下载扩展源代码](https://github.com/swoole/swoole-src/releases)

建议直接下载，然后扔到/usr/local/src 目录

1. 解压
   tar -zxvf swoole-src-5.0.2.tar.gz
2. 编译(PHP 扩展都可以用这种方法安装)
   cd swoole-src-5.0.2
   phpize
   ./configure
   make && make install
3. vim /usr/local/php8/etc/php.ini 加入 extension=swoole.so 且添加 swoole.use_shortname='Off'
4. 查看扩展是否添加成功
   php -m
   php --ri swoole
5. 还需要安装进程控制扩展 pcntl
   cd /usr/local/src/php-8.0.1/ext/pcntl
   phpize
   ./configure
   make && make install
6. vim /usr/local/php8/etc/php.ini 加入 extension=pcntl.so
7. 查看扩展是否添加成功
   php -m

# Hyperf

## 创建环境

### windows

windows 先安装 docker，就不需要其他环境了，前置条件只需控制面板-程序功能-启用或关闭 Windows 功能:开启 hyper-V(wsl/wsl2 是装 linux 子系统的还得下系统,开启后夜神模拟器无法启动需关闭)

[下载](https://www.docker.com/)

镜像源加速设置 json docker 桌面版 setting->docker engine

```
 "registry-mirrors": [
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://registry.docker-cn.com"
  ]
```

安装到其他盘

1. 创建 D:\docker
2. 创建软链 mklink /j "C:\Program Files\Docker" "D:\docker"
   最后安装 exe
3. 安装后重启电脑才能启动

4. 拉取官方镜像

   cmd 输入 docker pull hyperf/hyperf

   运行容器 hyperf/hyperf 就是你拉取的镜像
   并绑定项目目录 E:/code/gitserver/本机共享目录 /data/projectlinux 共享目录

   ```
   #官方php8.0 他的镜像是Alpine hyperf3.0需要swoole5.0+ 安装后需要重新安装swoole扩展
   docker run -d --name hyperf -v E:/code/gitserver/:/data/project -p 9501:9501 -p 22:22 -it --privileged -u root --entrypoint /bin/sh hyperf/hyperf:8.0-alpine-v3.15-swoole
   #centos手动安装php8也可以 这样可以保持环境完全一致
   docker run -d --name centos7 -v E:/code/hyperf:/data/project -p 9501:9501 -p 22:22 -it --privileged -u root --entrypoint /bin/sh centos:7
   ```

5. docker
   ```
   #查看容器ID
   docker ps -a
   #退出
   exit
   #启动
   docker start 容器ID
   #进入
   docker exec -it --user root 容器ID /bin/bash
   #先停止
   docker stop 容器ID
   #再删除
   docker rm  容器ID
   ```
6. 安装 mysql 容器
   启动参数需要加上 MYSQL_ROOT_PASSWORD 123456 否则无法启动

!> windows 是无法 ping 通容器内的 ip 的，传文件的话直接利用共享目录,另外需要注意 Linux 内核版本，查看不同的安装命令

### yasdDebug

1. 需要手动在 centos7 里面安装 php 环境，包括 swoole 扩展，yasd 扩展，以及他们的前置扩展,或者直接下载官方的镜像

2. 容器启动 SSH ssh-keygen -A 需要在最后加上&符号 /usr/sbin/sshd -D &

3. Docker 可以通过多个-p 映射多个 win 到 docker 容器的端口 数据都是通过这个端口转发,容器可以提交保存后以新的方式启动

4. PHP_IDE_CONFIG 错误 export PHP_IDE_CONFIG="serverName=servername",或者发现监听不到了重新执行一下

5. 调试只需要配置 PHP->DEBUG 的端口,别的都不需要

6. 调试先打开小电话开启监听 9000，在缓存类打上断点，接着用 php -e bin/hyperf.php start 启动，最后浏览器访问

7. yasd 需要用低版本扩展 0.2.5 版本,配置的端口为 IDE 的端口，IP 为主机的局域网 IP

8. 如需要配置 IDE PHP 版本为容器 php 则需要容器启动 ssh 去配置

9. hyper 只能调试缓存代理类

10. Aphine 内核安装 php 编译工具 apk add autoconf dpkg-dev file g++ gcc libc-dev make php8-dev php8-pear re2c pcre pcre-dev,注意使用时 phpize8 [如果缺少 config.m4](https://zhuanlan.zhihu.com/p/565444042)

11. Aphine boost: apk add --no-cache boost boost-dev

### linux

直接安装 composer，并且已经安装好 php8,mysql

```
#安装程序
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
#可以全局使用 有环境变量
mv composer.phar /usr/local/bin/composer
#查看版本
composer -v
#配置阿里云
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer
```

composer 常用命令

```
 1、composer list：获取帮助信息；
 2、composer init：以交互方式填写composer.json文件信息；
 3、composer install：从当前目录读取composer.json文件，处理依赖关系，并安装到vendor目录下；
 4、composer update：获取依赖的最新版本，升级composer.lock文件；
 5、composer require：添加新的依赖包到composer.json文件中并执行更新；
       composer remove twbs/bootstrap; 卸载依赖包
 6、composer search：在当前项目中搜索依赖包；
 7、composer show：列举所有可用的资源包；
 8、composer validate：检测composer.json文件是否有效；
 9、composer self-update：将composer工具更新到最新版本；
       composer self-update -r ：回滚到安装的上一个版本
10、composer diagnose：执行诊断命令
11、composer clear：清除缓存
12、composer create-project：基于composer创建一个新的项目；
13、composer dump-autoload：在添加新的类和目录映射是更新autoloader
```

## 起步

### 创建项目

```
#除了mysql redis 其他全部n

composer create-project hyperf/hyperf-skeleton:dev-master hyperf-test

cd hyperf-test
#docker 的话是cd /data/project/hyperf-test
php bin/hyperf.php start
```

访问 http://127.0.0.1:9501/

!> 直接用服务器的 composer 下载依赖，vendor 提交到 git

### 热更新

```
composer require hyperf/watcher --dev
#生成配置文件
php bin/hyperf.php vendor:publish hyperf/watcher
#启动
php bin/hyperf.php server:watch
```

### 部署

```
# 优化 Composer 索引
composer dump-autoload -o
# 生成代理类和注解缓存
php bin/hyperf.php
```

## 核心架构

### 启动执行流程

第一个生命周期

```
启动入口 bin/hyperf.php
↓
container.php 中 new DefinitionSourceFactory(true))()调用DefinitionSourceFactory中的invoke方法
↓
ProviderConfig::load()加载所有插件配置 然后/autoload/dependencies.php引入自己的配置两者合并 得到容器中的名称和插件(如Redis)配置的键值对，用于从容器中获取类实例的
↓
$container->get(Hyperf\Contract\ApplicationInterface::class) 从容器中获取vendor/hyperf/framework/src/ApplicationFactory.php类实例,并调用invoke方法
↓
注册命令行代码类 包括服务器启动的命令行类vendor/hyperf/server/src/Command/StartServer.php
↓
最终$application->run()调用vendor/hyperf/server/src/Server.php的initServers()->makeServer()初始化各种swoole的服务器(http/websocket)
下
↓
swoole的start()启动服务器
```

### 请求执行流程

```
vendor/hyperf/server/src/Server.php中$this->registerSwooleEvents注册了vendor/hyperf/http-server/src/Server.php中的onRequest回调方法
↓
$this->dispatcher->dispatch分发请求，中间件
↓
vendor/hyperf/dispatcher/src/AbstractRequestHandler.php中的$handler->process($request, $this->next())执行处理请求
↓
进入vendor/hyperf/http-server/src/CoreMiddleware.php中调用
$this->handleFound()->$controllerInstance->{$action}(...$parameters)执行控制器方法
↓
最后执行完返回输出浏览器响应结果
```

### 协程

协程与线程很相似，都有自己的上下文，可以共享全局变量，但不同之处在于，在同一时间可以有多个线程处于运行状态，但对于 Swoole 协程来说只能有一个，其它的协程都会处于暂停的状态。此外，普通线程是抢占式的，哪个线程能得到资源由操作系统决定，而协程是协作式的，执行权由用户态自行分配。

#### 创建协程

```php
co(function () {
    sleep(10);
    echo "10S后输出";
});

go(function () {
    sleep(5);
    echo "5S后输出";
});

Coroutine::create(function () {
    sleep(2);
    echo "2秒后输出";
});
```

#### 协程间通讯

```php
$channel = new Channel();

Coroutine::create(function () use ($channel) {
    sleep(10);
    echo "10秒后输出";
    $channel->push("data");
});

Coroutine::create(function () use ($channel) {
    echo $channel->pop();
    echo "我被阻塞";
});
```

#### 等待所有协程结束

```php
$wg = new WaitGroup();
$wg->add(2);//计数器加2

co(function () use ($wg) {
    sleep(5);
    echo "5秒后输出";
    $wg->done();//计数器减1
});

co(function () use ($wg) {
    sleep(5);
    echo "10秒后输出";
    $wg->done();//计数器减1
});

$wg->wait();//等待计数器为0
echo "我被阻塞";
```

parallel 相同功能也是等待所有协程执行完毕后输出 下面的是简写 也有 wait 写法类似

```php
$result = parallel([
    function () {
    sleep(5);
    return "5秒后输出";
    },
    function () {
    sleep(10);
    return "10秒后输出";
    },
]);
var_dump($result);
echo "我被阻塞";
```

#### 协程上下文

用户信息可以保存在协程上下文里面 具体可以在权限中间中进行保存用户信息

```php
use Hyperf\Context\Context;

$foo = Context::set('foo', 'bar');
echo $foo;//bar
echo Context::get('foo');
Context::destroy('foo');
echo Context::has('foo');//1
```

### 配置

获取配置在 config/config.php 或者/config/autoload/xxx.php(通过文件名.配置)

#### 通过对象获取

```php
#[Inject()]
private ConfigInterface $config;

var_dump($this->config->get('app_name'));//app_name_test
var_dump($this->config->get('server.mode'));//2
```

#### 通过注解赋值

```php
#[Value("config.app_name")]
private $appName;

#[Value("config.server.mode")]
private $mode;

echo $this->appName;
echo $this->mode;
```

### 注解

#### 定义

```php
<?php


namespace App\Annotation;

use Attribute;
use Hyperf\Di\Annotation\AbstractAnnotation;

#[Attribute(Attribute::TARGET_CLASS | Attribute::TARGET_METHOD)]
class SxcAnnotation extends AbstractAnnotation {

  //注解传入的参数
  public array $values = [];

}
```

#### 使用

```php
use App\Annotation\SxcAnnotation;

#[SxcAnnotation(values: ["permission1", "permission2"])]
public function index() {}
```

### 依赖注入 DI

相当于把类放入一个容器中，想用就拿一下

#### 构造方法注入

```php
private UserService $userService;

// 通过在构造函数的参数上声明参数类型完成自动注入
public function __construct(UserService $userService)
{
    $this->userService = $userService;
}
```

#### 注解注入

```php
use Hyperf\Di\Annotation\Inject;

#[Inject]
private UserService $userService;

#[Inject(required: FALSE)]//不退在时注入null而不是抛出异常
private UserService $userService;

#[Inject]
private $userService;//可省略 可读性差不推荐
```

#### 懒加载

调用时才会实例化对象

```php
#[Inject(lazy: true)]
private UserService $userService;
```

#### 获取容器对象

**构造方法获取**

```php
use Psr\Container\ContainerInterface;

protected ContainerInterface $container;

// 通过在构造函数的参数上声明参数类型完成自动注入
public function __construct(ContainerInterface $container)
{
    $this->container = $container;
}
```

**注解获取**

```php
#[Inject]
protected ContainerInterface $container;
```

**通过应用上下文静态方法获取**

```php
$container = ApplicationContext::getContainer();
```

### 事件

事件就是一个普通类，比如用户注册事件；用户注册后，实例化事件类，然后将注册事件分发给指定监听器，然后这个监听器只要监听了这个类，就会执行监听器里的回调方法

#### 定义事件

App\Event\UserRegister.php

```php
<?php


namespace App\Event;


/**
 * Class UserRegister
 * 用户注册事件类 就是一个管理数据的普通类 类似于前端的store状态类
 *
 * @package App\Event
 */
class UserRegister {

  public string $username = "";//声明为public

  public function __construct($username) {
    $this->username = $username;
  }

}
```

#### 定义监听器

App\Listener\UserRegisterListener.php

```php
<?php


namespace App\Listener;


use App\Event\UserRegister;
use Hyperf\Event\Annotation\Listener;
use Hyperf\Event\Contract\ListenerInterface;

/*
 * 通过注解注册这个监听器，让Dispatcher事件调度器发现
 * 也可以把  \App\Listener\UserRegisteredListener::class
 * 配置在config/autoload/listeners.php数组中
 */

#[Listener]
class UserRegisterListener implements ListenerInterface {

  /**
   * 监听器返回事件数组 就是你要监听哪个类写在数组里
   */
  public function listen(): array {
    return [
      UserRegister::class,
    ];
  }

  /**
   * 这里是监听器执行触发的方法在这里相当于vue emit触发的方法
   */
  public function process(object $event) {
    echo $event->username;//张三
    //这里就可以用来写比如注册成功短信通知等逻辑
  }

}
```

#### 触发事件

也就是分发事件

```php
use App\Event\UserRegister;
use Psr\EventDispatcher\EventDispatcherInterface;

#[Inject]
private EventDispatcherInterface $eventDispatcher;

//分发事件 相当于emit触发监听器的方法
$this->eventDispatcher->dispatch(new UserRegister("张三"));
```

### AOP 切面

可以用来做权限处理，或者对某些方法增强

#### 定义

app/Aspect/DemoAspect.php

SxcAnnotation 参考注解章节

```php
<?php


namespace App\Aspect;


use App\Annotation\SxcAnnotation;
use Hyperf\Di\Annotation\Aspect;
use Hyperf\Di\Aop\AbstractAspect;
use Hyperf\Di\Aop\ProceedingJoinPoint;

#[Aspect] //定义为切面注解
class DemoAspect extends AbstractAspect {

  //要切入的类
  public $classes = [
    //SomeClass::class,
  ];

  // 要切入的注解，仅可切入类注解和类方法注解
  public $annotations = [
    SxcAnnotation::class,
  ];


  /**
   * @inheritDoc
   */
  public function process(ProceedingJoinPoint $proceedingJoinPoint) {
    // 切面切入后，执行对应的方法会由此来负责
    // $proceedingJoinPoint 为连接点，通过该类的 process() 方法调用原方法并获得结果

    // 前置钩子
    //比如这里可以获取权限注解参数，获取不到注意是否有缓存
    $annotaions = $proceedingJoinPoint->getAnnotationMetadata()->method;
    foreach ($annotaions as $key => $item) {
      if ($key == SxcAnnotation::class) {
        var_dump($item->values);//这里就可以去判断当前账号是否有这个角色或权限 进行返回 可以修改响应内容
        /* array(2) {
                   [0]=>
           string(11) "permission1"
                   [1]=>
           string(11) "permission2"
         }*/

      }
    }

    $result = $proceedingJoinPoint->process();
    var_dump($result);//就是Response内容
    // 后置钩子
    return $result;
  }

}
```

## 基础功能

### 路由

#### 定义

```php
Router::addRoute([
  'GET',
  'POST',
  'HEAD',
  'OPTIONS',
  'TRACE',
  'CONNECT',
], '/', 'App\Controller\IndexController@index');
```

#### 分组路由

```php
//http://127.0.0.1:9501/index/xxxFn
Router::addGroup('/index', function () {
  Router::addRoute(['GET'], "/getFn", 'App\Controller\IndexController@getFn');
  Router::addRoute(['POST'], "/postFn", 'App\Controller\IndexController@postFn');
  Router::addRoute(['PUT'], "/putFn", 'App\Controller\IndexController@putFn');
  Router::addRoute(['DELETE'], "/deleteFn", 'App\Controller\IndexController@deleteFn');
});
```

#### 注解路由

**自动生成 GET/POST 路由**(控制器名称+方法名)小写全拼+下划线方式)
适用于简单的方法,感觉用不到

```php
use Hyperf\HttpServer\Annotation\AutoController;

#[AutoController]
class IndexController extends AbstractController {
}
```

**手动编写**

```php
use Hyperf\HttpServer\Annotation\Controller;
use Hyperf\HttpServer\Annotation\GetMapping;
use Hyperf\HttpServer\Annotation\RequestMapping;

#[Controller]
class IndexController extends AbstractController {

  //http://127.0.0.1:9501/index/getFnTwo
  #[GetMapping(path: "getFnTwo")]
  public function getFnTwo() {
    return ["data" => "getFnTwo"];
  }

  //http://127.0.0.1:9501/index/requestFn
  #[RequestMapping(path: "requestFn", methods: "get,post,put,delete")]
  public function requestFn() {
    return ["data" => "requestFn all methods"];
  }
}
```

!> #[Controller] 和 #[AutoController] 都提供了 prefix 和 server 两个参数。默认为控制器名小写下划线

#### 传参

**定义**

```php
Router::get('/getFn/{id}', 'App\Controller\IndexController@getFn');
```

**获取 1**

```php
public function getFn(int $id) {
    return ["data" => "hello get methods id:" . $id];
}
```

**获取 2**

```php
use Hyperf\HttpServer\Contract\RequestInterface;

public function getFn(RequestInterface $request) {
    return ["data" => "hello get methods id:" . $request->route("id", 0)];
}
```

**推荐**

```php
use Hyperf\HttpServer\Contract\RequestInterface;

//http://127.0.0.1:9501/index/requestFn/1
#[RequestMapping(path: "requestFn/{id}", methods: "get,post,put,delete")]
public function requestFn(RequestInterface $request) {
    return ["data" => "requestFn all methods id:" . $request->route("id", 0)];
}
```

**选填参数**

```php
/user/[{id}]
```

### 中间件

#### 常用中间件

**跨域中间件**

跨域配置也可以直接挂在 Nginx 上

```php
<?php


namespace App\Middleware;


use Hyperf\Context\Context;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Server\RequestHandlerInterface;

class CorsMiddleware implements MiddlewareInterface {

  /**
   * 拦截处理方法
   */
  public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface {
    $response = Context::get(ResponseInterface::class);
    $response = $response->withHeader('Access-Control-Allow-Origin', '*')
      ->withHeader('Access-Control-Allow-Credentials', 'true')
      // Headers 可以根据实际情况进行改写。
      ->withHeader('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept,  X-Token')
      ->withHeader('Access-Control-Expose-Headers', 'Token,Code')
      ->withHeader('Access-Control-Max-Age', 3600);

    Context::set(ResponseInterface::class, $response);

    if ($request->getMethod() == 'OPTIONS') {
      return $response;
    }

    return $handler->handle($request);
  }

}
```

**权限中间件**

```php
<?php

declare(strict_types=1);

namespace App\Middleware;


use Psr\Container\ContainerInterface;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Hyperf\HttpServer\Contract\RequestInterface;
use Hyperf\HttpServer\Contract\ResponseInterface as HttpResponse;

class AuthMiddleware implements MiddlewareInterface {

  protected ContainerInterface $container;

  protected RequestInterface $request;

  protected HttpResponse $response;

  public function __construct(ContainerInterface $container, HttpResponse $response, RequestInterface $request) {
    $this->container = $container;
    $this->response = $response;
    $this->request = $request;
  }

  public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface {
    // 根据具体业务判断逻辑走向，这里假设用户携带的token有效
    $isValidToken = TRUE;
    if ($isValidToken) {
      return $handler->handle($request);
    }

    return $this->response->json(
      [
        'code' => -1,
        'data' => [
          'error' => '中间件验证token无效，阻止继续向下执行',
        ],
      ]
    );
  }

}
```

#### 全局中间件

只能在配置文件中 config/autoload/middlewares.php 声明，一般用这个

#### 配置文件路由中间件

```php
//http://127.0.0.1:9501/index/xxxFn
Router::addGroup('/index', function () {
  Router::addRoute(['GET'], "/getFn", 'App\Controller\IndexController@getFn');
  Router::addRoute(['POST'], "/postFn", 'App\Controller\IndexController@postFn');
  Router::addRoute(['PUT'], "/putFn", 'App\Controller\IndexController@putFn');
  Router::addRoute(['DELETE'], "/deleteFn", 'App\Controller\IndexController@deleteFn');
}, ['middleware' => [CorsMiddleware::class]]);
```

#### 注解路由中间件

**类单个中间件**

```php
use App\Middleware\CorsMiddleware;
use Hyperf\HttpServer\Annotation\Middleware;

#[Middleware(CorsMiddleware::class)]
class IndexController extends AbstractController {}
```

**类多个中间件**

```php
//使用这种方式即可
#[Middleware(CorsMiddleware1::class)]
#[Middleware(CorsMiddleware2::class)]
#[Middleware(CorsMiddleware3::class)]
class IndexController extends AbstractController {}

#[Middlewares([CorsMiddleware::class])]//实测无效
```

**方法中间件**

同类上定义即可,类中间件先执行，然后执行方法中间件

#### 快速生成中间件

```
php ./bin/hyperf.php gen:middleware AuthMiddleware
```

### 控制器

#### 定义

`$request和$response`对象是由依赖注入容器自动注入了，直接使用，且是单例

```php
<?php

declare(strict_types=1);


namespace App\Controller;


use Hyperf\HttpServer\Annotation\Controller;
use Hyperf\HttpServer\Annotation\RequestMapping;
use Hyperf\HttpServer\Contract\RequestInterface;
use Hyperf\HttpServer\Contract\ResponseInterface;

#[Controller]
class IndexController extends AbstractController {

  //http://127.0.0.1:9501/index/requestFn
  #[RequestMapping(path: "requestFn", methods: "get,post,put,delete")]
  public function requestFn(RequestInterface $request, ResponseInterface $response) {
    return ["data" => "requestFn all methods"];
  }

}
```

### 请求

#### URL 相关方法

```php
//获取请求路径 /index/requestFn
echo $request->path();
//获取请求完整路径 http://127.0.0.1:9501/index/requestFn?
echo $request->fullUrl();
//获取请求方法 POST
echo $request->getMethod();
//判断请求方法 1
echo $request->isMethod('post');
```

#### 获取传参相关方法

假设 json 传参

```json
{
  "form": {
    "id": 1
  }
}
```

```php
//获取路由GET参数 不存在默认为0
echo $request->route('id',0);
//获取所有传参 GET/POST会合并到一个数组里面
var_dump($request->all());
//获取指定key 不存在默认为0
echo $request->input("id", 0);
//获取多个key GET/POST会合并到一个数组里面
var_dump($request->inputs(["id", "name"], ["0", "默认名称"]));
//只获取GET传参
echo $request->query('id', 0);
//获取所有GET传参
var_dump($request->query());
//获取json
var_dump($request->input('form.id'));
//获取所有json 同样会把GET参数也获取到
var_dump($request->all());
//判断参数是否存在
var_dump($request->has('id'));
var_dump($request->has(['id', 'name']));
```

#### 文件上传相关方法

```php
//判断请求中文件是否存在 && 验证是否上传有效
if ($request->hasFile('imgFile') && $request->file('imgFile')->isValid()) {
    //获取文件对象
    $file = $request->file('imgFile');
    var_dump($file);
    //获取临时路径
    $path = $file->getPath();
    echo $path;
    //获取扩展名
    $extension = $file->getExtension();
    echo $extension;
    //保存文件 需要目录实际存在 实际可以按日期建目录保存
    $file->moveTo(BASE_PATH . '/public/upload/' . $file->getClientFilename());
    //判断保存文件是否成功
    if ($file->isMoved()) {
    echo "上传成功";
    }
}
```

### 响应

#### 定义

```php
use Hyperf\HttpServer\Contract\ResponseInterface;
use Psr\Http\Message\ResponseInterface as Psr7ResponseInterface;

public function json(ResponseInterface $response): Psr7ResponseInterface {
    ...
}
```

#### 返回 json

```php
<?php

declare(strict_types=1);


namespace App\Controller;


use Hyperf\HttpServer\Annotation\Controller;
use Hyperf\HttpServer\Annotation\RequestMapping;
use Hyperf\HttpServer\Contract\RequestInterface;
use Hyperf\HttpServer\Contract\ResponseInterface;
use Psr\Http\Message\ResponseInterface as Psr7ResponseInterface;

#[Controller]
class IndexController extends AbstractController {

  //http://127.0.0.1:9501/index/requestFn
  #[RequestMapping(path: "requestFn", methods: "get,post,put,delete")]
  public function requestFn(RequestInterface $request, ResponseInterface $response): Psr7ResponseInterface {
    return $response->json(["data" => "requestFn all methods"]);
  }

}

```

#### 重定向

```php
return $response->redirect('https://www.baidu.com');
```

#### 文件下载

```php
return $response->download(BASE_PATH . '/public/upload/1.txt', '1.txt');
```

### 异常拦截器

#### 自定义异常

```php
<?php


namespace App\Exception;


use RuntimeException;

class CustomException extends RuntimeException {
}
```

#### 自定义拦截器

```php
<?php


namespace App\Exception\Handler;


use App\Exception\CustomException;
use Hyperf\ExceptionHandler\ExceptionHandler;
use Hyperf\HttpMessage\Stream\SwooleStream;
use Psr\Http\Message\ResponseInterface;
use Throwable;

class CustomExceptionHandler extends ExceptionHandler {

  public function handle(Throwable $throwable, ResponseInterface $response) {
    // 判断被捕获到的异常是希望被捕获的异常
    if ($throwable instanceof CustomException) {
      // 阻止异常冒泡
      $this->stopPropagation();
      //返回自定义信息
      $data = json_encode([
        'code' => $throwable->getCode(),
        'message' => $throwable->getMessage(),
      ], JSON_UNESCAPED_UNICODE);
      //返回给浏览器
      return $response->withStatus(500)->withBody(new SwooleStream($data));
    }

    // 交给下一个异常处理器
    return $response;
  }

  public function isValid(Throwable $throwable): bool {
    return TRUE;
  }

}
```

#### 注册异常拦截器

config/autoload/exceptions.php

```php
return [
  'handler' => [
    'http' => [
      CustomExceptionHandler::class,
      XXXX::class,
    ],
  ],
];
```

### redis 缓存

#### 配置 redis

config/autoload/redis.php

#### 使用

假如缓存数据库用户信息 app/Service/UserService.php,当调用$this->userService->getInfoById(1)该方法时，就会缓存

```php
<?php


namespace App\Service;


use Hyperf\Cache\Annotation\Cacheable;

class UserService {

  //热点数据 比如首页信息，热点新闻等等,缓存存入redis key为hot-data:id 过期时间为1小时 listener用于更新数据 触发该方法
  #[Cacheable(prefix: "user-data", ttl: 3600, listener: "user-data-update")]
  public function getInfoById(int $id): array {
    return ["id" => $id];
  }

}
```

#### 清除缓存

一般写一个类，想要清除时，去清除一下就行了
**定义**

```php
<?php


namespace App\Service;


namespace App\Service;

use Hyperf\Di\Annotation\Inject;
use Hyperf\Cache\Listener\DeleteListenerEvent;
use Psr\EventDispatcher\EventDispatcherInterface;

class SystemService {

  #[Inject]
  protected EventDispatcherInterface $dispatcher;

  /**
   *
   * 用于清除指定key缓存
   *
   * @param $id
   *
   * @return bool
   */
  public function flushCache($id) {
    $this->dispatcher->dispatch(new DeleteListenerEvent('user-data-update', [$id]));
    return TRUE;
  }

}
```

**使用**

```php
use App\Service\SystemService;

#[Inject()]
private SystemService $systemService;

$this->systemService->flushCache(1);
```

### 日志

#### 基础使用

```php
<?php

declare(strict_types=1);


namespace App\Controller;


use App\Log\CustomLog;
use Hyperf\HttpServer\Annotation\Controller;
use Hyperf\HttpServer\Annotation\RequestMapping;
use Hyperf\HttpServer\Contract\RequestInterface;
use Hyperf\HttpServer\Contract\ResponseInterface;
use Psr\Http\Message\ResponseInterface as Psr7ResponseInterface;
use Hyperf\Logger\LoggerFactory;
use Psr\Log\LoggerInterface;

#[Controller]
class IndexController extends AbstractController {

  protected LoggerInterface $logger;

  public function __construct(LoggerFactory $loggerFactory) {
    parent::__construct();
    // 第一个参数对应日志的 name, 第二个参数对应 config/autoload/logger.php 内的 key
    $this->logger = $loggerFactory->get('logname', 'default');
  }


  //http://127.0.0.1:9501/index/requestFn
  #[RequestMapping(path: "requestFn", methods: "get,post,put,delete")]
  public function requestFn(RequestInterface $request, ResponseInterface $response): Psr7ResponseInterface {
    //这种基本不用
    $this->logger->info("访问" . $request->getUri());
    $this->logger->warning("warning");
    $this->logger->debug("debug");
    $this->logger->error("error");

    //推荐从容器里拿
    CustomLog::get("logname")->info("容器中拿的日志实例写入");
    return $response->json(["data" => "requestFn all methods"]);
  }

}
```

#### 关于封装 CustomLog 有两种方法

app/Log/CustomLog.php

1. 普通方法

```php
<?php


namespace App\Log;


use Hyperf\Logger\LoggerFactory;
use Hyperf\Utils\ApplicationContext;

class CustomLog {

  public static function get(string $name = 'app') {
    return ApplicationContext::getContainer()
      ->get(LoggerFactory::class)
      ->get($name);
  }

}
```

2. 魔术方法(推荐)

魔术方法就是当静态方法不存在时，会调用魔术方法，并把方法和参数传入，\_\_call 也是相同的作用

```php
<?php


namespace App\Log;


use Hyperf\Logger\LoggerFactory;
use Hyperf\Utils\ApplicationContext;

class CustomLog {

  /**
   * 魔术方法 当静态方法CustomLog::XXX(arg)不存在时，自动调用并传入参数 (XXX,[arg])
   *
   * @param string $name 方法名
   * @param array $arguments 参数
   *
   * @return mixed
   * @throws \Psr\Container\ContainerExceptionInterface
   * @throws \Psr\Container\NotFoundExceptionInterface
   */
  public static function __callStatic(string $name, array $arguments) {
    $instance = ApplicationContext::getContainer()->get(LoggerFactory::class);
    return $instance->$name(...$arguments);
  }

}
```

#### 配置日志按日期保存

config/autoload/logger.php

```php
...
'default' => [
    'handler' => [
      'class' => Monolog\Handler\RotatingFileHandler::class,
      'constructor' => [
        'filename' => BASE_PATH . '/runtime/logs/hyperf.log',
        'level' => Monolog\Logger::DEBUG,
      ],
    ],
    ...
]
```

#### sql 日志

默认已经写好了 app/Listener/DbQueryExecutedListener.php，注意如果要事务的日志自己加一下就行

https://hyperf.wiki/3.0/#/zh-cn/db/event?id=%e8%bf%90%e8%a1%8c%e4%ba%8b%e4%bb%b6

### 验证器

#### 安装

需要安装的扩展

```shell
composer require hyperf/validation
```

#### 配置

配置验证器为全局中间件 config/autoload/middlewares.php

```php
use Hyperf\Validation\Middleware\ValidationMiddleware;

return [
  'http' => [
    //验证器组件
    ValidationMiddleware::class,
  ],
];
```

配置自带的验证失败异常处理类 config/autoload/exceptions.php

```php
use Hyperf\Validation\ValidationExceptionHandler;

return [
  'handler' => [
    'http' => [
      //使用内置的验证器异常处理类 这个一般不行
      ValidationExceptionHandler::class,
      //使用自定义的验证器异常处理类 推荐
      CustomValidationExceptionHandler::class,
    ],
  ],
];
```

也可以配置自己的异常处理器去捕获验证器异常（用这个，因为一般验证不通过也是返回 json）app/Exception/Handler/CustomValidationExceptionHandler.php

```php
<?php


namespace App\Exception\Handler;


use Hyperf\ExceptionHandler\ExceptionHandler;
use Hyperf\HttpMessage\Stream\SwooleStream;
use Hyperf\Validation\ValidationException;
use Psr\Http\Message\ResponseInterface;
use Throwable;

class CustomValidationExceptionHandler extends ExceptionHandler {

  public function handle(Throwable $throwable, ResponseInterface $response) {
    // 判断被捕获到的异常是希望被捕获的异常
    if ($throwable instanceof ValidationException) {
      // 阻止异常冒泡
      $this->stopPropagation();
      //返回自定义信息
      $data = json_encode([
        'code' => 1001,
        'message' => $throwable->validator->errors()->first(),
      ], JSON_UNESCAPED_UNICODE);
      //返回给浏览器
      if (!$response->hasHeader('content-type')) {
        $response = $response->withAddedHeader('content-type', 'application/json; charset=utf-8');
      }
      return $response->withStatus(200)->withBody(new SwooleStream($data));
    }

    // 交给下一个异常处理器
    return $response;
  }

  public function isValid(Throwable $throwable): bool {
    return TRUE;
  }

}
```

验证器的消息依赖多语言组件

```shell
#生成多语言config文件 config/autoload/translation.php
php bin/hyperf.php vendor:publish hyperf/translation
#生成验证器的语言文件 storage/languages
php bin/hyperf.php vendor:publish hyperf/validation
```

#### 使用

生成验证器类

```shell
php bin/hyperf.php gen:request UserRequest
```

app/Request/UserRequest.php

```php
<?php

declare(strict_types=1);

namespace App\Request;

use Hyperf\Validation\Request\FormRequest;

class UserRequest extends FormRequest {

  /**
   * Determine if the user is authorized to make this request.
   */
  public function authorize(): bool {
    return TRUE;
  }

  /**
   * 增加rules方法添加验证规则
   *
   * @return string[]
   */
  public function rules(): array {
    return [
      'username' => 'required|max:3',
      'password' => 'required',
    ];
  }

}

```

使用验证器类替换原来的 RequestInterface app/Controller/IndexController.php

```php
<?php

declare(strict_types=1);


namespace App\Controller;


use App\Request\UserRequest;
use Hyperf\HttpServer\Annotation\Controller;
use Hyperf\HttpServer\Annotation\RequestMapping;
use Hyperf\HttpServer\Contract\ResponseInterface;
use Psr\Http\Message\ResponseInterface as Psr7ResponseInterface;

#[Controller]
class IndexController extends AbstractController {

  //http://127.0.0.1:9501/index/requestFn
  #[RequestMapping(path: "requestFn", methods: "get,post,put,delete")]
  public function requestFn(UserRequest $request, ResponseInterface $response): Psr7ResponseInterface {
    //验证通过 获取验证通过的字段
    $validated = $request->validated();
    print_r($validated);

    return $response->json(["data" => "requestFn all methods"]);
  }

}
```

访问http://127.0.0.1:9501/index/requestFn 页面提示 username 字段是必须的

#### 场景

app/Request/UserRequest.php

```php
<?php

declare(strict_types=1);

namespace App\Request;

use Hyperf\Validation\Request\FormRequest;

class UserRequest extends FormRequest {

  //添加和编辑场景 编辑场景需要额外的id参数
  protected $scenes = [
    'add' => ['username', 'password'],
    'edit' => ['id', 'username', 'password'],
  ];

  /**
   * Determine if the user is authorized to make this request.
   */
  public function authorize(): bool {
    return TRUE;
  }

  /**
   * 增加rules方法添加验证规则
   *
   * @return string[]
   */
  public function rules(): array {
    return [
      'id' => 'required',
      'username' => 'required|max:3',
      'password' => 'required',
    ];
  }

  /**
   * 自定义错误提示
   *
   * @return array
   */
  public function messages(): array {
    return [
      'id.required' => "缺少字段id",
      'username.required' => '用户名必填',
      'username.max' => '用户名最多3个字符',
      'password.required' => '密码必填',
    ];
  }

}
```

使用方法验证场景

```php
    //从容器里拿到请求
    $request = $this->container->get(UserRequest::class);
    //切换场景验证
    $request->scene('edit')->validateResolved();
    //验证通过 获取验证通过的字段
    $validated = $request->validated();
```

使用注解 推荐 3.0.0 版本以上才支持

```php
<?php

declare(strict_types=1);


namespace App\Controller;


use App\Request\UserRequest;
use Hyperf\HttpServer\Annotation\Controller;
use Hyperf\HttpServer\Annotation\RequestMapping;
use Hyperf\HttpServer\Contract\ResponseInterface;
use Hyperf\Validation\Annotation\Scene;
use Psr\Http\Message\ResponseInterface as Psr7ResponseInterface;

#[Controller]
class IndexController extends AbstractController {

  //http://127.0.0.1:9501/index/requestFn
  #[RequestMapping(path: "requestFn", methods: "get,post,put,delete")]
  //使用注解
  #[Scene(scene: 'add')]
  public function requestFn(UserRequest $request, ResponseInterface $response): Psr7ResponseInterface {
    //验证通过 获取验证通过的字段
    $validated = $request->validated();
    print_r($validated);

    return $response->json(["data" => "requestFn all methods"]);
  }

}
```

## ORM

### 配置

主要配置一下几个 注意编码如果是 utfmb4 也配置这个即可

```php
<?php

declare(strict_types=1);

return [
  'default' => [
    'host' => env('DB_HOST', 'localhost'),//连接地址
    'database' => env('DB_DATABASE', 'hyperf'),//数据库名
    'port' => env('DB_PORT', 3306),//端口
    'username' => env('DB_USERNAME', 'root'),//用户名
    'password' => env('DB_PASSWORD', ''),//密码
    'collation' => env('DB_COLLATION', 'utf8_general_ci'),//编码
    'prefix' => env('DB_PREFIX', ''),//前缀
    ...
  ],
];

```

### 原生查询

```php
//增
Db::insert("INSERT INTO user (name,age) VALUES (?,?)", ['王五', 18]);
//改
DB::update("UPDATE user SET name = ? where name = ?", ["王六", "王五"]);
//删
Db::delete("DELETE FROM user WHERE name = ?", ["王六"]);
//查
Db::select("SELECT * FROM user ");
```

### 输出最后 sql 语句

仅限开发环境

```php
Db::enableQueryLog();
$res = Db::select("SELECT * FROM user ");
var_dump(Arr::last(Db::getQueryLog()));
var_dump(Db::getQueryLog());
/*
array(3) {
  ["query"]=>
  string(19) "SELECT * FROM user "
  ["bindings"]=>
  array(0) {
  }
  ["time"]=>
  float(0.86)
}
*/
```

### 事务

#### 自动事务

```php
Db::transaction(function () {
   ...
});
```

#### 手动事务

```php
Db::beginTransaction();
try{
    ...
    Db::commit();
} catch(\Throwable $exception){
    Db::rollBack();
}
```

### 查询构造器

#### 查询多条

```php
//返回数组
$users = Db::table('user')->get();
foreach ($users as $user) {
    var_dump($user);//默认是stdClass对象 所有对象的父类 就是类似java的Object对象
}
//查询指定列 并 返回数组
$usersColumn = Db::table('user')->select("name", "age")->get();
```

如果要返回纯数组则需要创建一个监听器 并监听修改数据 app/Listener/FetchModeListener.php

```php
<?php
declare(strict_types=1);

namespace App\Listener;

use Hyperf\Database\Events\StatementPrepared;
use Hyperf\Event\Annotation\Listener;
use Hyperf\Event\Contract\ListenerInterface;
use PDO;

#[Listener]
class FetchModeListener implements ListenerInterface {

  public function listen(): array {
    return [
      StatementPrepared::class,
    ];
  }

  public function process(object $event): void {
    if ($event instanceof StatementPrepared) {
      $event->statement->setFetchMode(PDO::FETCH_ASSOC);
    }
  }

}
```

然后触发监听器

```php
//分发事件 相当于emit触发监听器的方法
$this->eventDispatcher->dispatch(new FetchModeListener());

//返回数组
$users = Db::table('user')->get();
foreach ($users as $user) {
    var_dump($user);//默认是stdClass对象 所有对象的父类 就是类似java的Object对象
}
```

#### 查询单条

```php
$user = Db::table("user")->first();
//find(id,column)
$users = Db::table('user')->find(1);
```

#### 查询单个值

```php
$name = Db::table('user')->value("name");
```

#### 查询单列

```php
//单列
$column = Db::table('user')->pluck("name");
//单列指定索引
$columnKeyValue = Db::table('user')->pluck('name', 'id');
```

#### 查询数量或判断存在

```php
//统计数量
$count = Db::table('user')->count();
//判断记录是否存在
$exists = Db::table('user')->exists();
$doesntExist = Db::table('user')->doesntExist();
```

#### 指定列去重

```php
//查询指定name和age列不重复的所有记录
$users = Db::table('user')->select('name', 'age')->distinct()->get();
```

#### 查询指定别名

```php
$users = Db::table('user')->select('name as n', 'age as a')->get();
```

#### 强制索引

EXPLAIN 执行后没有使用的索引的可以使用查询索引，一些情况下会破坏索引，例如使用!=,<>

```php
$users = Db::table('user')->forceIndexes(['age_index'])
    ->where('age', '!=', 18)
    ->get();
```

#### 连接查询

##### 连接查询

内连查询两表公共部分

```php
$users = Db::table('user as u')
    ->join('user_address as ua', 'u.id', '=', 'ua.user_id')
    ->select('u.*', 'ua.address', 'ua.id as user_address_id')
    ->get();
```

左连右连查询 leftJoin/rightJoin

##### 子查询

```php
$sub = Db::table('user')->select('id')->where('age', '=', 18);
$users = Db::table('user')
  ->joinSub($sub, 'sub', 'sub.id', '=', 'user.id')
  ->get();
```

类似方法还有 leftJoinSub/rightJoinSub

#### 条件语句

##### 基本 where 查询

```php
//简单等值比较
$users = Db::table('user')->where('age', 18)->get();
//其他比较 第二参数是运算符 > < = >= <= <> like等
$users = Db::table('user')->where('age', '>', '18')->get();
//多个条件使用数组
$users = Db::table('user')
  ->where([
    ['id', '=', 1],
    ['age', '=', 18],
  ])
  ->get();
```

##### and 查询

```php
$users = Db::table('user')
  ->where('name', "张三")
  ->where('age', 18)
  ->get();
```

##### or 查询

```php
$users = Db::table('user')
  ->where('name', "张三")
  ->orWhere('age', 18)
  ->get();
```

##### andor 查询

条件结构类似 1 and (2 or 3) 使用闭包来构造此类查询

```php
$users = Db::table('user')
  ->where('name', '李四')
  ->where(function ($query) {
    $query->where('age', 18)
      ->orWhere('age', 20);
  })
  ->get();
```

生成的 sql 语句

```sql
select * from `user` where `name` = ? and (`age` = ? or `age` = ?)
```

##### 区间查询

```php
$users = Db::table('user')
  ->whereBetween('id', [1, 2])
  ->get();
```

类似方法还有 whereNotBetween/whereIn/whereNotIn

##### exists 存在查询

exists 只查询连接表中的左表 类似与 leftjoin 只返回主表内容

比如我要查询所有有地址的用户

```php
$users = Db::table('user as u')
    ->whereExists(function ($query) {
      //Db::raw(1) 就是select 1 这样写对匹配的记录返回的是1 效率最快
      $query->select(Db::raw(1))
        ->from("user_address as ua")
        //whereRaw 使用原生sql
        ->whereRaw("u.id = ua.user_id");
    })
    ->get();
```

生成的 sql 语句

```sql
select * from `user` as `u` where exists (select 1 from `user_address` as `ua` where u.id = ua.user_id)
```

##### 原生查询条件

selectRaw/whereRaw/orWhereRaw 分别替代 select/where/orWhere 方法

##### 排序

```php
$users = Db::table('user')
  ->orderBy('id', 'desc')
  ->get();
```

##### groupBy

一般用于指定字段进行分组统计,也可用于去重，效率优于 distinct

配合 having 及统计函数 count/sum/等使用

```php
    $users = Db::table('user')
      ->select("*")
      ->selectRaw('count(*) as count')
      ->groupBy(['name'])
      ->having('age', '=', '18')
      ->get();

```

##### 限制数量查询

limit/offset

```php
$users = Db::table("user")->limit(2)->get();
```

#### 分页

```php
//假设pageSize为10 当前页默认传page字段即可
$pageSize = 10;
//返回分页对象
$users = Db::table('user')->paginate($pageSize);
//获取总记录数
$total = $users->total();
//是否还有更多记录
$hasMorePages = $users->hasMorePages();
//模型分页
$users = User::query()->paginate($pageSize);
```

#### 新增

```php
//插入单条 返回bool
$bool = Db::table('user')
  ->insert(['name' => '王五', 'age' => 1]);
//插入单条并返回Id
$id = Db::table('user')
  ->insertGetId(['name' => '王五', 'age' => 1]);
//插入多条 返回bool
$bool = Db::table('user')
  ->insert([
    ['name' => '王五', 'age' => 1],
    ['name' => '王五', 'age' => 1],
  ]);
```

#### 更新

##### 修改

```php
//更新 返回受影响的行数
$rows = Db::table('user')
  ->where("name", "王五")
  ->update(["age" => 99]);
```

##### 自增自减

increment/decrement

##### 修改数据加锁

可以防止修改时 数据被其他操作篡改

sharedLock(共享锁 大家都能读 但修改只能是自己 容易死锁 使用场景：事务期间查询最新数据且不允许修改)

lockForUpdate（排他锁 只能是自己增删该查 别人无法操作 使用场景：事务期间查询数据且可以修改）

```sql
select * from user lock in share mode
select * from user for update
```

#### 删除

delete 如果要清空表使用 truncate 且重置自增 id

### 模型

#### 配置

config/autoload/databases.php

```php
<?php

declare(strict_types=1);
use Hyperf\Database\Commands\ModelOption;
return [
  'default' => [
    ...
    'commands' => [
      'gen:model' => [
        'path' => 'app/Model',//生成模型的路径
        'inheritance' => 'Model',//父类
        'force_casts' => TRUE,//是否强制重置生成Model类中 casts 参数
        'refresh_fillable' => TRUE,//是否刷新生成Model类中 fillable 参数
        'table_mapping' => [],//为表名 -> 模型增加映射关系 比如 ['users:Account']
        'with_comments' => TRUE,//是否增加字段注释
        'property_case' => ModelOption::PROPERTY_CAMEL_CASE,//数据库字段转驼峰
      ],
    ],
  ],
];

```

#### 生成模型

```shell
php bin/hyperf.php gen:model 表名
```

比如生成用户地址 model 如下，驼峰记得引入 tarit CamelCase 否则无效

```php
<?php

declare(strict_types=1);

namespace App\Model;

use Hyperf\DbConnection\Model\Model;

//引入trait公共方法 查询结果自动转换为驼峰式
use  Hyperf\Database\Model\Concerns\CamelCase;

/**
 * @property int $id id
 * @property string $address 地址
 * @property int $userId userId
 */
class UserAddress extends Model {

  use CamelCase;

  /**
   * The table associated with the model.
   */
  protected ?string $table = 'user_address';

  /**
   * The attributes that are mass assignable.
   */
  protected array $fillable = ['id', 'address', 'user_id'];

  /**
   * The attributes that should be cast to native types.
   */
  protected array $casts = ['id' => 'integer', 'user_id' => 'integer'];

}
```

#### 时间戳

表格创建时固定创建两个字段 created_at 和 updated_at 框架自动维护这两个时间

created_at timestamp notnull 默认值为 CURRENT_TIMESTAMP 创建时间

updated_at timestamp null 默认值为 NULL 勾上根据当前时间更新 更新时间

!> 如果发现时区不对在启动文件中加上 date_default_timezone_set('Asia/Shanghai');即可

#### 刷新模型

##### 复制模型

复制一个模型对象 深拷贝

```php
$newUser = User::query()->fresh();
```

##### 重置模型

重置一个模型对象为查询出来的初始数据

```php
$user = User::query();
$user->refresh();
```

#### 查询

用法都差不多 可以省略 query() 还是不要省略好

```php
$users = User::query()->get();

foreach ($users as $user) {
  var_dump($user);//返回User模型对象 object(App\Model\User)
}
```

##### 数据不存时抛出异常

不存在会抛出 Hyperf\Database\Model\ModelNotFoundException 配合异常拦截器可以统一返回数据

```php
$users = User::query()
  ->where("id", 9999)
  ->firstOrFail();
```

#### 新增

##### 新增单条

```php
//模型对象用法 注意命名需要驼峰
$userAddress = new UserAddress([
  "address" => "test",
  "userId" => 18,
]);
$bool = $userAddress->save();
```

或者下面这种批量赋值,如果已经有一个对象可以用 fill([])来填充

```php
//返回新增的记录
$address = UserAddress::create(['address' => '上海', "userId" => 18]);
```

#### 批量新增

这个新增时不会复制 updated_at 字段 无语

```php
$bool = UserAddress::query()
  ->insert([[
    "address" => "test",
    "user_id" => 18,
  ]]);
```

#### 更新

##### 单条更新

```php
$userAddress = UserAddress::query()->find(2);
$userAddress->address = "上海杭州";
$bool = $userAddress->save();
```

##### 批量更新

```php
$bool = UserAddress::query()->update([
    "address" => '浙江杭州',
  ]);
```

#### 模型 N 对 N 的关系

可以简化操作 join 操作 在模型中定义关系的方法 就可以在控制直接调用方法

##### 基本关系定义

app/Model/User.php

```php
//定义一对一
public function userAddress(): HasMany {
  return $this->hasMany(UserAddress::class, 'user_id', 'id');
}
```

调用

```php
$user = User::query()->find(1)->userAddress;
```

类似的还有 hasOne/belongsTo/belongsToMany 具体要用到在[查看文档](https://hyperf.wiki/3.0/#/zh-cn/db/relationship?id=%e5%a4%9a%e5%af%b9%e5%a4%9a)即可

##### 预加载

比如要查所有用户的所有地址 不使用左连接查询,需要查 1 \* n 次，可以简化为 2 次

```php
$users = User::query()->with('userAddress')->get();
```

生成的 sql 类似

```sql
SELECT * FROM `user`;
SELECT * FROM `user_address` WHERE id in (1, 2, 3, ...);
```

#### 模型事件

#### 模型增删改查钩子函数

在模型里可以添加各种[钩子函数](https://hyperf.wiki/3.0/#/zh-cn/db/event?id=%e9%92%a9%e5%ad%90%e5%87%bd%e6%95%b0) 在新增修改删除等操作前后进行扩展方法

#### 模型增删改查监听

本来想用他来实现操作日志，发现批量新增修改不监听 以及原生语句不监听等，所以操作日志还是用一个方法去记录比较好

### 资源构造器

这玩意除非你的响应里只要 data 不需要状态码和 message 否则还是使用之前的返回方法

#### 安装扩展

```shell
composer require hyperf/resource
```

#### 生成单个实体类

```shell
php bin/hyperf.php gen:resource User
```

#### 自定义返回数据

app/Resource/User.php 修改生成的 User 类的 toArray 方法

```php
  public function toArray(): array {
    return [
      'data' => [
        'id' => $this->id,
        'name' => $this->name,
        'age' => $this->age,
      ],
    ];
    //    return parent::toArray();
  }
```

##### 单个对象

然后使用需要使用 toResponse 返回数据 这里只能返回单条记录

```php
$user = User::query()
  ->select('*')
  ->first();

return (new \App\Resource\User($user))->toResponse();
```

##### 数组

返回数组使用 collection 方法 也可以使用上面的方法 注意：两种方法都不能自定义返回数据

```php
$users = User::query()
    ->select('*')
    ->get();

return (new \App\Resource\User($users))->toResponse();
return \App\Resource\User::collection($users)->toResponse();
```

#### 生成复杂实体类

```shell
#php bin/hyperf.php gen:resource Users --collection
php bin/hyperf.php gen:resource UserCollection
```

##### 数组

app/Resource/UserCollection.php 修改生成的 UserCollection 类的 toArray 方法

注意这里$this->collection 会映射单个 User 实体类 所以单个 User 实体类不能添加额外的属性

```php
public function toArray(): array {
  return [
    'data' => $this->collection,
  ];
  //    return parent::toArray();
}
```

使用

```php
$pageSize = 10;
//返回分页对象
$users = User::query()->paginate($pageSize);
return (new UserCollection($users))->toResponse();
```

## 客户端

### redis

#### 配置

config/autoload/redis.php

```php
return [
  'default' => [
    'host' => env('REDIS_HOST', '119.45.19.125'),
    'auth' => env('REDIS_AUTH', "password"),
    'port' => (int) env('REDIS_PORT', 6379),
    'db' => (int) env('REDIS_DB', 0),
    'pool' => [
      'min_connections' => 1,
      'max_connections' => 10,
      'connect_timeout' => 10.0,
      'wait_timeout' => 3.0,
      'heartbeat' => -1,
      'max_idle_time' => (float) env('REDIS_MAX_IDLE_TIME', 60),
    ],
  ],
  //Pool 的 key 值
  'custom'=>[
      'db' => (int) env('REDIS_DB', 1),
  ]
  ...可以配置多个redis db redis有16个db
];
```

#### 切换 redisdb 库

##### 使用 Redis 代理类切换

自定义一个 Redis 类通过继承 Redis 类，重写 poolName 属性。完成对 Redis db 的切换

```php
<?php
use Hyperf\Redis\Redis;

class CustomRedis extends Redis {
    // 对应的 Pool 的 key 值
    protected $poolName = 'custom';
}

// 通过 DI 容器获取或直接注入当前类
$redis = $this->container->get(CustomRedis::class);

$result = $redis->keys('*');
```

##### 使用工厂类切换

```php
  private Redis $redis;

  public function __construct(RedisFactory $redisFactory) {
    $this->redis = $redisFactory->get('custom');
    parent::__construct();
  }
```

或者 直接从容器里拿

```php
$result = $this->container->get(RedisFactory::class)
    ->get('custom')
    ->getDbNum();
```

#### 示例

存储用户 token 10 秒过期

```php
   $result = $this->container->get(RedisFactory::class)
      ->get('default')
      ->set("token:user-id", "123456", 10);
```

## 消息队列

### redis 异步队列

```shell
composer require hyperf/async-queue
```

提供异步延时处理,原理就是启动消费进程去监听消息队列，然后把消息主动投递到队列里，消费进程去执行 job 任务，消费这个消息

#### redis 队列驱动配置

创建 config/autoload/async_queue.php

```php
<?php

return [
  'default' => [
    'driver' => Hyperf\AsyncQueue\Driver\RedisDriver::class,
    'redis' => [
      'pool' => 'default',//redis 连接池
    ],
    'channel' => 'queue',//队列前缀
    'timeout' => 2,//pop 消息的超时时间
    'retry_seconds' => [1, 5, 10, 20],//失败后重新尝试间隔
    'handle_timeout' => 10,//消息处理超时时间
    'processes' => 1,//消费进程数
    'concurrent' => [
      'limit' => 5,//同时处理消息数
    ],
  ],
];
```

#### 消费进程配置

配置一个类到进程文件或再消费进程类上写上 async_queue 的注解,来表示该类是消费进程类

config/autoload/processes.php

```php
<?php

declare(strict_types=1);

return [
  Hyperf\AsyncQueue\Process\ConsumerProcess::class,
];
```

或创建一个消费进程类 app/Process/AsyncQueueConsumer.php

```php
<?php

declare(strict_types=1);

namespace App\Process;

use Hyperf\AsyncQueue\Process\ConsumerProcess;
use Hyperf\Process\Annotation\Process;

#[Process(name: "async-queue")]
class AsyncQueueConsumer extends ConsumerProcess {

}
```

#### 生产消息

就是消息投递

##### 传统方式

直接序列化对象 存入 redis 队列 投递的是延时的消息 存储的数据结构是 zset

如果对象的属性值完全一致，则前面的会覆盖后面的,会重新计时延迟的消费时间，唯一，如果想要不覆盖,再参数里添加一个 uniqid 即可 用"uniqId" => uniqid()

新建一个任务类 app/Job/ExampleJob.php

```php
<?php

declare(strict_types=1);

namespace App\Job;

use Hyperf\AsyncQueue\Job;

class ExampleJob extends Job {

  //成员变量为消费的数据
  public $params;

  //任务执行失败后的重试次数，即最大执行次数为 $maxAttempts+1 次
  protected int $maxAttempts = 2;

  public function __construct($params) {
    // 这里最好是普通数据，不要使用携带 IO 的对象，比如 PDO 对象
    $this->params = $params;
  }

  //这个就是消费的逻辑
  public function handle() {
    // 根据参数处理具体逻辑
    // 通过具体参数获取模型等
    // 这里的逻辑会在 ConsumerProcess 进程中执行
    var_dump($this->params);
  }

}

```

投递消息的 Service

```php
<?php

declare(strict_types=1);

namespace App\Service;

use App\Job\ExampleJob;
use Hyperf\AsyncQueue\Driver\DriverFactory;
use Hyperf\AsyncQueue\Driver\DriverInterface;

class QueueService {

  protected DriverInterface $driver;

  public function __construct(DriverFactory $driverFactory) {
    $this->driver = $driverFactory->get('default');
  }

  /**
   * 生产消息.
   *
   * @param $params string|array|object 数据
   * @param int $delay 延时时间 单位秒
   */
  public function push($params, int $delay = 0): bool {
    // 这里的 `ExampleJob` 会被序列化存到 Redis 中，所以内部变量最好只传入普通数据
    // 同理，如果内部使用了注解 @Value 会把对应对象一起序列化，导致消息体变大。
    // 所以这里也不推荐使用 `make` 方法来创建 `Job` 对象。
    return $this->driver->push(new ExampleJob($params), $delay);
  }

}
```

接下去投递消息

```php
  #[Inject]
  protected QueueService $service;

  $this->service->push([
    "name" => "下单",
    "userId" => 1,
  ], 5);
  $this->service->push([
    "name" => "下单",
    "userId" => 2,
  ], 10);
  $this->service->push([
    "name" => "下单",
    "userId" => 3,
  ], 10);
  $this->service->push([
    "name" => "下单",
    "userId" => 4,
  ], 15);
```

显示再 redis 中就是 key 为 queue:delayed 的 zset 数据结构，每个数据是一个,会根据设定的延时时间，逐个执行

```
O:28:"Hyperf\AsyncQueue\JobMessage":2:{i:0;O:18:"App\Job\ExampleJob":2:{s:6:"params";a:2:{s:4:"name";s:6:"下单";s:6:"userId";i:4;}s:14:"*maxAttempts";i:2;}i:1;i:0;}
```

##### 注解方式

这种方式是即使消费，不能设置延时时间，且可以重复进入队列，数据结构是 list,只要写一个投递消费的类即可，直接在这个类里面执行任务

app/Service/QueueServiceAnnotation.php

```php
<?php

declare(strict_types=1);

namespace App\Service;

use Hyperf\AsyncQueue\Annotation\AsyncQueueMessage;

class QueueServiceAnnotation {

  #[AsyncQueueMessage]
  public function push($params) {
    // 需要异步执行的代码逻辑
    // 这里的逻辑会在 ConsumerProcess 进程中执行
    var_dump($params["userId"]);
  }

}
```

调用他投递消息并消费消息

```php
  #[Inject]
  protected QueueServiceAnnotation $service;

  for ($i = 0; $i < 1000; $i++) {
    go(function () use ($i) {
      $this->service->push([
        "name" => "下单",
        "userId" => $i,
        "uniqId" => uniqid(),
      ]);
    });
  }
```

或者去掉循环使用 ab 测试 ab -n 1000 -c 1000 http://127.0.0.1:9501/index/requestFn

#### 超时消息重启时自动消费

保证消息是幂等的才能开启 就是这个消息执行一次或执行多次的结果是一样的 配置 listener config/autoload/listeners.php

```php
Hyperf\AsyncQueue\Listener\ReloadChannelListener::class
```

## 定时任务

定时发送优惠券，定时修改商品价格，定时删除日志，定时备份数据等等

### 安装

```shell
composer require hyperf/crontab
```

### 配置

config/autoload/processes.php 添加定时任务进程处理类

```php
Hyperf\Crontab\Process\CrontabDispatcherProcess::class
```

创建 config/autoload/crontab.php 并开启定时任务

```php
<?php
return [
  // 是否开启定时任务
  'enable' => TRUE,
];
```

### 创建定时任务

```php
<?php

namespace App\Crontab;

use Carbon\Carbon;
use Hyperf\Crontab\Annotation\Crontab;

#[Crontab(rule: "*\/5 * * * * *", name: "CustomCrontab", callback: "execute", memo: "描述", enable: TRUE)]
class CustomCrontab {

  public function execute() {
    var_dump(Carbon::now()->toDateTimeString());
  }

}
```

# CodeIgniter4

## 创建项目

```shell
composer create-project codeigniter4/appstarter project-root
```

### 运行

PHP>7.2 开启扩展

```ini
extension=intl
extension=curl
```

配置 url app/Config/App.php

```php
public string $baseURL = 'http://www.ci4.com/';
```

配置自动路由 app/Config/Routes.php

```php
$routes->setAutoRoute(TRUE);
```

最后 nginx 配置 public 为 root 目录 重写.php,pathinfo 等,最后即可访问

## 自动加载

如果不配置自动加载，则引入第三方类或者是自定义类时需要 require_once

配置自动加载 /app/Config/Autoload.php 配置命名空间映射对应的所在类的目录

```php
public $psr4
    = [
        APP_NAMESPACE => APPPATH, // For custom app namespace
        'Config'      => APPPATH . 'Config',
        'Libraries'   => APPPATH . 'Libraries',
        'ThirdParty'  => APPPATH . 'ThirdParty',
    ];
```

例如 Libraries 下有一个 Library，接下去就能在 controller 或者是 model 中正常使用该类了

```php
<?php

namespace Libraries;

class Library {

    public function fn() {
        echo "Test";
    }

}
```

## 辅助函数

创建 /app/Helpers/test_helper.php

```php
<?php

function helperFn() {
    echo 333;
}
```

使用

```php
helper('test');
helperFn();
```

或者在控制器中定义属性

```php
protected $helpers = ['test'];
```

## 请求和响应

### 请求

```php
<?php

namespace App\Controllers;

use CodeIgniter\HTTP\RequestTrait;
use CodeIgniter\HTTP\ResponseInterface;
use Config\Services;

class Home extends BaseController {

    use RequestTrait;

    public function index(): ResponseInterface {
        var_dump($this->request->getPost());//表单请求
        var_dump($this->request->getGet());//表单请求
        var_dump($this->request->getPostGet());//表单请求
        var_dump($this->request->getJSON(TRUE));//*重点 TRUE 则是转PHP数组 否则是std对象
        var_dump($this->request->getVar("name"));//JSON或者表单都可以
        var_dump($this->request->getRawInput());
        var_dump($this->request->getFile("fileName"));
        var_dump(Services::router()->controllerName() . "\\" . Services::router()->methodName());
        var_dump($this->getIPAddress());
        var_dump($this->getServer("HTTP_TOKEN"));
        var_dump($this->fetchGlobal("get"));//表单请求
        var_dump($this->fetchGlobal("post"));//表单请求
        var_dump($this->fetchGlobal("request"));//表单请求
        return $this->respond(["hello" => "world"], 200, 'success');
    }

}
```

### 响应

返回 json

```php
<?php

namespace App\Controllers;

use CodeIgniter\API\ResponseTrait;
use CodeIgniter\HTTP\ResponseInterface;

class Home extends BaseController {

    use ResponseTrait;

    public function index(): ResponseInterface {
        $this->response->setStatusCode(200)
            ->setBody(json_encode(['hello' => 'test1'], TRUE))
            ->setContentType('application/json')
            ->send();
        $this->response->setJSON(['hello' => 'test2'])->send();
        return $this->respond(['hello' => 'test3'], 200, 'success');
    }

}
```
