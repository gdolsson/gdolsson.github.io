---
layout: "post"
title: "SED, search and ..."
date: "2021-08-16 13:15"
---
This will allow you to search files (`[path]/[to]/*.input`) for "string" and if found modify the file in place and delete that/those line(s) while also generating a backup file with `.bak` extension. Leaving `-i` empty will skip the backup (`-i ''`)

`sed -i '.bak' '/string/d' [path]/[to]/*.input`
