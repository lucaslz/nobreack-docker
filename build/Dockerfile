ARG PHP_VERSION="8.1"

ARG PHP_TYPE="cli"

ARG SEPARATOR="-"

ARG CODENAME="alpine"

FROM php:${PHP_VERSION}${SEPARATOR}${PHP_TYPE}${SEPARATOR}${CODENAME}

LABEL MAITAINER="Lucas Lima <lucas.developmaster@gmail.com>"

WORKDIR /var/www

ENV TZ=UTC

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apk add -U --no-cache git \
    curl \
    wget \
    vim \
    zip \
    unzip

RUN docker-php-ext-install bcmath \
    calendar \
    dba \
    exif

RUN apk --no-cache add \
    icu-dev \
    gettext \
    gettext-dev

RUN docker-php-ext-configure gettext \
    && docker-php-ext-install gettext

RUN docker-php-ext-install gettext \
    mysqli \
    opcache \
    pcntl \
    pdo \
    pdo_mysql \
    shmop \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm 

RUN apk --no-cache add libzip-dev \
    && docker-php-ext-install zip

RUN apk add --no-cache \
    libjpeg-turbo-dev \
    libpng-dev \
    libwebp-dev \
    freetype-dev \
    && docker-php-ext-configure gd --with-jpeg --with-webp --with-freetype \
    && docker-php-ext-install -j$(nproc) gd

RUN apk --no-cache add libbz2 \
    && docker-php-ext-install bz2

RUN apk --no-cache add enchant2-dev \
    && docker-php-ext-install enchant

RUN docker-php-ext-install ffi

RUN apk --no-cache add gmp-dev \
    && docker-php-ext-install gmp

RUN apk --no-cache add imap-dev krb5-dev openssl-dev \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) imap

RUN apk --no-cache add icu-dev \
    && docker-php-ext-install -j$(nproc) intl

RUN apk --no-cache add openldap-dev \
    && docker-php-ext-configure ldap --with-ldap=/usr \
    && docker-php-ext-install -j$(nproc) ldap

RUN apk --no-cache add freetds-dev \
    && docker-php-ext-install pdo_dblib

RUN apk --no-cache add libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql pgsql

RUN apk --no-cache add libxml2-dev \
    && docker-php-ext-install soap

RUN apk --no-cache add libxslt-dev \
    && docker-php-ext-install xsl

RUN apk --no-cache add tidyhtml-dev \
    && docker-php-ext-install tidy

RUN apk --no-cache add net-snmp-dev \
    && docker-php-ext-install snmp

RUN apk --no-cache add aspell-dev \
    && docker-php-ext-install pspell

RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer

ARG WWWGROUP
ARG NODE_VERSION=16

RUN apk --no-cache add nodejs=16.15.0-r1 npm yarn
RUN apk --no-cache add gnupg
RUN apk --no-cache add git
RUN apk --no-cache add -U --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing/ gosu \
    && rm -rf /var/cache/apk/*

RUN addgroup -S -g $WWWGROUP nobreack
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "$(pwd)" \
    --ingroup "nobreack" \
    --no-create-home \
    --uid "$WWWGROUP" \
    "nobreack"

COPY ./bin/start-container /usr/local/bin/start-container
RUN chmod +x /usr/local/bin/start-container

ENTRYPOINT ["start-container"]