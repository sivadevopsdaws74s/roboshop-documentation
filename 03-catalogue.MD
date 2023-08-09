### Catalogue
Catalogue is a microservice that is responsible for serving the list of products that displays in roboshop application.

Setup NodeJS repos. Vendor is providing a script to setup the repos.

```
curl -sL https://rpm.nodesource.com/setup_lts.x | bash
```

Install NodeJS

```
yum install nodejs -y
```

Configure the application.

Our application developed by the developer of our company and it is not having any RPM software just like other softwares. So we need to configure every step manually

Add application User

```
useradd roboshop
```

User roboshop is a function / daemon user to run the application. Apart from that we don't use this user to login to server.

Also, username roboshop has been picked because it more suits to our project name.

We keep application in one standard location. This is a usual practice that runs in the organization.

Lets setup an app directory.

```
mkdir /app
```

Download the application code to created app directory.

```
curl -o /tmp/catalogue.zip https://roboshop-builds.s3.amazonaws.com/catalogue.zip
```
```
cd /app 
```
```
unzip /tmp/catalogue.zip
```

Every application is developed by development team will have some common softwares that they use as libraries. This application also have the same way of defined dependencies in the application configuration.

Lets download the dependencies.

```
cd /app
```
```
npm install 
```

We need to setup a new service in systemd so systemctl can manage this service

Setup SystemD Catalogue Service

```
vim /etc/systemd/system/catalogue.service
```

```
[Unit]
Description = Catalogue Service

[Service]
User=roboshop
Environment=MONGO=true
Environment=MONGO_URL="mongodb://<MONGODB-SERVER-IPADDRESS>:27017/catalogue"
ExecStart=/bin/node /app/server.js
SyslogIdentifier=catalogue

[Install]
WantedBy=multi-user.target
```

Load the service.

```
systemctl daemon-reload
```

Start the service.

```
systemctl enable catalogue
```
```
systemctl start catalogue
```

* For the application to work fully functional we need to load schema to the Database.
* Schemas are usually part of application code and developer will provide them as part of development.


We need to load the schema. To load schema we need to install mongodb client.

To have it installed we can setup MongoDB repo and install mongodb-client

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
```
yum install mongodb-org-shell -y
```

Load Schema

```
mongo --host MONGODB-SERVER-IPADDRESS </app/schema/catalogue.js
```

**NOTE: You need to update catalogue server ip address in frontend configuration. Configuration file is /etc/nginx/default.d/roboshop.conf**