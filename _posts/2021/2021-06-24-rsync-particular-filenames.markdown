---
layout: "post"
title: "rsync particular filenames"
date: "2021-06-24 09:29"
---
This

`rsync -nrv --include="*/" --include="filename*.[ab]*.txt" --exclude="*" /path/to/src/base/ /path/to/target/base/`

Will rsync files matching `filename*.[ab]*.txt` from the source to the target, including folder structure
