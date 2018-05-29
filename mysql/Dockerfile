FROM centos:latest
MAINTAINER liufee job@feehi.com


#mysql版本
ARG MYSQL_VER=5.7.21
#mysql data目录
ARG MYSQL_DATA_DIR=/data/mysql
#mysql pid目录
ARG MYSQL_PID_DIR=/var/run/mysql
#mysql log目录
ARG MYSQL_LOG_DIR=/var/log/mysql
#mysql sock目录
ARG MYSQL_SOCK_DIR=/var/lib/mysql


#编译环境
RUN yum install -y gcc-c++ gcc make automake cmake ncurses-devel bison bison-devel


#安装mysql
RUN cd /usr/src \
    && curl -o mysql.tar.gz https://dev.mysql.com/get/Downloads/MySQL-${MYSQL_VER%.*}/mysql-boost-${MYSQL_VER}.tar.gz -L \
    && tar zxf mysql.tar.gz \
    && cd mysql-${MYSQL_VER} \
    && cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DWITH_BOOST=./boost -DMYSQL_DATADIR=${MYSQL_DATA_DIR} -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 \
        -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=${MYSQL_SOCK_DIR}/mysql.sock -DMYSQL_TCP_PORT=3306 \
        -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_UNIT_TESTS=OFF \
    && gmake \
    && gmake install \
    && mkdir -p ${MYSQL_DATA_DIR} \
    && mkdir -m 755 -p ${MYSQL_LOG_DIR} \
    && mkdir -m 755 -p ${MYSQL_SOCK_DIR} \
    && mkdir -m 755 -p ${MYSQL_PID_DIR} \
    && echo -e "[mysqld]\ndatadir=${MYSQL_DATA_DIR}\nsocket=${MYSQL_SOCK_DIR}/mysql.sock\nsymbolic-links=0\nlog-error=${MYSQL_LOG_DIR}/mysqld.log\npid-file=${MYSQL_PID_DIR}/mysqld.pid\nuser=root\n" > /etc/my.cnf \
    && echo -e "#!/bin/sh \n\
        files=\`ls ${MYSQL_DATA_DIR}\` \n\
        if [ -z \"\$files\" ];then \n\
            if [ ! \${MYSQL_PASSWORD} ]; then \n\
                MYSQL_PASSWORD='123456' \n\
            fi \n\
            /usr/local/mysql/bin/mysqld --initialize --basedir=/usr/local/mysql --datadir=${MYSQL_DATA_DIR} > ${MYSQL_LOG_DIR}/mysqld.log 2>&1 \n\
            MYSQLOLDPASSWORD=\`awk -F \"localhost: \" '/A temporary/{print \$2}' ${MYSQL_LOG_DIR}/mysqld.log\` \n\
            /usr/local/mysql/bin/mysqld & \n\
            echo -e \"[client] \\\n  password=\"\${MYSQLOLDPASSWORD}\" \\\n user=root\" > ~/.my.cnf \n\
            sleep 8s \n\
            /usr/local/mysql/bin/mysql --connect-expired-password -e \"alter user 'root'@'localhost' identified by '\$MYSQL_PASSWORD';update mysql.user set host='%' where user='root' && host='localhost';flush privileges;\" \n\
            echo -e \"[client] \\\n  password=\"\${MYSQL_PASSWORD}\" \\\n user=root\" > ~/.my.cnf \n\
            while true \n\
            do \n\
              let \"1\" \n\
            done \n\
        else \n\
            rm -rf \${MYSQL_SOCK_DIR}/mysql.sock.lock \n\
            /usr/local/mysql/bin/mysqld \n\
        fi" > /mysql.sh \
    && chmod +x /mysql.sh \
    && rm -rf /usr/src/mysql.tar.gz && rm -rf /usr/src/mysql-${MYSQL_VER}


#服务器基础设置
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && echo 'Asia/Shanghai' > /etc/timezonesource \
     && source /etc/profile


EXPOSE 3306


CMD ["/mysql.sh"]
