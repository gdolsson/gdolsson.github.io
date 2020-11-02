information---
layout: post
title: ParmEd - System Size (and other info)
date: '2020-09-23 09:54'
---

```bash
$ parmed
    > parm *.prmtop
    # Load parm file, the same can be accomplished with -p *.prmtop when starting
    > loadRestrt filename.ncrst
    # Load a restart file. The same can be accomplished with -c *.ncrst
    > loadCoordinates *.nc
    # Load coordinate file(s), can also be accomplished with -c
    > summary
    # Print useful information regarding the system
    > printFlags
    # Print a list of available Flags
    %FLAG TITLE
    %FLAG POINTERS
    ...
    ...
    %FLAG BOX_DIMENSIONS

    > printInfo BOX_DIMENSIONS
    # Print the information related to chosen flag
            90.00000        101.52932        101.53099        101.53166

    > quit
    # Quit
```
This will launch parmEd, load the parm file and a restart file (as the box size will have changed after equilibration), find out what flags are available and print out the info related to `BOX_DIMENSIONS` flag.
