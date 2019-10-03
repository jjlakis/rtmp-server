[![Build Status](https://travis-ci.com/jjlakis/rtmp-server.svg?branch=master)](https://travis-ci.com/jjlakis/rtmp-server)

# RTMP-Server

Deploy your own RTMP Server (nginx-rtmp) & WEB video players (RTMP, HLS) on your domain. docker-compose setup containing:
* `nginx-rtmp` - built from custom [Dockerfile](docker-compose/nginx-rtmp/Dockerfile), based on [tiangolo/nginx-rtmp-docker](https://github.com/tiangolo/nginx-rtmp-docker) (basically nginx with nginx-rtmp plugin from [Sergey Dryabzhinsky's fork](https://github.com/sergey-dryabzhinsky/nginx-rtmp-module)) - acting as a RTMP server, `1935` port exposed.
* `nginx` - just a nginx for WEB endpoints: Simple Flash-based RTMP & HLS players relying on [video.js](https://github.com/videojs/video.js) scripts,`443` & `80` exposed, auto-redirect to HTTPS
* `certbot` - for generating LE TLS certificates and dropping them automatically to the correct place 

## Requirements

* VPS / local server with public IP and ports `80`, `443` & `1935` accessible from public
* `you.yourdomain.com` domain with DNS A record for pointing to your VPS IP
* `docker-compose` installed

## How to use

1. Ensure that you have all requirements met. Check if subdomains actually points to your public IP:
```
$ dig +noall +answer you.yourdomain.com
you.yourdomain.com.               3600    IN      A       51.92.19.220
```

2. Generate TLS certificates for your HTTP domains:
```
$ docker-compose run --rm --service-ports certbot certonly -d you.yourdomain.com --standalone
```
Note: certbot runs in stand-alone mode - it binds `:80` on your system.

3. Start servers:
```
docker-compose up -d
```

4. Now you can:
* Stream (e.g. by OBS) to your endpoint: `rtmp://rtmp.yourdomain.com/live/<your-key>` & receive RTMP signal from the same endpoint.
* Watch your stream on Flash-based WEB RTMP player: `https://flash.yourdomain.com`
* Watch your stream via HLS: `https://hls.yourdomain.com`


### TODOs:
 - [ ] Information in README.md when no certs needed (or exists)
 - [ ] Check directory for generated certs (certbot CLI)
 - [ ] Find some way to template domains in NGINX config
 - [ ] HLS sharing between services
 - [ ] Condition if we want to use public or local addresses (??)
 - [ ] RTMPS support (stunnel)
 - [ ] Multiple endpoints on the same VPS: starting with vhost-based rtmp endpoint
 - [ ] Helm chart
