---
layout: post
title: "Delopy and Run a Rails 4 App on AWS Elastic Beanstalk"

tags: rails ruby amazon aws
image: beanstalk.png
---

Once a rails 4 application is set up correctly on amazons elastic beanstalk,
there is nothing easier than updating this application. But for me there was
a bit of trouble to get things starting. Maybe this article is useful for you too.

<!--more-->

## EB Setup

First at all, I found this guide from amazon on [how to deploy a rails application](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_Ruby_rails.html).

This was quite useful. Setting up the main part was straightforward - I mainly used the suggestions from amazons guide. You should do the same, but for your information here are my parameters that I used from the eb wizard:


#### Keys

    $ eb init
    To get your AWS Access Key ID and Secret Access Key,
      visit "https://aws-portal.amazon.com/gp/aws/securityCredentials".
    Enter your AWS Access Key ID : ...
    Enter your AWS Secret Access Key : ...

Enter your keys. Use credentials for a role with sufficient permissions


#### Region

    Select an AWS Elastic Beanstalk service region (current value is "EU West (Ireland)").
    Available service regions are:
    1) US East (Virginia)
    2) US West (Oregon)
    3) US West (North California)
    4) EU West (Ireland)
    5) Asia Pacific (Singapore)
    6) Asia Pacific (Tokyo)
    7) Asia Pacific (Sydney)
    8) South America (Sao Paulo)

I chose `4` for Ireland, because I live in Germany


#### Application name

    Select (1 to 8):
    Enter an AWS Elastic Beanstalk application name (current value is "myapp"):
    Enter an AWS Elastic Beanstalk environment name (current value is "myapp-env"):

Name it whatever you like and press enter.

#### Tier

    Select an environment tier (current value is "WebServer::Standard::1.0").
    Available environment tiers are:
    1) WebServer::Standard::1.0
    2) Worker::SQS/HTTP::1.0
    Select (1 to 2):

I chose `1`

#### The server stack

    Select a solution stack (current value is "64bit Amazon Linux 2014.03 v1.0.3 running Ruby 2.0 (Passenger Standalone)").
    Available solution stacks are:
    1) 64bit Amazon Linux 2014.03 v1.0.3 running PHP 5.5
    2) 64bit Amazon Linux 2014.03 v1.0.3 running PHP 5.4
    3) 32bit Amazon Linux 2014.03 v1.0.3 running PHP 5.5
    4) 32bit Amazon Linux 2014.03 v1.0.3 running PHP 5.4
    5) 32bit Amazon Linux running PHP 5.3
    6) 64bit Amazon Linux running PHP 5.3
    7) 64bit Amazon Linux 2014.03 v1.0.3 running Node.js
    8) 32bit Amazon Linux 2014.03 v1.0.3 running Node.js
    9) 64bit Windows Server 2008 R2 running IIS 7.5
    10) 64bit Windows Server 2012 running IIS 8
    11) 64bit Amazon Linux 2014.03 v1.0.3 running Tomcat 7 Java 7
    12) 64bit Amazon Linux 2014.03 v1.0.3 running Tomcat 7 Java 6
    13) 32bit Amazon Linux 2014.03 v1.0.3 running Tomcat 7 Java 7
    14) 32bit Amazon Linux 2014.03 v1.0.3 running Tomcat 7 Java 6
    15) 32bit Amazon Linux running Tomcat 7
    16) 64bit Amazon Linux running Tomcat 7
    17) 32bit Amazon Linux running Tomcat 6
    18) 64bit Amazon Linux running Tomcat 6
    19) 64bit Amazon Linux 2014.03 v1.0.3 running Python 2.7
    20) 32bit Amazon Linux 2014.03 v1.0.3 running Python 2.7
    21) 64bit Amazon Linux 2014.03 v1.0.3 running Python
    22) 32bit Amazon Linux 2014.03 v1.0.3 running Python
    23) 32bit Amazon Linux running Python
    24) 64bit Amazon Linux running Python
    25) 64bit Amazon Linux 2014.03 v1.0.4 running Ruby 2.0 (Puma)
    26) 64bit Amazon Linux 2014.03 v1.0.3 running Ruby 2.0 (Puma)
    27) 64bit Amazon Linux 2014.03 v1.0.2 running Ruby 2.0 (Puma)
    28) 64bit Amazon Linux 2014.03 v1.0.3 running Ruby 2.0 (Passenger Standalone)
    29) 64bit Amazon Linux 2014.03 v1.0.2 running Ruby 2.0 (Passenger Standalone)
    30) 64bit Amazon Linux 2014.03 v1.0.3 running Ruby 1.9.3
    31) 32bit Amazon Linux 2014.03 v1.0.3 running Ruby 1.9.3
    32) 32bit Amazon Linux 2014.03 v1.0.2 running Ruby 1.9.3
    33) 64bit Amazon Linux 2014.03 v1.0.2 running Ruby 1.9.3
    34) 32bit Amazon Linux 2014.02 v1.0.1 running Ruby 1.8.7
    35) 64bit Amazon Linux 2014.02 v1.0.1 running Ruby 1.8.7
    36) 64bit Amazon Linux 2014.03 v1.0.5 running Docker 0.9.0
    Select (1 to 36):

