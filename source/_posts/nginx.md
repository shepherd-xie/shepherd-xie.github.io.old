---
title: Nginx
date: 2019-04-25 22:17:00
tags: 
categories: 
top:
---

# Nginx

<!-- more -->

```yml
version: '3.4'
services:
  nginx:
    restart: always
    image: nginx
    container_name: nginx
    # network_mode: host
    ports:
      - 80:80
    volumes:
      - ./conf/nginx.conf:/etc/nginx/nginx.conf
      - ./wwwroot:/usr/share/nginx/wwwroot
```

```conf
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;

    keepalive_timeout  65;
    client_max_body_size 0;

    server {
        listen       80;
        server_name  192.168.80.135;

        location / {
            add_header Access-Control-Allow-Origin *;
            add_header Access-Control-Allow-Headers X-Requested-With;
            add_header Access-Control-Allow-Methods GET,POST,OPTIONS;

            root   /usr/share/nginx/wwwroot;
            index  index.html index.htm;
        }
    }

    server {
        listen 80;
        server_name git.orkva.com;

        location / {
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://192.168.80.135:8080;
        }
    }

    server {
        listen 80;
        server_name hub.orkva.com;

        location / {
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://192.168.80.135:8082;
        }
    }

    server {
        listen 80;
        server_name registry.orkva.com;

        location / {
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://192.168.80.135:5000;
        }
    }

    server {
        listen 80;
        server_name mvn.orkva.com;

        location / {
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://47.93.241.176:8081;
        }
    }
}
```