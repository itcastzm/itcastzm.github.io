# nginx 

 Nginx是一款轻量级的网页服务器、反向代理服务器。相较于Apache、lighttpd具有占有内存少，稳定性高等优势。
 **它最常的用途是提供反向代理服务。**
 - 负载均衡 -

## centos上nginx安装

在Centos下，yum源不提供nginx的安装，可以通过切换yum源的方法获取安装。也可以通过直接下载安装包的方法，
**以下命令均需root权限执行**：
首先安装必要的库（nginx 中gzip模块需要 zlib 库，rewrite模块需要 pcre 库，ssl 功能需要openssl库）。
选定**/usr/local**为安装目录，以下具体版本号根据实际改变。

Nginx 是C语言开发的, 所以要安装gcc编译器。
安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境，如果没有 gcc 环境，则需要安装：

``` shell
    yum install -y gcc-c++
```

PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。

``` shell
yum install -y pcre pcre-devel
```

```shell
cd /usr/local/
wget http://jaist.dl.sourceforge.net/project/pcre/pcre/8.33/pcre-8.33.tar.gz
tar -zxvf pcre-8.33.tar.gz
cd pcre-8.33
./configure
make && make install

```

3.zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库。

``` shell
yum install -y zlib zlib-devel
```

```shell
cd /usr/local/
wget http://zlib.net/zlib-1.2.11.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make && make install

```

4. OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。

``` shell
yum install -y openssl openssl-devel
```

```shell
cd /usr/local/
wget https://www.openssl.org/source/old/1.0.1/openssl-1.0.1j.tar.gz
tar -zxvf openssl-1.0.1j.tar.gz
cd openssl-1.0.1j
./config
make && make install
```

下载nginx 到一个自己创建到文件夹中 随意

下载 `wget https://nginx.org/download/nginx-1.13.0.tar.gz`
解压 `tar -zxvf nginx-1.10.1.tar.gz进入 cd nginx-1.10.1`
配置 
 `./configure`
编译 
 `make`
安装 
 `make install`
一次完成也可以 ` ./configure --prefix=/opt/software/nginx && make install`
配置环境变量：（注意是sbin不是bin）

```shell
 cd /usr/local/
 wget http://nginx.org/download/nginx-1.8.0.tar.gz
 tar -zxvf nginx-1.8.0.tar.gz
 cd nginx-1.8.0 
 ./configure --user=nobody --group=nobody --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_gzip_static_module --with-http_realip_module --with-http_sub_module --with-http_ssl_module --with-pcre=/usr/local/pcre-8.33 --with-zlib=/usr/local/zlib-1.2.11
(注: --with-http_ssl_module:这个不加后面在nginx.conf配置ssl:on后,启动会报nginx: [emerg] unknown directive "ssl" in /opt/nginx/conf/nginx.conf 异常)
 make && make install
```

 ` echo 'export PATH=$PATH:/opt/software/nginx/sbin' > /etc/profile.d/nginx.sh`
环境变量生效 `source /etc/profile` 查看安装路径 ` whereis nginx`
前提进入安装路径里面 sbin 目录下 ` cd /opt/software/nginx/sbin/ ` 启动 `./nginx ` 查看进程 ` ps aux|grep nginx` 重启 先停止再启动（推荐）：

``` shell
./nginx -s quit ./nginx
```

有很多时候只改配置 不用重启了就 当 ngin x的配置文件 nginx.conf 修改后，要想让配置生效需要重启 nginx，即可将配置信息在 nginx 中生效，如下：

``` shell
./nginx -s reload
```

默认端口为80 不用加 直接外部IP 访问 就能访问到页面

## nginx操作命令

启动nginx服务
```shell
/usr/local/nginx/sbin/nginx
```

重启nginx服务

```shell
/usr/local/nginx/sbin/nginx –s reload
```

停止nginx服务

```shell
/usr/local/nginx/sbin/nginx –s stop
```

强制关闭nginx服务

```shell
pkill nginx
```

 nginx正反向代理基本配置

 ```shell
 #nginx配置
#user  nobody;
worker_processes  1;    #服务器并发处理服务关键配置

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;   #最大连接数为 1024.
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    tcp_nopush     on;
    keepalive_timeout  65;

    #gzip  on;  #http头压缩
    
    #正向代理配置
    server {    
        listen       8080;  # 代理监听端口
        resolver 223.5.5.5; #代理DNS配置
        
        #charset koi8-r;

        access_log  /home/lich/logs/fproxy.access.log;  #accesslog输出路径
        error_log /home/lich/logs/fproxy.error.log;     #errorlog输出路径
        
        location / {
           
            proxy_pass $scheme://$host$request_uri;     # 配置正向代理参数
            proxy_set_header Host $http_host;           # 解决如果URL中带"."后Nginx 503错误

            proxy_buffers 256 4k;   # 配置缓存大小
            proxy_max_temp_file_size 0;     # 关闭磁盘缓存读写减少I/O
            proxy_connect_timeout 30;       # 代理连接超时时间

            # 配置代理服务器HTTP状态缓存时间
            proxy_cache_valid 200 302 10m;
            proxy_cache_valid 301 1h;
            proxy_cache_valid any 1m;
        }
    }

    
    #反向代理配置
    server {
        listen       80;
        server_name  test.fw.com;   #代理转发域名配置

        access_log  /home/lich/logs/rproxy.access.log;
        error_log /home/lich/logs/rproxy.error.log;

        location / {
            proxy_pass http://10.60.221.22:8001;    #代理到后段实际应用服务器地址
            index  index.html index.htm index.jsp;
        }

        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}

 ```

