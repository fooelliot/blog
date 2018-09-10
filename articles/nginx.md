## nginx

#### # nginx的配置详解

* nginx的安装
$ yum install openssl openssl-devel 

$ yum install gcc-c++

$ yum install pcre-devel

$ yum install zlib

yum -y install gcc gcc-c++ make libtool zlib zlib-devel openssl openssl-devel pcre pcre-devel

$ wget http://nginx.org/download/nginx-1.8.0.tar.gz

$ tar -zxvf nginx-1.8.0.tar.gz

$ ./configure --prefix=/usr/local/nginx && make && make install

$ /usr/local/nginx/sbin/nginx -v

$ /usr/local/nginx/sbin/nginx -t

$ /usr/local/nginx/sbin/nginx     # 默认配置文件 /usr/local/nginx/conf/nginx.conf，-c 指定

$ /usr/local/nginx/sbin/nginx -s reload # 重新载入配置文件

$ /usr/local/nginx/sbin/nginx -s reopen #重启Nginx，不会改变启动时指定的配置文件

$ /usr/local/nginx/sbin/nginx -s stop #停止Nginx

 
安装ssl模块
./configure --prefix=/usr/local/nginx --with-http_stub_status_module  --with-http_ssl_module  


worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

#nginx工作模式配置

events {
    
    worker_connections  1024;   #单个后台worker process进程的最大并发链接数
    multi_accept        on;
    use                 epoll;    #epoll是多路复用IO(I/O Multiplexing)中的一种方式,但是仅用于linux2.6以上内核,可以大大提高nginx的性能          

}

#http设置
http {

    include           mime.types;
    default_type      application/octet-stream;

    sendfile           on;

    keepalive_timeout  65;

    access_log         logs/access.log; #存储访问记录的日志目录
    error_log          logs/error.log;  #存储记录错误发生的日志目录

    server {
        listen 80;
        server_name    *.xxx.com;
        return         301 https://$server_name$request_uri;
    }

    server {
        listen                  443 ssl;
        server_name             *.xxx.com;

        charset                 utf-8;
        ssl                     on;
        ssl_certificate         /xxx/yyy/fullchain.pem;
        ssl_certificate_key     /xxx/yyy/privkey.pem;

        location / {
            root       /xxx/xxx/;
            index      index.html;

            if ($host ~ ^(api)\.rllin\.cn$){
                  proxy_pass http://0.0.0.0:9901;
            }
            if ($host ~ ^(dev)\.rllin\.cn$){
                  proxy_pass http://0.0.0.0:9902;
            }
        }
        error_page   500 502 503 504  /50x.html;

        location = /50x.html {
            root   html;
        }

    }


    server {
      listen                443 ssl;
      charset               utf-8;

      server_name           xxx.com;
      root                  /xxx/xxx;

      ssl_certificate       "/xxx/full_chain.pem";
      ssl_certificate_key   "/xxx/private.key";
      ssl_protocols         TLSv1 TLSv1.1 TLSv1.2;
      ssl_prefer_server_ciphers on;


      location /api/ {
            proxy_pass http://0.0.0.0:9096;
      }

      location /ws/ {
        proxy_pass http://localhost:9096;
      }

      location /share/ {
        proxy_pass http://localhost:9096;
      }

      location /configuration/ {
        proxy_pass http://localhost:9096;
      }

      location /webjars/ {
        proxy_pass http://localhost:9096;
      }

      location /swagger {
        proxy_pass http://localhost:9096;
      }

      location = /v2/api-docs {
        proxy_pass http://localhost:9096;
        proxy_redirect     off;
        proxy_set_header   Host              $host;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
      }

      error_page 500 502 503 504 /50x.html;
        location = /50x.html {
      }

    include /usr/local/nginx/conf.d/*.xxx.conf;

}
