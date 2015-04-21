---
layout: post
title: "Track Events in Google Analytics using the Measurement REST API"

tags: javascript google analytics measurement rest
---

Google provides a nifty method to track events in your web application. They are referring to this
API as the **Measurement Protocol**. The big advantage with this low-level protocol compared to
the traditional web tracking with `analytics.js` is, that you have to power of full control what is
going to be tracked and when.

The whole API is using one of following endpoints:

	http://www.google-analytics.com/collect		// for http
	https://ssl.google-analytics.com/collect	// for ssl

You have to make a GET or preferential a POST request to one of the endpoints. Only use GET when
you have not technical opportunity to do it with a POST.

There is a number of available parameters accepted by the API. At the end of this post you´ll find
a link to the parameter reference. Following example shows a POST-Request with the typical parameters
required for Event-Tracking:

```js
var trackingId = "UA-XXXXXXXX-X";
var clientId = "35009a79-1a05-49d7-b876-2b884d0f825b";

var trackEvent = function(category, action, label) {
	$.post('https://ssl.google-analytics.com/collect', {
		"t": 	"event",
		"ec": 	category,
		"ea": 	action,
		"el": 	label,
		"v": 	"1",
		"tid": 	trackingId,
		"cid": 	clientId
	});
};
```

So what is what?

- `t`: hit type - We chose `event`, this makes the additional parameters *ec* and *ea* mandatory:
- `ec`: event category - User defined string
- `ea`: event action - User defined string
- `el`: event label - User defined string, optional
- `v`: protocol version, currently only "1" is supported
- `tid`: tracking id in format UA-XXXX-Y
- `cid`: client id - Used to identify the client within multiple tracks. Should be a valid UUID format (e.g. 35009a79-1a05-49d7-b876-2b884d0f825b)


To track any event, just call the method with your category, action and label:

```js
trackEvent('homeScreen', 'userSelectsFilter', 'Active Items')
```

The response returns a small tracking gif image with the size of 1x1 px. When you create the tracking
 request in the way described above, you can simply ignore the response.





For full documentation please see the official references:

- [Measurement Protocol Reference](https://developers.google.com/analytics/devguides/collection/protocol/v1/reference)
- [Measurement Protocol Parameter Reference](https://developers.google.com/analytics/devguides/collection/protocol/v1/parameters)