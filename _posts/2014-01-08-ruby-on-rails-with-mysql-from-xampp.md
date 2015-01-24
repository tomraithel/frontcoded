---
layout: post
title: "Run ruby on rails together with XAMPPs mysql socket on mac"

tags: xampp ruby rails mysql
image: /assets/article_images/xampp.jpg
---

If you want to run rails on your local machine and you already have XAMPP installed, then you propably want to use your mysql
installation that comes with XAMPP. Here´s a short description, how to do that.

<!--more-->

{% comment %}
![An image]({{ BASE_PATH }}/images/medium/phusion-passenger.png)
{% endcomment %}


First you need to tell the mysql gem the path to your files installed with XAMPP. This can be achieved by setting some flags during `gem install`:

    sudo env ARCHFLAGS="-arch i386" gem install mysql -- --with-mysql-dir=/Applications/XAMPP/xamppfiles/lib/mysql --with-mysql-lib=/Applications/XAMPP/xamppfiles/lib/mysql/ --with-mysql-include=/Applications/XAMPP/xamppfiles/include/mysql/

After that, you can adjust your `database.yml` from your rails app:

{% highlight yaml %}
development:
  adapter: mysql2
  encoding: utf8
  database: your_db
  pool: 5
  username: root
  password:
  socket: /Applications/XAMPP/xamppfiles/var/mysql/mysql.sock
{% endhighlight %}

Finally, run bundle in the rails project again and it should work.
