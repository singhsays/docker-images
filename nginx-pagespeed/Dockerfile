FROM ubuntu:yakkety
EXPOSE 80
EXPOSE 443

ARG BUILD_DIR=/usr/src

RUN mkdir -p ${BUILD_DIR}
COPY pagespeed-deps-linux-amd64 ${BUILD_DIR}/pagespeed-deps

# Enable deb-src for build-dep
RUN sed -ie "s/^# deb-src/deb-src/" /etc/apt/sources.list && \
    apt-get update && \
    apt-get -y --no-install-recommends install openssl ca-certificates && \
    apt-get -y --no-install-recommends build-dep nginx && \
    ${BUILD_DIR}/pagespeed-deps && \
    cd ${BUILD_DIR}/nginx && make && make install

RUN mkdir -p /var/cache/nginx && \
    mkdir -p /var/cache/ngx_pagespeed && \
    cd && rm-rf ${BUILD_DIR} && \
    apt-get -y autoremove

COPY nginx.conf /etc/nginx/nginx.conf
CMD ["nginx", "-g", "daemon off;"]
