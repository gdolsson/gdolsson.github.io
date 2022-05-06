---
layout: "post"
title: "OpenBB Terminal"
date: "2022-05-06 20:06"
---
The [OpenBB terminal](https://www.openbb.co/products/terminal) is a free, custom built financial terminal that will help you make more informed decisions, faster. Binaries can be downloaded and installed with source code available on [github](https://github.com/OpenBB-finance/OpenBBTerminal). Installation instructions are available [here](https://github.com/OpenBB-finance/OpenBBTerminal/blob/main/openbb_terminal/README.md).

Install and launch, the interface may seem a bit "simplistic", don't be fooled. There are more options and tools available than you'd think.

Below are some notes scribbled down while watching the introduction to ["gamestonk terminal" on youtube](https://www.youtube.com/watch?v=fqGPK8OVHLk).

Documentation is available [here](https://openbb-finance.github.io/OpenBBTerminal/)

Help is always displayed when starting/entering the terminal or a menu/submeny. "help" will re-list the top help menu
`<thing> --help/-h`
Will display the help entry for that "thing"/menu item etc.

`Exit` shuts down termial
`quit/q` exits current

`<thing> is a command in menu`
`> <thing> means enter menu`

Timezone is incorrect, find the command for editing timezone via `help` (`tz`)

`tz --help` (may not work)

`$ tz` followed by a space will provide dropdown so you can choose

`/` is "root" as usual, `/crypto` will take you to `root` and `crypto` regardless of where you are. `home` is equivalent to `/`
`..` is "one-up", like CLI normally works

**API Keys**
You need to add API keys to take full advantage of the terminal.

The top level meny for key management is `keys`

Upon entry, all available/missing keys are listed. They can be re-listed with `help`
Example, `news`. Get your free API key at https://newsapi.org. In the terminal, enter the `keys` menu and add through:

`/keys/ $ news <key>`

Puting the API key after the defined key you want to include.

**Chaining commands**
You can chain together commands with `/` as in `$ ../ins/stats/../ta/ema`

You can put commands in a file and run with python terminal, one per line:

``` bash
stocks
load pltr
dd
ot
../ins
stats
../ta
fib
```
Then run the file as 
```bash
python3 terminal.py path/to/routines/example.gst (inside openBB terminal)
```

There is excel integration, you can export to CSV and PNG and other formats. Jupyter is also integrated making it easy to produce notebooks with results of analyses.

These can be modified with markdown, adding code to add, for example, a plot will be included in future generation of results/reports as these are based on cached data.

There is a lot more, these are just some brief notes to get started.

"Danglewood" published a nice writeup on [dev.to](https://dev.to/danglewood/how-to-navigate-in-the-openbb-terminal-jl2) as well, good reference. Some of the information above has been added from this resource as well.