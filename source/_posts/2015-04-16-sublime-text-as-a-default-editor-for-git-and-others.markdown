---
layout: post
title: "Sublime Text as a Default Editor for Git and others"
date: 2015-04-16 13:37:09 +1000
comments: true
categories: Sublime Text , Git , Terminal , Mac OSx, 
---
If you want to set sublime as the main editor for git and other programs on your mac, depends on what you bash you use you should edit  ~/.bash_profile or ~/.bashrc or ~/.zshrc and put these lines on it:

```
export VISUAL='subl -w'
export EDITOR="$VISUAL"
export GIT_EDITOR="$VISUAL"

```

Then open a new terminal and run this command :
```
ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" ~/bin/subl
```
 Now sublime can be run as a console program and when you hit ```git commit``` command sublime shows up in a new Tab. Refer to [Sublime documentation](https://www.sublimetext.com/docs/2/osx_command_line.html).
