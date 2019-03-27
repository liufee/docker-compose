FROM centos:latest
MAINTAINER liufee job@feehi.com


#java版本
ARG JDK_VER=1.8
#maven版本
ARG MAVEN_VER=3.6.0


#安装Java JDK
RUN cd /usr/src \
    && curl -o jdk.tar.gz http://d.feehi.com/jdk-${JDK_VER} -L \
    && mkdir -p /usr/local/java && tar -xzvf jdk.tar.gz -C /usr/local/java --strip-components 1 \
    && sed -i "s/export PATH/PATH=\/usr\/local\/java\/bin:\$PATH\nJAVA_HOME=\/usr\/local\/java\nJRE_HOME=\/usr\/local\/java\/jre\nCLASSPATH=\.:\$JAVA_HOME\/lib:\$JRE_HOME\/lib\nexport PATH JAVA_HOME JRE_HOME CLASSPATH/" /etc/profile \
    && rm -rf jdk.tar.gz


#安装maven
RUN cd /usr/src && curl -o maven.tar.gz http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/${MAVEN_VER}/binaries/apache-maven-${MAVEN_VER}-bin.tar.gz -L \
    && tar -xvf maven.tar.gz && mv apache-maven-${MAVEN_VER} /usr/local/maven \
    && sed -i "s/<mirrors>/<mirrors><mirror><id>nexus-aliyun<\/id><mirrorOf>central<\/mirrorOf><name>Nexus aliyun<\/name><url>http:\/\/maven.aliyun.com\/nexus\/content\/groups\/public<\/url><\/mirror>/" /usr/local/maven/conf/settings.xml \
    && sed -i "s/export PATH/MAVEN_HOME=\/usr\/local\/maven \nexport MAVEN_HOME\nPATH=\/usr\/local\/maven\/bin:\$PATH\nexport PATH/" /etc/profile \
    && rm -rf maven.tar.gz


#环境变量设置
ENV PATH /usr/local/java/bin:/usr/local/maven/bin:$PATH
ENV JAVA_HOME /usr/local/java
ENV JRE_HOME /usr/local/java/jre
ENV CLASSPATH .:$JAVA_HOME/lib:/$JRE_HOME/lib:$CLASSPATH
ENV MAVEN_HOME /usr/local/maven


#服务器基础设置
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && echo 'Asia/Shanghai' > /etc/timezonesource \
     && source /etc/profile


CMD ["/usr/local/jdk/bin/java", "/usr/local/app/app.jar"]