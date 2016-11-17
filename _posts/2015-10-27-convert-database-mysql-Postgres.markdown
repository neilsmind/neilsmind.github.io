---
layout: post
title: "So you want to convert your database from MySQL to Postgres?"
date:   2015-10-27 15:35:00 -0500
categories: database postgres mysql
---

Working on a fairly large conversion project, the database switch was fairly critical for us. My requirement was pretty simple…I want to copy a database from one platform (MySQL) to another (Postgres) without having to learn every intricacy of each platform. My best world was if it could be something like the copy command:

~~~
copy source-file-path destination-file-path
~~~

We checked out Mysql2Postgres, yaml_db, and others. After much gnashing of teeth, we discovered Sequel.

Following is the entirety of what I needed to type to convert a MySQL database with more than 1,000,000 records to Postgres in under 15 minutes:

~~~
gem install sequel
gem install mysql2
gem install postgres

sequel -C mysql2://username:password@localhost/dbname postgres://username:password@localhost/dbname
~~~
