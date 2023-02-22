# Web服务器
# apcahe
# nginx
## Nginx开启gzip压缩
<p align="left" style="color:#777777;">发布日期：2019-04-01</p>

开启gzip压缩，可以大大减少请求文件的大小，加快网页的访问速度
配置如下：
```nginx
server {
	......

    # 开启gzip
    gzip on;
    # 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
    gzip_min_length 1k;
    # gzip 压缩级别，1-10，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
    gzip_comp_level 2;
    # 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到。
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
    # 是否在http header中添加Vary: Accept-Encoding，建议开启
    gzip_vary on;
    # 禁用IE 6 gzip
    gzip_disable "MSIE [1-6]\.";
}
```
查看是否开启，
看浏览器->F12->network –>response headers 
Content-Encoding: gzip
那便是开启了，而且文件的SIZE和你上传的文件大小变小了很多

## Nginx配置php可访问
<p align="left" style="color:#777777;">发布日期：2019-04-01</p>

- 配置包括__访问.php文件__  
- 支持__pathinfo模式__  
- 支持__url重写隐藏index.php__  

以下是配置文件
```nginx
server {
    listen       80;
    server_name    domain;
    root   /usr/local/nginx/html;

    #以下是上传文件
    client_max_body_size 2m;
    location / {
        index  index.php index.html index.htm;

        #以下是url重写
        if (!-e $request_filename) {
            rewrite  ^(.*)$  /index.php?s=/$1  last;
            break;
        }
    }

    #以下是支持php访问
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
}
```

## Nginx配置Https
<p align="left" style="color:#777777;">发布日期：2019-04-02</p>
现在很多网站都开启了https,而且例如开发微信小程序,也需要https，
那么记录一下https nginx的配置方式  

首先，博主用的是腾讯云的服务器，去官网申请一个域名的ssl证书
几分钟就好了  

!>例如 www.domain.com 和 test.domain.com 算是不同的单域名
所以要都要加上ssl证书的话，得申请2个  

在下面这个网址 购买域名免费版 即可 1年到期了重写弄一个就行了  
https://buy.cloud.tencent.com/ssl  

证书申请完后，下载下来，我们需要用到的2个文件  
1. /www.domain.com/nginx/*.bundle.crt  
2. /www.domain.com/nginx/*.key  

利用各种工具，将2个文件上传至nginx/conf目录下  
博主这里上传到了nginx/conf/sss/www文件夹下  

- 下面上配置文件  
    ```
    server {
        listen       80;
        server_name   www.domain.com domain.com;
        rewrite ^(.*)$ https://$host$1 permanent; #把http的域名请求转成https
    }
    server {
        listen       443;
        server_name    www. domain.com domain.com; #填写绑定证书的域名
        root   /usr/local/nginx/html;
        ssl on;
        ssl_certificate                   ssl/www/*.bundle.crt;
        ssl_certificate_key                   ssl/www/*.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;#按照个套件配置

        ssl_prefer_server_ciphers on;

        location / {
            index index.html; 
        }
    }
    ```

!>配置完后 需要打开防火墙443端口，切记，否则配置无效

- 永久开放443端口  
    firewall-cmd --zone=public --add-port=443/tcp --permanent  
- 重启防火墙  
    firewall-cmd --reload  

- 重新启动nginx服务  
    停止systemctl stop nginx.service  
    启动systemctl start nginx.service  

!>这里需要stop之后重新start 也是需要注意的

## Nginx配置反向代理服务器
爬虫的时候很有用  
例如我想爬一个www.a.com的网站 那么我可以这么设置  
nginx配置文件  
```ini
server{
    listen       80;

    server_name  www.b.com;

    root   D:\wnmp\nginx-1.12.2\html; # 测试能否访问用的 实际可去掉
    
    location / {
        proxy_pass https://www.a.com;
        
        # root html;# 测试能否访问用的 实际可去掉

        # index  index.html index.htm;# 测试能否访问用的 实际可去掉

    }
}
```
host文件  
```
127.0.0.1 www.b.com
```
那么我通过请求 http://www.b.com:80 ，就能访问到 https://www.a.com ，而且对方服务器只能抓取到你的代理服务器ip 127.0.0.1 不能获取到你的真实ip。
相当于你的nginx变成了代理服务器 这个时候请求的时候不用带上代理，直接请求你自己的代理服务器就完事了

## Nginx开启跨域

```ini
location / {
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
    add_header Access-Control-Allow-Headers 'Origin, X-Requested-With, Content-Type, Accept,  X-Token';
    add_header Access-Control-Expose-Headers 'Token,Code';
    add_header Access-Control-Max-Age 3600;

    if ($request_method = 'OPTIONS') {
        return 204;
    }
}
```