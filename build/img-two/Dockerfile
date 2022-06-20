ARG PHP_VERSION="8.1"

ARG PHP_TYPE="cli"

ARG SEPARATOR="-"

FROM php:${PHP_VERSION}${SEPARATOR}${PHP_TYPE}

LABEL MAITAINER="Lucas Lima <lucas.developmaster@gmail.com>"

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /var/www

ENV TZ=UTC

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install --no-install-recommends -y \
    unzip \
    vim

RUN docker-php-ext-install bcmath \
    calendar \
    dba \
    exif \
    gettext \
    mysqli \
    opcache \
    pcntl \
    pdo \
    pdo_mysql \
    shmop \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm \
    zend_test

RUN apt-get install -y zip libzip-dev \
    && docker-php-ext-install zip

RUN apt-get install -y libpng-dev libjpeg62-turbo-dev libfreetype6-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get install -y libbz2-dev \
    && docker-php-ext-install bz2

RUN apt-get install -y libenchant-2-dev \
    && docker-php-ext-install enchant

RUN docker-php-ext-install ffi

RUN apt-get install -y libgmp-dev \
    && docker-php-ext-install gmp

RUN apt-get install -y libc-client-dev libkrb5-dev \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap

RUN apt-get install -y libicu-dev \
    && docker-php-ext-install -j$(nproc) intl

RUN apt-get install -y libldap2-dev \
    && docker-php-ext-configure ldap --with-ldap=/usr \
    && docker-php-ext-install ldap

RUN apt-get install -y freetds-dev \
    && docker-php-ext-install pdo_dblib

RUN apt-get -y install firebird-dev \
    && docker-php-ext-install pdo_firebird

RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql pgsql
    
RUN apt-get install -y libpspell-dev \
    && docker-php-ext-install pspell

RUN apt-get install -y libsnmp-dev \
    && docker-php-ext-install snmp

RUN apt-get install -y libxml2-dev \
    && docker-php-ext-install soap

RUN apt-get install -y libtidy-dev \
    && docker-php-ext-install tidy

RUN apt-get install -y libxslt-dev \
    && docker-php-ext-install xsl

RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer

ARG WWWGROUP
ARG NODE_VERSION=16

RUN curl -sLS https://deb.nodesource.com/setup_$NODE_VERSION.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g npm

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | tee /usr/share/keyrings/yarn.gpg >/dev/null \
    && echo "deb [signed-by=/usr/share/keyrings/yarn.gpg] https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get install -y yarn gnupg gosu curl ca-certificates git supervisor python2 \
    && mkdir -p ~/.gnupg \
    && chmod 600 ~/.gnupg \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN groupadd --force -g $WWWGROUP nobreack
RUN useradd -ms /bin/bash --no-user-group -g $WWWGROUP -u 1337 nobreack

COPY ./bin/start-container /usr/local/bin/start-container
RUN chmod +x /usr/local/bin/start-container

ENTRYPOINT ["start-container"]