# Для начала указываем исходный образ, он будет использован как основа
FROM php:7.2-fpm
# Необязательная строка с указанием автора образа
MAINTAINER nameless <4pavlovon@gmail.com>

# RUN выполняет идущую за ней команду в контексте нашего образа.
# В данном случае мы установим некоторые зависимости и модули PHP.
# Для установки модулей используем команду docker-php-ext-install.
# На каждый RUN создается новый слой в образе, поэтому рекомендуется объединять команды.
RUN apt-get update \
	&& apt-get install -y libmcrypt-dev \
	&& rm -rf /var/lib/apt/lists/* \
	&& pecl install mcrypt-1.0.1 \
	&& docker-php-ext-enable mcrypt

RUN apt-get update && apt-get install -y \
		mc \
        curl \
        wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
    && docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# Куда же без composer'а.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Добавляем настройки в конфиг файл php-fpm
RUN echo "php_admin_flag[log_errors] = On" >> /usr/local/etc/php-fpm.conf
RUN echo "php_admin_value[error_log] = /dev/stdout" >> /usr/local/etc/php-fpm.conf
RUN echo "access.log = /dev/null" >> /usr/local/etc/php-fpm.conf
RUN echo "catch_workers_output = yes" >> /usr/local/etc/php-fpm.conf

#  Enable the error log
#RUN sed -i '/^;catch_workers_output/ccatch_workers_output = yes' /usr/local/etc/php-fpm.conf && \
#    sed -i '/^;access.log/caccess.log = /var/log/fpm-access.log' /usr/local/etc/php-fpm.conf && \
#    sed -i '/^;php_flag\[display_errors\]/cphp_flag[display_errors] = Off' /usr/local/etc/php-fpm.conf && \
#    sed -i '/^;php_admin_value\[error_log\]/cphp_admin_value[error_log] = /var/log/fpm-php.www.log' /usr/local/etc/php-fpm.conf && \
#    sed -i '/^;php_admin_flag\[log_errors\]/cphp_admin_flag[log_errors] = On' /usr/local/etc/php-fpm.conf

# Redirect fpm logs to stdout and stderr so they are forwarded to the docker log collector
#RUN ln -sf /dev/stdout /var/log/fpm-access.log && \
#    ln -sf /dev/stderr /var/log/fpm-php.www.log

# Добавим свой php.ini, можем в нем определять свои значения конфига
ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

# Указываем рабочую директорию для PHP
WORKDIR /var/www

# Запускаем контейнер
# Из документации: The main purpose of a CMD is to provide defaults for an executing container. These defaults can include an executable,
# or they can omit the executable, in which case you must specify an ENTRYPOINT instruction as well.
CMD ["php-fpm"]
