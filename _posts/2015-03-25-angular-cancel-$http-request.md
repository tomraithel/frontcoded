---
layout: post
title: "Angular: Cancel a running $http request"

tags: javascript angular ajax
---

In a perfect world without any network latencies and processing time an ajax-request
would be instant. Unfortunately we don´t live in such a world... but I´m sure you knew that already :)

Every time we have to make requests to our backend, we have to think about how long
this requests could take and would could possibly happen while we are waiting for
the response to come back. This example shows how to deal with this problem in AngularJS.

Let´s take an input field with an auto-complete feature for example: If the user is quick
enough with his keyboard, he might fire a second requests although the first one is already
running and fetching the auto-suggestions. It´s not only request-overplay, it could also be, that
the first request finishes later than the second one. In this case the auto-suggestions would
not match the users input anymore. Pretty bad.

## Cancelling Requests

Typically you will use the [$http](https://docs.angularjs.org/api/ng/service/$http) service to perform
your ajax requests in angular. Good thing is, that it takes an `options` object which can
optionally configure a `timeout` property.

As described in the docs, the timeout could either be a number or a Promise object:

> timeout – {number|Promise} – timeout in milliseconds, or promise that should abort the request when resolved.

With that information, it´s easy to set up a cancellable request:

```js
	var canceller = $q.defer();

	var requestPromise = $http({
		url: 'data/en.json',
		timeout: canceller.promise
	});

	requestPromise.
		success(function(data, status, headers, config) {
			// Everything fine and not cancelled
		}).
		error(function(data, status, headers, config) {
			if (data === null && status === 0) {
				// Request cancelled
			}
			else {
				// Other error
				throw 'Error ' + status
			}
		});

	// This would happen at some later point in your code
	canceller.resolve();
```

Keep in mind, that `canceller.resolve()` should be called when you actually want to cancel that request.
If the request was already made and you resolve the canceller, nothing will happen... you can´t cancel
a finished request.

`canceller.resolve()` will evoke the `error` callback of your request promise. **One Note here:** in order
to determine if request was cancelled and did not fail because of any other error, I check for
`data === null && status === 0`. Another option would be to set a flag that can be checked in the error
callback. So far, the upper solution worked well for me but I don´t know if there are
some edge cases where a *normal error* matches the `data === null && status === 0` condition.
Please let me know, if you know a cooler way to check a cancellation.
