# working with echo commands
## What is echo?
echo command print text to the terminal (standard output). Its one of the most used commands in Linux, for scripting, writing to files, checking variables, and many more.
## redirection operators 
```bash
>   = write to file, overwrite everything
>>  = add to the end
```
## >  write (overwrite)
```
echo "Hello" > myfile.txt      # creates or overwrites myfile.txt
echo "New content" > myfile.txt # previous content is gone
```
## >>  append
```
echo "Line 1" > myfile.txt     # creates file with "line 1"
echo "Line 2" >> myfile.txt    # adds "line 2" below  file now has both
```
*** note : when unsure use >> accidentally overwriting a config file.
## echo flags
-n no newline at the end echo 
```
-n "no newline"
```
-e enable escape characters (like \n, \t) 
```
echo -e "line1\nline2"
```
## -e escape characters
```
\n new line
\t  tab
\\ backslash
echo -e "Name:\tJoe\nRole:\tSysadmin"
display as 
Name:   Joe
Role:   Sysadmin
```
## printing variables

```bash
NAME="joe"
echo $NAME           # joe
echo "Hello, $NAME"  # Hello, joe
echo "Hello, ${NAME}man"  # Hello, joeman  
note :  use {} to avoid confusion
```

## single quotes vs double quotes
"double" = variables and escapes are expanded
'single' = everything is treated as literal text
```
NAME="joe"
echo "$NAME"    # joe    (variable expanded)
echo '$NAME'    # $NAME  (printed literally as it is) 
```

## practical uses

add a line to a config file
```
echo "PermitRootLogin no" >> /etc/ssh/sshd_config
```
## make a quick file
```
echo "192.168.1.50  rhel-vm" >> /etc/hosts
```
## check an environment variable
```
echo $PATH
echo $HOME
echo $USER
```
## write a simple script header
```
echo "#!/bin/bash" > myscript.sh
echo "echo Hello from script" >> myscript.sh
```
## echo vs printf 
printf is more powerful and predictable than echo -e. and it is more prefer in scripts. 
```
printf "Name:\t%s\nRole:\t%s\n" "Joe" "Sysadmin"
```
```
display 
Name:   Joe
Role:   Sysadmin
note: use echo for quick one liners in the terminal. 
use printf inside shell scripts for consistent formatting.
```
## quick echo command references 
```
echo "text"                  # print to terminal
echo "text" > file.txt       # write to file (overwrites)
echo "text" >> file.txt      # append to file
echo -n "text"               # no trailing newline
echo -e "line1\nline2"       # interpret escape chars
echo $VARIABLE               # print variable value
echo "${VAR}text"            # variable with adjacent text
echo '$VARIABLE'             # print literally   
```
### Note : 
```bash
instead of using touch command, empty output redirection can be use to create a file, example:
> ~/rhcsa/labs/backup_set/system_token.txt
```

# tar command - archiving, compressing and decompressing files and directories

## tar command concepts 
### archiving 
(means tape archiving back in the days): combining multiple files and directories into a single file (.tar). 
however, archiving alone not reduce file size on its own.
for that we need to use 
### compressing: 
reducing the file size of the archive using an algorithm.
### decompressing: 
restoring the compressed archive back to its original size before extraction.
### 3 core operations
always have to choose exactly one of these primary modes:
```bash
-c : Create a new archive
-x : Extract an existing archive
-t : list (test) the contents of an archive without extracting

```
## compression modifiers or compression algorithm types
those flags tell tar which compression tool to use alongside the core operation:

```bash
-z means gzip compression type with tar.gz or .tgz extension and speed is fast.
-j means bzip2 compression type with tar.bz2 or .tbz2 extension and speed is slow.
-J means xz compression type with .tar.xz extension and speed is slowest among all. 
```
### additional flags
```bash
-f (f means File here): specifies the archive file name. -f flag must always be the last flag in the option block because tar command require the filename to immediately follows it.
-C (C means directory here or its' from Change directory): instruct tar to switch to the target directory before doing the extration. this command is use to prevents files from unpacking into current working directory.
```
## daily commands
### making archives

```bash
tar -cvf backup.tar /path/to/source

# create a gzip compressed archive (common)
tar -czvf backup.tar.gz /path/to/source

# create a highly compressed xz archive (usually for production backups)
tar -cJvf backup.tar.xz /path/to/source
```
### viewing archives without extrating
```bash
list contents of a compressed archive without extracting it
tar -tzf backup.tar.gz
```
### extracting archives
```bash
extract to the current working directory
tar -xzvf backup.tar.gz
# extract to a specific target directory (-C changes directory first)
tar -xzvf backup.tar.gz -C /tmp
```

