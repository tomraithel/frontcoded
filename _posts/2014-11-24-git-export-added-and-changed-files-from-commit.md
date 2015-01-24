---
layout: post
title: "Git: Export new and changed files from commits into a tar archive"

tags: git
image: /assets/article_images/git.jpg
---

I still have projects, where i need to push my changed local files manually to an FTP-Server.
Often these changes on different files are big enough and so widely spread, that I
lose track which files I have changed since the last update.

<!--more-->

In order to avoid updating *ALL* remote files I found this nice little command line snippet
which you can use, if your local codebase is managed via git (If not, DO IT! :P). So every time you made some
changes, commit them and then just type in following command.

    git diff-tree -r --no-commit-id --name-only --diff-filter=ACMRT HEAD | xargs tar -rf ~/Desktop/update.tar

All changes you made with your last commit are exported into the `update.tar` file on your desktop.

Of course you can replace `HEAD` with any other commit id or even a commit range like `HEAD..e26f194`.
