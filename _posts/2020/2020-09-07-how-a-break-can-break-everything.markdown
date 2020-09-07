---
layout: post
title: How a break can break everything
date: '2020-09-07 15:00'
---

Turns out that if there is a missing end of line \(EOL\) character in files used for input for some softare \(CPPTRAJ\), the command on that line is not executed.

To find files with missing EOL and you don't want to open each and every file:  
`tail -c 1 filename`

To add that crappy character without having to open every file:  
`sed -i '' -e '$a\' *.ptraj`

This solves that problem. Much in the same sense, when working with gaussian, **do not** forget to end with an empty line!
