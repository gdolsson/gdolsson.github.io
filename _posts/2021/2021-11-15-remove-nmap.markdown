---
layout: "post"
title: "Remove nmap"
date: "2021-11-15 19:32"
---
At some point in time, the brew version of nmap stopped working on macOS for a while. While this was going on I installed using the installer from nmap. However, this does not come with an "uninstaller" and while the zenmap application is installed, removing this does not uninstall nmap. So a little digging and searching led do the following.

The installer readme suggested where things are installed in macOS `/usr/local/bin` and `/usr/local/share`

> "The nmap, ncat, ndiff, and nping command-line binaries will be installed in `/usr/local/bin` and additional support files will be installed in `/usr/local/share` The Zenmap application bundle will be installed in `/Applications/Zenmap.app`."

So to confirm this:

```bash
sudo find /usr/local/bin \( -name "*nmap*" -o -name "*ncat*" -o -name "*ndiff*" -o -name "*nping*" \)
sudo find /usr/local/share \( -name "*nmap*" -o -name "*ncat*" -o -name "*ndiff*" -o -name "*nping*" \)
```

Then using the output to select what to remove:

```bash
sudo rm -r /Applications/Zenmap.app
sudo rm /usr/local/bin/{ncat,ndiff,nmap,nmap-update,nping}
	/usr/local/bin/ndiff
	/usr/local/bin/ncat
	/usr/local/bin/nping
	/usr/local/bin/nmap

sudo rm /usr/local/share/man/man1/nmap.1

sudo rm /usr/local/share/man/*/man1/nmap.1
	/usr/local/share/man/sk/man1/nmap.1
	/usr/local/share/man/pl/man1/nmap.1
	/usr/local/share/man/pt_BR/man1/nmap.1
	/usr/local/share/man/ja/man1/nmap.1
	/usr/local/share/man/it/man1/nmap.1
	/usr/local/share/man/pt_PT/man1/nmap.1
	/usr/local/share/man/ru/man1/nmap.1
	/usr/local/share/man/ro/man1/nmap.1
	/usr/local/share/man/zh/man1/nmap.1
	/usr/local/share/man/man1/nmap.1
	/usr/local/share/man/man1/nmap-update.1
	/usr/local/share/man/man1/ndiff.1
	/usr/local/share/man/man1/ncat.1
	/usr/local/share/man/man1/nping.1
	/usr/local/share/man/hr/man1/nmap.1
	/usr/local/share/man/hu/man1/nmap.1
	/usr/local/share/man/de/man1/nmap.1
	/usr/local/share/man/fr/man1/nmap.1
	/usr/local/share/man/es/man1/nmap.1

sudo rm /usr/local/share/man/man1/{ncat,ndiff,nping,nmap-update}.1

sudo rm -r /usr/local/share/{nmap,ncat}
	/usr/local/share/ncat
	/usr/local/share/nmap
	/usr/local/share/nmap/nmap-protocols
	/usr/local/share/nmap/nselib/nmap.	luadoc
	/usr/local/share/nmap/nselib/data/  	psexec/nmap_service.vcproj
	/usr/local/share/nmap/nselib/data/  	psexec/nmap_service.c
	/usr/local/share/nmap/nmap-services
	/usr/local/share/nmap/  	nmap-mac-prefixes
	/usr/local/share/nmap/nmap-os-db
	/usr/local/share/nmap/nmap-rpc
	/usr/local/share/nmap/  	nmap-service-probes
	/usr/local/share/nmap/nmap.xsl
	/usr/local/share/nmap/scripts/  	nping-brute.nse
	/usr/local/share/nmap/nmap.dtd
	/usr/local/share/nmap/nmap-payloads

# As a zsh user, there was one more file which does not seem to have caused problems when removed
	/usr/local/share/zsh/functions/_nmap
```
Then after this I was able to install and use the brew version again (now that it was working).