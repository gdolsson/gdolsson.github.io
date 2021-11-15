---
layout: "post"
title: "Homebrew Permissions Issues"
date: "2021-11-15 19:22"
---
After a drive change and OS update, trying to install/link brew packages failed as the `/usr/local/` directory partially was owned by root. The best option should be to reinstall brew and all packages/apps using the brewfile backup. Though as I have done this once before, realizing that brew needs to download every app before realizing it is already installed, took forever. The "good" way to resolve this should have been to remove brew and all packages and apps.

```bash
# Remove
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall.sh)"
# Reinstall
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
# Install from brewfile restored to home directory
brew bundle install
```
[One example of a writeup](https://scott-bollinger.com/using-brew-bundle-to-backup-and-restore-mac-app-store-and-brew-apps)

So the lazy solution was instead to modify the folder permissions.

```bash
sudo chown -R $(whoami) /usr/local
sudo chown -R $(whoami) /Library/Caches/Homebrew
# OR
sudo chown -R "$USER":admin /usr/local
sudo chown -R "$USER":admin /Library/Caches/Homebrew
```
Taken from [stackoverflow](https://stackoverflow.com/questions/16432071/how-to-fix-homebrew-permissions). Though according to the people there, some macOS systems will not allow you to change permissions in `/usr/local/` and suggested the following:
```bash
sudo chown -R $(whoami) $(brew --prefix)/*
```
