---
layout: post
title: "JavaScript: Split an array into chunks of a given size"

tags: javascript snippets
---

Here is a super simple snippet to create a grouped version of an array. I use this sometimes to prepare
data for my views that needs a grouped representation.

```js
var createGroupedArray = function(arr, chunkSize) {
	var groups = [], i;
	for (i = 0; i < arr.length; i += chunkSize) {
		groups.push(arr.slice(i, i + chunkSize));
	}
	return groups;
}
```


Usage example:

```js
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14];
var groupedArr = createGroupedArray(arr, 4);
var result = JSON.stringify(groupedArr);
// result: "[[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14]]"
```