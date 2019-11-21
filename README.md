# Docker Drupal Redis Sentinel

## What do I get?
Following services are created:

* master: Master Redis Server
* slave: Slave Redis Server
* sentinel: Sentinel Server
* Nginx: For Drupal
* PHP: PHP FPM for Drupal
* MySQL: For Drupal DB

## Setup this locally
### Get this repo
```
git clone 
```

### Get Drupal
#### Composer install Drupal
Composer install Drupal into subdir called `drupal`.
```
composer create-project drupal-composer/drupal-project:8.x-dev drupal --no-interaction
```
#### Drupal Config
See README-DRUPAL.md

## Run all this locally.
### Get it up
```
docker-compose up --build -d
....
....
....
```

### Scale up
```
docker-compose slave=2 sentinel=3
....
....
....
Creating docker_redis_sentinel_slave_2 ... done
Starting docker_redis_sentinel_sentinel_1 ... done
Creating docker_redis_sentinel_sentinel_2 ... done
Creating docker_redis_sentinel_sentinel_3 ... done
```

### Is it up?
```
docker-compose ps
              Name                            Command               State           Ports
--------------------------------------------------------------------------------------------------
docker_redis_sentinel_db_1         docker-entrypoint.sh mysqld      Up      0.0.0.0:3306->3306/tcp
docker_redis_sentinel_master_1     docker-entrypoint.sh redis ...   Up      6379/tcp
docker_redis_sentinel_php_1        docker-php-entrypoint php-fpm    Up      9000/tcp
docker_redis_sentinel_sentinel_1   entrypoint.sh                    Up      6379/tcp
docker_redis_sentinel_sentinel_2   entrypoint.sh                    Up      6379/tcp
docker_redis_sentinel_sentinel_3   entrypoint.sh                    Up      6379/tcp
docker_redis_sentinel_slave_1      docker-entrypoint.sh redis ...   Up      6379/tcp
docker_redis_sentinel_slave_2      docker-entrypoint.sh redis ...   Up      6379/tcp
docker_redis_sentinel_web_1        nginx -g daemon off;             Up      0.0.0.0:80->80/tcp
```