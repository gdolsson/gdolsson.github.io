---
layout: "post"
title: "MSG file analysis"
date: "2022-06-04 16:48"
---
I saw a nice writeup regarding file analysis of MSG files over at [SANS ISC](https://isc.sans.edu/forums/diary/Quick+Analysis+Of+Phishing+MSG/28646/)

MSG files are OLE files and can be analyzed using [oledump.py](https://blog.didierstevens.com/programs/oledump-py/), with the recommended plugins [plugin_msg.py](https://github.com/DidierStevens/DidierStevensSuite/blob/master/plugin_msg.py) and [plugin_msg_summary.py](https://github.com/DidierStevens/DidierStevensSuite/blob/master/plugin_msg_summary.py).

A new feature of the latter now searches the stream for URLs and reports them. Hence, you can see phishing URLs in MSG email files.