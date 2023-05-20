ARG PHP_VERSION=8.2.6
FROM php:${PHP_VERSION}-fpm-alpine

LABEL org.opencontainers.image.authors="Roni Kumar Saha <roni.csed@gmail.com>"

#install Exetentions and dependencies
RUN set -ex; \
	\
	apk add --no-cache --virtual .build-deps \
	    libjpeg-turbo \
        libpng \
        libjpeg-turbo-dev \
        libpng-dev \
		libzip-dev \
		icu-dev \
		gettext-dev \
		$PHPIZE_DEPS \
		imagemagick-dev libtool\
		rabbitmq-c-dev\
		libxslt-dev libgcrypt-dev \
		postgresql-dev openldap-dev yaml-dev \
	; \
	\
	curl -L https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions > /usr/local/bin/install-php-extensions \
	; \
	chmod +x /usr/local/bin/install-php-extensions ; \
	\
	apk add --no-cache \
        imagemagick gettext graphviz; \
    install-php-extensions imagick amqp sockets; \
	pecl install apcu; \
	pecl install mongodb; \
	pecl install yaml; \
    pecl install igbinary; \
    docker-php-ext-enable apcu yaml igbinary; \
	echo 'yes,yes' | pecl install redis; \
    docker-php-ext-enable redis; \
	docker-php-ext-configure xsl; \
	docker-php-ext-configure intl; \
	docker-php-ext-configure gd; \
	docker-php-ext-install bcmath gd pdo pdo_pgsql pdo_mysql opcache zip exif xsl ldap intl gettext soap; \
	\
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add $runDeps ; \
	apk del .build-deps wget ; \
	rm -rf /tmp/* /usr/local/lib/php/doc/* /var/cache/apk/* /usr/local/bin/install-php-extensions

## Set php configuration
COPY configs/conf.d/*.ini /usr/local/etc/php/conf.d/
COPY configs/fpm.d/zz-docker.conf /usr/local/etc/php-fpm.d/

