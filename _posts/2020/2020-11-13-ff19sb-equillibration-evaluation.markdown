---
layout: "post"
title: "FF19SB Equillibration Evaluation"
date: "2020-11-13 09:04"
---
With the newest addition to the amber force fields (FFs), the ff19SB FF, there was an additional output added to energy minimization output files as well as nPT simulation output files. There is a "CMAP" term added by ff19SB. This breaks the reading frame for `process_minout.perl` and `process_mdout.perl` scripts which I routinely use to evaluate the equilibration. I am quite sure that there are other ways to do this using parmed and cpptraj though it is just a really simple and fast way to get some columns of data to check with gnuplot or grace.

The problem is that the broken read frame produces empty output for Emin and pressure output (temperature (nVT) equilibration still works just fine with `process_mdout.perl`). To get I quick fix for this, until I can figure out how to write perl scripts, was to lean on what I already know, awk and friends. These are the horrible one-liners I managed to produce to be able to extract the same plots as I regularly inspect, first out is the extraction of system density evolution over time:

```
# The one-liner
paste <(grep "TIME(PS)" eqPres.out | awk '{print $6}') <(grep "Density" eqPres.out | awk '{print $3}') | column -s $'\t' -t | head -n $(( $(grep "TIME(PS)" eqPres.out | wc -l)-2 )) >> summary.DENSITY

# WTF is going on:
paste \
# Put the output of the command below in two aligned columns
<(grep "TIME(PS)" eqPres.out | awk '{print $6}') \
# Get the "TIME(PS)" values from eqPres.out filename and print only the time ($6)
<(grep "Density" eqPres.out | awk '{print $3}') | \
# Get the "Density" values from eqPres.out filename and print only density ($3)
column -s $'\t' -t | \
# Add a tab between columns to separate them a bit
head -n $(( $(grep "TIME(PS)" eqPres.out | wc -l)-2 )) \
# Remove the last two lines => First get the number of lines in file eqPres.out
# by greping and counting (wc), remove 2 from number and print lines 1-number)
>> summary.DENSITY
# Finally, pipe to output.

DONT FORGET TO MODIFY FILENAMES (eqPres.out) AND PATHS AS NEEDED
```

Then we have the total energy of the system over time:
```
# The one-liner
paste <(grep -A1 "NSTEP" emin.out | awk ' {print $1}' | tr -ds "\-\-" "\n" | egrep -v NSTEP) <(grep -A1 "ENERGY" emin.out | awk ' {print $2}' | tr -s "\n" | egrep -v ENERGY) | column -s $'\t' -t | sed '$ d' >> summary.ENERGY

# WTF is going on:
paste \
# Paste as columns
<(grep -A1 "NSTEP" emin.out | awk ' {print $1}' | tr -ds "\-\-" "\n" | egrep -v NSTEP) \
# Get the value below NSTEP (the iteration) and remove "--" and empty lines and header
<(grep -A1 "ENERGY" emin.out | awk ' {print $2}' | tr -s "\n" | egrep -v ENERGY) | \
# Get the energy value and remove blank line and header
column -s $'\t' -t | \
# Add a tab between columns to separate them a bit
sed '$ d' \
# Remove the last line
>> summary.ENERGY
# Finally, pipe to output.

DONT FORGET TO MODIFY FILENAMES (emin.out) AND PATHS AS NEEDED
```

Truly horrendous though it got the job done directly from the CLI without opening any other software, loading any files or writing any input. A bandaid until I manage to produce a real solution.
