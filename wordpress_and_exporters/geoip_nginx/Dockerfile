FROM ubuntu:20.04

RUN apt-get update \
 && export DEBIAN_FRONTEND=noninteractive \
 && apt-get install -y mc vim libgd-dev libxml2-dev libxslt1-dev libssl-dev zlib1g-dev sudo binutils build-essential automake curl libmaxminddb-dev autoconf libtool libgeoip-dev libpcre3 libpcre3-dev libpcre2-posix2 libpcre2-dev \
 && cd /opt \
 && curl -L https://github.com/maxmind/libmaxminddb/releases/download/1.5.2/libmaxminddb-1.5.2.tar.gz | tar xz \
 && curl -L https://github.com/leev/ngx_http_geoip2_module/archive/master.tar.gz | tar xz \
 && curl -L http://nginx.org/download/nginx-1.18.0.tar.gz | tar xz \
 && chmod -R 755 /opt/ \
 && cd /opt/libmaxminddb-1.5.2 \
 && ./configure \
 && make \
 && sudo make install \
 && cd /opt/nginx-1.18.0 \
 && ./configure --prefix=/usr/local/nginx --with-compat --add-dynamic-module=/opt/ngx_http_geoip2_module-master \
    --with-cc-opt='-g -O2 -fdebug-prefix-map=/build/nginx-lUTckl/nginx-1.18.0=. \
    -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time \
    -D_FORTIFY_SOURCE=2' \
    --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -fPIC' \
    --conf-path=/etc/nginx/nginx.conf \
    --http-log-path=/var/log/nginx/access.log \
    --error-log-path=/var/log/nginx/error.log \
    --lock-path=/var/lock/nginx.lock \
    --pid-path=/run/nginx.pid \
    --modules-path=/usr/lib/nginx/modules \
    --http-client-body-temp-path=/var/lib/nginx/body \
    --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
    --http-proxy-temp-path=/var/lib/nginx/proxy \
    --http-scgi-temp-path=/var/lib/nginx/scgi \
    --http-uwsgi-temp-path=/var/lib/nginx/uwsgi \
    --with-debug \
    --with-pcre-jit \
    --with-http_ssl_module \
    --with-http_stub_status_module \
    --with-http_realip_module \
    --with-http_auth_request_module \
    --with-http_v2_module \
    --with-http_dav_module \
    --with-http_slice_module \
    --with-threads \
    --with-http_addition_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_image_filter_module=dynamic \
    --with-http_sub_module \
    --with-http_xslt_module=dynamic \
    --with-stream=dynamic \
    --with-stream_ssl_module \
    --with-mail=dynamic \
    --with-mail_ssl_module \
 && mkdir -p /var/lib/nginx/ \
 && make modules \
 && make install \
 && mkdir -p /usr/local/nginx/modules/ \
 && cp /usr/lib/nginx/modules/* /usr/local/nginx/modules/ \
 && rm -rf /etc/nginx/nginx.conf
EXPOSE 80 8080

COPY GeoLite2-Country.mmdb /usr/share/GeoIP
#ENTRYPOINT ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]


