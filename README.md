# Docker Drupal Redis Sentinel
Installing and running “redis” seem to be enough to run Drupal with it - but sometimes you want to scale Redis for HA. 

Tool named “redis-sentinel” provides “redis” services such as: 
* *Monitoring*: It can check master or slave services whether is working correctly and healthy or not.
* *Notification*: It can notify when a service goes down.
* *Automatic Failover*: It starts a failover scenario and assigns one of slaves as a master when master service goes down.
* *Configuration Provider*: It can configure and share master service information when a new service is added to cluster.

![Redis Sentinel Drupal Docker](https://miro.medium.com/max/1069/1*gszoEBW0lupbMDDGGgYOPA.png)


## What do I get with this Repo?
Following services are created using Docker:

* Redis master: Master Redis Server
* Redis slave: Slave Redis Server
* Redis sentinel: Sentinel Server
* Nginx: For Drupal
* PHP: PHP FPM for Drupal
* MySQL: For Drupal DB

## Setup Drupal + Redis + Sentinels locally
### Get this repo
```
git clone {this_repo}
```

### Get Drupal
#### Composer install Drupal
Composer install Drupal into subdir called `drupal`.

```
composer create-project drupal-composer/drupal-project:8.x-dev drupal --no-interaction
```

#### Drupal Config
See README-DRUPAL.md

## Run all services locally
### Get it up
```
docker-compose up --build -d
....
....
....
```

### Scale up Redis slaves & Sentinels
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

### References:
https://medium.com/@mustafa.ileri/redis-clustering-with-redis-sentinel-on-docker-1090ab876f97
https://medium.com/@amila922/redis-sentinel-high-availability-everything-you-need-to-know-from-dev-to-prod-complete-guide-deb198e70ea6