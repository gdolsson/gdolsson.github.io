---
layout: post
title: Fixing Jekyll Ruby Bundle Problems
date: '2020-10-20 09:36'
---

I don't know what happened or why but homebrews ruby version or something changed and I lost bundler. I don't remember if I was using system ruby and installed bundler or if I managed to solve this with `brew` versions. Trying to use the `brew` version over macOS ruby seems to be discouraged:

```bash
brew link ruby

Warning: Refusing to link macOS provided/shadowed software: ruby
If you need to have ruby first in your PATH run:
  echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.profile

For compilers to find ruby you may need to set:
  export LDFLAGS="-L/usr/local/opt/ruby/lib"
  export CPPFLAGS="-I/usr/local/opt/ruby/include"

For pkg-config to find ruby you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/ruby/lib/pkgconfig"
```
Now, doing all this did not magically fix all as the "newer" homebrew version did not come with the old gems installed. Using homebrew ruby turned out to work, after a little tinkering.

```bash
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.profile
which ruby
    /usr/local/opt/ruby/bin/rub
which gem
    /usr/local/opt/ruby/bin/gem
gem install bundler
cd /path/to/jekyll/site
bundle install
bundle update
bundle update github-pages

```
This makes the homebrew `ruby` and `gem` the "defaults". As old gems are lost when `brew` updates (apparently) you then have to reinstall bundler and then reinstall gems and dependencies according to the correct "new" `ruby` version in your jekyll site root directory.

Obviously, one could just run or put these in a script to temporarily switch between version:
```bash
export PATH=/usr/local/opt/ruby/bin:$PATH
export LDFLAGS="-L/usr/local/opt/ruby/lib"
export CPPFLAGS="-I/usr/local/opt/ruby/include"
```
This would revert to "default" when closing the termial session.

However, putting the `brew` version of `ruby` ahead in the path seems to be discouraged and hence will probably cause some problems somewhere down the line. I would think that there is a solution to this, like the `python venv` or something similar though when `brew` switches versions and removes old versions I suspect that this would still brake. So in order to "switch" between the two, potentially reverting to the built in macOS ruby version, doing the above though on system level should work (I suspect that this is what I did before).

Comment out `echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"'` from `~/.profile `. This can be accomplished by running

`sed -i '' '/usr\/local\/opt\/ruby\/bin/d' ~/.profile`

though this can surely backfire. If you just temporarily added to you path by executing the export commands instead of appending to ".profile", just restart the terminal session.

```bash
which gem
    /usr/bin/gem
which ruby
    /usr/bin/ruby
sudo gem install bundler
cd /path/to/jekyll/site
bundle install
bundle update
bundle update github-pages
```

Of course not... This caused dependency issues as the macOS version will be outdated...

```bash
bundle install
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
Fetching gem metadata from https://rubygems.org/.........
zeitwerk-2.4.0 requires ruby version >= 2.4.4, which is incompatible with the
current version, ruby 2.3.7p456
```
So by setting the `brew` version and installing/updating you messed up the packages. So now a problem for a future time is "How to downgrade"...

Turns out there is a really simple solution...

Place a executable shell script somewhere, source this script in you .profile so that the functions inside this script are available, and then make a function as so:

```bash
set_brew_ruby (){
    export PATH=/usr/local/opt/ruby/bin:$PATH
    export LDFLAGS="-L/usr/local/opt/ruby/lib"
    export CPPFLAGS="-I/usr/local/opt/ruby/include"
}
```
Then when you run `set_brew_ruby` you are using the `brew` version of `ruby` for that session. When you start a new session you are back to system `ruby`. At some point I will place some solution for removing from `$PATH` as well though for now, this is enough of a workable solution which should be unaffected bu brew updating ruby as well.
