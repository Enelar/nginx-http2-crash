FROM nginx as build
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]
COPY localhost* /
COPY default.conf /etc/nginx/conf.d/default.conf