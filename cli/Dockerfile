FROM php:8.0.2-cli-alpine

MAINTAINER Roni Kumar Saha <roni.csed@gmail.com>

#install Exetentions and dependencies
RUN set -ex; \
	\
	apk add --no-cache --virtual .build-deps \
	    libjpeg-turbo \
        libpng \
        freetype-dev \
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
	curl -L https://github.com/mlocati/docker-php-extension-installer/releases/download/1.2.12/install-php-extensions > /usr/local/bin/install-php-extensions \
	; \
	chmod +x /usr/local/bin/install-php-extensions ; \
	\
	apk add --no-cache \
        imagemagick gettext; \
	install-php-extensions imagick amqp; \
	pecl install apcu; \
	pecl install mongodb; \
	pecl install yaml; \
	pecl install redis; \
	pecl install xdebug; \
	docker-php-ext-configure xsl; \
	docker-php-ext-configure intl; \
	docker-php-ext-configure gd \
          --with-freetype=/usr/include/ \
          --with-jpeg=/usr/include/; \
	docker-php-ext-install gd pdo pdo_pgsql pdo_mysql opcache zip exif xsl ldap intl gettext iconv; \
	docker-php-ext-enable apcu yaml redis xdebug; \
	\
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add $runDeps ; \
	apk del .build-deps ; \
	rm -rf /tmp/* /usr/local/lib/php/doc/* /var/cache/apk/* /usr/local/bin/install-php-extensions

## Set php configuration
RUN { \
       echo 'date.timezone=Asia/Dhaka'; \
    } > /usr/local/etc/php/conf.d/00-override.ini ;

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER=1
