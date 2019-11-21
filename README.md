# Docker Redis Sentinel

## What do I get?
Following services are created:

* master: Master Redis Server
* slave: Slave Redis Server
* sentinel: Sentinel Server

## Sentinel Conf
The sentinels are configured with *mymaster* instance and following properties (see ./sentinel/Dockerfile):

```
ENV SENTINEL_QUORUM 2
ENV SENTINEL_DOWN_AFTER 5000
ENV SENTINEL_FAILOVER 10000
ENV SENTINEL_PORT 26000
ENV SENTINEL_PAR_SYNC 1
```

## Get it
```
git clone 
```

## Get it up
```
docker-compose up --build -d

Building sentinel
Step 1/10 : FROM redis:5-alpine
 ---> 6f63d037b592
Step 2/10 : ENV SENTINEL_QUORUM 2
 ---> Using cache
 ---> 8fa864ebfe0a
Step 3/10 : ENV SENTINEL_DOWN_AFTER 5000
 ---> Using cache
 ---> 005b35a3e30e
Step 4/10 : ENV SENTINEL_FAILOVER 10000
 ---> Using cache
 ---> 2c9c79ccf6e8
Step 5/10 : ENV SENTINEL_PORT 26000
 ---> Using cache
 ---> 81fb3db31646
Step 6/10 : ADD sentinel.conf /etc/redis/sentinel.conf
 ---> Using cache
 ---> 5913b70a9c74
Step 7/10 : RUN chown redis:redis /etc/redis/sentinel.conf
 ---> Using cache
 ---> 41e5653dc670
Step 8/10 : ADD entrypoint.sh /usr/local/bin/
 ---> Using cache
 ---> b7ec820d2af9
Step 9/10 : RUN chmod +x /usr/local/bin/entrypoint.sh
 ---> Using cache
 ---> ff3647286519
Step 10/10 : ENTRYPOINT ["entrypoint.sh"]
 ---> Using cache
 ---> 2bfb6c00e9d6
Successfully built 2bfb6c00e9d6
Successfully tagged docker_redis_sentinel_sentinel:latest
Creating docker_redis_sentinel_master_1 ... done
Creating docker_redis_sentinel_slave_1  ... done
Creating docker_redis_sentinel_sentinel_1 ... done
```

## Scale up
```
docker-compose slave=2 sentinel=3
...
Creating docker_redis_sentinel_slave_2 ... done

docker-compose scale sentinel=3
...
Starting docker_redis_sentinel_sentinel_1 ... done
Creating docker_redis_sentinel_sentinel_2 ... done
Creating docker_redis_sentinel_sentinel_3 ... done
```

## Is it up?
```
docker-compose ps

              Name                            Command               State    Ports
------------------------------------------------------------------------------------
docker_redis_sentinel_master_1     docker-entrypoint.sh redis ...   Up      6379/tcp
docker_redis_sentinel_sentinel_1   entrypoint.sh                    Up      6379/tcp
docker_redis_sentinel_slave_1      docker-entrypoint.sh redis ...   Up      6379/tcp
```

## Check Sentinel Config
```
docker exec -it docker_redis_sentinel_sentinel_1 /bin/sh

cat /etc/redis/sentinel.conf
```

## Check
```
docker-compose ps
              Name                            Command               State    Ports
------------------------------------------------------------------------------------
docker_redis_sentinel_master_1     docker-entrypoint.sh redis ...   Up      6379/tcp
docker_redis_sentinel_sentinel_1   entrypoint.sh                    Up      6379/tcp
docker_redis_sentinel_sentinel_2   entrypoint.sh                    Up      6379/tcp
docker_redis_sentinel_sentinel_3   entrypoint.sh                    Up      6379/tcp
docker_redis_sentinel_slave_1      docker-entrypoint.sh redis ...   Up      6379/tcp
docker_redis_sentinel_slave_2      docker-entrypoint.sh redis ...   Up      6379/tcp
```

## Check Sentinel Info
```
docker-compose exec sentinel redis-cli -p 26000 SENTINEL get-master-addr-by-name mymaster

1) "172.17.0.2"
2) "6379"
```

## Test Failover
```
docker-compose pause master

Pausing docker_redis_sentinel_master_1 ... done

docker-compose exec sentinel redis-cli -p 26000 SENTINEL get-master-addr-by-name mymaster

1) "172.17.0.3"
2) "6379"
```

## Redis Sentinel Cheat Sheet
### Get full info 
```
docker-compose exec sentinel redis-cli -p 26000 info
```

### Get Masters Info
```
docker-compose exec sentinel redis-cli -p 26000 sentinel masters
```

### Force failover
```
docker-compose exec sentinel redis-cli -p 26000 sentinel failover <your cluster id>
```