---
layout: post
title: "Create unique ids in javascript"

tags: javascript ids unique
image: id.png
---

Need to create HTML `id` attributes dynamically? Use this simple helper function.

<!--more-->


{% highlight javascript %}
/**
   * Creates a string that can be used for dynamic id attributes
   * Example: "id-so7567s1pcpojemi"
   * @returns {string}
   */
var uniqueId = function() {
  return 'id-' + Math.random().toString(36).substr(2, 16);
};
{% endhighlight %}