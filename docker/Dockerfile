FROM php:7.2-apache-buster
# for rebuild image docker-compose up --build -d \
# update and install some packages for working with mailHog
RUN apt-get update &&\
    apt-get install --no-install-recommends --assume-yes --quiet ca-certificates curl git &&\
    rm -rf /var/lib/apt/lists/*
RUN curl -Lsf 'https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz' | tar -C '/usr/local' -xvzf -
ENV PATH /usr/local/go/bin:$PATH
RUN go get github.com/mailhog/mhsendmail
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail
RUN echo 'sendmail_path = /usr/bin/mhsendmail --smtp-addr mailhog:1025' > /usr/local/etc/php/conf.d/docker-php-ext-mhsendmail.ini
# Install MySql PDO and mysqli (default image doesn't include it)
RUN docker-php-ext-install pdo_mysql && docker-php-ext-install mysqli
# Enable rewrite mod for apache
RUN a2enmod rewrite
# Install xdebug
RUN pecl install redis-5.1.1 \
    && pecl install xdebug-2.8.1 \
    && docker-php-ext-enable redis xdebug
# Configure xdebug for PhpStorm
RUN echo  '                                                                  \n\
xdebug.default_enable=1                                                      \n\
xdebug.remote_enable=1                                                       \n\
xdebug.max_nesting_level=1000                                                \n\
xdebug.remote_connect_back=1                                                 \n\
xdebug.remote_autostart=1                                                    \n\
xdebug.client_host=172.17.0.1                                                \n\
xdebug.ide_key=XDEBUG_IDE_KEY                                                \n\
xdebug.serverName=local'                                                     \n\
>> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# RUN a2enmod rewrite && service apache2 restart
# Enable logs
RUN echo  '                                                                  \n\
log_errors = On                                                              \n\
error_reporting = E_ALL'                                                     \n\
>> /usr/local/etc/php/conf.d/errors.ini \
