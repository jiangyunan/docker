# 注意: 需要翻墙
FROM php:7.4-fpm

# 设置时区
ENV TZ=Asia/Shanghai

# 设置软件源
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
COPY bullseye.sources.list /etc/apt/sources.list

RUN apt-get update -y && apt-get install -y sendmail libpng-dev git libicu-dev unzip libzip-dev libonig-dev

RUN docker-php-ext-install pdo_mysql mysqli gd calendar intl zip mbstring
RUN pecl install xdebug redis protobuf mongodb
RUN docker-php-ext-enable xdebug redis protobuf mongodb

# 安装composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
RUN apt-get install -y wget
RUN wget https://phar.phpunit.de/phpunit-9.5.10.phar -O phpunit && chmod +x ./phpunit && mv ./phpunit /usr/local/bin/
RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com

# 安装xhprof监控
WORKDIR /tmp
RUN git clone https://github.com/tideways/php-xhprof-extension.git
WORKDIR /tmp/php-xhprof-extension
RUN phpize && ./configure && make && make install
COPY tideways_xhprof.ini ${PHP_INI_DIR}/conf.d/tideways_xhprof.ini
WORKDIR /var/www/html

