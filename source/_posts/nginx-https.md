---
title: nginx_https
date: 2018-03-06 10:37:52
tags: NGINX
---

nginx关于https的配置

<!-- more -->

首发于:
勘误地址:https://github.com/icngor/icngor.github.io/issues/1

## 1.获取证书

### 手动生成
http://www.cnblogs.com/yanghuahui/archive/2012/06/25/2561568.html

### 免费证书
* https://www.sslforfree.com/
* https://letsencrypt.org/

安装证书
* https://www.namecheap.com/support/knowledgebase/article.aspx/9419/0/nginx

> 合并证书文件时开头结尾必须换行

### 收费证书
收费证书由代理商负责生成证书文件,我们只需要提交域名及公司或个人信息.
不同Web服务器需要不同的证书文件,一般证书代理商会全部给到.
nginx服务器需要两个文件`*.crt,*.key`,

假设放在服务器的ssl_certs/目录

```
ssl_certs/www.abc.com.crt
ssl_certs/www.abc.com.key
```

## 2.Nginx配置
[ngx_http_ssl_module模块说明](http://nginx.org/en/docs/http/ngx_http_ssl_module.html)
```
server {
    # 443 端口设置为https监听的端口,80 端口仍作为http监听端口
    listen 80;
    listen 443 ssl; 
    ssl_certificate ssl_certs/www.abc.com.crt;
    ssl_certificate_key ssl_certs/www.abc.com.key;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDH:AESGCM:HIGH:!RC4:!DH:!MD5:!aNULL:!eNULL;
    ssl_prefer_server_ciphers  on;
    server_name www.abc.com;
    location / {
            root /www/;
    }
    access_log logs/www_abc_com.log;
}
```
### 80转发到443
```
server {
	listen 80;
	server_name ~^(.*).abc.com;
	set $domain $1;
	rewrite ^(.*) https://$domain.abc.com$1 permanent;
}
```
## 3.原理

### 证书服务机构

## 4.场景实战
client https -> nginx http -> tomcat

参考nginx代理的相关文章

## 5.相关工具
工具：https://csr.chinassl.net/free-ssl.html


## 6.参考列表
- [https原理](https://www.cnblogs.com/zhangshitong/p/6478721.html)
- [css,js,等请求同步转换成https](https://www.cnblogs.com/interdrp/p/4881785.html)
- [反向代理参数说明](https://www.cnblogs.com/zzzhfo/p/6039477.html)
- http://blog.csdn.net/zhanlanmg/article/details/49684803
- https://segmentfault.com/a/1190000007168673
