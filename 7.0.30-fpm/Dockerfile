FROM php:7.0.30-fpm-alpine
MAINTAINER linh@jmango360.com
VOLUME /magento
RUN    apk update \                                                                                                               
    && apk add ca-certificates wget curl \                                                                                             
    && update-ca-certificates

RUN docker-php-ext-install mysqli      \
    && docker-php-ext-install pdo_mysql 

## install gd  mcrypt
## refer https://github.com/docker-library/php/issues/225

RUN apk add --no-cache --virtual .persistent-deps icu-dev libxml2-dev \
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-configure soap --enable-soap  \
    && docker-php-ext-install intl 		\
    && docker-php-ext-install soap      \
	&& ( \
		apk add --no-cache \
			freetype \
			libpng \
			libjpeg-turbo \
			freetype-dev \
			libpng-dev \
			libjpeg-turbo-dev\
		&& docker-php-ext-configure gd \
			--with-gd \
			--with-freetype-dir=/usr/include/ \
			--with-png-dir=/usr/include/ \
			--with-jpeg-dir=/usr/include/ \
		&& docker-php-ext-install gd \
		&& apk del \
			--no-cache \
			freetype-dev \
			libpng-dev \
			libjpeg-turbo-dev \
	) \
	&& ( \
		apk add --no-cache libmcrypt-dev  libltdl \
		&& docker-php-ext-configure mcrypt --with-mcrypt \
		&& docker-php-ext-install  mcrypt \
	) 
# install Required PHP Extensions for magento2
RUN docker-php-ext-install bcmath mbstring xml zip  opcache

RUN apk add --no-cache  libxslt-dev &&  docker-php-ext-install xsl  

RUN apk add --no-cache wget ca-certificates  \
    && wget -O /etc/apk/keys/phpearth.rsa.pub https://repos.php.earth/alpine/phpearth.rsa.pub \
    && echo "https://repos.php.earth/alpine/v3.7" >> /etc/apk/repositories \
    && apk add --no-cache php7.0-common

# install Xdebug

RUN apk add --no-cache $PHPIZE_DEPS  autoconf \
	&& pecl install xdebug-2.5.0 \
	&& docker-php-ext-enable xdebug \
	&& echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_mode=req" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.idekey=DBGP_IDEKEY" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=127.0.0.1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini 


