---
layout: "post"
title: "find multiple filenames"
date: "2021-03-09 13:18"
---
Finding "a" file is not a huge problem:

```
find . -type f -name "*.sh"
```

Finding any file ending in "`.sh`". Then what if you want to find more files?

```
find . -type f \( -name "*.sh" -o -name "*.txt" -o -name "*.dat" \)
```

Should do the trick finding any files ending in "`.sh`", "`.txt`" or "`.dat`".

Adding `-print` will show you the results and you can delete these files as well adding `-exec rm {} +` for files or `-exec rm -rf {} +` for directories (where you also switch `-type f` to `-type d`)

  ```
  find . -type f \( -name "*.sh" -o -name "*.txt" -o -name "*.dat" \) -print -exec rm {} +

  find . -type d \( -name "*.sh" -o -name "*.txt" -o -name "*.dat" \) -print -exec rm -rf {} +
  ```

Another way to find many files if you are looking for something "not" called something

```
find . -type f ! -name something
```

This would find all files which are NOT named "something" and can be combined with the `-exec` command to do "stuff" You sould be able to do this as `-not`though not on macOS. Importantly, you also need to escape the `!` character on macOS, potentially on other systems as well.

```
find . -type f \! -name something
```
