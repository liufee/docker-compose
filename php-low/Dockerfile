FROM centos:latest
MAINTAINER liufee job@feehi.com


#必须只能为php<7的低版本
ENV PHP_VER=5.4.0
ENV PORT=9054


#更换yum源
RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup && curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo


#编译环境
RUN yum install epel-release -y && yum update -y\
    && yum -y install wget pcre pcre-devel zlib zlib-devel openssl openssl-devel libxml2 libxml2-devel libjpeg libjpeg-devel libpng libpng-devel curl curl-devel libicu libicu-devel libmcrypt  libmcrypt-devel freetype freetype-devel libmcrypt libmcrypt-devel autoconf gcc-c++


#安装php
RUN yum install epel-release -y && yum update -y\
    && yum -y install pcre pcre-devel zlib zlib-devel openssl openssl-devel libxml2 libxml2-devel libjpeg libjpeg-devel libpng libpng-devel curl curl-devel libicu libicu-devel libmcrypt  libmcrypt-devel freetype freetype-devel libmcrypt libmcrypt-devel autoconf gcc-c++
WORKDIR /usr/src
RUN wget -O php.tar.gz "https://github.com/php/php-src/archive/php-${PHP_VER}.tar.gz" && mkdir php && tar -xzvf php.tar.gz -C ./php --strip-components 1
WORKDIR php
RUN mkdir -p /usr/local/php/etc && cp php.ini-development /usr/local/php/etc/php.ini
RUN wget http://ftp.gnu.org/gnu/bison/bison-2.4.1.tar.gz && tar -zxvf bison-2.4.1.tar.gz && cd bison-2.4.1 && ./configure && make && make install
RUN ./buildconf --force && ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-soap --enable-mbstring=all --enable-sockets --enable-fpm --with-gd --with-freetype-dir=/usr/include/freetype2/freetype --with-jpeg-dir=/usr/lib64 --with-zlib --with-iconv --enable-libxml --enable-xml  --enable-intl --enable-zip --enable-pcntl --enable-bcmath --enable-maintainer-zts --with-curl --with-mcrypt --with-openssl --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --disable-dom --disable-simplexml\
    && make && make install
WORKDIR /usr/local/php/etc
RUN cp php-fpm.conf.default php-fpm.conf \
    && sed -i "s/listen = 127.0.0.1:9000/listen = 0.0.0.0:${PORT}/" /usr/local/php/etc/php-fpm.conf \
    && sed -i "s/;daemonize = yes/daemonize = no/" /usr/local/php/etc/php-fpm.conf


#安装composer
RUN /usr/local/php/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && /usr/local/php/bin/php composer-setup.php  --install-dir=/usr/local/bin --filename=composer && rm -rf composer-setup.php


#服务器基础设置
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && echo 'Asia/Shanghai' > /etc/timezonesource \
     && source /etc/profile


EXPOSE $PORT


CMD  ["/usr/local/php/sbin/php-fpm"]
