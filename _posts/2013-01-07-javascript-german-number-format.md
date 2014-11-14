---
layout: post
title: "Howto: German number format in JavaScript"

tags: javascript german
image: null
---

If you ever came across *formatting numbers* in JavaScript you may have discovered, that it´s not as simple as with server-side formatting (like in Ruby, PHP or Java). There are some jQuery plugins for that, but if you just want to format a number into the german format you can use the snippets below.

<!--more-->

The rules for german numbers are:

* Thousands delimiter is a `.`
* Zero delimiter is a `,`

### Coffeescript

{% highlight coffeescript %}
germanFormat = (number) ->
    stringReverse = (str)->
        str.split('').reverse().join('')

    [preComma, postComma] = number.toFixed(2).split('.')
    preComma = stringReverse(stringReverse(preComma).match(/.{1,3}/g).join('.'))
    "#{preComma},#{postComma}"
{% endhighlight %}


### JavaScript

{% highlight javascript %}
var germanFormat = function(number) {
  var postComma, preComma, stringReverse, _ref;
  stringReverse = function(str) {
    return str.split('').reverse().join('');
  };
  _ref = number.toFixed(2).split('.'), preComma = _ref[0], postComma = _ref[1];
  preComma = stringReverse(stringReverse(preComma).match(/.{1,3}/g).join('.'));
  return "" + preComma + "," + postComma;
};
{% endhighlight %}
