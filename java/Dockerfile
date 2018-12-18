FROM centos:latest
MAINTAINER liufee job@feehi.com


ADD ./src /usr/src


#安装java
RUN cd /usr/src \
    && cp -rf jdk /usr/local/jdk \
    && sed -i "s/export PATH/PATH=\/usr\/local\/jdk\/bin:\$PATH\nexport PATH\n export JAVA_HOME=\/usr\/local\/jdk\n export JRE_HOME=\/usr\/local\/jdk\/jre\n export CLASSPATH=.:\$JAVA_HOE\/lib:\$JRE_HOME\/lib:\/\$CLASSPATH/" /etc/profile


#服务器基础设置
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && echo 'Asia/Shanghai' > /etc/timezonesource \
     && source /etc/profile


EXPOSE 3306


CMD ["/usr/local/jdk/bin/java -jar /app/app.jar"]
