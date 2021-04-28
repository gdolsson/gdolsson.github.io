---
layout: "post"
title: "Check PREP charges"
date: "2021-04-28 11:09"
---
I routinely use antechamber for parameter generation, using AM1-BCC charge method it is not uncommon to receive a non-zero charges for structures. This requires using resp or making minor modifications to the produced prep file.

You can quickly check the charge in any of a number of ways though if you have awk available, this little one-liner comes in handy for a quick check after running antechamber:

   awk '{ sum += $11 } END { print sum }'' [filename].prep
