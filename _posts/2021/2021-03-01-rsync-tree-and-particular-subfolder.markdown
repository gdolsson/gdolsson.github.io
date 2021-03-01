---
layout: post
title: rsync tree and particular subfolder
date: '2021-03-01 13:06'
---

To copy a folder structure while excluding all files:
```
rsync -av --include='*/' --exclude='*' /path/to/src /path/to/dest/
```
Meaning you include all folders though exclude all files.

With the folder structure set-up locally you can then proceed to sync particular subfolders by excluding the undesired files:
```
rsync -av --exclude={'*.in','*.out','folder'} path/to/src /path/to/dest/
```
This obviously limits the folders you can sync using include/exclude as files need to have unique filenames/types to work as intended.
