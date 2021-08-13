---
layout: "post"
title: "LEaP sourcing priority, bottom up"
date: "2021-08-13 10:07"
---
Can't believe I have not put a not here regarding this, when loading (sourcing) parameters in leap, the ordet in which you source them is very relevant. The order of priority is "bottom-up"

If you want to use, for example, the OPC water model with AMBER it needs to be sourced to not use parameters for TIP3P.

```
source leaprc.protein.ff19SB
source leaprc.water.opc
```

This is not the same as

```
source leaprc.water.opc
source leaprc.protein.ff19SB
```

If you use the prior, everything is sourced for `protein.ff19SB` the parameters from `water.opc` are applied with priority over parameters for water available in `protein.ff19SB`. However, trying the second alternative, then the OPC parameters will be de-prioritised when sourcing `protein.ff19SB` last and you will NOT be using OPC parameters. This will most likely break you simulation.
