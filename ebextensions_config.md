
Configuration to redirect http to https and remove www of URL.

Test success in AWS using Elastic Beanstalk with Tomcat.

Eg:
```bash
http://reinaldopinto.com.br - http://www.reinaldopinto.com.br ->> https://reinaldopinto.com.br
```

ebextensions config:

```
RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Proto} ^http$ [OR]
RewriteCond %{HTTP_HOST} ^www\. [NC]
RewriteCond %{SERVER_NAME} ^(www\.)?(.*)$ [NC]
RewriteRule ^/?(.*)$ https://%2/$1 [L,R=301]
```
