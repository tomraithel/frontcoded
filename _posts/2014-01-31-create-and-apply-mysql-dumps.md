---
layout: post
title: "Create and apply MySql dumps via command line"

tags: mysql dumps sql
image: mysql.png
---

Database dumps are handy when you want to create backups or restore an old database state in case something went wrong 
in your application. With the following commands, it should be easy.

<!--more-->

Create dump
-----------

The syntax for creating a dump into a SQL file named `dump.sql` is following:

    mysqldump -u [user] -p [dbname] > dump.sql

After you´ve entered your correct database password, you´ll get the dump. You can also set the password directly as an argument like this:

    mysqldump -u [user] -p[password] [dbname] > dump.sql

> Note, that there is no whitespace before the password!

However, it´s not recommended to pass the password directly with the command, because then it would be visible in the bash history or in ps aux.


### Dynamic filename

To create a file with current date and time, you can execute this command:

    mysqldump -u [user] -h [host] -p [dbname] > $(date +%Y-%m-%d-%H.%M).sql

> Within cronjobs, you need to escape the % characters: `$(date +\%Y-\%m-\%d-\%H.\%M).sql`

### Notes for cronjobs

If you want to create dumps periodically with a cronjob, it´s recommended to store the mysql credentials within a *MySQL Option File*. Usually this file is located at `~/.my.cnf`. You can store your default username and password there without the need, to re-type it for every dump:

1. Create / edit config:

        vi ~/.my.cnf

2. Put default options for `mysqldump` into the file and save

        [mysqldump]
        user = your_username
        password = your_password

3. Change permissions for the config

        chmod 600 ~/.my.cnf

After you have done these steps, you don´t have to provide username and password all the time:

    mysqldump your_db > dump.sql


### Zip 

To save some disk space, you can zip the dump before i´ts saved:

    mysqldump your_db --lock-tables=false | gzip -9 > ~/backups/$(date +%Y-%m-%d-%H.%M).dump.sql.gz


Apply dump
-----------

To apply an existing dump, the syntax is nearly the same as for `mysqldump`, but this time the `mysql` command is required:

    mysql -u [user] -p [dbname] < dump.sql

Sometimes you need a plain new database with no tables in it before you can apply the dump. To get that, simply drop and recreate the database:

    mysql -u [user] -p -e 'drop database `dbname`;'
    mysql -u [user] -p -e 'create database `dbname`;'