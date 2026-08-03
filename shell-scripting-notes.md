# Scripting
## The shebang line `#!/bin/bash` 

first line as always. this tells the system which interpreter to use. Scripts without having it, the system doesn't know what to do with the file. it consists of a hash (#) and an exclamation point (!), followed by the absolute path to the interpreter engine.
Essentially means, The shebang tells Linux which interpreter should execute the script. `#!/bin/bash  just means Run this script using Bash.`
The shebang allows the script to be executed directly: `./script.sh`
Without a shebang,usually need to specify the interpreter manually.
- Example `bash script.sh`

All the normal linux commands like `echo, find, grep, cp` all of it works the same inside a script.
---
## Executable permission

the file needs `+x` or the system refuses to run it as a program. 
by default, when creating a new text file using vi, vim, or nano, Linux marks it as a read/write file not as a program.

To make it as a script so that the system can run it, chmod must grant execution rights : `chmod +x /path/to/scripts/script.sh`
---
## Exit status codes (exit)

Every commands or script run in Linux leaves behind an invisible report card called an Exit Status (an integer between 0 and 255).

- 0 means Success, Everything completed perfectly.
- any number from 1 to 255 means failure or an error has occurred.
In a script, the script will automatically return the exit status of the very last command that ran inside it. declaring `exit 0` at the very bottom when a script fulfills its duties cleanly would be a good habbit.
the variable `$?` holds the exit code of the last command that ran. example `echo $?    # prints 0 if last command succeeded, non-zero if it failed`

Simple example script 

```bash
#!/bin/bash

# display the current logged-in user
echo "Current Administrator:"
whoami

# terminate the script and declaring success
exit 0
```
### More Examples
```bash
0 = success
non-zero = failure
```
```bash
ping -c 1 google.com
echo $?
```
```bash
if ping -c 1 google.com
then
    echo "Network OK"
else
    echo "Network Down"
fi
```
---
# IF Statements
```bash
#!/bin/bash

if [ -f /etc/passwd ]
then
    echo "File exists"
fi
```
```bash
#!/bin/bash

if [ -f /etc/passwd ]
then
    echo "File exists"
fi
```
### Comparing Values
```bash
if [ "$name" = "Morty" ]   # equal
if [ "$name" != "Morty" ]  # not equal
```
```bash
if [ $age -gt 18 ]
```

## Common operators
```
-eq  equal
-ne  not equal
-gt  greater than
-lt  less than
-ge  greater/equal
-le  less/equal
```
# FOR Loops
```bash
for user in john mary bob
do
    echo $user
done
```
Checking multiple servers
```bash
for server in web01 web02 db01
do
    ping -c 1 $server
done
```
## Process files
```bash
for file in *.txt
do
    echo $file
done
```

# WHILE Loops
```bash
count=1

while [ $count -le 5 ]
do
    echo $count
    count=$((count+1))
done
```
# Command Output
Capturing command output -  `Capture command output:`
using it `echo $hostname`
example 
```bash
disk=$(df -h / | tail -1 | awk '{print $5}')

echo $disk
```

# && and ||
```bash
if command
then
   echo success
fi
```
is same as 
```bash
command && echo success

example use case :  id morty && echo "User exists"
                    id morty || echo "User missing"
```
# Functions
```bash
check_disk() {
    df -h /
}

check_disk
```
examples 
```bash
#!/bin/bash

service=sshd

if systemctl is-active --quiet $service
then
    echo "$service running"
else
    echo "$service stopped"
fi
```
```bash
#!/bin/bash

for user in alice bob charlie
do
    useradd $user
done
```

```bash
#!/bin/bash

echo "===== HOSTNAME ====="
hostname

echo
echo "===== MEMORY ====="
free -h

echo
echo "===== DISK ====="
df -h
```
---
# RHEL 10 vs 9 changes and updates 
```bash
cat /etc/shadow
root:$y$j9T$ctXl8i82b9f3YW4tEYn0ckpJ$KVZAE1eaSsVkBoNAGkb.rVz41xWiOn9Wkg.BBuU5fh1::0:99999:7:::
```
- One thing worth noticing in that shadow output is the password hashes starting with `$y$` is known as `yescrypt`, the new default hashing algorithm in RHEL 10. 
- Older systems used `SHA-512` which starts with $6$. 

