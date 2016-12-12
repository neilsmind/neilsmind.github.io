---
layout: post
title: Running swagger editor locally (and all the time)
date: 2016-12-12 9:30:00 -500
---

It seems that [Docker Compose](https://docs.docker.com/compose/) is becoming my friend
for all things local. I needed to have Swagger Editor available to me locally at
all times and here's how I accomplished that with a docker-compose.yml file:

```
> mkdir swagger
> cd swagger
```

Create the following docker-compose.yml in that directory:

```
version: '2'
networks:
  public-network:
    external:
      name: proxy

services:
  web:
    image: swaggerapi/swagger-editor
    networks:
      - public-network
    expose:
      - 8080
    environment:
      VIRTUAL_HOST: swagger.dev
```

Now, just browse to `http://swagger.dev` and voila!

Note that this depends on [nginx-proxy](https://github.com/jwilder/nginx-proxy) as described in this article:
[Setting up Pow-like DNS using Docker on a Macbook Pro]({% post_url 2016-11-21-docker-config-on-new-macbook %})
