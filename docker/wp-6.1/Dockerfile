FROM wordpress:6.1.1-php8.2-apache

LABEL maintainer="Maikel Carballo"

ARG WWWGROUP

WORKDIR /var/www/html

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=America/Caracas

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y \
            locales \
            mariadb-client; \
    \
    apt-get -y autoremove; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN localedef -i es_VE -c -f UTF-8 -A /usr/share/locale/locale.alias es_VE.UTF-8

ENV LANG es_VE.utf8

RUN groupadd --force -g $WWWGROUP devuser
RUN useradd -ms /bin/bash --no-user-group -g $WWWGROUP -u 1337 devuser

COPY php.ini "$PHP_INI_DIR/conf.d/"

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions soap sockets xsl
