---
layout: post
title: "Manage your AWS RDS database with Sequel Pro on Mac"

tags: rds aws sql database
image: sequel-pro.png
---

Sequel Pro is a really nice app for managing databases. With this technique you can remotely
access your AWS RDS database from your mac.

<!--more-->

For my elastic beanstalk rails application I needed a way to inspect the live database which
is managed by Amazon RDS. Unfortunately there seems to be no easy way to access the DB
from a remote server.

Thankfully Jeremy Keeshin found a solution for that problem, which he shared with us on his [Blog Post](http://thekeesh.com/2014/01/connecting-to-a-rds-server-from-a-local-computer-using-ssh-tunneling-on-a-mac/).

I just want to write it down for me in my words, as an easy access for later.

At first, you create an SSH tunnel to your EC2 server. In my case it is the server
that has been created from the elastic beanstalk service. If you have assigned some key-pairs
to this server, you can simply tunnel into that machine with following pattern:

    ssh -N -L <LOCAL_PORT>:<RDS_ENDPOINT_URL>:3306 ec2-user@<EC2_ENDPOINT> -i <PATH_TO_PEMFILE>

With *real* data it would look like this:

    ssh -N -L 1234:desduzzsir34.sd4tfdsad.eu-west-1.rds.amazonaws.com:3306 ec2-user@myapp-env-basdfdf34d.elasticbeanstalk.com -i /Volumes/pemfiles/elasticbeanstalk.pem

Of course, this tunnel has to be kept alive as long as you are working with sequel pro - otherwise the connection will be closed.

In Sequel Pro, just create a connection with the port from above. The RDS username created from elastic beanstalk is `ebroot` and the password is the one you have defined while creating your EB application.

> You can check the RDS username and enpoint in your AWS console by navigating to
Services > Elastic Beanstalk > myapp > Configuration > Settings-Icon on Card 'RDS'

![SequelPro]({{ BASE_PATH }}/images/medium/2014-06-14-rds.png)

That´s it!
