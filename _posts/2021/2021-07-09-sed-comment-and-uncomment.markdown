---
layout: "post"
title: "sed comment and uncomment"
date: "2021-07-09 09:25"
---
Sometimes I need to edit a lot of files where lines containing something should not be run while lines that do contain something should be run. This should also be reversible to later run the stuff that was skipped and all changes should be "removable" restoring the original file.

This is the `test` file used:
```
some
some
some
some_more
some
some_more
ass more_some
```

Rrinning the following:

`sed -i /more/s/^/#/ test`

Finds `more` and if present on the line then adds `#` to the beginning of the line `^`

```
some
some
some
#some_more
some
#some_more
#ass more_some
```

Removing those particular comments just added (reversing/restoring)

 `sed -i /more/s/^#// test`

By finding lines containing `more` and if present replaces leading `#` (`^#`) with nothing

```
some
some
some
some_more
some
some_more
ass more_some
```

Commenting out the skipped lines (inverting):

 `sed -i /more/\!s/^/#/ test`

So this will look for lines not containing more (`more/\!`) and if true add a leading `#` (`s/^/#/`)

```
#some
#some
#some
some_more
#some
some_more
ass more_some
```

This last one will remove **ALL** leading `#` which is maybe not what you are looking for.

`sed -i s/^#// test`

```
some
some
some
some_more
some
some_more
ass more_some
```
