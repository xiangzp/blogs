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



