---
layout: post
title: Securing Graylog with NGINX
date: 2019-10-07 17:22:10
author: Peter Stevenson
summary: Using NGIX reverse proxy to secure Graylog.
categories: networking
thumbnail:
tags:
 - Linux
 - NGINX
 - security
---

# Securing Graylog

## Option 1 (Modifying the local nginx server on Graylog)

Create a password file for basic auth.

```sh
ubuntu@graylog:/etc/nginx/sites-enabled$ sudo vim /etc/nginx/.htpasswd
```

Add a .htpasswd user and password. In this case username is foo and password is bar.

```
foo:$apr1$vqJ3Ea4I$fHhdVhmLBKflt.3wyKpLu/
```

Change default site on nginx.

```sh
ubuntu@graylog:/etc/nginx/sites-enabled$ sudo vim default
```

Should look like

```
server {
      listen 80;
      location / {
        proxy_pass http://127.0.0.1:9000/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass_request_headers on;
        proxy_connect_timeout 150;
        proxy_send_timeout 100;
        proxy_read_timeout 100;
        proxy_buffering off;
        client_max_body_size 8m;
        client_body_buffer_size 128k;
        expires off;
      }
      error_page 502 /502.html;
      location  /502.html {
        internal;
      }

        # 2E0PGS changes
        location /sgelf {
                proxy_pass http://127.0.0.1:12202/gelf;
                auth_basic "Restricted Area";
                auth_basic_user_file /etc/nginx/.htpasswd;
        }
}
```

Restart the service

```sh
ubuntu@graylog:/etc/nginx/sites-enabled$ sudo service nginx restart
```

if you get errors check syslog.

### Checking it (CURL)

Normal http gelf on port 12202

```sh
peter@desktop:~$ curl 192.168.1.22:12202/gelf -X POST -H 'Content-Type: application/json' -d '{ "short_message": "A short message", "level": 5 }'
```

Checking http reverse proxy on port 80

```sh
peter@desktop:~$ curl 192.168.1.22:80/sgelf -X POST -H 'Content-Type: application/json' -d '{ "short_message": "from sgelf", "level": 5 }'
```

```html
<html>
<head><title>401 Authorization Required</title></head>
<body bgcolor="white">
<center><h1>401 Authorization Required</h1></center>
<hr><center>nginx/1.14.0 (Ubuntu)</center>
</body>
</html>
```

Try with auth now

```sh
peter@desktop:~$ curl 192.168.1.22:80/sgelf -X POST -H 'Content-Type: application/json' -H "Authorization: Basic $(echo -n foo:bar | base64)" -d '{ "short_message": "from sgelf with pass", "level": 5 }'
```

## Option 2 (Separate ngix proxy server) (recommended method)

Create a new site in sites-available named after your domain.

Add the following.

```
server {
        server_name logger.example.com;
        location / {
                proxy_pass http://192.168.1.22:12202/gelf;
                auth_basic "Restricted Area";
                auth_basic_user_file /etc/nginx/.htpasswd;
        }
}
```

Symlink that site into sites-enabled to enable it.

Remove default-site.

### Add Let's Encrypt SSL

```sh
sudo add-apt-repository ppa:certbot/certbot

sudo apt-get update

sudo apt install python-certbot-nginx

sudo certbot --nginx -d logger.example.com
```

### Checking it (CURL)

```sh
peter@desktop:~$ curl https://logger.example.com -X POST -H 'Content-Type: application/json' -H "Authorization: Basic $(echo -n foo:bar | base64)" -d '{ "short_message": "https reverse basic auth proxy", "level": 5 }'
```

## Library for C#

I wrote a C# library for Graylog which also supports reverse proxies: [graylog-client](https://bitbucket.org/2E0PGS/graylog-client)

### Example

```csharp
using System;
using System.Diagnostics;

namespace ConsoleApp4
{
    class Program
    {
        static void Main(string[] args)
        {
            Graylog.Client.GraylogClient graylogClient = new Graylog.Client.GraylogClient("https://logger.example.com", "", "foo", "bar");

            int i = 0;
            while (i <= 5)
            {
                Stopwatch stopwatch = new Stopwatch();
                stopwatch.Start();
                try
                {
                    graylogClient.Log("library", 3, "200ms test: " + i.ToString());
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Exception caught: " + ex.ToString());
                }
                stopwatch.Stop();
                Console.WriteLine("Time elapsed in ms: " + stopwatch.ElapsedMilliseconds);
                i++;
                System.Threading.Thread.Sleep(200);
            }
        }
    }
}
```

## Finally

I then suggest whitelisting this behind a firewall or using it across a VPN for added security.
