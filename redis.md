## download
```
wget http://download.redis.io/releases/redis-4.0.11.tar.gz
tar -xzvf redis-4.0.11.tar.gz -C /usr/local
cd /usr/local
mv redis-4.0.11 redis
cd redis
make
make PREFIX=/usr/local/redis install
```
## env path
```
export REDIS_HOME=/usr/local/redis
export PATH=$PATH:$REDIS_HOME/src

```

## start master
```
redis-server /codeyuguo/redis/conf/master.conf
```

## start slave
```
redis-server /codeyuguo/redis/conf/slave.conf
```

## start sentinel
```
redis-sentinel /codeyuguo/redis/conf/sentinel.conf
```

## connect redis
```
redis-cli -h ip
auth mypwd
info replication
```