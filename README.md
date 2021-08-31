# php-8.0-alpine
Docker image with php-8.0.10-alpine

All image contain following php extension graphviz and composer.

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

## Image Variants
### php-8.0-alpine:cli
This use the cli base image 

### php-8.0-alpine:cli-npm
This use the cli base image also contain nodejs and npm

### php-8.0-alpine:fpm
This use the fpm base image and contain the wkhtmltopdf with patched QT build binary
