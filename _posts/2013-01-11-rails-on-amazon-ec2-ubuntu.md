---
layout: post
title: "Step-by-step: Setting up rails on Amazon EC2 Ubuntu instance"

tags: rails amazon ec2 ubuntu
image: 'rails-on-ec2-ubuntu.png'
---

If you want to set up a rails application on an amazon´s *EC2 Instance*, this might be something for you. Please note, that this instruction only covers the installation of rails and all its dependencies. If you want to use rails for production, you should also install an apache.

<!--more-->


1. If not already done, go to Amazon's EC2 Dashboard and launch a new *Ubuntu* Instance.

2. After that, you get a permission file `*.pem`. Save that on your disc and restrict the permissions to `600`:

        chmod 600 ~/myinstance.pem

3. *Connect* to the server via ssh

        ssh ubuntu@ec2-x-x-x-x.compute-1.amazonaws.com -i ~/myinstance.pem

> Replace the -x-x-x-x with your real instance name and `~/myinstance.pem` with the path to your .pem-file

4. All following steps take place on your ubuntu machine. First of all, update the *package manager*

        sudo apt-get update

5. Install *ruby*

        sudo apt-get install ruby-full build-essential

6. Install *rubygems*

        sudo apt-get install rubygems


7. Install *rails*

        sudo gem install rails

8. Install *sqlite* and the ruby gem

        sudo apt-get install libsqlite3-dev
        sudo gem install sqlite3-ruby

9. Install *nodejs*

        sudo apt-get install nodejs npm

10. Create a rails test application

        mkdir home/ubuntu/www
        cd home/ubuntu/www
        rails new testapp

11. Start the rails *server*

        rails server

    > Please note, that you should not use this server in production. This just a test, to see if rails runs on your EC2 instance.

12. Finally, set up the *security group* in your AWS console, so that port 3000 is activated. In your AWS console click on 'NETWORK & SECURITY > Security Groups'. Choose the security group of your instance. In settings klick on 'Inbound' and create a new rule:

        TCP
        3000
        0.0.0.0/0

    > Don´t forget to press 'Apply Rule Changes' when done!

13. Go to `ubuntu@ec2-x-x-x-x.compute-1.amazonaws.com:3000`. There you should see the rails default screen!

    ![Rails]({{ BASE_PATH }}/images/medium/rails-on-ec2-ubuntu.png)

14. Next steps: [Set up an apache to deliver your application]({{BASE_PATH}}/rails-with-apache-ec2-ubuntu.html).
