---
layout: "post"
title: "Linux Log Sources"
categories: [Learning, Cybersecurity, IT, Log]
tags: [Learning, Cybersecurity, IT, Log]
date: "2022-08-05 20:52"
---
I stumbled across a nice youtube video (can't remember by whom sadly) talking about incident response and/or investigations showing a nice cheat sheet while walking though the content of files and folders. This included a lot of useful log files, sources of web content, scheduled tasks and more.

This is a rundown of some of the talking points from that video, a nice overview which could be used as a cheat sheet of where to look for anomalies.

**User Account/Data**
- /home/\<username\>/*
- /etc/passwd
- /etc/shadow
- /etc/sudoers
- /etc/group

**Web Browsing Activity**
- /home/\<username\>/.config/google-chrome/
- /home/\<username\>/.mozilla/Firefox/
- /home/\<username\>/.config/Opera
- /home/\<username\>/.cache/

**Startup Items**
- /etc/systemd/system
- /usr/lib/systemd/system
- /etc/init*

**Scheduled Tasks**
- /etc/cron*
- /var/spool/crontabs
- /var/spool/atjobs
- /etc/anacron

**System & Application Logs**
-  /var/log/*

**System Files**
- /etc/*-release
- /etc/hostname
- /etc/hosts
- /var/lib/networkmanager
- /var/lib/dhclient
- /var/lib/dhcp

**Bash History**
- /home/\<username\>/.bash_history

**Trash**
- /home/\<username\>/.local/share/Trash/

**Recent Files**
- /home/\<username\>/.local/share/recently-used.xbel

**SSH Files**
- /home/\<username\>/.ssh/authorized_keys
- /home/\<username\>/.ssh/known_hosts
- /home/\<username\>/.ssh/config
- /home/\<username\>/.ssh/id_*