I decided to use `28` because I am already familiar with Passenger. No big deal here if you decided someting different, just make sure it´s a ruby server with ruby > 1.9

#### Environment type

    Select an environment type (current value is "SingleInstance").
    Available environment types are:
    1) LoadBalanced
    2) SingleInstance
    Select (1 to 2):

I kept it simple and picked `2`

#### RDS Database

    Create an RDS DB Instance? [y/n] (current value is "Yes"):

Of course we want a database, so the answer is `y`

#### Snapshot

    Create an RDS BD Instance from (current value is "[No snapshot]"):
    1) [No snapshot]
    2) ...
    3) ...
    4) [Other snapshot]
    Select (1 to 4):

Starting from beginning means we want a clean database - therefore no snapshot is required -> Pick `1`.

#### RDS master db

    Enter an RDS DB master password (current value is "******"):

Pick a password. Don´t be too fancy, my first pw had a lot of special chars. The caused the RDS creation to fail. So I suggest using a long one but only with lowercase, uppercase and numbers.

#### Snapshots

    If you terminate your environment, your RDS DB Instance will be deleted and you will lose your data.
    Create snapshot? [y/n] (current value is "Yes"):

If you not want to lose your data after your server has been terminated, you better choose `y`

#### Instance profile

    Attach an instance profile (current value is "aws-elasticbeanstalk-ec2-role"):
    1) [Create a default instance profile]
    2) aws-elasticbeanstalk-ec2-role
    3) [Other instance profile]
    Select (1 to 3):

Let AWS do the work and pick `1`


### Deploy Application to AWS

If you followed the remaining steps from the [AWS guide](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_Ruby_rails.html)
- Including adding the `mysql2` gem and changing the production environment to use the `ENV` placeholders, you are ready to go.

Start your application using `eb start`. EB is now setting up the environment that you have specified in the wizard with `eb init`. No matters, it should take a while.

Once it´s finished, type `eb status` to see whats going on with your app

    $ eb status
    URL		: myapp-env-XXX.elasticbeanstalk.com
    Status		: Ready
    Health		: Green

    RDS Database: AWSEBRDSDatabase | XXX.XXX.eu-west-1.rds.amazonaws.com:3306

Go to the URL displayed in the first line, and check out your app.


## Problems

For me - unfortunately - it didn´t work so well at the first try. I got a passenger error alert as i visited the URL (Sorry, don´t remember the exact wording of that message).

But what I´ve done is following:

1. Go to AWS console
2. Create new key pairs the access the ec2 server: Services > EC2 > NETWORK & SECURITY > Key Pairs
3. Assign the new key-pair to your EB application: Services > Elastic Beanstalk > myapp > Configuration > Settings-Icon on Card Instances > Select it from Selectbox "EC2 key pair" > Save

