# docker-php

## build

backend build image Dockerfile

```dockerfile

FROM xiaodi93/php:7.3

ARG TZ="Asia/Shanghai"
ENV TZ ${TZ}

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

RUN apk upgrade --update \
    && apk add bash tzdata \
    && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && rm -rf /var/cache/apk/*

# copy source code to image
COPY . /var/www/html

VOLUME ["/var/www/html"]

```

## deploy

`docker-compose.yml`

```
version: "3"

services:
  nginx:
    image: nginx:alpine
    volumes:
      - data:/var/www/html
      - ./default.conf:/etc/nginx/conf.d/default.conf
  admin:
    image: your-image
    volumes:
      - data:/var/www/html

volumes:
  data:

```
