FROM php:7.4-fpm
LABEL maintainer "Rogiiii <rogiiii.develop@gmail.com>"

COPY ./php.ini /usr/local/etc/php/

RUN apt-get update
RUN apt-get install -y git zip unzip
RUN docker-php-ext-install pdo_mysql
RUN apt-get install -y libzip-dev
RUN docker-php-ext-install zip
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'e5325b19b381bfd88ce90a5ddb7823406b2a38cff6bb704b0acc289a09c8128d4a8ce2bbafcd1fcbdc38666422fe2806') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir="/usr/local/bin" --filename="composer"
RUN php -r "unlink('composer-setup.php');"

# Node.js
RUN apt-get install -y nodejs
RUN curl -sS https://www.npmjs.com/install.sh | sh
RUN npm install -g n
RUN n stable
RUN apt-get purge -y nodejs
RUN npm install -g yarn

# User
RUN groupadd -g 1000 gpagent && \
    useradd -g gpagent -G sudo -m -s /bin/bash agent &&\
    echo 'agent:password' | chpasswd

USER agent
