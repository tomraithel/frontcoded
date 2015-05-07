---
layout: post
title: "Use NGINX as HTTP proxy to test pages on your VM"

tags: nginx http proxy virtual-box vm
---

> This guide is for OSX machines

1. Install Homebrew [http://brew.sh/](http://brew.sh/)

2. Install NGINX with homebrew: `brew install nginx`

3. Open `/usr/local/etc/nginx/nginx.conf` with your favourite editor and add the server configuration:

	```
	server {
        listen 80;
        # don´t use *.local on mac - its reserved
        server_name www.mywebsite.mobile;

        location / {
                proxy_pass http://www.mywebsite.dev/;
        }
    }
	```

4. Start nginx via `sudo nginx`
5. Optionally: test if your config is OK by typing `sudo nginx -t`
6. Go to the settings of your mobile device and enter the IP of your local machine as HTTP proxy
7. Open `www.mywebsite.mobile` (or whatever you have entered above)
