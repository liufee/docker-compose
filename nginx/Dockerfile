FROM centos:latest
MAINTAINER liufee job@feehi.com


#nginx版本
ARG NGINX_VER=1.12.0


#更换yum源
RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup && curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo


#安装基础工具
RUN yum install gcc-c++ make pcre-devel openssl-devel -y


#安装nginx
WORKDIR /usr/src
RUN curl -o nginx.tar.gz http://nginx.org/download/nginx-${NGINX_VER}.tar.gz && mkdir nginx && tar -zxvf nginx.tar.gz -C ./nginx --strip-components 1
WORKDIR nginx
RUN ./configure --prefix=/usr/local/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/lock/nginx.lock --user=nginx --group=nginx --with-http_ssl_module --with-http_flv_module --with-http_stub_status_module --with-http_gzip_static_module --http-client-body-temp-path=/tmp/nginx/client/ --http-proxy-temp-path=/tmp/nginx/proxy/ --http-fastcgi-temp-path=/tmp/nginx/fcgi/ --with-pcre --with-http_dav_module \
     && make && make install \
     && useradd nginx \
     && mkdir -p -m 777 /tmp/nginx \
     && sed -i "3a daemon off;" /etc/nginx/nginx.conf \
     && sed -i "s/index  index.html index.htm;/index  index.php index.html index.htm;/" /etc/nginx/nginx.conf \
     && sed -i "s/# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000/location ~ \.php\$ { \nfastcgi_pass php-fpm:9000;\nfastcgi_index  index.php;\nfastcgi_param  SCRIPT_FILENAME  \$document_root\$fastcgi_script_name;\ninclude fastcgi_params;\n }/" /etc/nginx/nginx.conf \
     && echo "<?php phpinfo()?>" > /usr/local/nginx/html/index.php


#服务器基础设置
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && echo 'Asia/Shanghai' > /etc/timezonesource \
     && source /etc/profile


EXPOSE 80 443


CMD  ["/usr/local/nginx/sbin/nginx"]