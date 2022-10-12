---
layout: "post"
title: "Linux Log Sources: User Account Data"
categories: [Learning, Cybersecurity, IT, Log]
tags: [Learning, Cybersecurity, IT, Log]
date: "2022-08-08 08:30"
---
Started in the [first post about log sources](2022-07-24-linux-log-sources.markdown), adding some more information.

# User Account/Data
## /home/\<username\>/*  
Individual user home folder, contains the users individual files and configuration which should be inaccessible to other users on the system.
## [/etc/passwd](https://linuxize.com/post/etc-passwd-file/)
In short a plaintext database of user accounts on the system. Permissions should bee 644 and owner should be root, hence only modifiable by root or sudoers while readable by all users.  

This file should not be edited by hand, commands like [usermod](https://linuxize.com/post/usermod-command-in-linux/) and [useradd](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/) are present to modify or add users.

The structure of the file is fairly simple, on entry per line. Each line consists of seven comma-separated fields:

1. Username - Must be unique and limited to 32 characters
2. Password - On older systems, the encrypted user password. On newer systems set to `x` while the password is stored in `/etc/shadow`.
3. UID - Unique user identifier used by the operating system to refer to a user, numerical.
4. GID - Group identifier number, the user’s primary/default group, typically the name of the group is the same as the name of the user. Secondary groups are listed in the `/etc/group(s)` file.
5. GECOS - Contains detailed user information, comma-separated values such as full name of user/applications, room designator, phone number(s) and more. ([GECOS - General Electric Comprehensive Operating Supervisor](https://en.wikipedia.org/wiki/General_Comprehensive_Operating_System)).
6. Home directory - Absolute path to the user’s home directory, typically named using username and residing in the `/home` folder.
7. Login shell - Absolute path to the user’s login shell

```bash
<username>:<password>:<UID>:<GID>:<GECOS>,,,:<HOME>:<SHELL>

user:x:1000:1000:User Name,Room 51,,:/home/user:/bin/bash
```
First line normally describes root user, new entries are appended to the end of the file.

## [/etc/shadow](https://linuxize.com/post/etc-shadow-file/)
Contains information about user passwords, owned by root and group shadow with 640 permissions.

One entry per line for each user account. First line normally describes root user, new entries are appended to the end of the file. Each line contains 9 comma-separated fields.

1. Username
2. Encrypted Password
3. Last password change
4. Minimum password age
5. Maximum password age
6. Warning period
7. Inactivity period
8. Expiration date
9. Unused

```bash
mark:$6$.n.:17736:0:99999:7:::
[--] [----] [---] - [---] ----
|      |      |   |   |   |||+-----------> 9. Unused
|      |      |   |   |   ||+------------> 8. Expiration date
|      |      |   |   |   |+-------------> 7. Inactivity period
|      |      |   |   |   +--------------> 6. Warning period
|      |      |   |   +------------------> 5. Maximum password age
|      |      |   +----------------------> 4. Minimum password age
|      |      +--------------------------> 3. Last password change
|      +---------------------------------> 2. Encrypted Password
+----------------------------------------> 1. Username
```

## [/etc/sudoers](https://www.hostinger.com/tutorials/sudo-and-the-sudoers-file/)
The `sudo` command is configured in `/etc/sudoers`. 

```bash
#
# Sample /etc/sudoers file.
#
# This file MUST be edited with the 'visudo' command as root.
#
# See the sudoers man page for the details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification 
root        ALL = (ALL:ALL) ALL

# Members of the admin group may gain root privileges 
%admin      ALL = (ALL) ALL

# Allow members of group sudo to execute any command
%sudo       ALL = (ALL:ALL) ALL

## See sudoers(5) for more information on "#include" directives
## Read drop-in files from /private/etc/sudoers.d
## (the '#' here does not indicate a comment)
#includedir /private/etc/sudoers.d
```

`root ALL=(ALL:ALL) ALL`
Root user has unlimited privileges and can run any command on the system
`%admin ALL=(ALL) ALL` 
% sign specifies a group. Members of the admin group has the same privileges as of root user
`%sudo ALL=(ALL:ALL) ALL`
 Members of sudo group have the privileges to run any command

The `/etc/sudoers` file should be edited using `sudo visudo -f /etc/sudoers`. 

Check sudoers using:
`grep 'sudo' /etc/group`

Add a user to the sudo group:
`adduser <user> sudo`

Remove a user from sudo group:
`deluser <user> sudo`

### Specific privileges
Example, create configuration file `/etc/sudoers.d/networking`:

`sudo visudo -f /etc/sudoers.d/networking`

```bash
Cmnd_Alias      CAPTURE = /usr/sbin/tcpdump
Cmnd_Alias      SERVERS = /usr/sbin apache2ctl, /usr/bin/htpasswd
Cmnd_Alias      NETALL = CAPTURE, SERVERS
%netadmin       ALL = NETALL
```

Run:
`addgroup netadmin`

This creates a "netadmin" group. Group members of netadmin can run commands specified in NETALL. NETALL include all commands under CAPTURE and SERVERS aliases. `tcpdump` is under the CAPTURE alias i.e. `/usr/sbin/tcpdump`.

Next we add `<user>` to the netadmin group:
`sudo adduser <user> netadmin`

Now `<user>` can run tcpdump command

## [/etc/group(s)](https://www.cyberciti.biz/faq/understanding-etcgroup-file/)
Defines group memberships, dividing users into groups allow granular access control.
One entry per line, separated by colon.

```bash
<group_name>:<password>:<group ID (GID)>:<group list>
cdrom:x:24:vivek,student13,raj
```
`<group_name>`  
Name of the group
`<password>`  
Generally empty as passwords are not normally used, useful for privileged groups
`<group ID (GID)>`  
Every user must have a GID, found in `/etc/passwd/`
`<group list>`  
Comma separated list of users in the group

`[less|more|cat] /etc/groups`

To check group memberships. The `groups` command can also be used:

```bash
$ groups <username>
$ groups
$ groups linuxize
   linuxize : linuxize sudo
```
`<user> : <primary group> <secondary group>`

To display only the group ID:
```bash
$ id -g
$ id -g <user>
$ id -gn <user>
$ id -Gn <user>
$ id linuxize
    uid=1001(linuxize) gid=1001(linuxize) groups=1001(linuxize),27(sudo)
```
ID (`uid`), `gid` and secondary groups `groups`. `-n` will print only names and not numbers, `-g` only primary group and `-G` all groups

[List all members of group](https://linuxize.com/post/how-to-list-groups-in-linux/):
```bash
$ getent group
$ getent group <group>
```
`awk` or `cut` can be used to get the name of group:
```bash
$ getent group | awk -F: '{ print $1}'
$ getent group | cut -d: -f1
```
Thats a nice place to leave this post. Will likely be a follow-up at some point in time.