# php-8.2.x-alpine
Docker image with php-8.2.x-alpine

All image contain following php extension and [composer](https://github.com/composer/composer).

- bcmath
- gd
- intl
- json
- libxml
- mbstring
- openssl
- PDO
- pdo_pgsql
- pdo_mysql
- SimpleXML
- tokenizer
- Zend OPcache
- amqp
- redis
- apcu
- yaml
- zip
- curl
- zip 
- exif
- iconv
- sockets
- soap

## Image Variants
### php-8-alpine:cli or php-8-alpine:cli-{php-version}
This use the cli base image also contain nodejs-18.x and npm

### php-8-alpine:fpm or php-8-alpine:fpm-{php-version}
This use the fpm base image

### php-8-alpine:dev or php-8-alpine:dev-{php-version}
This use the apache base image with xdebug enabled

### php-8-alpine:supervisor or php-8-alpine:supervisor-{php-version}
This use the cli base image and set supervisor as entry point. Mount any supervisor config file as /etc/supervisor.d/*.ini
