---
title: Setup SSL for Nginx using LetsEncrypt
date: 2023-08-18 22:24:00 +0700
categories: [server_administrator]
tags: [ssl, nginx, letsencrypt]
---

```console
domain=<your domain>
sudo mkdir -p /var/www/$domain/html
sudo chown -R $USER:$USER /var/www/$domain
sudo chmod -R 755 /var/www/$domain
sudo nano /var/www/$domain/html/index.html
```

```html
<html>
    <head>
        <title>Welcome!</title>
    </head>
    <body>
        <h1>Success!</h1>
    </body>
</html>
```

```console
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot certonly --nginx --agree-tos -d $domain
```

```console
sudo nano /etc/nginx/sites-available/$domain
```

```txt
server {
	server_name <your domain>;

	listen 443 ssl;
	ssl_certificate     /etc/letsencrypt/live/<your domain>/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/<your domain>/privkey.pem;

	root /var/www/<your domain>;
	index index.html index.htm index.nginx-debian.html;

	location / {
		try_files $uri $uri/ =404;
	}
}
```
```console
sudo ln -s /etc/nginx/sites-available/$domain /etc/nginx/sites-enabled/
sudo nano /etc/nginx/nginx.conf
```

```txt
uncomment this line below (ctrl + w for find)
server_names_hash_bucket_size 64;
```
```console
sudo nginx -t
sudo service nginx reload
sudo ufw allow "Nginx HTTPS"
curl -vI https://$domain
```

