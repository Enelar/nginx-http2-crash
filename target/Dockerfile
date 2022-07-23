FROM alpine:3.14 AS build
RUN apk add git wget gcc musl-dev pcre-dev zlib-dev make
RUN wget https://nginx.org/download/nginx-1.23.0.tar.gz
RUN tar zxvf ./nginx-1.23.0.tar.gz
RUN git clone https://github.com/nginx-modules/ngx_http_early_hints
RUN apk add brotli
RUN git clone https://github.com/google/ngx_brotli.git && cd /ngx_brotli && git submodule update --init --recursive
WORKDIR /nginx-1.23.0/
RUN ./configure \
    --add-module=/ngx_brotli/ \
    --add-module=/ngx_http_early_hints/ \
    --prefix=/ \
    --pid-path=/nginx.pid \
    --conf-path=/etc/nginx/nginx.conf \
    --sbin-path=/usr/local/sbin/nginx \
    --error-log-path=stderr \
    --http-log-path=/dev/stdout \
    --with-http_v2_module \
    --with-http_gzip_static_module
RUN make
RUN make install
RUN mkdir -p /out/etc /out/sbin
RUN mv /usr/local/sbin/nginx /out/sbin
RUN mv /etc/nginx /out/etc/nginx
COPY nginx.conf /out/etc/nginx/nginx.conf
COPY default.conf /out/etc/nginx/conf.d/default.conf

FROM alpine:3.14
RUN apk add --no-cache pcre
ENTRYPOINT ["nginx", "-c", "/etc/nginx/nginx.conf", "-g", "daemon off;"]
COPY --from=build /out  /
RUN nginx -c /etc/nginx/nginx.conf -t
COPY . /html