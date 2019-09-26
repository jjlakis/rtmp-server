# RTMP-Server

Deploy your own RTMP Server (nginx-rtmp) & WEB video players (RTMP, HLS) on your domain. docker-compose setup containing:
* `nginx-rtmp` - built from custom [Dockerfile](docker-compose/nginx-rtmp/Dockerfile), based on [tiangolo/nginx-rtmp-docker](https://github.com/tiangolo/nginx-rtmp-docker) (basically nginx with nginx-rtmp plugin) - acting as a RTMP server, `1935` port exposed.
* `nginx` - just a nginx for WEB endpoints: Simple Flash-based RTMP & HLS players relying on [video.js](https://github.com/videojs/video.js) scripts,`443` & `80` exposed, auto-redirect to HTTPS
* `certbot` - for generating LE TLS certificates and dropping them automatically to the correct place 

## Requirements

* VPS server with public IP
* `yourdomain.com` domain with DNS A records for `yourdomain.com` & `*.yourdomain.com` pointing to your VPS IP
* `docker-compose` installed