## nginx配置参数介绍

listen

配置监听的ip地址

```shell
listen address[:port] [default_server][setfib=number][backlog=number][rcvbuf=size][sndbuf=size][deferred][accept_filter=filter][bind][ssl];
```

配置监听端口

```shell
listen port[default_server] [setfib=number][backlog=number][rcvbuf=size][sndbuf=size][accept_filter=filter][deferred][bind][ipv6only=on|off][ssl];
```

配置Unix Domain Socket

```shell
listen unix:path [default_server][backlog=number][rcvbuf=size][sndbuf=size][accept_filter=filter][deferred][bind][ssl];
```

监听配置用法

```shell
listen *:80 | *:8080        #监听所有80端口和8080端口
listen  IP_address:port     #监听指定的地址和端口号
listen  IP_address          #监听指定ip地址所有端口
listen port                 #监听该端口的所有IP连接
```

 参数解释

 ```shell
 - address:IP地址，如果是 IPV6地址，需要使用中括号[] 括起来，比如[fe80::1]等。
- port:端口号，如果只定义了IP地址，没有定义端口号，那么就使用80端口。
- path:socket文件路径，如 var/run/nginx.sock等。
- default_server:标识符，将此虚拟主机设置为 address:port 的默认主机。（在 nginx-0.8.21之前使用的是default指令）
- setfib=number:Nginx-0.8.44 中使用这个变量监听 socket 关联路由表，目前只对 FreeBSD 起作用，不常用。
- backlog=number:设置监听函数listen()最多允许多少网络连接同时处于挂起状态，在 FreeBSD 中默认为 -1,其他平台默认为511.
- rcvbuf=size:设置监听socket接收缓存区大小。
- sndbuf=size:设置监听socket发送缓存区大小。
- deferred:标识符，将accept()设置为Deferred模式。
- accept_filter=filter:设置监听端口对所有请求进行过滤，被过滤的内容不能被接收和处理，本指令只在 FreeBSD 和 NetBSD 5.0+ 平台下有效。filter 可以设置为 dataready 或 httpready 。
- bind:标识符，使用独立的bind() 处理此address:port，一般情况下，对于端口相同而IP地址不同的多个连接，Nginx 服务器将只使用一个监听指令，并使用 bind() 处理端口相同的所有连接。
- ssl:标识符，设置会话连接使用 SSL模式进行，此标识符和Nginx服务器提供的 HTTPS 服务有关。
 ```

 server_name
该指令用于虚拟主机的配置。通常分为以下两种

基于名称的虚拟主机配置

```shell
- 语法格式如下：
# server_name   name ...;

- 对于name 来说，可以只有一个名称，也可以有多个名称，中间用空格隔开。而每个名字由两段或者三段组成，每段之间用“.”隔开。
# server_name 123.com www.123.com

- 可以使用通配符“*”，但通配符只能用在由三段字符组成的首段或者尾端，或者由两端字符组成的尾端。
# server_name *.123.com www.123.*

- 还可以使用正则表达式，用“~”作为正则表达式字符串的开始标记。
# server_name ~^www\d+\.123\.com$;

#该表达式“~”表示匹配正则表达式，以www开头（“^”表示开头），紧跟着一个0~9之间的数字，在紧跟“.123.co”，最后跟着“m”($表示结尾)以上匹配的顺序优先级如下：
①、准确匹配 server_name
②、通配符在开始时匹配 server_name 成功
③、通配符在结尾时匹配 server_name 成功
④、正则表达式匹配 server_name 成功
```

基于IP地址的虚拟主机配置

```shell
#语法结构和基于域名匹配一样，而且不需要考虑通配符和正则表达式的问题。
server_name 192.168.1.1
```

location
- 该指令用于匹配 URL

```shell
location [ = | ~ | ~* | ^~] uri {
}

= ：用于不含正则表达式的 uri 前，要求请求字符串与 uri 严格匹配，如果匹配成功，就停止继续向下搜索并立即处理该请求。
~ ：用于表示 uri 包含正则表达式，并且区分大小写。
~* ：用于表示 uri 包含正则表达式，并且不区分大小写。
^~ ：用于不含正则表达式的 uri 前，要求 Nginx 服务器找到标识 uri 和请求字符串匹配度最高的 location 后，立即使用此 location 处理请求，而不再使用 location 块中的正则 uri 和请求字符串做匹配。
注意：如果 uri 包含正则表达式，则必须要有 ~ 或者 ~* 标识。

```

