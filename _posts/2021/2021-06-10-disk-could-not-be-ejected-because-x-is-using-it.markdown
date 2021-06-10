---
layout: "post"
title: "Disk could not be ejected because X is using it"
date: "2021-06-10 13:44"
---
One of the more annoying events to happen when you are using an external drive. From experience, it seems that it is either "spotlight" or "quicklook" causing the hub-hub.

Why? Well either "finder" is trying to "index" the drive and just refusing to stop. This can be overcome by setting it to never being indexed in system preferences, I'll write something about this at some point. However, I have also "broken" this function by deleting hidden macOS folders on drives which apparently is needed to tell the mac not to look at the content.

Anyway, what do you do when faced with this problem? You can fire up your terminal and run some commands:

`lsof | grep [Volume Name]`

or

`lsof +D /Volumes/[Volume Name]`

How do you get the `Volume Name`, you can either just `ls /Volumes/` or use the `diskutil list` command to find it.

Anyway, the above should produce some output along the lines of:

`Atom      52956 blabla  txt       REG                1,4     206752            96532247 /Applications/Atom.app/Contents/MacOS/Atom`

As the "header" seems to never show up, at least on my system, this is rougly what should be there:

`COMMAND     PID     USER   FD   TYPE DEVICE SIZE/OFF     NODE NAME`

We are interrested in the "PID" of the process (command) which is using our drive, then we want to kill that process:

`kill -9 [PID]`

Well, the standard and default `SIGTERM (kill -15)` normally does not solve this problem. The `SIGHUP (kill -1)` option should be second in line to try as safer then `-9` though usually, I end up going down the list and finishing with `-9`.
