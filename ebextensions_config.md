
Configuration to redirect http to https and remove www of URL.

Test success in AWS using Elastic Beanstalk with Tomcat.

Eg:
```bash
http://reinaldopinto.com.br - http://www.reinaldopinto.com.br ->> https://reinaldopinto.com.br
```

#### Step 1
Create your application with the directory/folder:
```
./ebextensions/httpd/conf.d/
ssl.conf
```

#### Step 2
File ssl.config with configuration bellow:
```
<VirtualHost *:80>
  RewriteEngine On
  RewriteCond %{HTTP:X-Forwarded-Proto} ^http$ [OR]
  RewriteCond %{HTTP_HOST} ^www\. [NC]
  RewriteCond %{SERVER_NAME} ^(www\.)?(.*)$ [NC]
  RewriteRule ^/?(.*)$ https://%2/$1 [L,R=301]
  <Proxy *>
    Order Allow, Deny
    Allow from all
  </Proxy>
  ProxyPass / http://localhost:8080/ retry=0
  ProxyPassReverse / http://localhost:8080/
  ProxyPreserveHost on
  
  ErroLog /var/log/httpd/elasticbeanstalk-erro_log
</VirtualHost>
```

#### Step 3
Configure ports ELB in EB to allow:
```
port 80 ->> port 80
port 443 ->> port 80
```

#### Step 4
- Build application.
- Do the upload of package.
- Change the type of deployment in "Rolling updates and deployments" to Immutable.
- To do a new deploy in your environment (Elastic Beanstalk) with the new package/version.

