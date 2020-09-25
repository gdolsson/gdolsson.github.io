---
layout: post
title: X11 Font Change
date: '2020-09-25 11:53'
---

I found a question on the amber mailing list (forum) asking if it was possible to change the font of xleap. This might not work for all applications over X however, it works for xLEaP at least.

Launching `xleap` from the CLI uses the default font set for the applications. However, launching with the additional `-fn '*times-bold-r*150*'` will start the application `xleap` with the defined font.
```bash
    xleap -fn '*times-bold-r*150*'
    xleap -fn '*courier-bold-r*150*'
```
This can also be aliased in your profile (`.bashrc, .profile`)
```bash
    alias xleap_times = "xleap -fn '*times-bold-r*180*'"
    alias xleap_courier = "xleap -fn '*courier-bold-r*180*'"
```
How do you find the fonts? I found this [fantastic O'Reilly chapter](https://www.oreilly.com/library/view/x-window-system/9780937175149/Chapter05.html) online. I dod not have time to read it in detail though I figured there should be enough information to solve the problem. Quite correctly assumed, there was.

`xlsfonts` Outputs a list of the fonts available on the server (as well as aliases) This list is horribly long and hard to read.  
`xfd` Allows you to display character set of any individual font, "font displayer"  
`xfontsel` Allows previewing of fonts and selecting a name for the font ("alias")  

Combinding these with `grep` helps finding the correct font you want. Naming conventions are horrible though allows for the use of wildcards (as seen above) where `-adobe-times-bold-r-normal--18-180-75-75-p-99-iso10646-1` is shortened to `*times-bold-r*180*`.

This is a jungle though, check the O'Reilly site for details as this is a long read. There are caveats using wildcards as fonts as presented in alphabetical order and not by any other logic. You must therefore make sure that the wildcard expansion produces the correct font in alphabetical order. There is a long list of things you can do, set aliases for fonts and more. So this is just a quick start/introduction.
