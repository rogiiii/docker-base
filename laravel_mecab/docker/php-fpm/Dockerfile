FROM php:7.4-fpm
LABEL maintainer "Rogiiii <rogiiii.develop@gmail.com>"

COPY ./php.ini /usr/local/etc/php/

RUN apt-get update
RUN apt-get install -y git zip unzip sudo
RUN docker-php-ext-install pdo_mysql
RUN apt-get install -y libzip-dev
RUN docker-php-ext-install zip

COPY --from=composer:1.10.0 /usr/bin/composer /usr/bin/composer

# Node.js
RUN apt-get install -y nodejs
RUN curl -sS https://www.npmjs.com/install.sh | sh
RUN npm install -g n
RUN n stable
RUN apt-get purge -y nodejs
RUN npm install -g yarn

# Mecab
USER root
RUN apt-get install -y mecab libmecab-dev mecab-ipadic-utf8
RUN cd /usr/local/src && git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git
RUN cd /usr/local/src/mecab-ipadic-neologd && ./bin/install-mecab-ipadic-neologd -n -y
RUN cd /usr/local/src && git clone https://github.com/rsky/php-mecab
RUN cd /usr/local/src/php-mecab/mecab && phpize && ./configure && make && make install
COPY ./docker-php-ext-mecab.ini /usr/local/etc/php-conf.d/docker-php-ext-mecab.ini
COPY ./mecabrc /etc/mecabrc

# User
RUN groupadd -g 1000 gpagent && \
    useradd -g gpagent -G sudo -m -s /bin/bash agent &&\
    echo 'agent:password' | chpasswd

USER agent
