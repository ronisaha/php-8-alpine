FROM php:8.0.10-fpm-alpine3.13

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
		fontconfig \
        msttcorefonts-installer \
		imagemagick-dev libtool\
		rabbitmq-c-dev\
		libxslt-dev libgcrypt-dev \
		postgresql-dev openldap-dev yaml-dev \
	; \
	\
	curl -L https://github.com/mlocati/docker-php-extension-installer/releases/download/1.2.24/install-php-extensions > /usr/local/bin/install-php-extensions \
	; \
	chmod +x /usr/local/bin/install-php-extensions ; \
	\
	apk add --no-cache \
        imagemagick gettext \
        wkhtmltopdf \
        freetype \
        ttf-dejavu \
        ttf-droid \
        ttf-freefont \
        ttf-liberation \
        ttf-ubuntu-font-family; \
    update-ms-fonts; \
    fc-cache -f; \
    install-php-extensions imagick amqp; \
	pecl install apcu; \
	pecl install mongodb; \
	pecl install yaml; \
	pecl install redis; \
	docker-php-ext-configure xsl; \
	docker-php-ext-configure intl; \
	docker-php-ext-configure gd; \
	docker-php-ext-install bcmath gd pdo pdo_pgsql pdo_mysql opcache zip exif xsl ldap intl gettext iconv; \
	docker-php-ext-enable apcu yaml redis; \
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
            echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini ; \
    { \
            echo 'date.timezone=Asia/Dhaka'; \
            echo 'upload_max_filesize=256M'; \
            echo 'post_max_size=2048M'; \
            echo 'max_input_vars=10000'; \
            echo 'memory_limit=3504M'; \
            echo 'expose_php = off'; \
            echo 'max_execution_time = 300'; \
    } > /usr/local/etc/php/conf.d/00-override.ini ; \
    { \
        echo 'error_reporting = 4339'; \
        echo 'display_errors = Off'; \
        echo 'display_startup_errors = Off'; \
        echo 'log_errors = On'; \
        echo 'error_log = /dev/stderr'; \
        echo 'log_errors_max_len = 1024'; \
        echo 'ignore_repeated_errors = On'; \
        echo 'ignore_repeated_source = Off'; \
        echo 'html_errors = Off'; \
    } > /usr/local/etc/php/conf.d/error-logging.ini ; \
    { \
            echo 'request_terminate_timeout = 300'; \
            echo 'rlimit_files = 131072'; \
            echo 'rlimit_core = unlimited'; \
    } >> /usr/local/etc/php-fpm.d/zz-docker.conf

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER=1

#Replace with local patched QT build
COPY --from=registry.hub.docker.com/ronisaha/wkhtmltopdf-qt-alpine-3.12 /usr/bin/wkhtmltopdf /usr/bin/wkhtmltopdf

