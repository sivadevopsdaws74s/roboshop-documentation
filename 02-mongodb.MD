### MongoDB
* Developer has chosen the database MongoDB. 
* Hence, we are trying to install it up and configure it. </br>
**NOTE: Versions of the DB Software you will get context from the developer, Meaning we need to check with developer. Developer has shared the version information as MongoDB-4.x**

Setup the MongoDB repo file
```
vim /etc/yum.repos.d/mongo.repo
```
```
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=0
enabled=1
```

Install MongoDB
```
yum install mongodb-org -y
```

Start & Enable MongoDB Service

```
systemctl enable mongod
```
```
systemctl start mongod
```

Usually MongoDB opens the port only to localhost(127.0.0.1), meaning this service can be accessed by the application that is hosted on this server only. However, we need to access this service to be accessed by another server(remote server), So we need to change the config accordingly.

Update listen address from 127.0.0.1 to 0.0.0.0 in /etc/mongod.conf

You can edit file by using 
```
vim /etc/mongod.conf
```

Restart the service
```
systemctl restart mongod
```