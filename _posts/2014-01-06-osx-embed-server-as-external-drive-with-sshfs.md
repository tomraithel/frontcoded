---
layout: post
title: "Embed a server as external drive with SSHFS on OSX"

tags: ssh sshfs osx
image: drive.png
---

Sometimes you need to edit files on a remote server via Secure Shell (SSH). In OSX you can easily mount a remote server to a virtual drive.

<!--more-->

{% comment %}
![An image]({{ BASE_PATH }}/images/medium/phusion-passenger.png)
{% endcomment %}


First you need an empty local directory - in this case I created `/Volumes/sshfs-mount/`. This directory
will be the mount point for `sshfs` - your ssh server will be connected to this folder.

> You can put it somewhere else, but make sure it exists!

If not already installed, install FUSE for OSX which can be downloaded here http://osxfuse.github.io.


After everything is set up, you can run the following command to mount your server to your previously created folder:

    sshfs username@hostname.com:/ -p 22 /Volumes/sshfs-mount/
