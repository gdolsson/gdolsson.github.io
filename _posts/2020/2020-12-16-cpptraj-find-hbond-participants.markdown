---
layout: "post"
title: "CPPTRAJ find HBOND participants"
date: "2020-12-16 10:11"
---
One thing which takes up a long time is creating input for hydrogen bond analyses. This can be done somewhat easier if you use CPPTRAJ in a clever way.

Load you parameter/topology file as usual and your input coordinates file as a trajectory using CPPTRAJ CLI:  
`$ cpptraj -p *.prmtop -y *.inpcrd`  

Set the debug level for `actions` to something equal to or above 1:  
`> debug actions 1`

Que up the hbond command without any additional input:  
`hbond`

Then run the analysis:  
`run`

This will execute the hbond analysis and also output the identified donors/acceptors:  
```
---------- RUN BEGIN -------------------------------------------------

PARAMETER FILES (1 total):
 0: xxx.prmtop, 28176 atoms, 7015 res, box: Orthogonal, 7005 mol, 7000 solvent

INPUT TRAJECTORIES (1 total):
 0: 'xxx.inpcrd' is an AMBER restart file, no velocities, Parm xxx.prmtop (Orthogonal box) (reading 1 of 1)
  Coordinate processing will occur on 1 frames.

BEGIN TRAJECTORY PROCESSING:
.....................................................
ACTION SETUP FOR PARM 'xxx.prmtop' (1 actions):
  0: [hbond]
	Acceptor-only atoms: 17
	               ACE_1@O        6
	               CYX_2@O       16
	                   ...      ...
                     ...      ...
	             AAC_12@O1      147
	             AAC_13@O1      156
	             AAC_14@O1      165
	Donor/acceptor sites: 16
	               CYX_2@N        7 H
	               TYR_3@N       17 H
	              TYR_3@OH       30 HH
                     ...      ... ..
                     ...      ... ..
	              NHE_11@N      138 HN1 HN2
	              AAC_12@O      148 H3
	              AAC_13@O      157 H3
	              AAC_14@O      166 H3
	Donor-only sites: 0
	19 solute hydrogens.
	Estimated max potential memory usage: 23.204 kB
```

These are the automatically detected hydrogen bond donors and acceptors. There might be something missed here or there though this will at least make it more of a "check" then a "identify" using a GUI program.  

**DO NOTE** that there will be an entry for EVERY molecule with the same residue name (`AAC_12,_13,_14`). At some point in time I will try to make a regex/grep thing to remove the residue numbering and then display only one hit per donor/acceptor.