proxy_pass
- 该指令用于设置被代理服务器的地址。可以是主机名称、IP地址加端口号的形式

```shell
proxy_pass URL;

# URL 为被代理服务器的地址，可以包含传输协议、主机名称或IP地址加端口号，URI等。
proxy_pass  http://www.123.com/uri;
```

index

- 该指令用于设置网站的默认首页。

```shell
index  filename ...;

# 后面的文件名称可以有多个，中间用空格隔开。
index  index.html index.jsp;

ps：通常该指令有两个作用：第一个是用户在请求访问网站时，请求地址可以不写首页名称；第二个是可以对一个请求，根据请求内容而设置不同的首页。
```

ngxin负载均衡

1 轮询算法负载均衡

```shell
upstream OrdinaryPolling {
    server 192.168.1.100:8081;
    server 192.168.1.100:8082;
}
server {
listen       80; 
server_name  test.fw.com;

access_log  /home/lich/logs/rproxy_slb.access.log;
error_log /home/lich/logs/rproxy_slb.error.log;

location / {
             proxy_pass http://OrdinaryPolling;
             index  index.html index.htm index.jsp;
             # deny ip
             # allow ip
         
        }
}

```

基于比例加权轮询负载均衡

```shell
upstream OrdinaryPolling {
    server 10.60.220.60:8081 weight=2;
    server 10.60.220.60:8082 weight=5;
}
server {
listen       80; 
server_name  test.fw.com;

access_log  /home/lich/logs/rproxy_slb.access.log;
error_log /home/lich/logs/rproxy_slb.error.log;


location / {
             proxy_pass http://OrdinaryPolling;
             # index  index.html index.htm index.jsp;
             # deny ip
             # allow ip
         
        }
}

```

基于IP路由负载均衡

场景解释：我们知道一个请求在经过一个服务器处理时，服务器会保存相关的会话信息，比如session，但是该请求如果第一个服务器没处理完，通过nginx轮询到第二个服务器上，那么这个服务器是没有会话信息的。

　　最典型的一个例子：用户第一次进入一个系统是需要进行登录身份验证的，首先将请求跳转到web1应用服务器进行处理，登录信息是保存在web1应用服务器上的，这时候需要进行别的操作，那么可能会将请求轮询到web2应用服务器上，那么由于 web2没有保存会话信息，web2服务器会以为该用户没有登录，然后继续登录一次，如果有多个服务器，每次第一次访问都要进行登录，这显然是很影响用户体验的。


这里产生的一个问题也就是集群环境下的 session 共享，如何解决这个问题？

　　通常由两种方法：

　　1、第一种方法是选择一个中间件，将登录信息保存在一个中间件上，这个中间件可以为 Redis 这样的数据库。那么第一次登录，我们将session 信息保存在 Redis 中，跳转到第二个服务器时，我们可以先去Redis上查询是否有登录信息，如果有，就能直接进行登录之后的操作了，而不用进行重复登录。

　　2、第二种方法是根据客户端的IP地址划分，每次都将同一个 IP 地址发送的请求都分发到同一个 Tomcat 服务器，那么也不会存在 session 共享的问题。

而 nginx 的基于 IP 路由负载的机制就是上诉第二种形式。大概配置如下：

```shell
upstream OrdinaryPolling {
    server 10.60.220.60:8081 weight=2;
    server 10.60.220.60:8082 weight=5;
    ip_hash;
}
server {
listen       80; 
server_name  test.fw.com;

access_log  /home/lich/logs/rproxy_slb.access.log;
error_log /home/lich/logs/rproxy_slb.error.log;


location / {
             proxy_pass http://OrdinaryPolling;
             # index  index.html index.htm index.jsp;
             # deny ip
             # allow ip
         
        }
}
```

注意：我们在 upstream 指令块中增加了 ip_hash 指令。该指令就是告诉 nginx 服务器，同一个 IP 地址客户端发送的请求都将分发到同一个 Tomcat 服务器进行处理。

基于服务器响应时间负载均衡

根据服务器处理请求的时间来进行负载，处理请求越快，也就是响应时间越短的优先分配。

```shell
upstream OrdinaryPolling {
    server 10.60.220.60:8081 weight=2;
    server 10.60.220.60:8082 weight=5;
    fair;
}
server {
listen       80; 
server_name  test.fw.com;

access_log  /home/lich/logs/rproxy_slb.access.log;
error_log /home/lich/logs/rproxy_slb.error.log;


location / {
             proxy_pass http://OrdinaryPolling;
             # index  index.html index.htm index.jsp;
             # deny ip
             # allow ip
         
        }
}
```