---
layout: "post"
title: "CLI Open Apps"
date: "2021-04-21 09:43"
---
This will, as I am currently using my mac, work on macOS. There are likely, options for both windows and linux as well though I will not list them here.

To open an application in "normal" mode (like clicking the icon in the Applications folder):

```
open -a Firefox
open -a 'Brave Browser'
```

This can of course be added as an alias to be able to launch this using shorthand:

```
alias firefox="open -a Firefox"
alias brave="open -a 'Brave Browser'"
```

Then to open a website, in this example, the adress is just added to the end of the command:

```
open -a Firefox https://www.google.se
open -a 'Brave Browser' https://www.google.se
```

However, to get access to the CLI options for the applications you need to point to the executable and not the application package:

```
/Applications/Firefox.app/Contents/MacOS/firefox
/Applications/Brave\ Browser.app/Contents/MacOS/Brave\ Browser
```

This will allow you to add additional input to the application in question and use the help function (if such is available for the application in question):

```
/Applications/Firefox.app/Contents/MacOS/firefox --help
```

This can also become an alias to allow shorthand usage.

```
alias firefox=""/Applications/Firefox.app/Contents/MacOS/firefox"
alias brave="/Applications/Brave\ Browser.app/Contents/MacOS/Brave\ Browser"
```

This can be useful if you have a series of sites you visit regularly, then these can be turned into a for loop function:

```
for i in https://www.qwant.com https://www.duckduckgo.com; do brave $i; done
```

For Brave it is really easy as this will open every item as a new tab. Using Firefox you'll have to add something like `--new-tab` before each url to get the expected behaviour.
