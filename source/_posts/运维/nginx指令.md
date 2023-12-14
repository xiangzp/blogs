---
title: nginx指令
tags: 运维
categories: 运维
date: 2020/3/12 22:14
---

**systemctl**

```shell
sudo systemctl status nginx

sudo systemctl stop nginx

sudo systemctl start nginx

# Gracefully Restart Nginx
sudo systemctl reload nginx

#Force Restart Nginx
sudo systemctl restart nginx
```



**nginx**

```shell
# start
sudo nginx

#restart
sudo /etc/init.d/nginx restart
sudo nginx -s restart

#stop
sudo nginx -s stop

sudo nginx -s reload

sudo nginx -s quit
```



