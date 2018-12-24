FROM centos:latest
MAINTAINER liufee job@feehi.com


ARG MAVEN_VER=3.6.0


ADD ./src /usr/src


#安装java
RUN cd /usr/src \
    && cp -rf jdk /usr/local/jdk \
    && sed -i "s/export PATH/MAVEN_HOME=\/usr\/local\/maven \nexport MAVEN_HOME\nPATH=\/usr\/local\/jdk\/bin:\/usr\/local\/maven\/bin:\$PATH\nexport PATH\n export JAVA_HOME=\/usr\/local\/jdk\n export JRE_HOME=\/usr\/local\/jdk\/jre\n export CLASSPATH=.:\$JAVA_HOE\/lib:\$JRE_HOME\/lib:\/\$CLASSPATH\n/" /etc/profile


RUN cd /usr/src && curl -o maven.tar.gz http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/${MAVEN_VER}/binaries/apache-maven-${MAVEN_VER}-bin.tar.gz -L \
    && tar -xvf maven.tar.gz && mv apache-maven-${MAVEN_VER} /usr/local/maven \
    && sed -i "s/<mirrors>/<mirrors><mirror><id>nexus-aliyun<\/id><mirrorOf>central<\/mirrorOf><name>Nexus aliyun<\/name><url>http:\/\/maven.aliyun.com\/nexus\/content\/groups\/public<\/url><\/mirror>/" /usr/local/maven/conf/settings.xml


#服务器基础设置
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && echo 'Asia/Shanghai' > /etc/timezonesource \
     && source /etc/profile


CMD ["/usr/local/jdk/bin/java", "-version"]