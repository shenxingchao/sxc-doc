# Web 服务器

# apcahe

# nginx

## Nginx 开启 gzip 压缩

<p align="left" style="color:#777777;">发布日期：2019-04-01</p>

开启 gzip 压缩，可以大大减少请求文件的大小，加快网页的访问速度
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
那便是开启了，而且文件的 SIZE 和你上传的文件大小变小了很多

## Nginx 配置 php 可访问

<p align="left" style="color:#777777;">发布日期：2019-04-01</p>

- 配置包括**访问.php 文件**
- 支持**pathinfo 模式**
- 支持**url 重写隐藏 index.php**

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

## Nginx 配置 Https

<p align="left" style="color:#777777;">发布日期：2019-04-02</p>
现在很多网站都开启了https,而且例如开发微信小程序,也需要https，
那么记录一下https nginx的配置方式

首先，博主用的是腾讯云的服务器，去官网申请一个域名的 ssl 证书
几分钟就好了

!>例如 www.domain.com 和 test.domain.com 算是不同的单域名
所以要都要加上 ssl 证书的话，得申请 2 个

在下面这个网址 购买域名免费版 即可 1 年到期了重写弄一个就行了  
https://buy.cloud.tencent.com/ssl

证书申请完后，下载下来，我们需要用到的 2 个文件

1. /www.domain.com/nginx/*.bundle.crt
2. /www.domain.com/nginx/*.key

利用各种工具，将 2 个文件上传至 nginx/conf 目录下  
博主这里上传到了 nginx/conf/sss/www 文件夹下

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

!>配置完后 需要打开防火墙 443 端口，切记，否则配置无效

- 永久开放 443 端口  
   firewall-cmd --zone=public --add-port=443/tcp --permanent
- 重启防火墙  
   firewall-cmd --reload

- 重新启动 nginx 服务  
   停止 systemctl stop nginx.service  
   启动 systemctl start nginx.service

!>这里需要 stop 之后重新 start 也是需要注意的

## Nginx 配置反向代理服务器

爬虫的时候很有用  
例如我想爬一个www.a.com的网站 那么我可以这么设置  
nginx 配置文件

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

host 文件

```
127.0.0.1 www.b.com
```

那么我通过请求 http://www.b.com:80 ，就能访问到 https://www.a.com ，而且对方服务器只能抓取到你的代理服务器 ip 127.0.0.1 不能获取到你的真实 ip。
相当于你的 nginx 变成了代理服务器 这个时候请求的时候不用带上代理，直接请求你自己的代理服务器就完事了

## Nginx 开启跨域

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

## 状态码

```
1xx （信息型状态码）- 服务器已接受请求，但需要更多的信息来完成请求。

100 Continue：请求者应当继续发送请求，服务器最终将返回响应。
101 Switching Protocols：请求者已要求服务器切换协议，服务器已确认并准备切换。
2xx（成功状态码）- 服务器成功处理了请求。

200 OK：请求已成功，请求所希望的响应头或数据体将随此响应返回。
201 Created：请求成功并且服务器创建了新资源。
204 No Content：请求成功但不需要返回任何数据。
3xx（重定向状态码）- 需要客户端执行进一步的操作才能完成请求。

301 Moved Permanently：所请求的资源已被永久移动到新位置。
302 Found：所请求的资源在新位置中临时找到。
307 Temporary Redirect：重定向应该在用户的浏览器中发生，用户看到这个状态码时，浏览器应该自动转到新的地址。
4xx（客户端错误状态码）- 请求包含错误语法或无法完成请求。

400 Bad Request：表示请求报文中存在语法错误。
401 Unauthorized：表示发送的请求需要身份验证。该状态码表示请求者需要验证自己才能得到请求的回应。
403 Forbidden：表示请求被拒绝，服务器没有执行请求，权限不足等原因。
5xx（服务器错误状态码）- 服务器在处理请求时发生错误。

500 Internal Server Error：表示服务器在执行请求时遇到了错误。
503 Service Unavailable：表示服务无法完成请求，一般是因为服务器过于繁忙或停机维护等原因导致。
```
