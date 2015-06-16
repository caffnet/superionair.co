
---
layout: post
title: "The Enterprise"
subtitle: "So what's under the hood at Superion Air"
cover_image: dc3-i-logo.png
excerpt: "Let's take a peak under the hood at Superion Air"
author:
  name: John Stauffacher
  twitter: g33kspeed
  gplus: 115459133183253104599
  bio: Defensive Security Expert
  image: js.jpg
---

##Superion Air
<span class="firstLetter">T</span>he staff at Superion Air have done quite a bit with not a lot for some time. Their main infrastructure features a cloud enabled application, and a database server. All of these run on a cluster of VMWare ESX 5.5 servers. These servers are aging Dell 2950 servers with dual quad core processors. Two of the machines have 16GB of ram the other has 32GB. Their storage is provided by a commodity iSCSI array.

## The Web Application
<span class="firstLetter">T</span>he key piece of their infrastructure is their website. This website runs under the WordPress CMS platform, and is the main interface between Superion Air and their customers. This Website is where Superion Air books travel for their customers, and updates their pilot schedules and maintenance operations. This website is key to Superion Air, and their current Cloud expense is making management think that they may have to start raising prices. They need to take down the bot traffic to get their Cloud expenses under control. A wpscan report is listed below and available [here]({% postfile client-superionair-co-wpscan.txt %}) :

### WordPress
```
    [+] URL: https://client.superionair.co/
    [+] Started: Fri Jun 05 12:51:58 2015

    [+] robots.txt available under: 'https://client.superionair.co/robots.txt'
    [+] Interesting header: LINK: <https://client.superionair.co/>; rel=shortlink
    [+] Interesting header: SERVER: nginx/1.8.0
    [+] Interesting header: X-POWERED-BY: PHP/5.4.16
    [+] XML-RPC Interface available under: https://client.superionair.co/xmlrpc.php

    [+] WordPress version 4.2.2 identified from meta generator

    [+] WordPress theme in use: theme1770

    [+] Name: theme1770
     |  Location: https://client.superionair.co/wp-content/themes/theme1770/
     |  Style URL: https://client.superionair.co/wp-content/themes/theme1770/style.css
     |  Theme Name: theme1770
     |  Theme URI: https://superionair.co
     |  Description: Superion Air
     |  Author: Superion Air

    [+] Enumerating plugins ...
    [+] We found 7 plugins:

    [+] Name: akismet - v3.1.2
     |  Location: https://client.superionair.co/wp-content/plugins/akismet/
     |  Readme: https://client.superionair.co/wp-content/plugins/akismet/readme.txt

    [+] Name: booking
     |  Location: https://client.superionair.co/wp-content/plugins/booking/

    [+] Name: cherry-plugin
     |  Location: https://client.superionair.co/wp-content/plugins/cherry-plugin/

    [+] Name: contact-form-7 - v4.2
     |  Location: https://client.superionair.co/wp-content/plugins/contact-form-7/
     |  Readme: https://client.superionair.co/wp-content/plugins/contact-form-7/readme.txt

    [+] Name: motopress-content-editor
     |  Location: https://client.superionair.co/wp-content/plugins/motopress-content-editor/

    [+] Name: revslider
     |  Location: https://client.superionair.co/wp-content/plugins/revslider/

    [+] Name: wordpress-importer - v0.6.1
     |  Location: https://client.superionair.co/wp-content/plugins/wordpress-importer/
     |  Readme: https://client.superionair.co/wp-content/plugins/wordpress-importer/readme.txt

    [+] Enumerating installed themes  ...
    [+] We found 2 themes:

    [+] Name: slide
     |  Location: https://client.superionair.co/wp-content/themes/slide/
     |  Style URL: https://client.superionair.co/wp-content/themes/slide/style.css
     |  Description: 

    [+] Name: theme1770
     |  Location: https://client.superionair.co/wp-content/themes/theme1770/
     |  Style URL: https://client.superionair.co/wp-content/themes/theme1770/style.css
     |  Theme Name: theme1770
     |  Theme URI: https://superionair.co
     |  Description: Superion Air
     |  Author: Superion Air

    [+] Finished: Fri Jun 05 12:56:17 2015
    [+] Requests Done: 2022
    [+] Memory used: 38.754 MB
    [+] Elapsed time: 00:04:19
```
### Webserver 
<span class="firstLetter">T</span>he team at Superion Air decided to go ahead and rely on Nginx for their main webserving needs. Nginx is quick, compact and doesn't have the memory footprint of Apache. The nginx config is listed below and available [here]({% postfile superion-air-nginx.conf %}) 
```
    user  nginx;
    worker_processes  1;

    error_log  /var/log/nginx/error.log warn;
    pid        /var/run/nginx.pid;


    events {
        worker_connections  1024;
    }


    http {
        include       /etc/nginx/mime.types;
        default_type  application/octet-stream;

        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';

        access_log  /var/log/nginx/access.log  main;
        client_max_body_size 50M;
        sendfile        on;
        #tcp_nopush     on;

        keepalive_timeout  65;

        #gzip  on
        server {
            listen       443 ssl;
            server_name  client.superionair.co;

            ssl_certificate      /etc/nginx/cert.pem;
            ssl_certificate_key  /etc/nginx/cert.key;

            ssl_session_cache shared:SSL:1m;
            ssl_session_timeout  5m;

            ssl_ciphers  HIGH:!aNULL:!MD5;
            ssl_prefer_server_ciphers   on;
            access_log  /var/log/nginx/host.ssl.access.log  main;
            root /opt/superionair-test/wordpress;
            index index.php index.html index.htm;
            location / {
             try_files $uri $uri/ /index.php?$args;
            }
            location ~ \.php$ {
                fastcgi_split_path_info ^(.+?\.php)(/.*)$;
                if ( !-f $document_root$fastcgi_script_name) {
                        return 404;
                }
                fastcgi_pass   127.0.0.1:9000;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include        fastcgi_params;
            }

        
        }
    }

```


### Database Backend
<span class="firstLetter">T</span>he backend database is served by a MySQL database. 



