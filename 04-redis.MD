### Redis
Redis is used for in-memory data storage(Caching) and allows users to access the data of database over API.

Redis is offering the repo file as a rpm. Lets install it

```
yum install https://rpms.remirepo.net/enterprise/remi-release-8.rpm -y
```

Enable Redis 6.2 from package streams.

```
yum module enable redis:remi-6.2 -y
```

Install Redis

```
yum install redis -y 
```

Usually Redis opens the port only to localhost(127.0.0.1), meaning this service can be accessed by the application that is hosted on this server only. However, we need to access this service to be accessed by another server, So we need to change the config accordingly.

Update listen address from 127.0.0.1 to 0.0.0.0 in /etc/redis.conf & /etc/redis/redis.conf

```
vim /etc/redis.conf
```

Start & Enable Redis Service

```
systemctl enable redis
```

```
systemctl start redis
```