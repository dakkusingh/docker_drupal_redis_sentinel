## Setup Redis Sentinel in Docker
### Sentinel Conf
The sentinels are configured with *mymaster* instance and following properties 
see ./docker-config/sentinel/Dockerfile

```
ENV SENTINEL_QUORUM 2
ENV SENTINEL_DOWN_AFTER 5000
ENV SENTINEL_FAILOVER 10000
ENV SENTINEL_PORT 26000
ENV SENTINEL_PAR_SYNC 1
```

### Check Sentinel Config
```
docker exec -it docker_redis_sentinel_sentinel_1 /bin/sh

cat /etc/redis/sentinel.conf
```


### Check Sentinel Info
```
docker-compose exec sentinel redis-cli -p 26000 SENTINEL get-master-addr-by-name mymaster

1) "172.17.0.2"
2) "6379"
```

### Test Failover
```
docker-compose pause master

Pausing docker_redis_sentinel_master_1 ... done
```

Check info again to confirm failover.
```
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

### Check Cache Keys
```
docker exec -it docker_redis_sentinel_master_1 redis-cli KEYS '*'

or

docker-compose exec master redis-cli KEYS '*'
```