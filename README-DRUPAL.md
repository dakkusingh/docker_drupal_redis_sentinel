# Drupal Redis Setup
How to configure Drupal for Redis.

## Install Pecl Redis
```
pecl install redis
```
See: https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown

## Download Drupal Redis Module
```
composer require 'drupal/redis:^1.2'
```
See: https://www.drupal.org/project/redis/releases

## Enable Redis module
```
drush en redis
```

## Configure Redis Module for Drupal
```
## REDIS ##
$settings['redis.connection']['interface'] = 'PhpRedis';

// Sentinels instances list with hostname:port format.
$settings['redis.connection']['host'] = ['sentinel:26000'];

// Redis instance name.
$settings['redis.connection']['instance']  = 'mymaster';

// Set the redis backend.
$settings['cache']['default'] = 'cache.backend.redis';

// Connect via UNIX socket
$conf['redis_cache_socket'] = '/tmp/redis.sock';
```

See: https://git.drupalcode.org/project/redis/blob/8.x-1.x/README.md
See: https://git.drupalcode.org/project/redis/blob/8.x-1.x/README.PhpRedis.txt