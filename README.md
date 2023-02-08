# Nginx multistream example
Stream to many sources from nginx

brew tap denji/nginx
brew install nginx-full --with-upload-module

Conflicts
If you have trouble installing this tap then please make sure the default version of nginx is removed first. 
You can do that with the following commands.

brew unlink nginx
brew link nginx-full

nginx.conf

```
#user  nobody;
worker_processes  1;
 
error_log  logs/error.log;
error_log  logs/error.log  notice;
error_log  logs/error.log  info;
 
#pid        logs/nginx.pid;
 
 
events {
    worker_connections  1024;
}
 
 
rtmp {
        server {
                listen 1935;
                chunk_size 4096;
 
                application live {
                        live on;
                        record off;
                        push rtmp://live-ams.twitch.tv/app/STREAMKEY;
                        push rtmp://a.rtmp.youtube.com/live2/STREAMKEY;
                }
        }
}
```

```
==> nginx-full
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

- Tips -
Run port 80:
 $ sudo chown root:wheel /usr/local/opt/nginx-full/bin/nginx
 $ sudo chmod u+s /usr/local/opt/nginx-full/bin/nginx
Reload config:
 $ nginx -s reload
Reopen Logfile:
 $ nginx -s reopen
Stop process:
 $ nginx -s stop
Waiting on exit process
 $ nginx -s quit

To start denji/nginx/nginx-full now and restart at login:
  brew services start denji/nginx/nginx-full
Or, if you don't want/need a background service you can just run:
  nginx
```

brew services list
to check status

Local Server to stream rtmp://localhost/live

brew services restart nginx to update config

For rtmps:
https://dev.to/lax/rtmps-relay-with-stunnel-12d3
