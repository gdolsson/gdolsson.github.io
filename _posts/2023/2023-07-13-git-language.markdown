---
layout: "post"
title: "Changing or Setting git Language"
date: "2023-07-13 23:59"
---
Using `brew install git` will use the latest version of git on intel macs, however if the system language is not recognized as you'd like you can change this by adding to `~/.zprofile` as such:

```
# Set Git language to English
alias git='LC_ALL=en_US git'
```

As per [Stack Overflow](https://stackoverflow.com/questions/10633564/how-does-one-change-the-language-of-the-command-line-interface-of-git)