FROM alpine:3.12

LABEL maintainer="syushin 1147070314@qq.com"

# 设置apk源为阿里云
RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories

# 安装依赖包
RUN apk  update && apk  add  vim iotop gcc libgcc libc-dev libcurl libc-utils pcre-dev zlib-dev libnfs make pcre pcre2 zip unzip net-tools pstree wget libevent libevent-dev iproute2

# 将nginx源码包解压到容器内
ADD nginx-1.18.0.tar.gz /usr/local/src

# 编译安装，设置环境变量，创建nginx虚拟主机目录
RUN cd /usr/local/src/nginx-1.18.0 && \
./configure --prefix=/usr/local/nginx && \
make && \
make install && \
ln -sv /usr/local/nginx/sbin/nginx /usr/sbin/nginx && \
mkdir -p /usr/local/nginx/conf/vhost && \
mkdir -p /usr/local/nginx/html/www 

# 拷贝nginx配置文件
COPY conf.d/nginx.conf /usr/local/nginx/conf/nginx.conf
COPY conf.d/www.conf /usr/local/nginx/conf/vhost/www.conf

# 创建用户
RUN addgroup -g 2020 -S www && adduser -s /sbin/nologin -S -D -u 2020 -G www www && chown -R www.www /usr/local/nginx

# 设置进入容器的默认目录
WORKDIR /usr/local/nginx/html

# 启动nginx
CMD ["nginx"]
