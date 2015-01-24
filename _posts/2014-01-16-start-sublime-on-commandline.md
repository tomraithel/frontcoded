---
layout: post
title: "OSX: Start Sublime Text 2 from the commandline"

tags: sublime terminal commandline
image: /assets/article_images/sublime.jpg
---

I often used `vi` to edit my local text files from the commandline. But actually `vi` is not my favourite choice for editing text. 
Here´s a tip, how to add **Sublime Text 2** to the commandline.

<!--more-->

First, create a symlink from your `/usr/local/bin` directory, to the sublime binary:

    ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/sublime

After that, check if the `/usr/local/bin` dir is part of your `$PATH` environment variable, by typing in:

    $PATH

If `/usr/local/bin` can not be found in the output of the above command, you need to add it to the $PATH. Type in `vi ~/.bash_login` and add the path (`:/usr/local/bin`) at the end of the export command. If this line does not exits, just add it:

    export PATH=${PATH}:/usr/local/bin

Save and quit the file. To reload the changes into the current shell session, type `source ~/.bash_login`.

That should be it. You should now be able to execute the `sublime` command from anywhere. For example, to open the current folder, just type:

    sublime .