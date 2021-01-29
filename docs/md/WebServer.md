# Web服务器
## apcahe
## nginx
### Nginx开启gzip压缩
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

