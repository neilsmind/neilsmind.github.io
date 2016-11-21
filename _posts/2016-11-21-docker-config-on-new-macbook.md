---
layout: post
title: Setting up Pow-like DNS using Docker on a Macbook Pro
date: 2016-11-21 10:35:00 -500
---
So the new MacBook Pro came in and it's time to install everything on my new machine. Oh, wait!
I don't need to do so because I'm using Docker now. Here's where the rubber meets
the road...going completely without local installs of servers. For all web apps,
I want to have a Pow-like experience where the .dev top level domain is
automatically resolved to localhost and there's a simple way for each "machine" to be
found.

## Domain Resolution
First, the .dev TLD is actually pretty simple to do using dns-masq...

~~~
# Install using brew
brew install dnsmasq

# Copy default config
cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
~~~

Add the following line to the config file:

~~~
# /usr/local/etc/dnsmasq.conf
address=/dev/127.0.0.1
~~~

Restart the nameserver `brew services dnsmasq restart`

Now we'll tell the mac OS to use dnsmasq...

~~~
sudo mkdir /etc/resolver
~~~

Now create a .dev TLD config file

~~~
# /etc/resolver/dev
nameserver 127.0.0.1
~~~

You should be able to ping anything on the .dev domain as 127.0.0.1

~~~
ping whatever.dev
~~~

## Proxying the sites
Now we'll configure a docker image / container to always run. It will automatically
discover and serve docker services that have an exposed port and `VIRTUAL_HOST`
environment variable in the docker-compose.yml, e.g.

~~~docker
#./docker-compose.yml
# Example service in docker compose for Jekyll
version: '2'
networks:
  public-network:
    external:
      name: proxy

volumes:
  gems:
    driver: local

services:
  web:
    image: neilsmind/ruby-app:2.3
    command: bundle exec jekyll serve
    volumes:
      - .:/app
      - gems:/usr/local/bundle
    working_dir: /app
    networks:
      - public-network
    expose:
      - 4000
    environment:
      VIRTUAL_HOST: blog.dev

~~~

~~~
# Make directory to hold certificates for SSL connections
mkdir ~/.certs

# Create network for proxied sites
docker network create proxy

# Now start the service (and set it to always run)
docker run -p 80:80 -d -p 443:443 --name nginx-proxy --restart always -v \
  /var/run/docker.sock:/tmp/docker.sock:ro \
  -v ~/.certs:/etc/nginx/certs \
  --network proxy jwilder/nginx-proxy
~~~
