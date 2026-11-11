# Linux Command Flags Cheat Sheet

A practical reference for commonly used Linux commands and their
important options (flags). This guide is intended for beginners
practising Linux on Ubuntu/WSL.

> **Important:** Linux flags are command-specific. The same flag can
> mean different things for different commands. Always check the help
> page or manual for the command you are using.

## Table of Contents

-   [1. Understanding Linux flags](#1-understanding-linux-flags)
-   [2. File and directory commands](#2-file-and-directory-commands)
-   [3. File viewing and reading
    commands](#3-file-viewing-and-reading-commands)
-   [4. Searching commands](#4-searching-commands)
-   [5. Permission and ownership
    commands](#5-permission-and-ownership-commands)
-   [6. System monitoring commands](#6-system-monitoring-commands)
-   [7. Archive and compression
    commands](#7-archive-and-compression-commands)
-   [8. Network and connectivity
    commands](#8-network-and-connectivity-commands)
-   [9. Package management commands
    (Ubuntu)](#9-package-management-commands-ubuntu)
-   [10. Important flags to learn
    first](#10-important-flags-to-learn-first)
-   [11. How to find command options](#11-how-to-find-command-options)
-   [12. Suggested learning order](#12-suggested-learning-order)

## 1. Understanding Linux flags

A flag (also called an option) changes or controls how a command
behaves.

### Short options

Usually use a single hyphen:

``` bash
ls -l
```

### Long options

Usually use two hyphens:

``` bash
ls --all
```

For example, `ls -a` and `ls --all` both show hidden files.

Some commands support combining short options:

``` bash
ls -lah
```

This is equivalent to:

``` bash
ls -l -a -h
```

It displays a detailed listing, includes hidden files, and uses
human-readable file sizes.

## 2. File and directory commands

### 2.1 `ls` --- List files and directories

  Flag   Meaning                                            Example
  ------ -------------------------------------------------- ----------
  `-l`   Long listing format                                `ls -l`
  `-a`   Show hidden files, including `.` and `..`          `ls -a`
  `-h`   Human-readable sizes (usually with `-l` or `-s`)   `ls -lh`
  `-R`   List directories recursively                       `ls -R`
  `-t`   Sort by modification time                          `ls -lt`
  `-r`   Reverse the sort order                             `ls -lr`
  `-S`   Sort by file size                                  `ls -lS`
  `-i`   Show inode numbers                                 `ls -i`

Useful combination:

``` bash
ls -lah
```

### 2.2 `mkdir` --- Create directories

  -----------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- -----------------------
  `-p`                    Create parent           `mkdir -p a/b/c`
                          directories as needed;  
                          do not fail if they     
                          already exist           

  `-v`                    Print a message for     `mkdir -v test`
                          each created directory  

  `-m`                    Set permissions when    `mkdir -m 755 test`
                          creating the directory  
  -----------------------------------------------------------------------

### 2.3 `cp` --- Copy files and directories

  -----------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- -----------------------
  `-r`                    Copy directories        `cp -r dir1 dir2`
                          recursively             

  `-i`                    Prompt before           `cp -i a.txt b.txt`
                          overwriting             

  `-v`                    Show copied files       `cp -v a.txt backup/`

  `-u`                    Copy only when the      `cp -u a.txt backup/`
                          source is newer or the  
                          destination is missing  

  `-n`                    Do not overwrite an     `cp -n a.txt backup/`
                          existing destination    
  -----------------------------------------------------------------------

### 2.4 `mv` --- Move or rename files

  -----------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- -----------------------
  `-i`                    Prompt before           `mv -i a.txt b.txt`
                          overwriting             

  `-v`                    Show what is moved      `mv -v a.txt backup/`

  `-n`                    Do not overwrite an     `mv -n a.txt b.txt`
                          existing destination    
  -----------------------------------------------------------------------

### 2.5 `rm` --- Remove files and directories

  -----------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- -----------------------
  `-r`                    Remove directories and  `rm -r test/`
                          their contents          
                          recursively             

  `-f`                    Force removal; do not   `rm -f file.txt`
                          prompt for nonexistent  
                          files                   

  `-i`                    Prompt before every     `rm -i file.txt`
                          removal                 

  `-v`                    Explain what is being   `rm -v file.txt`
                          removed                 
  -----------------------------------------------------------------------

> **Safety warning:** Be careful with `rm -rf`. It recursively removes
> files and directories without prompting. Check the path carefully
> before running it. Avoid practising destructive commands on important
> files.

## 3. File viewing and reading commands

### 3.1 `cat` --- Display file content

  Flag   Meaning                         Example
  ------ ------------------------------- -------------------
  `-n`   Number all output lines         `cat -n file.txt`
  `-b`   Number non-empty output lines   `cat -b file.txt`
  `-s`   Suppress repeated empty lines   `cat -s file.txt`

### 3.2 `head` --- Display the beginning of a file

  Flag   Meaning                                 Example
  ------ --------------------------------------- -----------------------
  `-n`   Display the specified number of lines   `head -n 5 file.txt`
  `-c`   Display the specified number of bytes   `head -c 20 file.txt`

### 3.3 `tail` --- Display the end of a file

  -----------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- -----------------------
  `-n`                    Display the specified   `tail -n 10 file.txt`
                          number of lines         

  `-f`                    Keep following new      `tail -f app.log`
                          content appended to the 
                          file                    

  `-c`                    Display the specified   `tail -c 20 file.txt`
                          number of bytes         
  -----------------------------------------------------------------------

A particularly useful command when checking application logs:

``` bash
tail -f application.log
```

It continuously displays new log entries as they are written. Press
`Ctrl+C` to stop following the file.

### 3.4 `less` --- Read files page by page

  -----------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- -----------------------
  `-N`                    Display line numbers    `less -N file.txt`

  `-S`                    Do not wrap long lines; `less -S file.txt`
                          allow horizontal        
                          scrolling               

  `-i`                    Ignore case in searches `less -i file.txt`
                          when appropriate        
  -----------------------------------------------------------------------

Inside `less`, press `q` to quit. Type `/word` to search for a word.

## 4. Searching commands

### 4.1 `grep` --- Search for text inside files

  -----------------------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- -----------------------------------
  `-i`                    Ignore                  `grep -i error app.log`
                          uppercase/lowercase     
                          differences             

  `-n`                    Show matching line      `grep -n error app.log`
                          numbers                 

  `-v`                    Show lines that do not  `grep -v error app.log`
                          match                   

  `-r`                    Search recursively in   `grep -r error logs/`
                          directories             

  `-l`                    Show filenames          `grep -l error *.log`
                          containing a match      

  `-c`                    Count matching lines in `grep -c error app.log`
                          each file               

  `-w`                    Match whole words       `grep -w error app.log`

  `-E`                    Use extended regular    `grep -E "error|warning" app.log`
                          expressions             
  -----------------------------------------------------------------------------------

### 4.2 `find` --- Search for files and directories

The following are expressions used by `find` rather than short flags.

  ----------------------------------------------------------------------------
  Option / expression     Meaning                 Example
  ----------------------- ----------------------- ----------------------------
  `-name`                 Search by filename      `find /home -name "*.txt"`
                          (case-sensitive)        

  `-type f`               Find regular files      `find . -type f`

  `-type d`               Find directories        `find . -type d`

  `-iname`                Search by filename,     `find . -iname "*.TXT"`
                          ignoring case           

  `-size`                 Search by file size     `find . -size +10M`

  `-mtime`                Search by modification  `find . -mtime -7`
                          time in days            
  ----------------------------------------------------------------------------

Examples: - `find . -type f` finds regular files under the current
directory. - `find . -type d` finds directories under the current
directory. - `find . -name "*.txt"` finds files whose names end in
`.txt`.

## 5. Permission and ownership commands

These commands are important for Linux administration and DevOps.

### 5.1 `chmod` --- Change file permissions

  Flag   Meaning                             Example
  ------ ----------------------------------- ------------------------
  `-R`   Change permissions recursively      `chmod -R 755 folder/`
  `-v`   Show permission changes             `chmod -v 755 file.sh`
  `-c`   Report only when a change is made   `chmod -c 755 file.sh`

Example:

``` bash
chmod 755 file.sh
```

Changes the file's permissions. The numeric mode `755` gives the owner
read/write/execute permissions and the group and others read/execute
permissions.

### 5.2 `chown` --- Change file ownership

  ------------------------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- ------------------------------------
  `-R`                    Change ownership        `sudo chown -R user:group folder/`
                          recursively             

  `-v`                    Show ownership changes  `sudo chown -v user file.txt`

  `-c`                    Report only when        `sudo chown -c user file.txt`
                          ownership changes       
  ------------------------------------------------------------------------------------

Example:

``` bash
sudo chown user file.sh
```

Changes the file owner to `user`.

**Remember:** - `chmod` changes permissions. - `chown` changes
ownership.

Use recursive permission or ownership changes carefully, especially on
system directories.

## 6. System monitoring commands

### 6.1 `ps` --- Display running processes

  Option   Meaning                    Example
  -------- -------------------------- ------------------
  `-e`     Select all processes       `ps -e`
  `-f`     Use full-format listing    `ps -ef`
  `-u`     Select processes by user   `ps -u username`
  `-p`     Select processes by PID    `ps -p 1234`

A commonly used form is:

``` bash
ps aux
```

It displays a detailed process list. Note that `ps aux` uses the
BSD-style option syntax, without a leading hyphen.

### 6.2 `kill` --- Send a signal to a process

  -----------------------------------------------------------------------
  Option                  Meaning                 Example
  ----------------------- ----------------------- -----------------------
  `-l`                    List signal names       `kill -l`

  `-15`                   Send SIGTERM (request   `kill -15 1234`
                          graceful termination)   

  `-9`                    Send SIGKILL (force     `kill -9 1234`
                          termination)            
  -----------------------------------------------------------------------

Prefer normal termination first. Use signal 9 only when a process does
not stop gracefully.

### 6.3 `df` --- Check filesystem disk space

  Flag   Meaning                                   Example
  ------ ----------------------------------------- ---------
  `-h`   Show sizes in human-readable units        `df -h`
  `-T`   Show filesystem type                      `df -T`
  `-i`   Show inode usage instead of block usage   `df -i`

### 6.4 `du` --- Check file and directory sizes

  Flag   Meaning                                Example
  ------ -------------------------------------- --------------------
  `-h`   Human-readable sizes                   `du -h folder/`
  `-s`   Display a summary only                 `du -sh folder/`
  `-a`   Include files as well as directories   `du -ah folder/`
  `-d`   Limit directory depth (GNU `du`)       `du -h -d 1 /home`

### 6.5 `free` --- Check memory usage

  Flag   Meaning                                        Example
  ------ ---------------------------------------------- -------------
  `-h`   Human-readable output                          `free -h`
  `-m`   Display values in megabytes                    `free -m`
  `-g`   Display values in gigabytes                    `free -g`
  `-s`   Repeat the display at an interval in seconds   `free -s 2`

## 7. Archive and compression commands

### 7.1 `tar` --- Create and extract archives

  Flag   Meaning                        Example
  ------ ------------------------------ ----------------------------------
  `-c`   Create an archive              `tar -cf backup.tar folder/`
  `-x`   Extract an archive             `tar -xf backup.tar`
  `-v`   Show files being processed     `tar -xvf backup.tar`
  `-f`   Specify the archive filename   `tar -cf backup.tar folder/`
  `-z`   Use gzip compression           `tar -czf backup.tar.gz folder/`
  `-t`   List archive contents          `tar -tf backup.tar`

Create a compressed archive:

``` bash
tar -czvf backup.tar.gz folder/
```

Extract a gzip-compressed archive:

``` bash
tar -xzvf backup.tar.gz
```

List the contents without extracting:

``` bash
tar -tzf backup.tar.gz
```

## 8. Network and connectivity commands

### 8.1 `ssh` --- Connect to a remote Linux server

  ------------------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- ------------------------------
  `-p`                    Specify the SSH port    `ssh -p 2222 user@server`

  `-i`                    Specify a private key   `ssh -i key.pem user@server`
                          file                    

  `-v`                    Show verbose            `ssh -v user@server`
                          connection/debugging    
                          information             

  `-X`                    Enable X11 forwarding   `ssh -X user@server`
  ------------------------------------------------------------------------------

### 8.2 `scp` --- Copy files between systems over SSH

  ---------------------------------------------------------------------------------------------
  Flag                    Meaning                 Example
  ----------------------- ----------------------- ---------------------------------------------
  `-r`                    Copy directories        `scp -r folder/ user@server:/tmp/`
                          recursively             

  `-P`                    Specify the SSH port    `scp -P 2222 file.txt user@server:/tmp/`

  `-i`                    Specify a private key   `scp -i key.pem file.txt user@server:/tmp/`
                          file                    

  `-v`                    Show verbose output     `scp -v file.txt user@server:/tmp/`
  ---------------------------------------------------------------------------------------------

**Important:** `ssh` uses lowercase `-p` for the port, while `scp` uses
uppercase `-P`.

## 9. Package management commands (Ubuntu)

### 9.1 `apt` --- Install and manage software

  -----------------------------------------------------------------------------
  Command / option        Meaning                 Example
  ----------------------- ----------------------- -----------------------------
  `update`                Refresh package lists   `sudo apt update`

  `upgrade`               Upgrade installed       `sudo apt upgrade`
                          packages                

  `install`               Install software        `sudo apt install nginx`

  `remove`                Remove software         `sudo apt remove nginx`
                          packages                

  `purge`                 Remove packages and     `sudo apt purge nginx`
                          their managed system    
                          configuration files     

  `autoremove`            Remove automatically    `sudo apt autoremove`
                          installed packages no   
                          longer needed           

  `-y`                    Automatically answer    `sudo apt install -y nginx`
                          yes to prompts          
  -----------------------------------------------------------------------------

## 10. Important flags to learn first

Do not try to memorize every option immediately. Start with these, and
remember that their meanings depend on the command.

  -----------------------------------------------------------------------
  Flag                                Common use (depends on command)
  ----------------------------------- -----------------------------------
  `-l`                                Long listing or list-related option

  `-a`                                Show all or include all

  `-h`                                Human-readable output

  `-r` / `-R`                         Recursive on commands that support
                                      it

  `-v`                                Verbose output

  `-i`                                Interactive prompt or ignore case,
                                      depending on command

  `-f`                                Force, follow, or specify a file,
                                      depending on command

  `-n`                                Number, limit, or no-overwrite,
                                      depending on command

  `-p`                                Parent directories, port, or
                                      permissions, depending on command

  `-u`                                User or update-related option,
                                      depending on command

  `-c`                                Count, create, or changed-only,
                                      depending on command

  `-s`                                Summary or silent, depending on
                                      command

  `-t`                                Time, type, or archive-related
                                      option, depending on command

  `-S`                                Sort by size with `ls`

  `-w`                                Match whole words with `grep`

  `-d`                                Distribution, directory, or depth,
                                      depending on command

  `-x`                                Extract with `tar`; other meanings
                                      in other commands

  `-z`                                Gzip compression with `tar`
  -----------------------------------------------------------------------

> **Remember:** Never assume a flag has the same meaning across
> different commands. Check the documentation for the exact command.

## 11. How to find command options

Linux provides built-in help and manual pages.

### Method 1: Manual pages

``` bash
man ls
```

Useful keys inside a manual page:

-   Press `q` to exit.
-   Type `/keyword` to search.
-   Press `n` to find the next match.

### Method 2: Help option

``` bash
ls --help
```

Displays the options supported by `ls`. Many GNU commands support
`--help`; not every command does.

### Method 3: Search manual-page descriptions

``` bash
man -k directory
```

Searches manual-page names and descriptions for the keyword `directory`.

## 12. Suggested learning order

Practise these groups in order:

1.  **File management:** `ls`, `mkdir`, `cp`, `mv`, `rm`
2.  **File reading and searching:** `cat`, `head`, `tail`, `less`,
    `grep`, `find`
3.  **Permissions and ownership:** `chmod`, `chown`, `umask`
4.  **Processes and system monitoring:** `ps`, `kill`, `df`, `du`,
    `free`
5.  **DevOps and networking basics:** `tar`, `ssh`, `scp`, `apt`

### Practice tip

Learn a few commands each day. For every command:

1.  Understand what the command does.
2.  Practise one flag at a time.
3.  Combine flags where supported.
4.  Check the output and explain it in your own words.
5.  Use `man command` or `command --help` when you are unsure.

------------------------------------------------------------------------

**Environment:** Ubuntu Linux (including Ubuntu on WSL)\
**Purpose:** Beginner Linux command and flag revision
