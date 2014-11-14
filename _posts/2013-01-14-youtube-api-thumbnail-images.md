---
layout: post
title: "Get the image thumbnails for a youtube video"

tags: youtube api thumbnails
image: 
---

<!--more-->

{% comment %}
![An image]({{ BASE_PATH }}/images/medium/phusion-passenger.png)
Image credits: <https://www.phusionpassenger.com/>
{% endcomment %}


YouTube has no real API method where you can retrieve your URL. Instead you have to follow YouTube´s simple convention, to put together the correct URLs for your video thumbnails. Let´s say, you want the thumbnail for following video:

    http://www.youtube.com/watch?v=u1zgFlCw8Aw


Just copy the *video id* - in this case `u1zgFlCw8Aw`. The URLs for the thumbnails are simply built with this rule: `http://img.youtube.com/vi/<VIDEO_ID>/<IMG_TYPE>.jpg`

In our example, the available sizes are:

    # Big image: 480x360
    http://img.youtube.com/vi/u1zgFlCw8Aw/0.jpg

    # Small thumbnail 1: 120x90
    http://img.youtube.com/vi/u1zgFlCw8Aw/1.jpg

    # Small thumbnail 2: 120x90
    http://img.youtube.com/vi/u1zgFlCw8Aw/2.jpg

    # Small thumbnail 3 (DEFAULT): 120x90
    http://img.youtube.com/vi/u1zgFlCw8Aw/3.jpg

That´s it!

