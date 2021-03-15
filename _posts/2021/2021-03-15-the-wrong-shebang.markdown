---
layout: "post"
title: "The wrong shebang"
date: "2021-03-15 10:25"
---
Turns out that when running scripts and/or executable files in macOS does **NOT** use the default `zsh` if there is no [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) specified.

If you have a shell script with some commands which include `zsh` specifics, then making this script executable and running it (`./script`) will run it in bash, not `zsh`. Interestingly enough, using the "quickie" and doing `#!/bin/sh` will not use `zsh` either but default to bash.

Hence, when putting CLI commands executable on the command line in `zsh` into an executable file, make sure you are specifying `#!/bin/zsh` to avoid issues. Issues such as thinking you are using basic glob when it turns out that `\*(a|b|c)\*` is not a general shell glob but specific to `zsh`.
