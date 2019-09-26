# RTMP-Server

Deploy your own RTMP Server (nginx-rtmp) & WEB video players (RTMP, HLS) on your domain. 
It's docker-compose setup with two nginx instances:
* `nginx-rtmp` - built from custom [Dockerfile](nginx-rtmp/Dockerfile), based on [tiangolo/nginx-rtmp-docker](https://github.com/tiangolo/nginx-rtmp-docker) - acting as a RTMP server, with `1935` port exposed.
* `nginx` - for WEB endpoints: Simple Flash-based RTMP & HLS players relying on [video.js](https://github.com/videojs/video.js) scripts.

## Requirements

* VPS server with public IP
* `yourdomain.com` domain with DNS A records for `yourdomain.com` & `*.yourdomain.com` pointing to your VPS IP
* `docker-compose` installed
