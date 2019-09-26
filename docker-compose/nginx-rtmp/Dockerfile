FROM buildpack-deps:stretch

# Maintainer kept 
LABEL Description="Based on tiangolo/nginx-rtmp-docker"

# Versions of Nginx and nginx-rtmp-module to use
ENV NGINX_VERSION nginx-1.17.0
ENV NGINX_RTMP_MODULE_COMMIT 3bf75232676da7eeff85dcd0fc831533a5eafe6b

# Install dependencies
RUN apt-get update && \
    apt-get install -y ca-certificates openssl libssl-dev && \
    rm -rf /var/lib/apt/lists/*

# Download and decompress Nginx
RUN mkdir -p /tmp/build/nginx && \
    cd /tmp/build/nginx && \
    wget -O ${NGINX_VERSION}.tar.gz https://nginx.org/download/${NGINX_VERSION}.tar.gz && \
    tar -zxf ${NGINX_VERSION}.tar.gz

# Download and decompress RTMP module
RUN mkdir -p /tmp/build/nginx-rtmp-module && \
    cd /tmp/build/nginx-rtmp-module && \
    git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module.git && \
    cd nginx-rtmp-module && \
    git reset --hard ${NGINX_RTMP_MODULE_COMMIT}


# Build and install Nginx
# The default puts everything under /usr/local/nginx, so it's needed to change
# it explicitly. Not just for order but to have it in the PATH
RUN cd /tmp/build/nginx/${NGINX_VERSION} && \
    ./configure \
        --sbin-path=/usr/local/sbin/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --pid-path=/var/run/nginx/nginx.pid \
        --lock-path=/var/lock/nginx/nginx.lock \
        --http-log-path=/var/log/nginx/access.log \
        --http-client-body-temp-path=/tmp/nginx-client-body \
        --with-http_ssl_module \
        --with-threads \
        --with-ipv6 \
        --add-module=/tmp/build/nginx-rtmp-module/nginx-rtmp-module && \
    make -j $(getconf _NPROCESSORS_ONLN) && \
    make install && \
    mkdir /var/lock/nginx

# Forward logs to Docker
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log

# Set up config file
COPY nginx.conf /etc/nginx/nginx.conf

# Set up sites such as stats webpage file
RUN mkdir -p /var/www/html && \
    cp /tmp/build/nginx-rtmp-module/nginx-rtmp-module/stat.xsl /var/www/html/stat.xsl

COPY crossdomain.xml /var/www/html/crossdomain.xml
COPY default /etc/nginx/sites-enabled/default

# remove temp files
RUN rm -rf /tmp/build

EXPOSE 1935
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
