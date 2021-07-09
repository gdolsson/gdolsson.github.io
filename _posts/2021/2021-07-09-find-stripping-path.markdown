---
layout: "post"
title: "find stripping path"
date: "2021-07-09 11:29"
---
Sometimes you need to get just filenames from a subfolder, this can be accomplished with sed/awk/basename and a lot of other options, however it also works with `find`

`find ./path/to/files/ -name "*.type" -printf '%f\n'`

should produce an output only matching files in the correct subdirectory `./path/to/files/` that ends in the `*.type` extension without the pathname. Normally, the path from the current working directory to the files found would be printed.
