---
layout: post
title: Python3 Virtual Environments
date: '2020-10-07 15:07'
---

From what I understand there is now a `venv` module included with `python3`. There is (I think) also a package called `virtualenv`(presented in the docs as the predecessor to `venv` though it seems is still [under active development](https://virtualenv.pypa.io/en/latest/)) or something similar, which can be installed (`pip installed`?) which is basically the same thing though with more functionality. I don't know enough about this to give any real insight, sadly.

If you run into software which is to be `pip` (sort of like a python package manager, installs from [pypi](https://pypi.org)) installed, this can cause problems if there are conflicts with other packages you have installed on your system. A solution is to install packages and dependencies in virtual environments where they are isolated from other virtual environments and you system as a whole.

So what you need to do is basically to create and use a virtual environment. This can undoubtedly be done in several ways and there are several ways and software solutions to manage this. I will just base this in the documentation for python3. (This will be for macOS use)

```bash
python3 -m venv tutorial-env
# Use python3 to create a virtual environment in  
# the current working directory named "tutorial-env"
source tutorial-env/bin/activate
# Activate the new virtual environment, present in
# the folder "tutorial-env" by running the script "activate"
# This will add the name of the env to you shell:
(tutorial-env) user$
```
Now, for some strange reason, pip almost never seems to be updated to the latest version even if the system version is. If this is the case then just follow the instructions (if so inclined):
```bash
# WARNING: You are using pip version 20.1.1; however, version 20.2.3 is available.
# You should consider upgrading via the 'python3 -m pip install --upgrade pip' command.
python3 -m pip install --upgrade pip
```
You now have a virtual environment created and activated. This comes with a contained version of python3 as well. If you check `which python` it will show a python package from inside the `venv`. Which version you want to use can be specified in some why though I'll leave that for another time. If you install packages and dependencies while this virtual environment is active, these will be available for immediate use. Though once you deactivate the virtual environment, installed  packages and dependencies will no longer be available. The last part here is rather important, once you are finished with your virtual environment you must deactivate it. This is done by simply running `deactivate`:
```bash
(tutorial-env) user$ deactivate
# Notice the change:
user$
```
While in the virtual environment (or not for that matter) `pip` can be used to install packages. There is extensive more options that this though here are some quick notes:
```bash
(tutorial-env) user$ pip search string
# Search pypi for string
(tutorial-env) user$ pip install package
# Installs a package you've found
(tutorial-env) user$ pip uninstall package
# Uninstalls a package you've installed
(tutorial-env) user$ pip show package
# Show details regarding package
(tutorial-env) user$ pip list
# List installed packages
(tutorial-env) user$ pip freeze > requirements.txt
# Pipe the output of a similar list as above into a file
# called "requirements.txt" in a format which pip expects.
# This file can then be used as follows:
(tutorial-env) user$ pip install -r requirements.txt
# Install the packages listed in the "requirements.txt" file
```
According to the official documentation, most of these commands are executed as `python -m pip xxx` except for the "search" alternative. For me, it seems that you can choose to include or exclude this part as it does not seem to make any difference.


For mor information regarding virtual environments, check out the [documentation/tutorial](https://docs.python.org/3/tutorial/venv.html), read further for details regarding [installing python modules (`pip install`)](https://docs.python.org/3/installing/index.html#installing-index).
