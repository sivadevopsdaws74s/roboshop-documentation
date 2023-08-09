# 01-WEB

* The Web/Frontend is the service in RoboShop to serve the web content over Nginx. 
* This will have the web page for the web application.
* Developer has chosen Nginx as a web server and thus we will install Nginx Web Server. 

Install Nginx 
```
yum install nginx -y
```

Start & Enable Nginx service 
```
systemctl enable nginx
```
```
systemctl start nginx
```

Try to access the service once over the browser and ensure you get some default content.
```
http://<public-IP>:80
```

Remove the default content that web server is serving. 

```
rm -rf /usr/share/nginx/html/*
```

Download the frontend content

```
curl -o /tmp/web.zip https://roboshop-builds.s3.amazonaws.com/web.zip
```

Extract the frontend content.

```
cd /usr/share/nginx/html
```
```
unzip /tmp/web.zip
```

Try to access the nginx service once more over the browser and ensure you get roboshop content.

Create Nginx Reverse Proxy Configuration.

``` 
vim /etc/nginx/default.d/roboshop.conf 
```

Add the following content 
```
vim /etc/nginx/default.d/roboshop.conf 
```

```
proxy_http_version 1.1;
location /images/ {
  expires 5s;
  root   /usr/share/nginx/html;
  try_files $uri /images/placeholder.jpg;
}
location /api/catalogue/ { proxy_pass http://localhost:8080/; }
location /api/user/ { proxy_pass http://localhost:8080/; }
location /api/cart/ { proxy_pass http://localhost:8080/; }
location /api/shipping/ { proxy_pass http://localhost:8080/; }
location /api/payment/ { proxy_pass http://localhost:8080/; }

location /health {
  stub_status on;
  access_log off;
}
```

Ensure you replace the `localhost` with the actual ip address of those component server. Word `localhost` is just used to avoid the failures on the Nginx Server.

Restart Nginx Service to load the changes of the configuration.

``` 
systemctl restart nginx 
```