---
layout: post
title: "All I needed (in S3) was a little privacy…"
date: 2014-02-10 16:07:00 -500
categories: paperclip s3 aws
---
As many helpful blog posts are, this one is offered in the hope that I save an afternoon for someone else. I'm leveraging Rails with Paperclip to upload files to an S3 repository. S3 is a cloud-based file storage system offered by Amazon (yes, that Amazon). It divides the storage into "buckets" which are like having separate drives. I was able to follow a number of tutorials including [this one](https://devcenter.heroku.com/articles/paperclip-s3) and [this one](http://icebergist.com/posts/paperclip-heroku-and-amazon-s3-credentials/) which were extremely helpful in initial setup.

The key for me was the [IAM capability](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in Amazon Web Services which allows me to create a unique user that has just the permissions needed for my app. Think of it like creating a MySQL user account instead of using root when building an app. Creating the user was easy. Setting a policy was pretty easy as well.

My issue was that I kept getting AccessDenied when I tried to upload. The simple answer would be "wrong login info" for most people and I thought of that. The reason that it wasn't that simple was that I used [Transmit](https://panic.com/transmit/) to login and transfer files using my new IAM account so I was positive that the permissions were right and that the login credentials worked. hmm...

I could also upload from my app if I specified my global (think "root") login so I knew that Paperclip was pretty much configured right. Spent three hours searching along with a couple of walks around the building to clear my head before I finally found the problem.

Turns out that Transmit defaults to https for connections to S3 but Paperclip's S3 connection defaults to http (and permissions of "public_read"). I changed the default permission to "private" and voila! https enabled and uploads go through.

{% highlight ruby %}
# /config/application.rb

config.paperclip_defaults = {
  :storage => :s3,
  :s3_credentials => {
  :bucket => ENV['S3_BUCKET_NAME'],
  :access_key_id => ENV['AWS_ACCESS_KEY_ID'],
  :secret_access_key => ENV['AWS_SECRET_ACCESS_KEY'],
  :url => 's3.amazonaws.com',
  :s3_permissions => :private
}
{% endhighlight %}

Now...[how to download private files](https://github.com/thoughtbot/paperclip/wiki/Restricting-Access-to-Objects-Stored-on-Amazon-S3)...
