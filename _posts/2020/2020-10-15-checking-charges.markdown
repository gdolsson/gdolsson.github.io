---
layout: post
title: Checking Charges
date: '2020-10-15 10:44'
---

So when you are running AMBER simulations and charges are not applied properly. Then this can be helpful.

```
$ parmed
    > help
    # Get help on commands
    > parm *.prmtop
    # Load topology file(s)
    > listParms
    # List out loaded topology file(s)
    > summary
    # summary [parm <idx>|<name>]
    # Get the goods on what is inside the topology files
    # including the residue labels and numbers of each
    > netcharge
    # netcharge [<mask>] [parm <idx>|<name>]
    # get the netCharge of the system or a particular residueM
    > quit
```

This way you can at least check that you have charges and that they seem to be correct.
