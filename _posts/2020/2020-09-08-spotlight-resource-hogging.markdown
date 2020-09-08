---
layout: post
title: Spotlight Resource Hogging
date: '2020-09-08 15:21'
---

Sometimes there is a process `mds_stores`, which hogs a lot of CPU resources. This is spotlight indexing something on your drive, sometimes something that keeps changing and will never finish. You can check what mds_stores is doing by running the following command:
```
$ sudo fs_usage -w -f filesys mdworker | egrep "open"
```
You will at least be able to see if there is something spotlight has gotten "stuck" on or if there is something that keeps returning over and over again.

So what do you do then? Temporarily turn off indexing:
```
$ sudo mdutil -a -i off
```
If this seems to solve the CPU hogging problem you might want to turn indexing back on.
```
$ sudo mdutil -a -i on
```
Now what do you do? If yo suspect there might be a problem with the indexing process itself. Then you might need to reindex the drive completely.
```
# Turn indexing off
$ mdutil -i off
# Remove the current index file
$ sudo rm -rf /.Spotlight-V100
# Turn indexing back on
$ mdutil -i on
# Force a re-index of the drive
$ mdutil -E
```
I think that it is only necessary to run the last command though this will definitely wipe and clean all of it and hopefully solve any problems along the way.
