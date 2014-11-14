---
layout: post
title: "Change terminal colors in OSX with apple script"

tags: terminal bash applescript
image: terminal.png
---

Sometimes it is useful to have your terminal tabs in different colors. This helps a lot for example, when you are working
on multiple ssh remote connections. In order to avoid confusion, it´s a good idea to give different servers a different
background color - like green for "staging" and red for "production".

<!--more-->

Setup the function
------------------

With some [AppleScript](http://en.wikipedia.org/wiki/AppleScript) you can change the terminal colors on the fly. Because
this is something useful, i put the `setTerminalColors` function into my `~/.bash_login` file to be able to access it
everywhere:


    function setTerminalColors {
        osascript \
            -e "tell application \"Terminal\"" \
            -e "tell selected tab of front window" \
            -e "set normal text color to $1" \
            -e "set background color to $2" \
            -e "end tell" \
            -e "end tell"
    }

After saving and opening a new terminal tab, you can change the color easily. The first parameter is the text color and
the second one is the background color of the terminal as RGB values:

    setTerminalColors '{65535,65535,65535}' '{0,25700,0}'

Use it
------

The real advantage comes to light, when the setTerminalColors function is used together with other scripts or tools like
[Shuttle](http://fitztrev.github.io/shuttle/).

Shuttle configuration example:

{% highlight javascript %}
"hosts": [
    {
        "name": "myserver.com",
        "cmd": "setTerminalColors '{65535,65535,65535}' '{0,25700,0}'; ssh myuser@myserver.com"
    },
]
{% endhighlight %}
