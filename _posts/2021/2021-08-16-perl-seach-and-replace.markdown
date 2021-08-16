---
layout: "post"
title: "PERL seach and replace"
date: "2021-08-16 13:10"
---
Using perl to search all files in a location, of one or any type of file, and replacing a "search string" with a new string.

`perl -pi -w -e 's/foo/bar/g;' ~/[path]/[to/]*.txt`

Will check all `TXT` files in `~/[path]/[to/]` for `foo`. If found will replace `foo` with `bar`. This is "permanent" and there is no way to "undo" this action.

`perl -pi.bak -w -e 's/foo/bar/g;' ~/[path]/[to/]*.txt`

This will, in addition to above, make a copy of the original input file appending `.bak` to the extension. The search can be extended using "boolean".

`'s/foo|FOO/bar/g;'`

Matching either `foo` or `FOO` and replacing with `bar`.
