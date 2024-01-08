---
layout: "post"
title: "Windows netstat and cmd/ps commands for network"
date: "2024-01-08 09:19"
---
There was a [post on SANS ISC](https://isc.sans.edu/diary/Netstat+but+Better+and+in+PowerShell/30532) with suggested ps commands replicating and surpassing netstat. This is just fo my own memoy.

## Netstat

|Command|function|
|-------|--------|
|netstat -na|will tell you what ports your system has listening, as well as what you are connected to via TCP.|
|netstat -nao|will display the owning process ID for each connection and port|
|netstat -naob|(d'oh!) This will fail unless you "run as administrator" This gives you the process name rather than the process ID|

## PS commands

Get ID and process name

`get-process | select id,processname,description`

```bash
27560 Notepad                         Notepad.exe
 5620 NVDisplay.Container
 7080 NVDisplay.Container
 7804 nvWmi64
13664 nvWmi64
18480 OBRecoveryServicesManagement...
 7672 OfficeClickToRun
11080 OneDrive                        Microsoft OneDrive
19180 ONENOTEM                        Send to OneNote Tool
```

To get process name with UDP/TCP information, where "OwningProcess" is the "process ID", for UDP:

`Get-NetUDPEndpoint  | select LocalAddress,LocalPort,CreationTime,OwningProcess,@{Name="Process";Expression={(Get-Process -Id $_.OwningProcess).ProcessName}} | ft -auto`

```bash
LocalAddress                 LocalPort CreationTime         OwningProcess Process
------------                 --------- ------------         ------------- -------
192.168.229.1                     5353 1/3/2024 1:22:15 PM           7400 mDNSResponder
192.168.217.1                     5353 1/1/2024 3:20:04 PM           8216 TeamViewer_Service
192.168.122.201                   5353 1/3/2024 1:22:15 PM           7400 mDNSResponder
192.168.24.3                      5353 1/3/2024 1:22:15 PM           7400 mDNSResponder
172.21.240.1                      5353 1/3/2024 1:22:15 PM           7400 mDNSResponder
172.21.240.1                      5353 1/1/2024 3:20:04 PM           8216 TeamViewer_Service
```

and for TCP:

`Get-NetTCPConnection |  select-object LocalAddress,LocalPort,RemoteAddress,RemotePort,State,CreationTime,OwningProcess, @{Name="Process";Expression={(Get-Process -Id $_.OwningProcess).ProcessName}} | ft -auto`

```bash
LocalAddress    LocalPort RemoteAddress   RemotePort       State CreationTime          OwningProcess Process
------------    --------- -------------   ----------       ----- ------------          ------------- -------
127.0.0.1           23659 0.0.0.0                  0      Listen 1/3/2024 7:56:00 AM           25660 vpnui
127.0.0.1           22272 0.0.0.0                  0      Listen 1/2/2024 6:36:20 PM           25660 vpnui
127.0.0.1           22186 0.0.0.0                  0      Listen 1/2/2024 6:28:17 PM           25660 vpnui
127.0.0.1           22185 0.0.0.0                  0      Listen 1/2/2024 6:33:47 PM           25660 vpnui
127.0.0.1           22020 0.0.0.0                  0      Listen 1/2/2024 6:21:54 PM           25660 vpnui
127.0.0.1           22019 0.0.0.0                  0      Listen 1/2/2024 6:30:36 PM           25660 vpnui
```

For long-lived TCP sessions, change the CreationTime to lifetime of the connection (in seconds), and sort that lifetime from newest to oldest only printing the ten longest lifetimes:

`$now = get-date
Get-NetTCPConnection |  select-object LocalAddress,LocalPort,RemoteAddress,RemotePort,State,@{Name="LifetimeSec";Expression={($now-$_.CreationTime).seconds}},OwningProcess, @{Name="Process";Expression={(Get-Process -Id $_.OwningProcess).ProcessName}} | sort-object -property LifetimeSec | select-object -last 10 | ft -auto`

```bash
LocalAddress LocalPort RemoteAddress RemotePort       State LifetimeSec OwningProcess Process
------------ --------- ------------- ----------       ----- ----------- ------------- -------
127.0.0.1        49817 127.0.0.1          49818 Established          55         17936 atmgr
127.0.0.1        49822 127.0.0.1          49821 Established          55         17936 atmgr
0.0.0.0          49816 0.0.0.0                0       Bound          55         17936 atmgr
```

This can be run from cmd using a script by putting the UDP and TCO commands in a ps1 file (ns.ps1) or separate files for UDP and TCP, and creating a ns.cmd file for cmd containing:

`powershell -noprofile -executionpolicy bypass c:\path\to\script\ns.ps1`