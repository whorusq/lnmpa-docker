##############################################################
# 基于官方镜像 php:7.1-apache 制作（系统 Debian 8 jessie）
# 安装的应用：Apache、PHP
# 附加的 PHP 扩展：
#   pdo_mysql、mysqli、gd、redis、gearman、iconv、mcrypt、pcntl
# PHP 配置文件：
#   - /usr/local/etc/php/php.ini
# WEB 根目录：
#   - /var/www/html
# Apache 配置文件：
#   - /etc/apache2/apache2.conf
# Apache 站点配置：
#   - /etc/apache2/sites-available
#   - /etc/apache2/sites-enabled
# VERSION 1.5
##############################################################

ARG TAG
ARG READIS
ARG GEARMAN
ARG GEARMAND

FROM php:$TAG-apache
MAINTAINER whoru.S.Q <whorusq@gmail.com>

# 修改软件更新源
COPY ./source.list /etc/apt/sources.list

# 安装、配置（拓展使用稳定版）
ARG READIS
ARG GEARMAN
ARG GEARMAND
RUN apt-get update \
    # 安装扩展依赖包
    && apt-get install -y wget \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libboost-dev libboost-all-dev gperf* libevent-dev \
    # 安装扩展：iconv、mcrypt、zip、gd、pdo_mysql、mysqli、pcntl
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) zip gd pdo_mysql mysqli pcntl \
    # 下载源码包安装其它扩展：redis、gearman
    && mkdir -p /tmp/redis \
    && wget -c https://github.com/phpredis/phpredis/archive/$READIS.tar.gz \
    && tar -xf $READIS.tar.gz -C /tmp/redis --strip-components=1 \
    # 安装扩展：redis
    && docker-php-ext-configure /tmp/redis --with-php-config=/usr/local/bin/php-config \
    && docker-php-ext-install /tmp/redis \
    # 开启 URL 重写
    # 或 ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load
    && a2enmod rewrite \
    # 更改 web 根目录属主、属组，避免权限问题
    # 运行时用户可通过 phinfo() 中查看 APACHE_RUN_USER 获取
    #usermod -u 1000 www-data && usermod -G staff www-data
    #&& chown www-data:www-data /var/www/html \
    # 清理安装过程产生的垃圾文件
    && apt-get clean \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# 追加 PHP 自定义配置文件
#COPY config/php.ini /usr/local/etc/php/

# 追加 Apache 自定义配置文件
#COPY ./config/apache2.conf /etc/apache2/apache2.conf
#COPY ./config/sites-available /etc/apache2/sites-available
#COPY ./config/sites-enabled /etc/apache2/sites-enabled
