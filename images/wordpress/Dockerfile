FROM wordpress
MAINTAINER PHPtoday.ru <4pavlovon@gmail.com>

RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git;

# Куда же без composer'а.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /usr/share/nginx/html

#RUN pwd;rm -rf ../html*;
RUN cd wp.loc;wget https://wordpress.org/latest.tar.gz;tar -xzvf latest.tar.gz;