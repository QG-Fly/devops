
## install mongo
```
cd /codeyuguo/packages
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.4.20.tgz
tar -zxvf mongodb-linux-x86_64-3.4.20.tgz
mv mongodb-linux-x86_64-3.4.20 /usr/local/mongodb
```
## config path
```
vi /etc/profile
## add command
export MONGODB_HOME=/usr/local/mongodb
export PATH=$PATH:$MONGODB_HOME/bin
source /etc/profile
```

## copy mongodb.conf
cp mongo-config/mongodb.conf /usr/local/mongodb/bin

## create data and logs dir
```
mkdir -p /codeyuguo/db/data
chmod -R 777 /codeyuguo/db/data

## create keyfile
openssl rand -base64 90 -out ./keyfile
chmod  600  keyfile

mkdir -p /codeyuguo/db/logs
chmod -R 777 /codeyuguo/db/logs
cd logs
touch mongodb.log
```

## start mongo service
```
mongo -f /usr/local/mongodb/bin/mongodb.conf
```

## config auth and replSet

```
mongo
use admin
db.createUser({user:"admin",pwd:"password",roles:[{role:"userAdminAnyDatabase",db:"admin"}]})
db.auth("admin","password")
db.grantRolesToUser( "admin" , [ { role: "dbOwner", db: "admin" },{ "role": "clusterAdmin", "db": "admin" },{ "role": "userAdminAnyDatabase", "db": "admin" },{ "role": "dbAdminAnyDatabase", "db": "admin" }])
rs.initiate()
rs.add('ip:port') ## add secondary
rs.add('ip:port',true) ## add arbiter
rs.status()
use codeyuguo
db.createUser({user:"codeyuguo",pwd:"password",roles:[{"role":"readWrite","db":"codeyuguo"},{"role":"dbAdmin","db":"codeyuguo"},{"role":"dbOwner","db":"codeyuguo"},{"role":"read","db":"codeyuguo"}]})
```