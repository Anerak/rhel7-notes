# Resume

## **`ls`** & **`redirect symbols`**

### Why two different things share a table? Because I'm trying to save space

ls option|Description|Redirect Symbol|Description
-|-|-|-
l   | Extended output (equivalent to command ll)|< \<filename>|uses file as stdin
ld  | Directory output|>|stodout overwrites file
a   | Shows all files, even hidden ones|>>|sdtout appends to file
Z   | SELinux context|2> \<filename>|stderr to file
R|Recursive|2>1|stderr to stdout
|-|-|&> \<filename>|stdout and stderr to file name

## Users

`/etc/passwd` contains the local user information.

`/etc/shadow` contains the user's passwords.

`/etc/group` contains the local group information.

`/etc/login.defs` contains the default parameters of accounts, such as password age.

**`authconfig`**&nbsp;`--passalgo [algorithm]`

**`useradd`**&nbsp;`[username]`

**`userdel`**&nbsp;`[username]` (add -r to remove the home directory)

**`usermod`**&nbsp;`[username]`

**`usermod`**&nbsp;`-s /sbin/nologin [username]` the user won't be able to log in.

Most of these options works for **`useradd`** and **`usermod`**

Option|Description
-|-
-a, --append|add the user to the supplementary group(s). use only with -G
-c, --comment \[COMMENT]| full name of the user for the GECOS field.
-d, --home \[HOME_DIR]|specify user's home directory
-e, --expiredate \[EXPIRE DATE]|date on which the user account will be disabled (YYYY-MM-DD)
-f, --inactive \[INACTIVE]|number of day after password expires until the account is disabled
-g, --gid \[GID]|specify primary group
-G, --groups \[GROUPS]|supplementary groups
-m, --move-home|moves the user's home directory to a new location, use with -d
-s, --shell \[SHELL]|specify a new login shell for the user
-L, --lock|lock the user's account
-U, --unlock|unlock the user's account

## Groups

**`groupadd`**&nbsp;&nbsp;`-g [GID] [group]` adds a group with the specified ID and name.

**`groupmod`**&nbsp;&nbsp;`-g [GID] [group]` changes the ID of the specified group (&nbsp;`-n` to change name).

**`groupdel`**&nbsp;`[group]` deletes the specified group.

**`gpasswd`**&nbsp;&nbsp;`-d [user][group]` remove the user from the group.

## Password

```none
    ||------------------max days (-M)---------------||
    |                   |       |                    |
    |                   |       |                    |
    ||---min days (-m)--|       |--warn days (-w)---|||--inactive days (-I)-|
time|------------------------------------------------|----------------------|
    |                                                |                      |
last change date (-d)                     password expiration date      inactive date
```

**`chage`**&nbsp;&nbsp;`-l [username]` list user's current settings.

**`chage`**&nbsp;&nbsp;`-E YYYY-MM-DD [username]` makes the account expire n the specified date.

**`chage`**&nbsp;&nbsp;`-d 0 [username]` forces a password change on the next login.

**`chage`**&nbsp;&nbsp;`-m 0 -M 90 -W 7 -I 14 [username]` change the settings to 0 days required to change password,  90 days for the password to expire, warning of password expiring 7 days before it happens, 14 days before the account inactivation.

Option|Description
-|-
-d|change the last time the password was changed
-E|set date (YYYY-MM-DD) of account's expiration
-I|days before the password becomes inactive
-m|minimum age/time before changing the password
-M|maximum age of the password
-W|warning before the password expiration date
