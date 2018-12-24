Docker lnmp
=================
基于最新版CentOS官方镜像,docker-compose编排，源码编译安装mysql、php、nginx、java、nodejs


简介
------------------------
默认版本

- [x] php7.1.2

- [x] nginx1.12.2

- [x] mysql5.7.21 默认root密码为123456，可以在docker-compose中指定

- [x] java

- [x] node8.11.4

>可以在docker-compose.yml services指定版本

    nginx:
        build:
            args:
                - NGINX_VER=1.12.2
               
    mysql:
        build:
            args:
                - MYSQL_VER=5.7.21
                
    php:
        build:
            args:
                - PHP_VER=7.1.2
                
    node:
        build:
            args:
                - NODE_VER=8.11.4


快速开始
-------------------
```shell
1. git clone https://github.com/liufee/docker-compose.git (或者手动下载并解压)
2. cd /path/to/docker-compose
3. cp docker-compose.yml.example docker-compose.yml
4. docker-compose up (这里包括下载nginx、php、mysql源码，并编译安装，可能耗时间比较久)
```


目录简介
-------------------
1. /path/to/docker-compose/nginx/www 站点目录，包含一个default默认站点。有新站点时，在www中新建目录即可

2. /path/to/docker-compose/nginx/conf nginx配置文件。有新站点时，在conf/sites.d中新建vhost即可

3. /path/to/docker-compose/logs 日志文件。分别为mysql、nginx、php-fpm日志

4. /path/to/docker-compose/mysql/data ```数据库data目录```。注：如果此目录不为空，则不会重新初始化mysql。如果需要重新初始化mysql，清空data目录重启容器即可
