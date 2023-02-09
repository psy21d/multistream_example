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
                        push rtmp://a.rtmp.youtube.com/live2/STREAMKEY2;
                        # push rtmps://127.0.0.1:19350/app/STREAMKEY;
                        # push rtmps://127.0.0.1:19351/live2/STREAMKEY2;
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

```
brew services list
```

to check status

Local Server to stream rtmp://localhost/live

```
brew services restart nginx 
```
to update config

For rtmps:
https://dev.to/lax/rtmps-relay-with-stunnel-12d3

# Install stunnel
brew install stunnel

nano /etc/stunnel/stunnel.conf
```
# Stunnel basic config
[live-ams.twitch.tv]
client = yes
accept = 127.0.0.1:19350
connect = live-ams.twitch.tv:1935;
verifyChain = no

[a.rtmp.youtube.com]
client = yes
accept = 127.0.0.1:19351
connect = a.rtmp.youtube.com:1935;
verifyChain = no

```

A bogus SSL server certificate has been installed to:
  /usr/local/etc/stunnel/stunnel.pem

This certificate will be used by default unless a config file says otherwise!
Stunnel will refuse to load the sample configuration file if left unedited.

In your stunnel configuration, specify a SSL certificate with
the "cert =" option for each service.

To use Stunnel with Homebrew services, make sure to set "foreground = yes" in
your Stunnel configuration.

```
To restart stunnel after an upgrade:
  brew services restart stunnel
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/stunnel/bin/stunnel
```

```
cd /usr/local/etc/stunnel/
cp stunnel.conf-sample stunnel.conf
```

Logs: 
```
/usr/local/var/log/stunnel.log
```  
  

https://docs.nginx.com/nginx/admin-guide/basic-functionality/runtime-control/#master-and-worker-processes

Because is it NOT WORK with telegram:
```
open -n -a OBS.app
```

https://github.com/sallar/mac-local-rtmp-server/releases
