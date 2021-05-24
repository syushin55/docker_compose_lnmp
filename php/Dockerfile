FROM php:7.2.34-fpm-alpine
LABEL MAINTAINER="syushin moliyoyoyo@163.com"
ENV TZ "Asia/Shanghai"
ENV TERM xterm
# 默认关闭opcode
ENV OPCODE 0

# 拷贝配置文件
COPY ./conf.d/ $PHP_INI_DIR/conf.d/
# 拷贝类库
COPY composer.phar /usr/local/bin/composer
# 拷贝php-fpm配置文件
COPY ./conf.d/www.conf /usr/local/etc/php-fpm.d/www.conf
# 创建u用户
RUN addgroup -g 1000 -S www && adduser -s /sbin/nologin -S -D -u 1000 -G www www
# 设置apk为阿里云国内镜像源
RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories

# PHPIZE_DEPS 包含 gcc g++ 等编译辅助类库，完成编译后删除
RUN apk add  $PHPIZE_DEPS \
    && apk add  libstdc++ libzip-dev vim\
    && apk update \
    && pecl install redis-5.3.4 \
    && pecl install zip \
    && pecl install swoole \
    && docker-php-ext-enable redis zip swoole\
    && apk del $PHPIZE_DEPS
# docker-php-ext-install 指令已经包含编译辅助类库的删除逻辑
RUN apk add  freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && apk update \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) opcache \
    && docker-php-ext-install -j$(nproc) bcmath \
    && chmod +x /usr/local/bin/composer

COPY ./conf.d/php.ini /usr/local/etc/php/php.ini
