---
layout: post
title: "Generate random passwords with javascript"

tags: javascript password generator
image: /assets/article_images/passwords.png
---

Nothing special - just a simple function that generates a random password in JavaScript.

<!--more-->

I wrote this function, because I often used other password generators but pressed the 'generate' button as long as I had
at least one digit and one special character in the generated result.

That annoyed me so i created a bookmarklet that generates 20 passwords that have a predefined number of different characters in it.

Some might say, that this is not so secure because the passwords are generated with a fixed pattern, but maybe you can
use it and adjust it for your requirements (e.g. add some variance).


{% highlight javascript %}
/**
 * Generates a random password
 *
 * @param numLc Number of lowercase letters to be used (default 4)
 * @param numUc Number of uppercase letters to be used (default 4)
 * @param numDigits Number of digits to be used (default 4)
 * @param numSpecial Number of special characters to be used (default 2)
 * @returns {*|string|String}
 */
var generatePassword = function(numLc, numUc, numDigits, numSpecial) {
  numLc = numLc || 4;
  numUc = numUc || 4;
  numDigits = numDigits || 4;
  numSpecial = numSpecial || 2;


  var lcLetters = 'abcdefghijklmnopqrstuvwxyz';
  var ucLetters = lcLetters.toUpperCase();
  var numbers = '0123456789';
  var special = '!?=#*$@+-.';

  var getRand = function(values) {
    return values.charAt(Math.floor(Math.random() * values.length));
  }

  //+ Jonas Raoni Soares Silva
  //@ http://jsfromhell.com/array/shuffle [v1.0]
  function shuffle(o){ //v1.0
    for(var j, x, i = o.length; i; j = Math.floor(Math.random() * i), x = o[--i], o[i] = o[j], o[j] = x);
    return o;
  };

  var pass = [];
  for(var i = 0; i < numLc; ++i) { pass.push(getRand(lcLetters)) }
  for(var i = 0; i < numUc; ++i) { pass.push(getRand(ucLetters)) }
  for(var i = 0; i < numDigits; ++i) { pass.push(getRand(numbers)) }
  for(var i = 0; i < numSpecial; ++i) { pass.push(getRand(special)) }

  return shuffle(pass).join('');
}
{% endhighlight %}

Bookmarklet
-----------

<p>
Drag this <a href='javascript:(function(){var generatePassword=function(e,t,n,r){function f(e){for(var t,n,r=e.length;r;t=Math.floor(Math.random()*r),n=e[--r],e[r]=e[t],e[t]=n);return e}e=e||4;t=t||4;n=n||4;r=r||2;var i="abcdefghijklmnopqrstuvwxyz";var s=i.toUpperCase();var o="0123456789";var u="!?=#*$@+-.";var a=function(e){return e.charAt(Math.floor(Math.random()*e.length))};var l=[];for(var c=0;c<e;++c){l.push(a(i))}for(var c=0;c<t;++c){l.push(a(s))}for(var c=0;c<n;++c){l.push(a(o))}for(var c=0;c<r;++c){l.push(a(u))}return f(l).join("")};console.log("Generating passwords:");console.log("_______________________________________________");for(var i=0;i<20;++i){console.log(generatePassword())}console.log("_______________________________________________")})();'>Generate Passwords</a>
Bookmarklet into your browser bar if you like it.
</p>

Console output

    Generating passwords:
    _______________________________________________
    ?Z0q5GvO9@bS2i
    Vk.83dNvR1#u6Y
    Dc*-NQFj42bh14
    Vcf5i7T.!KU66e
    K1=rd8Q8v0=IqN
    Gs7b.z40F$4qEM
    1#65-NfzBZvU3r
    T*8V4kEm-89pSn
    z422.Ik8HyG#Fq
    @tK1e8?33tNVzG
    r*94.ZZtD9ydC6
    04fJBi?pH3j$Y2
    9Dy!91js+LA9wJ
    iK8@b5F6eC8kT=
    4qdQ!!6n9VpR8H
    jSbxQN=bY@6484
    8kMX#!P78voe8J
    mAtYn3#6C!qX48
    X16Im-Hf0W+yn4
    1cu=2aBG?7r5CA
    _______________________________________________