# extract a single specific file from the archive
```bash
tar -xzvf backup.tar.gz path/to/file.txt
notes: modern versions of tar (including the one in RHEL 10) are intelligence enough to auto detect the compression formats during extraction. which means flags like -z, -j, or -J could be ignore when extrating. example:
tar -xvf any_archive.tar.xz -C /target/dir
```
## summarized common commands
```bash
tar -czvf archive.tar.gz folder/    create gzip compressed
tar -cjvf archive.tar.bz2 folder/   create bzip2 compressed
tar -cJvf archive.tar.xz folder/    create xz compressed
tar -xzvf archive.tar.gz            extract gzip
tar -xzvf archive.tar.gz -C /path/  extract to specific location
tar -tzvf archive.tar.gz            list contents without extracting
```
# using man pages and search inside it
```bash
man tar
/   press / to start searching (kinda like using less command) 
after that type what need to look for, for example
/-C (to find out more about tar -C )
press enter and it jumps straight to it. 
press n to find next stuffs
exit by pressing q
```
---
### -type d
```bash
find /etc -type d -name "nginx"
```
note : -type d restricts the search results to directories (folders in windows world) only.
There are three -type values:
```bash
-type d    directories only
-type f    regular files only
-type l    symbolic links only
```
```bash
find /var/log -type f -size +1M 2>/dev/null
```
note : +1M means greater than 1 megabytes. the + sign means greater than, - sign mean less than, and no signs mean exactly that size.

```bash
example: 
-size +1M    greater than 1 megabyte
-size -1M    less than 1 megabyte
-size +100k  greater than 100 kilobytes
```
##Linux ecosystem
```bash
Linux automatically rotates log files to keep them manageable. old entries get moved to rotated files to older files:

/var/log/messages-20260525

 the current /var/log/messages only has fresh and most recent entries.
when looking for old errors and grep returns nothing, check the rotated log files too: 

sudo grep -i "failed" /var/log/messages-20260525 | grep "dnf"
```

### common grep flags and use cases
```bash
grep "text" file          basic search
grep -i "text" file       case insensitive, ignore flag
grep -v "text" file       invert, exclude matches
grep -c "text" file       count matching lines,
grep -n "text" file       show line numbers
grep -r "text" /path      search recursively
```

# Vim
```bash
There are 3 modes in Vim
Normal mode    - default mode when you open vim, for navigation
Insert mode    - for actually typing and editing text
Command mode   - for saving, quitting, searching
to read a file.  vim ~/rhcsa/labs/module1/test.txt 
press i for Insert mode
press :wq for save and quit. (colon : switches to Command mode. w means write (save). q means quit.)
type :set number to see Line numbers in Normal mode. 
press Esc to return to normal mode. 
To make line numbers permanent run this in the terminal:
echo "set number" >> ~/.vimrc
note* .vimrc is vim's personal config file in the home directory. Anything put in there loads every time vim opens.
```
## Common Vim Commands
```bash
:q!          quit without saving, force exit (good habit to use :q! when only to read a file.)
:w           save but stay in vim
dd           delete entire line
yy           copy entire line
p            paste
u            undo
gg           jump to top of file
G            jump to bottom of file
/word        search for word
n            next search result
:set number  show line numbers (permanently via ~/.vimrc customizations)
Note* : some of these commands can be combined, for example :wq (save and quit)
```
---

# Additional notes Module 1 - Flutter SDK is set as example here
## the environment variable mystery ($PATH)

programs like ls and tar are binary files sitting inside directories like /usr/bin/. they usually locate in /usr/bin or /usr/sbin paths.
when typing ls command in the terminal Bash does not automatically scan the entire hard drives looking for a file named "ls".
instead, Bash relies on a single environment variable called $PATH.
the $PATH variable is a string of directory paths separated by colons. When typing any commands, Bash reads this list from left to right and checks those specific directories for the binary. 
if it finds it, it executes it. if it doesn't, get command not found will be output.
If this command run in the terminal right now: 

```bash
PATH=""
```
the system path variable have just wiped out that lookup list for thhe current session. and at this point if we run ls. 
the system will throw an error because it's path has been wiped out and broke its map. at this point, to run commands, the system is forced the user to type absolute path explicitly: /usr/bin/ls.

### to set new PATHs for the new programs/services etc. 
#### Method one - temporary fix (applied to current session only)
```bash
example : 
export PATH="$PATH:/home/strygwyr/flutter/bin"
```
each commands : 
export: this command tells Bash to make the variable available to any sub processes or tools launched from this terminal session.

PATH=: setting the value of the PATH variable.

"$PATH:...": by typing $PATH first, it's telling the system to look at the existing map (/usr/bin, /bin, etc.), 
and then the colon : appends to the new directory to the very end of that list.
important note: If forget to include $PATH and just run export PATH="/home/strygwyr/flutter/bin", this will completely wipe out current system's lookup map for that terminal window, and basic commands like ls will stop working until start the new terminal session.

#### Method two - permanent fix
to make sure Linus system remembers setting the custom system PATHs the admin must write that line into the user profile configuration file.
the configuration file is hidden in the home directory and names as .bashrc.
can use echo to safely append the configuration line to the bottom of the file without opening an editor:

```bash
echo 'export PATH="$PATH:/home/strygwyr/flutter/bin"' >> ~/.bashrc
```

