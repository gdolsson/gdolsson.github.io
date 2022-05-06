---
layout: "post"
title: "Mac Terminal Commands"
date: "2022-05-06 20:26"
---
lifehacker published [some nice macOS CLI commands](https://lifehacker.com/the-most-useful-terminal-commands-every-mac-user-should-1848799083):

# Network
`networkQuality`  
Rather self-explanatory

# Dock
`defaults write com.apple.dock static-only -bool true; killall Dock`  
Hide all inactive apps from dock

`defaults write com.apple.dock static-only -bool false; killall Dock`  
Return to normal

`defaults delete com.apple.dock; killall Dock`
Restore factory defaults

# Finder
`killall Finder`  
Relaunch finder

`defaults write com.apple.Finder QuitMenuItem 1; killall Finder`  
Add an option to quit an app (relaunch in the case of Finder) from the menu bar

`defaults write com.apple.Finder QuitMenuItem 0; killall Finder`  
Hide this option again

# Display
`caffinate`  
Did not know this was a built in (as with caffeine, now amphetamine)

`banner` as in `banner -w 50 text`  
Generate a ASCII banner width 50 consisting of "text"

`defaults write com.apple.screencapture type JPG`  
Change screen capture format to JPG

# General
`ditto` which seems to be an alternative to `cp`, have not checked on details