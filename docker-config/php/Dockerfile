FROM dakku/drupal-docker-php-fpm:v1.0

RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install redis \
    && docker-php-ext-enable redis