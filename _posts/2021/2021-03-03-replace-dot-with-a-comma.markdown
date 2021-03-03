---
layout: "post"
title: "Replace dot with a comma"
date: "2021-03-03 10:36"
---
Having experienced a recurring problem with decimal delimiters, I have also managed to produce several options for solving the same issue.

Now, this seems easy but depending on the file, not so. Replacing ALL dots with commas is simple using sed/awk, but replacing some turned out to be a nightmare as the correct regex to achieve only a single "." found only between digits turned out to be basically impossible.

The problem being that the file in question this time contained a lot of different number formats:

```
40027	1601.12	-4.64176E-001	-2.28836E-005	0.00000E+000	-4.66000E-001	3.92902E-005	9	...........
40028	1601.16	-4.66191E-001	-2.28923E-005	0.00000E+000	-4.68000E-001	3.30403E-005	9	...........
```

Using sed with regex `\d*\.\d*` managed to replace ALL the dots and not only the ones between digits. Using `\d+\.\d+`, which seemed logical replaced NONE of the dots. Using any combination of `[0-9]` instead of `\d` replaced the actual numbers, trying to used matching did not work either as `\\1` was not accepted and actually replaced using \1. After trying all possible combinations of wildcards, lookarounds, matching and what-not I gave up...

Then it came to me, **perl**. Good old perl, always there at the back of the line to solve those problems processing files.

```
perl -p -e 's/(\d)\.(\d)/$1,$2/g' old.txt >> fixed.txt
```

Managed to find all dots "." between any two digits and replace that dot with a comma! **Success**. With both belt and suspenders, adding that `+` to cover "all" numbers and not only matching single digits:

```
perl -p -e 's/(\d+)\.(\d+)/$1,$2/g' old.txt >> fixed.txt
```

Works just as well...
