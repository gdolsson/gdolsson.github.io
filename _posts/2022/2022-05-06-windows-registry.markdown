---
layout: "post"
title: "Windows Registry"
date: "2022-05-06 19:50"
---
Some notes from the post by Pavel Yosifovich on ["Mysteries of the Registry"](https://scorpiosoftware.net/2022/04/15/mysteries-of-the-registry/)

There are tools in [sysinternals](https://docs.microsoft.com/en-us/sysinternals/) which can be used to check parts of the registry. Pavel also developed a tool called [TotalRegistry](https://github.com/zodiacon/TotalRegistry) adding information and context to the registry.

<pre>
HKEY_LOCAL_MACHINE will be abbreviated "HKLM"

HKLM\SYSTEM\CurrentControlSet\Control\hivelist
Shows the full hive list, even if all hives are not "true" hives (as a hive has to be stored in a file)

HKLM\System\CurrentControlSet\Services 
Key where all services and device drivers are installed

HKLM
	SOFTWARE
		Installed applications store system level information
	SAM and SECURITY
		Local security policy and local account information is managed. 
		Hidden, even from admin, unless ownership is transferred. 
		Only SYSTEM has access:
		  psexec -s -i -d RegEdit
		Will launch RegEdit under the SYSTEM account to view content.
	BCD00000000
		Boot Configuration Data (BCD), normally accessed using bcedit.exe
HKEY_USERS
	Subkeys donatie user profiles for any user ever logged in locally
	Each subway name is a Security ID (SID)
		SYSTEM (S-1-5-18)
		LocalService (S-1-5-19)
		NetworkService (S-1-5-20) 
	HKEY_CURRENT_USER
		Link key pointing to users subway under HKEY_USERS
	HKEY_CLASSES_ROOT
		Not a "real" key, not stored in a file but not a link either. 
		A combination of HKLM\Software\Classes and HKCU\Software\Classes
		HKEY_CLASSES_ROOT can be overwritten by current user hive
		Shell-related info, extensions, associations, "Explorer.exe"
		Component Object Model (COM)
	HKEY_CURRENT_CONFIG
		A link to HKLM\SYSTEM\CurrentControlSet\Hardware\Profiles\Current
		List of standard hives
</pre>