# multistream_example
Stream to many sources from nginx

_______________________________________________________________
nginx.conf
_______________________________________________________________


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
