# RTMP-Server

Deploy your own RTMP Server (nginx-rtmp) & WEB video players (RTMP, HLS) on your domain. docker-compose setup containing:
* `nginx-rtmp` - built from custom [Dockerfile](docker-compose/nginx-rtmp/Dockerfile), based on [tiangolo/nginx-rtmp-docker](https://github.com/tiangolo/nginx-rtmp-docker) (basically nginx with nginx-rtmp plugin from [Sergey Dryabzhinsky's fork](https://github.com/sergey-dryabzhinsky/nginx-rtmp-module)) - acting as a RTMP server, `1935` port exposed.
* `nginx` - just a nginx for WEB endpoints: Simple Flash-based RTMP & HLS players relying on [video.js](https://github.com/videojs/video.js) scripts,`443` & `80` exposed, auto-redirect to HTTPS
* `certbot` - for generating LE TLS certificates and dropping them automatically to the correct place 

## Requirements

* VPS / local server with public IP and ports `80`, `443` & `1935` accessible from public
* `yourdomain.com` domain with DNS A record for `*.yourdomain.com` (more specific `hls.` & `flash.`) pointing to your VPS IP
* `docker-compose` installed

## How to use

1. Ensure that you have all requirements met. Check if subdomains actually points to your public IP:
```
$ dig +noall +answer hls.yourdomain.com
hls.yourdomain.com.               3600    IN      A       51.92.19.220
```

2. Generate TLS certificates for your HTTP domains:
```
$ docker-compose run --rm --service-ports certbot certonly -d hls.yourdomain.com -d flash.yourdomain.com --standalone
```
Note: certbot runs in stand-alone mode - it binds `:80` on your system.

3. Start servers:
```
docker-compose up -d
```

4. Now you can:
* Stream (e.g. by OBS) to your endpoint: `rtmp://rtmp.yourdomain.com/live`
* Watch your stream on Flash-based WEB RTMP player: `https://flash.yourdomain.com`
* Watch your stream via HLS: `https://hls.yourdomain.com`

