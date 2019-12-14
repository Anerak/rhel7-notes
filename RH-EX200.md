# Resume

Most of the stuff here comes from the .txt with notes. There are some cases where I copied the information from the man pages or Internet.

Some sections like **`vim`** or **`kickstart`** aren't present due printing reasons (paper and ink are expensive).

## **`ls` & `redirect symbols`**

### Why two different things share a table? Because I'm trying to save space

ls option|Description|Redirect Symbol|Description
-|-|-|-
l   | Extended output|< \<filename>|uses file as stdin
ld  | Directory output|>|stodout overwrites file
a   | Shows all files, even hidden ones|>>|sdtout appends to file
Z   | SELinux context|2> \<filename>|stderr to file
R|Recursive|2>1|stderr to stdout
&nbsp;|&nbsp;|&> \<filename>|stdout and stderr to file name

## **`touch` command**

Create files if they don't exist, otherwise, modify the timestamp.

**`touch`**&nbsp;`foo` creates the file `foo`

## **`ln` command**

The purpose of **`ln`** is create another name for a file. Reference the same contents of the file but with another name.

If you delete a file with a hard link, the content will be available on the hard link.

If you delete a file with a symbolic link, the symbolic link won't work.

**`ln`**&nbsp;`[source] [name of the link]`

**`ln`**&nbsp;`fileA fileB` creates a link where fileA is the original

**`ln`**&nbsp;&nbsp;`-s fileA symfileB` creates a symbolic link

## **`grep` command**

Option|Function
-|-
-i|case insensitivity
-v|lines without matches
-r|recursive search
-A `[n]`|display X of lines after the match
-B `[n]`|display X of lines before the match
-e|multiple RegEx can be supplied as OR
-n|display line number

### **RegEx**

Symbol|Usage|Example|Applies for
-|-|-|-
\^|beginning of the line|^cat|category
\$|end of the line|$dog|chilidog
\.|wildcard single character|c.t|cat/cbt/cct
\*|any amount of characters|c*t|cat/cbt/caaaaat
\.\*|zero to infinitely characters|c.*t|ct/cat/coat/culvert
.\\{\\}|explicit multiplier|c.\\{2\\}|coat
\\<&nbsp;\\>|word boundary|\\<ipsum\\>|Lorem ipsum et
[ ]|options for a single character|c\[abc]t|cat/cbt/cct

## **`locate` & `find`**

**`locate`**&nbsp;`[search term]` search every file with the search term on it's name.

**`locate`**&nbsp;&nbsp;`-i [search term]` case insensitive.

**`locate`**&nbsp;&nbsp;`-n [X] [search term]` search and stops after X results.

**`updatedb`** update the locate database.

**`find`**&nbsp;`[directory to start] [search term]`

Option|Function
-|-
-user|search files that belong to that username
-uid|same as -user but with the UID
-group|search files that belong to that group
-gid|same as -group but with GID
-perm `[permissions]`|search for permissions based on the operator
&nbsp;|764 only `-rwxrw-r--`
&nbsp;|\-324  at least `--wx-w-r--`
&nbsp;|/442 `u r--` OR `g r---` OR `o -w-`
-size `[n][k|M|G]`|search by size (round up to single units 995 KiB = 1MiB)
&nbsp;|\+10M more than 10 MiB
&nbsp;|\-1G less than 1 GiB
-mmin `[n]`|modified files since at least `[n]` minutes
-type| `r` regular file, `d` directory, `l` symlinks, `b` block device
-links|regular files with more names

**`find`**&nbsp;&nbsp;`/home -user foo` find all the files that belong to `foo`

**`find`**&nbsp;&nbsp;`/ -type l -links +3`

## **Users**

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
-c, --comment `[COMMENT]`| full name of the user for the GECOS field.
-d, --home `[HOME_DIR]`|specify user's home directory
-e, --expiredate `[EXPIRE DATE]`|date on which the user account will be disabled (YYYY-MM-DD)
-f, --inactive `[INACTIVE]`|number of day after password expires until the account is disabled
-g, --gid `[GID]`|specify primary group
-G, --groups `[GROUPS]`|supplementary groups
-m, --move-home|moves the user's home directory to a new location, use with -d
-s, --shell `[SHELL]`|specify a new login shell for the user
-L, --lock|lock the user's account
-U, --unlock|unlock the user's account

