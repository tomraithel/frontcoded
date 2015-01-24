---
layout: post
title: "Running Rails on Amazon EC2 with Apache (Ubuntu)"

tags: rails amazon ec2 apache
image: /assets/article_images/phusion-passenger.png
---

Setting up an *Apache* server to work with rails applications on an Amazon EC2 server is quite simple. In this blog post i assume, that rails and all dependecies are already installed on your machine. If that is not the case, please see [How to set up rails on ubuntu EC2]({{BASE_PATH}}/rails-on-amazon-ec2-ubuntu.html).
 
<!--more-->

![An image]({{ BASE_PATH }}/images/medium/phusion-passenger.png)
Image credits: <https://www.phusionpassenger.com/>


1. Install *Apache*

        sudo apt-get install apache2 apache2-mpm-prefork apache2-prefork-dev


2. Install *Passenger gem*.
    
        sudo gem install passenger

3. If not already installed, install *curl*

        sudo apt-get install libcurl4-openssl-dev

4. Install *Passenger* module to Apache (could take a while)

        sudo apt-get install libapache2-mod-passenger

    Once it´s installed, edit your apache configuration (`sudo vi /etc/apache2/httpd.conf`) and paste in the configuration that you got from the passenger installation:

        LoadModule passenger_module /var/lib/gems/1.8/gems/passenger-3.0.19/ext/apache2/mod_passenger.so
        PassengerRoot /var/lib/gems/1.8/gems/passenger-3.0.19
        PassengerRuby /usr/bin/ruby1.8

5. Create a new vhost for your application `sudo vi /etc/apache2/sites-available`. Paste in the following config and adjust the `DocumentRoot` and `ServerName` for your needs:

       <VirtualHost *:80>
          ServerName testapp
          # !!! Be sure to point DocumentRoot to 'public'!
          DocumentRoot /home/ubuntu/www/testapp/public
          <Directory /home/ubuntu/www/testapp/public>
             # This relaxes Apache security settings.
             AllowOverride all
             # MultiViews must be turned off.
             Options -MultiViews
          </Directory>
        </VirtualHost>

6. Disable default app, enable your app

        sudo a2dissite default
        sudo a2enssite testapp
        sudo /etc/init.d/apache2 reload

7. Restart Apache

       sudo /etc/init.d/apache2 restart

8. Make sure, port 80 ist activated in your AWS console under 'NETWORK & SECURITY > Security Groups'.

9. Browse to your page `http://ec2-x-x-x-x.compute-1.amazonaws.com/`. There your should finally see your rails application!


> TIP: In case you´ve got some problems check the `log/production.log` in your rails app. Also be sure, that `rake assets:precompile` has been executed before the apache is started.