---
# Normal output stdout and Error output stderr
Throwing away normal output ```>/dev/null```
Throwing away error output ```2>/dev/null```
More shorter way ```>/dev/null 2>&1``` meaning throw away stdout and stderr (normal outputs and errors)
---
# ? is special character
```echo $?``` means `The exit status of the last command.`
Example 
```bash
mkdir test
echo $?
```
Out put 0 or 

```bash
mkdir test
mkdir test
echo $?
```
Output 1
here in this command, $ means:
"Give me the value of the special variable ?."
---
## Other special characters
`$$` = Current shell's Process ID (PID).
Example : echo $$ might print  4521
`$USER` = Current username.
`$HOME` = Home directory.
`$1` = first argument
`$2` = second argument
---
## standard input/output/error >/dev/null and 2>$1
- stdin  = 0 
- stdout = 1  (stdout is File Descriptor 1)
- stderr = 2 
---
# Every Linux Command Produces 3 Things

- stdout = 1 (standard output or Normal output)
- stderr = 2  (standard errors or error messages)
- exit status = 0/1/2.. (Zero means sucess and rest are failures), could be view with `echo $?` command

---
## /dev/null
- Everything sent here discarded and gone. 
`echo Hello >/dev/null` output Hello would disappear and gone. 
 '>' means redirection stdout (Normal Outputs) somewhere else. 
 `hostname > server.txt` Instead of stdout to Terminal, it'll become stdout to server.txt file.
 If the file doesn't exist (in this case server.txt) Linux will create it. if it exist, it'll overwrite. 
 '>>' can be use to append to the files instead of overwriting. 

 ---
 ## 2>/dev/null
- 1 = stdout (normal outputs)
- 2 = stderr (error outputs)
so `2>/dev/null` means Errors discarded and gone. 

---
## >/dev/null 2>&1
`>/dev/null` means stdout to trash. 
`2>&1` means send stderr to wherever stdout is currently going.
basically means "Throw away everything."
---
# Order matters
`>/dev/null`
- Redirections happen from LEFT to RIGHT.
- Bash processes them one at a time.
- Every redirection changes where the pipes go.
- The next redirection uses the NEW locations.

Correct
`command >/dev/null 2>&1`
Wrong
`command 2>&1 >/dev/null`
The two commands are not the same.

`>/dev/null`
First move the normal output.
2>&1
Then tell the errors,
"Follow stdout"
- stdout goes first.
- stderr follows stdout.
---
# even shorter and clearer way
` command &>/dev/null` shorthand for `command >/dev/null 2>&1`
---
## if - fi and case - esac (backwards)
fi means ending block of if code block. 
esac means ending blocks of case code block. 
## ! and $ 
! sign means 'NOT', but not negative value as in other actual programming languages like c#, java etc. 
example : `while ! ping -c1 server01` means While
'server is NOT reachable Keep trying'

$ sign means `Give me the value of` in bash. 
Example : `$USER` means give me value of current username
Example2: `$?` means give me value of exist status.
---
# in summary

Bash = Command Orchestrator

- 1 = stdout (normal output)
- 2 = stderr (errors)
- 0 = stdin (keyboard input)

Exit Status:
0 = Success
!=0 = Failure

>    =  Overwrite file
>>   = Append file

/dev/null = Linux Trash bin 

>/dev/null
- Throw away stdout

2>/dev/null
- Throw away stderr

>/dev/null 2>&1
- Throw away EVERYTHING

`$HOME`
- Home directory

`$USER`
- Current user

`$?`
- Last command's exit status

`$1`
- First script argument

`if`
- Did the command succeed?

`fi`
- End of if

`case`
- Multiple choices decision

`esac`
- End of case

!
 means 'NOT'

 ---
 #Some Basic Scripts
 #!/bin/bash

- Capture command outputs into variables
MACHINE=$(hostname)
USER_NAME=$(whoami)

- Print the formatted output
echo "Running on: $MACHINE"
echo "As user: $USER_NAME"

Output on the host Debian machine 
```bash
strygwyr@debian:~/Documents/syseng-notes/testscripts$ bash hello.sh
Running on: debian
As user: strygwyr
```

The ```$()``` syntax is called command substitution.It runs the command inside the parentheses, captures whatever it prints to stdout, and hands that value to the variable.

- bash hello.sh explicitly calls bash to interpret it, ignoring the shebang. ```./hello.sh``` uses the shebang to decide the interpreter. On the exam, it's a good habbit to use use ```./```
---