## **Groups**

**`groupadd`**&nbsp;&nbsp;`-g [GID] [group]` adds a group with the specified ID and name.

**`groupmod`**&nbsp;&nbsp;`-g [GID] [group]` changes the ID of the specified group (&nbsp;`-n` to change name).

**`groupdel`**&nbsp;`[group]` deletes the specified group.

**`gpasswd`**&nbsp;&nbsp;`-d [user][group]` remove the user from the group.

## **Password**

```none
    ||--------------max days (-M)------------||
    |                |      |                 |
    |                |      |                 |
    ||-min days (-m)-|      |-warn days (-w)-|||-inactive days (-I)-|
time|-----------------------------------------|---------------------|
    |                                         |                     |
last change date (-d)             password expiration date      inactive date
```

**`chage`**&nbsp;&nbsp;`-l [username]` list user's current settings.

**`chage`**&nbsp;&nbsp;`-E YYYY-MM-DD [username]` makes the account expire n the specified date.

**`chage`**&nbsp;&nbsp;`-d 0 [username]` forces a password change on the next login.

**`chage`**&nbsp;&nbsp;`-m 0 -M 90 -W 7 -I 14 [username]` change the settings to 0 days required to change password,  90 days for the password to expire, warning of password expiring 7 days before it happens, 14 days before the account inactivation.

Option|Description
-|-
-d `[n]`|change the last time the password was changed
-E `[YYYY-MM-DD]`|set date of account's expiration
-I `[n]`|days before the password becomes inactive
-m `[n]`|minimum age/time before changing the password
-M `[n]`|maximum age of the password
-W `[n]`|warning before the password expiration date

## **Permissions**

### Word model

**`chmod`**&nbsp;`WhoWhatWhich <filename>`

**r** read, **w** write, **x** execute.

**`chmod`**&nbsp;`g=rw- foo` sets read and write for the group of the file _foo_.

**`chmod`**&nbsp;`u+x script` adds the execute permission for the owner of the file _script_

### Shared table for the Word model

Word|Operator|Permission|Special bit
-|-|-|-
u (owner)|+ (add permission)|r (read)|s (suid, using u)
g (group)|- (remove permission)|w (write)|s (sgid, using g)
o (other)|= (set permission)|x (execute)|t (sticky, only directories)

### Numeric model

Number|Permission|Special bit
-|-|-
4|read|suid
2|write|sgid
1|execute|sticky

**`chmod`**&nbsp;`0700 foo` equivalent to `-rwx------`

**`chmod`**&nbsp;`4554 script` equivalent to `-r-sr-xr--`

**`chmod`** supports `-R` for recursive operations.

**`chown`**&nbsp;`[user]:[group]` change file ownership.
  
### **`umask`**

Change the default permissions applied to a new created file/directory using **`umask`**.

Write the value for the permissions excluded.

**`umask`**&nbsp;`0022` new files will be created as `-rwxr--r--` and `drwxr--r--`.

## **Processes**

**`ps`**&nbsp;`aux` processes with `USER PID %CPU %MEM TTY STATUS`.

**`ps`**&nbsp;`lax` long listing style, avoid username lookup.

**`ps`**&nbsp;`-ef` display all processes.

**`ps`**&nbsp;`j` jobs running.

### **Process status**

Name|Flag|Kernel state|Description
-|-|-|-
Running|R|TASK_RUNNING|executing on a CPU or waiting to run
Sleeping|S|TASK_INTERRUPTIBLE|waiting for some condition (hw request, resources, signal)
&nbsp;|D|TASK_UNINTERRUPTIBLE|sleeping but won't respond to signals
&nbsp;|K|TASK_KILLABLE|like D but waiting for a signal to be killed
Stopped|T|TASK_STOPPED|stopped by being signaled (by user or another process)
Zombie|Z|EXIT_ZOMBIE|child process signals it's parent as it exists. Free resources
&nbsp;|X|EXIT_DEAD|parent reaps the remaining child process structure. Now free

### **Jobs**

Useful when you have access to only ONE terminal.

**`[command]`**&nbsp;`&` the ampersand moves the program to the background automatically.

**`jobs`** display running jobs on the background.

**`fg`**&nbsp;`%[job ID]` bring job to the foreground.

