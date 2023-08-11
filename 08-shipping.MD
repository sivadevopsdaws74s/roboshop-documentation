### Shipping

Shipping service is responsible for finding the distance of the package to be shipped and calculate the price based on that.

Shipping service is written in Java, Hence we need to install Java.

Maven is a Java Packaging software, Hence we are going to install maven, This indeed takes care of java installation.

```
yum install maven -y
```

Configure the application.

Add application User

```
useradd roboshop
```

Lets setup an app directory.

```
mkdir /app
```

Download the application code to created app directory.

```
curl -L -o /tmp/shipping.zip https://roboshop-builds.s3.amazonaws.com/shipping.zip
```
```
cd /app
```
```
unzip /tmp/shipping.zip
```

Every application is developed by development team will have some common softwares that they use as libraries. This application also have the same way of defined dependencies in the application configuration.

Lets download the dependencies & build the application

```
cd /app
```
```
mvn clean package
```
```
mv target/shipping-1.0.jar shipping.jar
```

We need to setup a new service in systemd so systemctl can manage this service

Setup SystemD Shipping Service

```
/etc/systemd/system/shipping.service
```

```
[Unit]
Description=Shipping Service

[Service]
User=roboshop
Environment=CART_ENDPOINT=<CART-SERVER-IPADDRESS>:8080
Environment=DB_HOST=<MYSQL-SERVER-IPADDRESS>
ExecStart=/bin/java -jar /app/shipping.jar
SyslogIdentifier=shipping

[Install]
WantedBy=multi-user.target
```

Load the service.

```
systemctl daemon-reload
```

Start the service.

```
systemctl enable shipping 
```
```
systemctl start shipping
```

For this application to work fully functional we need to load schema to the Database.

We need to load the schema. To load schema we need to install mysql client.

To have it installed we can use

```
yum install mysql -y 
```

Load Schema

```
mysql -h <MYSQL-SERVER-IPADDRESS> -uroot -pRoboShop@1 < /app/schema/shipping.sql 
```

This service needs a restart because it is dependent on schema, After loading schema only it will work as expected, Hence we are restarting this service. This

```
systemctl restart shipping
```