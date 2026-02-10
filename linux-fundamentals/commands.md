# Linux Commands Cheat Sheet

These are my Linux Module command notes as part of my DevOps learning journey.  

---


# Directory Navigation

List files and folders:
`ls`

Show hidden files too:
`ls -a`

Print current working directory:
`pwd`

Change directory:
`cd <directory_name>`

Example:
`cd Documents`


# Creating and Removing Directories

Create a directory:
`mkdir <directory_name>`

Create a nested directory:
`mkdir -p <nest1/nest2/nest3>`

Example:
`mkdir -p project/src/components`

Create nested directories:
`mkdir -p project/src/components`

Remove an empty directory:
`rmdir`

Remove a directory and its contents:
`rm -r <directory_name>`

**WARNING: Never run this** (deletes everything)
`rm -rf /`


# Working with Files
Create a new file:
`touch file.txt`

Remove a file (once you're in desired directory):
`rm file.txt`

Copy a file:
`cp file.txt newfile.txt`

Copy a directory:
`cp -r <directory_name> <new_dir_name>`

Move or rename a file (or can be a directory):
`mv file.txt renamed.txt`


# Viewing File Contents

Display file contents:
`cat file.txt`

View the first 10 lines:
`head file.txt`

View the first 5 lines:
`head -n 5 file.txt`

View the last 10 lines:
`tail file.txt`


# Writing Text into Files

Write text into a file (overwrite):
`echo "Hello World" > file.txt`

Append text to a file:
`echo "Another line" >> file.txt`


# Searching Inside Files

Find text using grep:
`grep "Hello" file.txt`


# Manual Pages

View documentation for a command:
`man <command>`

Example:
`man ls`


# Vim Basics

### There are 3 modes:
- command mode `Press ESC to access` → `h(left), j(down), k(up), l(right)`
- insert mode `Press i to access` → edit text
- visual mode `Press v to access`

Save and quit:
`:wq!`

Quit without saving:
`:q!`
> ### In command mode:
> 
> Delete Line: `dd`
> 
> Copy line:
> `yy`
>
> Paste line:
> `p`
>
> Undo:
> `u`
>
> Redo:
> `Ctrl + r`
>
> Move to end of line:
> `SHIFT + 4`
>
> Move forward by a word:
> `w`
>
> Navigate to a specific line:
> `:7` *(Goes to 7th line here)*
>
> Find a command or a word:
> `/<word>`
> Go to next occurence:
> `n`
> Go to prev. occurence of this <word>:
> `N`


# Users and Permissions

Switch to root user:
`sudo su`

Exit root:
`exit`

Display last 10 commands with `sudo` *(or as root user)*:
`sudo tail /var/log/auth.log`

Add a user:
`sudo useradd <username>`

Set password for a user:
`sudo passwd <username>`

Give sudo privileges:
`sudo usermod -aG sudo <username>`

Remove sudo privileges:
`sudo deluser <username> sudo`


# Groups

Create a group:
`sudo groupadd <groupname>`

Add user to group:
`sudo usermod -aG <groupname> <username>`

Remove user from group:
`sudo gpasswd -d <username> <groupname>`

Delete a group:
`sudo groupdel <groupname>`


# File Permissions

### File Permission Table (rwx)

| Category       | Read (r) | Write (w) | Execute (x) |
|---------------|----------|-----------|-------------|
| **Octal Value** | 4        | 2         | 1           |
| **Binary**      | 1        | 1         | 1           |



### Permission Groups

- **User / Owner → (u)**
- **Group → (g)**
- **Other Users → (o)**



### Full Breakdown Example

| Role          | r | w | x | Octal |
|--------------|---|---|---|-------|
| User (u)     | 1 | 1 | 1 | 7     |
| Group (g)    | 1 | 1 | 1 | 7     |
| Others (o)   | 1 | 1 | 1 | 7     |

**Total Permission:** `777` → `rwxrwxrwx`

---

View permissions
`ls -l`

Change permissions using symbolic mode:

    `chmod u+x file.txt`

    `chmod g+w file.txt`

    `chmod o-r file.txt`

Change permissions using octal notation:
`chmod 760 file.txt`

Change file owner to <username>:
`sudo chown <username> file.txt`

Change group ownership to <groupname>:
`sudo chgrp <groupname> file.txt`

Change file owner and group ownder at same time:
`sudo chown -R <username> : <groupname> <filename>`

Change directory owner and group ownder at same time:
`sudo chown -R <username> : <groupname> <directoryname>`

---

# Standard Input, Output & Error Streams

In Linux/Unix systems, every command uses three standard data streams:

- Standard Input (stdin) - stream 0  
  Receives input (usually from the keyboard, a file, or a pipe).

- Standard Output (stdout) - stream 1  
  Sends normal command output.

- Standard Error (stderr) - stream 2  
  Sends error messages.



## Redirection Operators

`<`   Redirect input from a file  
`>`   Redirect standard output (overwrite)  
`>>`  Redirect standard output (append)  
`2>`  Redirect standard error  
`2>&1` Redirect stderr to the same place as stdout  
`/dev/null` Discard output completely  


## Redirecting Error Messages

To save the error message to a file, redirect stderr:

`ls nonexistant 2> error.txt`


## Redirecting Both Output and Errors

To capture both normal output and error messages in one file:

`ls nonexistant > error.txt 2>&1`

## Example: Separating Output and Errors

`ls realfile nonexistant > output.txt 2> error.txt`

- `output.txt` contains normal output
- `error.txt` contains error messages

---

# Environment Variables

Create a temporary variable
`export MYVAR="Hello World"`

Print variable value
`echo $MYVAR`

View PATH variable
`echo $PATH`

Add a directory to PATH
`export PATH=$PATH:/home/ubuntu`

# Aliasses (Short-cut commands)

Print all existing aliasses:
`alias`

Create new alias called <Name> to execute command:
`alias <Name> = 'echo "Hello World"'`

Execute script in current shell:
`source file.sh` or `. file.sh`

# Command History

View command history
`history`

Run a previous command by number
`!124`

Reverse search through history
`Ctrl + r`

