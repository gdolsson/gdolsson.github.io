---
layout: "post"
title: "Homebrew Error SHA256 mismatch"
date: "2021-08-13 09:09"
---
Read this ENTIRE post before trying to apply this solution! This is just for my personal archiving and should in no way or form be taken as general advice suitable for YOUR situation and I take no responsibility for any outcomes from applying anything suggested to your system.

Sometimes when upgrading installed packages using `homebrew` I receive the following message:

```
==> Upgrading 1 outdated package:
x 1.1.0 -> 1.1.1
==> Upgrading x
==> Caveats
Cask x installs files under /usr/local. The presence of such
files can cause warnings when running `brew doctor`, which is considered
to be a bug in Homebrew Cask.

==> Downloading https://domain.com/macosx/base/x-1.1.1.pkg
Already downloaded: /Users/me/Library/Caches/Homebrew/downloads/randomnumbershere--x-1.1.1.pkg
==> Purging files for version 4.1.1 of Cask r
Error: x: SHA256 mismatch
Expected: somerandomnumbers
  Actual: someotherrandomnumbers
    File: /Users/me/Library/Caches/Homebrew/downloads/randomnumbershere--x-1.1.1.pkg
To retry an incomplete download, remove the file above.
```

So trashing the file, then running `brew cleanup && brew update` can sometimes solve the issue. Other times the hash for that file is actually wrong. Finding the file and going to the official download site I can run:

`openssl sha1 /Users/me/Library/Caches/Homebrew/downloads/randomnumbershere--x-1.1.1.pkg`

and I receive a hash, comparing this hash to the one published on the download site of the software they are identical. There are two options, either both the software and developer distribution site has been hacked putting a bad hash there. Or, the hash `homebrew` compares against is incorrect. In this case, I normally also do a:

`pkgutil --check-signature /Users/me/Library/Caches/Homebrew/downloads/randomnumbershere--x-1.1.1.pkg`

And if I feel convinced and secure in the fact that the downloaded file is indeed the official valid file, then there is something wrong with the `Expected: somerandomnumbers` hash. So how do you solve this, most likely waiting a few days may solve this problem when enough people have voiced complaints. Otherwise a small command comes in handy:

`brew edit x`

In the file that opens you can copy and paste the `someotherrandomnumbers` replacing `somerandomnumbers`, in essence correcting the incorrect hash with the correct hash. Running `brew upgrade` after this will perform a the upgrade.

Just keep in mind, sometimes the worst case scenario is the true scenario so manipulating the hashes could in essence mean that you "validate" a tampered with piece of software exposing yourself to a world of hurt!
