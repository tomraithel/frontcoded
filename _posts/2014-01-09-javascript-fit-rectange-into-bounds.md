---
layout: post
title: "Javascript: Resize a rectangle to fit into the bounds of another rectangle with proper ratio"

tags: javascript resize bounds
image: /assets/article_images/fit-in-bounds.png
---

Sometimes you have an image or something else that has to be resized to fit into the bounds of another object.

<!--more-->

{% comment %}
![An image]({{ BASE_PATH }}/images/medium/phusion-passenger.png)
{% endcomment %}

Following function returns an object that contains the new width and height:


{% highlight javascript %}
/**
 * Fits a rectangle into anothers rectangles bounds
 * @param rect
 * @param bounds
 * @returns {width: Number, height: Number}
 */
fitRectIntoBounds: function(rect, bounds) {
  var rectRatio = rect.width / rect.height;
  var boundsRatio = bounds.width / bounds.height;

  var newDimensions = {};

  // Rect is more landscape than bounds - fit to width
  if(rectRatio > boundsRatio) {
    newDimensions.width = bounds.width;
    newDimensions.height = rect.height * (bounds.width / rect.width);
  }
  // Rect is more portrait than bounds - fit to height
  else {
    newDimensions.width = rect.width * (bounds.height / rect.height);
    newDimensions.height = bounds.height;
  }

  return newDimensions;
}
{% endhighlight %}

Positionize image in the center of a frame
------------------------------------------

To positionize an image in the center of another element (`#imageFrame` in this case), you can use following code.

> The code only works if the image has been loaded already. Also note, that `IMG.naturalSize` does not work in IE8. However, this is just an example code to demonstrate how to use the function above.

{% highlight javascript %}
var frame = $("#imageFrame");
var img = frame.find('img');

// note: naturalSize does not work in IE8
var realSize = {
  width:  img.get(0).naturalWidth,
  height: img.get(0).naturalHeight
};

var bounds = {
  width:  frame.width(),
  height: frame.height()
};

var resized = fitRectIntoBounds(realSize, bounds);

img.css({
  width:  resized.width,
  height: resized.height,
  top:    (bounds.height - resized.height) / 2,
  left:   (bounds.width - resized.width) / 2
});
{% endhighlight %}
