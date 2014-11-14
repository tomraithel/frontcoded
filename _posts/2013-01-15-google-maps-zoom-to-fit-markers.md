---
layout: post
title: "Google Maps v3: Zoom map to fit a set of markers"

tags: google maps
image: google-maps.png
---

<!--more-->

{% comment %}
![An image]({{ BASE_PATH }}/images/medium/phusion-passenger.png)
{% endcomment %}


Following function returns a `google.maps.LatLngBounds` object for an array of `google.maps.Marker`

{% highlight javascript %}
/**
 * Returns a bounds object for a given list of markers
 * @param  {array} markers      The markers
 * @return {LatLngBounds}       The bounds object
 */
getBoundsForMarkers = function(markers) {
  var bounds, marker, _i, _len;
  bounds = new google.maps.LatLngBounds();
  for (_i = 0, _len = markers.length; _i < _len; _i++) {
    marker = markers[_i];
    bounds.extend(marker.getPosition());
  }
  return bounds;
};
{% endhighlight %}

Assuming we have an array with markers called `myMarkers` and a local `map` object, we can use the function to calculate the bounds and apply it to the map:

{% highlight javascript %}
var bounds;
bounds = getBoundsForMarkers(myMarkers);
// Apply the bounds to map
map.fitBounds(bounds);
{% endhighlight %}