Bash only reads the .bashrc file when a new terminal session starts.
To force the current terminal window to read the changes immediately without logging out, source command can be use here:
```bash
source ~/.bashrc
```
verifying new settings changes are there, using 
```bash
echo $PATH command.
```
original Debian path paths, and the newly added custom Flutter directory path should be seen. 

---
## cp vs mv SELinux tricks
SELinux uses labels to determine what processes can touch what files. For an Apache web server to show a webpage, that file must be labeled with a specific type, usually httpd_sys_content_t. the home directory files are labeled as user_home_t.
 
### scenario 1 -  the cp command (copy)
```bash
when command like  cp /home/batman/index.html /var/www/html/ run, the Linux kernel treats this as the creation of a brand new file inside the target directory.
because it is a new file, it automatically inherits the SELinux context of the parent folder it was born in (/var/www/html).
result: the file gets the correct httpd_sys_content_t label automatically. 
apache can read it, and the website works perfectly.
```

### scenario 2 - the mv command (move)
```bash
when command like  mv /home/batman/index.html /var/www/html/ run, the Linux kernel does not create a new file. 
it simply updates the filesystem pointers to say the file now lives in a different directory. because the file itself is not recreated, it keeps its original SELinux label (user_home_t).

As a result
ccenario 2 (mv) will cause a 403 Forbidden error. Apache will see a file labeled user_home_t and it will block access to protect the system because it cant read a users private home files and stuffs.

to fix this: If move a file and break the context, use the command restorecon -v /var/www/html/index.html to force the system to reset the files label back to its proper directory default.
```
----
## Firewalld Runtime vs Permanent State
```bash
firewalld manages system traffic using two completely separate configurations running in parallel: Runtime and Permanent.
how the configurations intersect

Runtime: thats what actively loaded into the servers RAM right now. when running a standard command like 
sudo firewall-cmd --add-port=8080/tcp, 
it applies instantly to the RAM/memory.

permanent: this is a sets of XML configuration files written directly to the hard drive (inside /etc/firewalld/). 
the system only reads these files when the firewall service starts up or reloads.

breakdowns of the scenario
because the first command did not specify where to save the rule, it was only written to the temporary Runtime memory/RAM.

when the second administrator ran sudo firewall-cmd --reload, they told the system to completely wipe out the current Runtime memory and reload the configuration from the permanent XML files on the disk. 
and since port 8080 was never saved to the disk, the port 8080 rule set by the current admin will completely disappears after the reload.

solution to fix : 
to make a firewall rule survive reboots and reloads, --permanent flag: command must be add.
```
for example : 

```bash
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```
```bash
explanation: running a command with --permanent does not activate it in the runtime environment immediately. 
--permanent command must always run first and then follow it up with a --reload to push that changes into active memory.
```
---

## Umask

`umask` (User File Creation Mode Mask) controls the default permissions for newly created files and directories.
its a permission filter. instead of deciding permissions every time creating something, Linux automatically removes permissions based on the current umask values.

### the way umask work
Linux starts with default maximum permissions:
#### for directories

777 (rwxrwxrwx)
everyone can read, write, execute. 

#### for files

666 (rw-rw-rw-)
everyone can only read and write, including the onwers. 

### Common umask values
#### umask 022
most common default on many Linux systems.
for files

```bash
666 - 022 = 644
```
result:

```bash
rw-r--r-- 
(Read/Write for you, Read for others).
```

for directories

```bash
777 - 022 = 755
```
result:
```bash
rwxr-xr-x 
(Read/Write/Execute for you and your group, Read/Execute for others).
```
#### umask 002

common in shared environments where users belong to the same group.
for files
```bash
666 - 002 = 664
```
result:
```bash
rw-rw-r--
(Read/Write for you and your group, Read for others).
```
for the directories

```bash
777 - 002 = 775
```

result:

```bash
rwxrwxr-x
(Read/Write/Execute for you and your group, Read/Execute for others).
```

#### umask 077
restrictive.
for files

```bash
666 - 077 = 600
```

result:

```bash
rw-------
(Read/Write strictly for you; no one else has access).
```

for directories

```bash
777 - 077 = 700
```
result:
```bash
rwx------
(Read/Write/Execute strictly for you; no one else has access).
```
the owner has access only.

### useful umask commands

checking current umask:

```bash
umask
```
settig a temporary umask for the current session:

```bash
umask 027
```
make umask permanent by adding it to:

```bash
~/.bashrc
```
or

```bash
~/.zshrc
```
main base values:
```bash
for files = 666 (everyone can only read and write, including the onwers.)
for directories = 777 (everyone can read, write, execute.)
```
most umask questions will become easy after memorizing above two numbers.
---
#Shell Expansion
- Shell expansion allows for more efficient command line in use. 
- Globbing expands filenames based on wildcards, 
```bash 
ls *
ls a?*
ls [a-e]*
```
- Other types of expansion also exist, 
    brace expansion : touch file{1..9}
    tide expansion : cd~
    command substitution : ls -l $(which ls)
    variable substitution : echo $PATH


