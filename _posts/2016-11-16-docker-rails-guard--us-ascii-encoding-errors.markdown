---
layout: post
title:  "Docker, Rails, Guard and freaking us-ascii encoding errors"
date:   2016-11-16 10:00:00 -0500
categories: rails docker
---
I’ve been running into an intermittent issue when running tests in Docker for Mac using guard and minitest similar to the following:

{% highlight ruby %}
ERROR["test_0001_should show blog", #<Class:0x0055ee8b101e48>, 42.78249460499501]
 test_0001_should show blog#BlogsController::on GET to :show (42.79s)
ActionView::Template::Error: ActionView::Template::Error: "\xE6" on US-ASCII
{% endhighlight %}

I've rectified it in the past based on going through the files and ensuring there are no special characters but now ran into a place where I simply couldn't find the offending characters...so, it was time to roll up sleeves and figure this out.

**The problem is that my Dockerfile inherits from ruby:2.3 and, as such, it does not have the locale set when used on Docker for Mac**

The explanation can be [found here](https://hub.docker.com/_/ruby/):

> #### Encoding
> By default, Ruby inherits the locale of the environment in which it is run. For most users running Ruby on their desktop systems, that means it's likely using some variation of *.UTF-8 (en_US.UTF-8, etc). **In Docker however, the default locale is C, which can have unexpected results.** If your application needs to interact with UTF-8, it is recommended that you explicitly adjust the locale of your image/container via -e LANG=C.UTF-8 or ENV LANG C.UTF-8.

The solution is to explicitly set the LANG environment variable in your Dockerfile:

`ENV LANG C.UTF-8`

You can find the implementation of this in my [docker-ruby-app repository Dockerfile](https://github.com/neilsmind/docker-ruby-app/blob/master/Dockerfile#L10)

Here's a sample:

{% highlight docker %}
# Dockerfile
FROM ruby:2.3

RUN apt-get update -qq && apt-get install -y \
  build-essential \
  cron \
  imagemagick \
  libpq-dev \
  nodejs

ENV LANG C.UTF-8 # <---- This is the important part

RUN mkdir -p /app
WORKDIR /app
{% endhighlight %}

Thanks to Jared Markell for his [article](http://jaredmarkell.com/docker-and-locales/) helping me figure this out
