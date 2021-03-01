---
layout: post
title: Delete apple files
date: '2021-03-01 13:28'
---

To remove "hidden" apple files, .DS_Store and .AppleDouble:
```
find . -depth -name ".AppleDouble" -exec rm -Rf {} \; && \
find . -depth -name ".DS_Store" -exec rm -Rf {} \;
```
Do this in the folder you want to clean up. Don't think `-depth` is necessary. 
