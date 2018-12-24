FROM centos:latest
MAINTAINER liufee job@feehi.com


#node.js版本
ARG NODE_VER=8.11.4


#安装node.js
RUN curl -o node.tar.xz https://nodejs.org/dist/v${NODE_VER}/node-v${NODE_VER}-linux-x64.tar.xz && tar -xvf node.tar.xz \
    && mv node-v${NODE_VER}-linux-x64 /usr/local/node \
    && sed -i "s/export PATH/PATH=\/usr\/local\/node\/bin:\$PATH\nexport PATH/" /etc/profile \
    && source /etc/profile && /usr/local/node/bin/npm install -g cnpm --registry=https://registry.npm.taobao.org \
    && /usr/local/node/bin/cnpm install -g pm2 \
    && rm -rf node.tar.gz


#服务器基础设置
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && echo 'Asia/Shanghai' > /etc/timezonesource \
     && source /etc/profile

RUN ln -s /usr/local/node/bin/node /bin/node \
    && ln -s /usr/local/node/bin/npm /bin/npm \
    && ln -s /usr/local/node/bin/pm2 /bin/pm2


CMD  ["/usr/local/node/bin/node", "-version"]