**`bg`**&nbsp;`%[job ID]` resume stopped process in the background.

**`Ctrl + Z`** suspends the process and send it to the background (use before **`bg`**).

**`kill`**&nbsp;`%[job ID]` kill the job running in the background.

### **`kill` command**

**`man`**&nbsp;`7 signal` for more details.

Number|Name|Definition|Purpose
-|-|-|-
1|SIGHUP|Hangup|report termination of the controlling process of a terminal
2|SIGINT|Keyboard interrupt|interrupt from keyboard (**`Ctrl + C`**)
3|SIGQUIT|Keyboard quit|quit from keyboard (**`Ctrl + \`**)
9|SIGKILL|Kill, unblockable|abrupt program termination. Always fatal
15|SIGTERM|Terminate|termination signal, process should close properly
18|SIGCONT|Continue|resume process if stopped
19|SIGSTOP|Stop, unblockable|suspend the process
20|SIGTSTP|Keyboard stop|can be blocked or handled (**`Ctrl + Z`**)

**`kill`**&nbsp;`[PID]` kill the process with the default signal (SIGTERM,15).

**`kill`**&nbsp;&nbsp;`-[signal] [PID]` send the specified signal (name or number).

**`kill`**&nbsp;&nbsp;`-l` list all the available signals.

**`killall`**&nbsp;`[command pattern]` kill all the processes that matches the command pattern.

**`killall`**&nbsp;&nbsp;`-[signal] [command pattern]` send the specified signal to all the process that matches the command pattern.

**`killall`**&nbsp;&nbsp;`-[signal] -u [username] [command]` same as before but only those that belong to the specified user.

### **`pkill` command**

It's like killall, and uses an advanced selection criteria.

#### Use **`pgrep`** to check which processes will be affected

Option|Name|Description
-|-|-
`[command]`|Command|processes matching that command
-U|User ID|processes owned by that user
-G|Group ID|processes owned by that group
-P|Parent|processes belonging to that parent process
-t|Terminal|processes controlled by that terminal

**`pkill`**&nbsp;`[command pattern]`

**`pkill`**&nbsp;&nbsp;`-U 1000` kill all the processes that belong to the user with ID 1000.

**`pgrep`**&nbsp;&nbsp;`-l -u foo` display all the processes running by the user `foo`

**`w`**&nbsp;&nbsp;`-f` display who's logged into the system and their activities.

**`pstree`**&nbsp;&nbsp;`-p [username]` tree representation of the processes running by the specified user.

### **Process activity**

**`uptime`** display the load average of the last 1, 5 and 15 minutes.

**`grep`**&nbsp;`"model name" /proc/cpuinfo |`&nbsp;**`wc -l`** Count the cores of the machine (both physical and hyperthread ones).

Divide each number by the amount of cores. If the result is greater than 1 (>1), the CPU is overloaded.

**`top`** real-time process monitoring

#### List of columns

Name|Description
-|-
USER|process owner
VIRT|virtual memory is all the memory that the process is using
RES|physical memory used by the process
S|process state.
&nbsp;|\[D] uninterruptable sleeping  \[R] Running or Runnable
&nbsp;|\[S] Sleeping \[T] Stopped or Traced \[Z] Zombie
TIME|total processing time since the process started
COMMAND|process command name

#### Keystrokes

Key|Purpose
-|-
?  \/ h|help for interactive keystrokes
l t m|toggles for load, threads and memory header lines
1|toggle showing individual CPUs or a summary in header
s|refresh rate in decimal seconds (0.5,1,5)
b|reverse highlighting for Running processes; default = bold
B|enables use of bold in display
H|toggle threads
u,U|filter for username
M|sort by memory usage
P|sort by processor utilization
k|kill a process, ask for PID and signal
r|renice a process, ask for PID and nice_value
W|save the current display configuration for the next restart
q|quit

### **`nice` & `renice`**

Nice levels of a process goes from -20 to 19 for users.

**`top`** displays them from RT,-99 to 39.

Nice level of 20 for users translates as 0 for **`top`**.

Use **`nice`** for run programs, **`renice`** for already running programs.

**`nice`**&nbsp;&nbsp;`-n [nice level] [command]` run the program with the specified nice level.

**`renice`**&nbsp;&nbsp;`-n [nice level] [PID]` renice the process that is already running.
