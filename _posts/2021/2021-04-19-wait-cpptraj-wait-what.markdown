---
layout: "post"
title: "WAIT CPPTRAJ, wait what?"
date: "2021-04-19 10:11"
---
Turns out that running a series of CPPTRAJ input analyses of shorter simulations of smaller systems on a large shared computational resource with about 20 cores can cause some problems.

It turns out that when you run a hbond analysis like this, reading the dataset fast, checking to find no interactions at all really quickly and then exiting to move on the the next line on the job script, there is a glitch.

Apparently, writing the output files and closing all open files is not as fast as beginning to run the next analysis. So in order to avoid this problem a little "wait" time should be included between exections of cpptraj:

```
cpptraj.MPI -i input1.ptraj
wait 60
cpptraj.MPI -i input2.ptraj
wait 60
cpptraj.MPI -i input3.ptraj
wait 60
cpptraj.MPI -i input4.ptraj
wait 60
cpptraj.MPI -i input5.ptraj
```

This should leave enough time between executions to finalise writing any output files and closing all open files before starting to read in the next dataset and get data leaking over from the next execution.