Now I was able to login to the machine via SSH.

The logs for my app are stored under `/var/app/support/logs`. A quick look into `passenger.log` reveled following stacktrace:

    [ 2014-06-14 16:18:29.6415 5140/7f14c66a6700 agents/HelperAgent/RequestHandler.h:2210 ]: [Client 20] Cannot checkout session.
    Error page:
    Did not recognize your adapter specification (cannot load such file -- json/ext/parser). (MultiJson::AdapterError)
      /usr/local/share/ruby/gems/2.0/gems/activesupport-4.1.1/lib/active_support/dependencies.rb:247:in `require'
      /usr/local/share/ruby/gems/2.0/gems/activesupport-4.1.1/lib/active_support/dependencies.rb:247:in `block in require'
      /usr/local/share/ruby/gems/2.0/gems/activesupport-4.1.1/lib/active_support/dependencies.rb:232:in `load_dependency'
      /usr/local/share/ruby/gems/2.0/gems/activesupport-4.1.1/lib/active_support/dependencies.rb:247:in `require'
      /usr/local/share/ruby/gems/2.0/gems/json-1.8.1/lib/json/ext.rb:13:in `<module:Ext>'
      /usr/local/share/ruby/gems/2.0/gems/json-1.8.1/lib/json/ext.rb:12:in `<module:JSON>'
      /usr/local/share/ruby/gems/2.0/gems/json-1.8.1/lib/json/ext.rb:9:in `<top (required)>'
      /usr/local/share/ruby/gems/2.0/gems/activesupport-4.1.1/lib/active_support/dependencies.rb:247:in `require'
      /usr/local/share/ruby/gems/2.0/gems/activesupport-4.1.1/lib/active_support/dependencies.rb:247:in `block in require'
      /usr/local/share/ruby/gems/2.0/gems/activesupport-4.1.1/lib/active_support/dependencies.rb:232:in `load_dependency'
      /usr/local/share/ruby/gems/2.0/gems/activesupport-4.1.1/lib/active_support/dependencies.rb:247:in `require'
      /usr/local/share/ruby/gems/2.0/gems/multi_json-1.10.1/lib/multi_json/adapters/json_gem.rb:1:in `<top (required)>'


So there seemed to be a problem with the `multi_json` gem on my machine.

I found a solution on [stackoverflow](http://stackoverflow.com/questions/22950020/multijson-adaptererror-rails-4-ruby-2-passenger).
I don´t exactly know what is causing the error here, but downgrading the gem worked well for me. If someone knows why, please add a comment
at the end of this article!


Update `Gemfile`:

    gem 'multi_json', '1.7.8'


And update gem via commandline:


    $ bundle update multi_json


Commit the new changes, and push it:

    git add .
    git commit -m "Downgrade multi_json gem due to eb problems"
    git aws.push


After everything was pushed to AWS, I discovered the next error message: `Bad Gateway`. Another look into `passenger.log` revealed the problem:


    App 12345 stderr: [ 2014-06-14 16:34:02.8529 24395/0x000000013dfee0(Worker 1) utils.rb:68 ]: *** Exception RuntimeError in Rack application object (Missing `secret_key_base` for 'production' environment, set this value in `config/secrets.yml`) (process 24395, thread 0x000000013dfee0(Worker 1)):


Ok, because the secret_key_base should not be set in the config for production environment directly, it has to be passed with an ENV variable.

So what we have to do is create a new secret with the commandline:

    $ bundle exec rake secret
    c2f86d0c1...00d67f


Copy the secret and add it to the configuration of your App by browsing to the AWS console:
Services > Elastic Beanstalk > myapp > Configuration > Settings-Icon on Card 'Software Configuration' > Scroll down and enter a new key-value pair into the blank fields:

left field: `SECRET_KEY_BASE`, right field: `c2f86d0c1...00d67f` (your key from above)

Save!

After the application has been updated, everything worked well for me. Hope it does the same for you!
