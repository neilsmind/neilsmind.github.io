---
layout: post
title: A different approach to ruby/rails dev environment on docker
date: 2016-12-01 10:35:00 -500
---

So I've been trying, over and over, to grok why I need a `Dockerfile` for every rails
project I build for development. It just seems to violate DRY principles with no
real benefit that I can find.

I know I need a `docker-compose.yml` to compose the different
services for the given app, e.g. postgres database, elasticsearch, etc but I'm
just not clear why I need a Dockerfile.

I understand that the purpose of Docker containers is to be
disposable and, hence, idempotent. I also understand that calling launch scripts
is _so_ 2014. Here's the thing...my development is, by design, messy. It's a scratch
pad where I can change gems in and out and play with things.

What I really want are two things:

1. An isolated environment where the services and their versions are individually
configurable; sort of like rbenv for EVERYTHING...databases, message queues, search
engines...everything.
2. I don't want to have to install/configure these services every time.

I have a [Docker image](https://hub.docker.com/r/neilsmind/ruby-app/) that I quite
like and covers just about every app I build with nothing more than a docker-compose.yml
file.

First, let's look at how a simple, jekyll based blog is set up:

~~~ docker
version: '2'
~~~

Just to be complete, we'll start with a version 2 declaration.

~~~
volumes:
  gems:
    driver: local
~~~

The volume declaration says to create a `gems` volume which will automatically
be prepended by docker-compose with the project directory name. In this case
it's called "blog".

~~~ bash
> docker volume ls
DRIVER              VOLUME NAME
local               blog_gems
~~~

We're going to store the gems in a persistent volume that will not only survive
building and tearing down containers, it'll be shared by containers we use for
`docker-compose run` commands so we can `bundle install`, `bundle update`, etc. without
downloading gems or rebuilding native extensions continually.

~~~
version: '2'
networks:
  public-network:
    external:
      name: proxy
~~~

There are really two parts to this volume declaration. The first three items are
for storing the data in persistent volumes so that we can blow away the services
but keep the data.


~~~ docker
networks:
  public-network:
    external:
      name: proxy
  private-network:
    driver: bridge
~~~

I have a persistent network called `proxy` available at all times in which [nginx-proxy
listens](http://neilsmind.com/2016/11/21/docker-config-on-new-macbook.html) for apps.
That network was created outside of docker-compose using `docker network` so that it
can be included in all projects. See the article for details.

`private-network` is used for all "backend" services that would not be exposed
publicly. That keeps the network space clean and allows projects running at the
same time to have duplicate service names, e.g. `db`, `elasticsearch`, or `redis`

Now let's get down to the service:

~~~
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

- **image**: This refers to my image that I use for all ruby/rails projects (or at least
  most)
- **command**: This is what will run when the service starts. In this case, we're serving
  a jekyll-based blog. (Note that it's going to run at http://blog.dev)
- **volumes**: First, we say that the source code in our local directory is going to
  be mounted in the `/app` directory of the container. Second and more importantly,
  we mount the `gems` volume we created in the `volumes` section of this config file
  to `/usr/local/bundle` which is the default directory for Bundler.
- **working_dir**: Nothing out of the ordinary here...just pointing to `/app` as our
  working directory for the app.
- **networks**: We assign this service to use the public (proxy) network so that
  nginx-proxy will find it.
- **expose**: We assign port 4000 to be found by nginx-proxy and served as port 80
- **environment**: We assign `blog.dev` as the VIRTUAL_HOST environment variable.
  This is required by `nginx-proxy` to name the site and make it available on the
  local machine.

All we need to get this box going is `docker-compose run --rm web bundle install` and
all of the gems will be installed to the right volume. Now `docker-compose up` and you
should be able to browse the site at http://blog.dev (assuming that Jekyll has some
posts).

There's no need for a `docker-compose build` because we're always just launching
the idempotent ruby-app docker image.

Here it is all together:

~~~
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

Here's a full blown Rails app (development environment) that would run at
http://my-project.dev

~~~
version: '2'

volumes:
  elasticsearch-data:
    driver: local
  postgres-data:
    driver: local
  redis-data:
    driver: local
  gems:
    driver: local

networks:
  public-network:
    external:
      name: proxy
  private-network:
    driver: bridge

services:
  db:
    image: postgres:9.5
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - private-network

  redis:
    image: redis:3.2
    volumes:
      - redis-data:/data
    networks:
      - private-network

  elasticsearch:
    image: elasticsearch:2.3
    volumes:
      - elasticsearch-data:/usr/share/elasticsearch/data
    networks:
      - private-network

  web:
    image: neilsmind/ruby-app:2.3
    command: bundle exec rails server Puma -b 0.0.0.0 -p 3000 -P /tmp/rails.pid
    networks:
      - private-network
      - public-network
    volumes:
      - .:/app
      - gems:/usr/local/bundle
    working_dir: /app
    environment:
      # Avoid a database name in the DATABASE_URL. Let database.yml handle
      # that for different environments.
      DATABASE_URL: "postgresql://postgres@db:5432/?pool=25&encoding=utf8"
      REDIS_URL: redis://redis:6379
      ELASTICSEARCH_URL: http://elasticsearch:9200
    depends_on:
      - db
      - redis
      - elasticsearch
    expose:
      - 3000
    environment:
      VIRTUAL_HOST: my-project.dev
~~~
