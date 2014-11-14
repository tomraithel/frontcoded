---
layout: post
title: "JavaScript Snippet: Extract a parameter from an URL string"

tags: javascript regex
image: 
---

There are a couple of ways, to extract a single URL-parameter from a given string. A simple and easy way is to do it with a regular expression.

<!--more-->

{% comment %}
![An image]({{ BASE_PATH }}/images/medium/phusion-passenger.png)
{% endcomment %}


{% highlight javascript %}
// The URL to check
var url = "http://www.frontcoded.com?myvalue=123&anothervalue=456";
// Here we search for the param 'myvalue'
var matchResult = url.match(/(\?|&)myvalue=([^&#]*).*$/);
var myvalue = null;
if(matchResult.length === 3) {
    myvalue = matchResult[2];
}
console.log(myvalue) // logs 123
{% endhighlight %}

> The regex also works, if there is an hash parameter at the end like `http://url.com?myvalue=123#top`