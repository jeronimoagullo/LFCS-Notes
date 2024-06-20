The **Essential commands** section covers the following content according to [LFCS official website](https://training.linuxfoundation.org/certification/linux-foundation-certified-sysadmin-lfcs/):
- Basic Git Operations
- Create, configure, and troubleshoot services
- Monitor and troubleshoot system performance and services
- Determine application and service specific constraints
- Troubleshoot diskspace issues
- Work with SSL certificates

**INDEX**
- [1. basic concepts and commands](#1-basic-concepts-and-commands)
  - [Shell](#shell)
    - [aliases](#aliases)
    - [shell configuration](#shell-configuration)
  - [man pages](#man-pages)
  - [commands and expanding options](#commands-and-expanding-options)
  - [Re-using past commands](#re-using-past-commands)
  - [wildcards](#wildcards)
- [2. Text editor: Vim](#2-text-editor-vim)
  - [Vim commands](#vim-commands)
    - [Inserting text](#inserting-text)
    - [Moving around the text](#moving-around-the-text)
    - [Deleting, copying, changing and pasting text](#deleting-copying-changing-and-pasting-text)
    - [Existing vim](#existing-vim)
    - [other useful commands](#other-useful-commands)
- [2. Using the shell](#2-using-the-shell)
  - [emacs text editor](#emacs-text-editor)
  - [Navigating command lines](#navigating-command-lines)
  - [Editing command lines](#editing-command-lines)
  - [Cutting and pasting text from within command lines](#cutting-and-pasting-text-from-within-command-lines)
- [2. Basic Git Operations](#2-basic-git-operations)
  - [Basic Workflow](#basic-workflow)
  - [Understanding the Stage](#understanding-the-stage)
  - [Restoring Modified Files](#restoring-modified-files)
  - [Branching](#branching)
  - [Advanced Commands](#advanced-commands)
- [3. programs, processes and services](#3-programs-processes-and-services)
  - [Processes](#processes)
    - [Init Process](#init-process)
  - [services managers: systemd](#services-managers-systemd)
    - [Systemd units](#systemd-units)
    - [Systemd unit files](#systemd-unit-files)
    - [systemd unit file structure](#systemd-unit-file-structure)
      - [`[Unit]` section](#unit-section)
      - [`[Install]` section](#install-section)
      - [`[Service]` section (only for service units)](#service-section-only-for-service-units)
      - [`[Socket]` section (only for socket units)](#socket-section-only-for-socket-units)
  - [Monitor and troubleshoot system performance and services](#monitor-and-troubleshoot-system-performance-and-services)
- [5. Determine application and service specific constraints](#5-determine-application-and-service-specific-constraints)
- [6. Troubleshoot diskspace issues](#6-troubleshoot-diskspace-issues)
- [7. Work with SSL certificates](#7-work-with-ssl-certificates)


# 1. basic concepts and commands

## Shell
The `shell` is a program that takes commands from the keyboard and gives them to the operating system to perform.

The shell stores information named **variables**. They can be defined by the user or predefined by the sytem. You can easily use them adding the dollar (`$`) symbol, for example:

```
echo $USER
```

The following is a list of the most relevant shell environment variables:
- **BASH**: This contains the full pathname of the current shell, it is usually `/bin/bash`.
- **HISTFILE**: This contains the full pathname of the history file.
- **HISTFILESIZE**: This is the number of history entreies that can be stored. The default value is 1000.
- **HOME**: full pathname of the user home directory.
- **OLDPWD**: This is the directory that was the working directory before you changed to the current working directory.
- **PATH**: This is the colon-separated list of directories used to find commands that you type.
- **RANDOM**: generates a random number between 0 and 9999.
- **SECONDS**: This is the number of seconds since the time the shell was started.


### aliases
Shell stores `aliases` too. An `alias` allows you to assign a short, easy-to-remember name to a command or a sequence of commands. For example, the following alias allows to run `pwd` to display current working directory and `ls -l` to list its content in detail just writing `p`:

```
alias p='pwd ; ls -l' 
```

The list of current aliases can be checked out witht he `alias` command. Besides, you can remove an alias with the `unalias` command.


### shell configuration
It is possible to configure the shell editing different configuration files:

- `/etc/profile`: Configuration options applied for all users at first log in.
- `/etc/bashrc`: Configuration opetions applied for all users at each log in. Values in this file can be overwritten by information in each user's `~bashrc` file.
- `~/.bash_profile`: It executs only once when the user logs in. By default, it sets a few environment variables and executes the user's `.bashrc` file. This is a good place to add environment variables because, once set, they are inherited by future shells.
- `~/.bashrc`: This contains the information that is specific to your bash shells. It is read when you log in and also each time you open a new bash shell. This is the best location to add aliases so that your shell picks them up.
- `~/.bash_logout`: This executes each time you log out (exit the last bash shell).

After editing one of these user's configuration file, you can apply the changes opening a new terminal or executing the following command:

```
source $HOME/.bashrc
```

## man pages
`man` pages are the most important and useful documentation source of Linux systems. Thus, it is very important to know how to use them.

man pages are structured into sections (from 1 to 8):
1.  **User commands (Programs)**: Commands that can be executed by the user from within a shell.
2.  **System calls**: Functions which wrap operations performed by the kernel.
3.  **Library calls**: All library functions excluding the system call wrappers(Most of the libc functions).
4.  **Special files (devices)**: Files found in /dev which allow to access to devices through the kernel.
5.  **File formats and configuration files**: Describes various human-readable file formats and configuration files.
6.  **Games**: Games and funny little programs available on the system.
7.  **Overview, conventions, and miscellaneous**: Overviews or descriptions of various topics, conventions, and protocols, character set standards, the standard filesystem layout, and miscellaneous other things.
8.  **System management commands**: Commands like mount(8), many of which only root can execute.

As system administrator, you will be insterested specially in the sections **1, 5 and 8**.

We can look for any command, function or feature trhough all man pages using the `-k` (equivalent to `--apropos`) option and passing a string. This option searches the short manual page descriptions for keywords and display any match:
```
man -k "string"
```

For a global (more intensive) search, we can use the option `-K` (capital letter, equivalent to `--global-apropos`). For example, we can look for which command can be used to set the **default route**, thus, we can write:

```
man -K "default route"
```

It will return and iterate over all matches. we can skip the first by `q` and then skip the rest by `CTRL+D` until we find the desired command. In this case we just need to press `ENTER` to show the man page. For example:

Once we found our man page and we are on it, we can use the "search" command to search for string matches. Press / followed by the search term to search within the man page. For example, if we want to find how to add a new default route, we can search for "add" in the "ip-route" man page:

```
/add
```

Finally, we can move forward and backward through the matches using `n` and `shift+n`.

**NOTE:** Many man pages contain a "example" section. You can navigate directly to it using the search command looking for **example**.

## commands and expanding options
In Linux a **command** is an executable program that can consist of a single world (like `ls`), a simple world with different options (like `ls -la`) or a combination of simple commands (like `echo $PATH | tr : "\n"`). The combination of different commands with the character `|` is named a **pipe**.

Most of the command reads and write text from and into the terminal, they read and write from:
- `stdin`: "Standard input". It is the data stream from keyboard
- `stdout`: "Standard output". It is the data stream that a command writes into the display.

When using a **pipe** (`|`), the stdout of the first command is redirected to the stdin of the second command. That is, for the second command its input is the output of the second command but it intreprets the input as if we had written it by the keyboard.

It is likely to execute commands sequentially with the `;` character. For example, we can display the time before and after a heavy or large command to know how much time it took to be executed:

```
date ; <heavy command> ; date
```

Another useful command to add at the end of a long command line is `mail`. It can be used to notify when a large command finished the execution.

```
... ; mail -s "Command was executed" example@gmail.com
```

Similar to the `xargs` command, you can expand your command, that is, you can have the standard output of a command become an argument for another command. For example, the following command will open in `vi` (one by one) all the files that start with *hi*:

``` 
vi $(find hi*)
```

## Re-using past commands
`CTRL+R` is used to search through history. Hitting CTRL-R multiple times allows you to scan backwards through previous commands.

The `history` command is used to view used previous command in chronological order.

The shell stores history in `~/.bash_history`. Besides, there are different environment variable to configure history:
- `HISTFILE`: Location of the history file
- `HISTFILESIZE`: Maximum number of lines in the history file 
- `HISTSIZE`: Maximum number of lines of history list in the current session

It is possible to use the commands of the history output just using the following keys:
- To start a history substitution: `!`
- To refer to the last argument in a line: `!$`
- To refer to the n-th command line: `!n`
- To refer to the most recent command starting with `!string`

## wildcards
Wildcards are useful in many ways for a GNU/Linux system and for various other uses. Commands can use wildcards to perform actions on more than one file at a time, or to find part of a phrase in a text file.

Most relevant wildcards:
- `?` **(question mark)**: this can represent any single character. If you specified something at the command line like "hd?" GNU/Linux would look for hda, hdb, hdc and every other letter/number between a-z, 0-9.

- `*` **(asterisk)**: this can represent any number of characters (including zero, in other words, zero or more characters). If you specified a "cd*" it would use "cda", "cdrom", "cdrecord" and anything that starts with “cd” also including “cd” itself.

- `[ ]` **(square brackets)**: specifies a range. If you did m[a,o,u]m it can become: mam, mum or mom. Besides, if you did: m[a-d]m it can become anything that starts and ends with m and has any character a to d inbetween.

- `{ }` **(curly brackets)**: terms are separated by commas and each term must be the name of something or a wildcard. This wildcard will copy anything that matches either wildcard(s), or exact name(s) (an “or” relationship, one or the other). For example, this would be valid:

```sh
cp {*.doc,*.pdf} ~
```

This will copy anything ending with .doc or .pdf to the users home directory. Note that spaces are not allowed after the commas (or anywhere else).

- `[!]`: This construct is similar to the [ ] construct, except rather than matching any characters inside the brackets, it'll match any character, as long as it is not listed between the [ and ]. This is a logical NOT. For example rm myfile[!9] will remove all myfiles* (ie. myfiles1, myfiles2 etc) but won't remove a file with the number 9 anywhere within it's name.

- `\` **(backslash)**: It is used as an "escape" character, i.e. to protect a subsequent special character. Thus, "\\” searches for a backslash.

- `^` **(caret)**: means "the beginning of the line". So "^a" means find a line starting with an "a".

- `$` **(dollar sign)**: It means "the end of the line". So "a$" means find a line ending with an "a". For example, this command searches the file myfile for lines starting with an "s" and ending with an "n", and prints them to the standard output (screen):

```sh
cat myfile | grep '^s.*n$'
```

# 2. Text editor: Vim
There are a lot of text editor avilable for Linux. The more commands are `vi`(or `vim`) and `emacs`. Nowadays, `nano` became quite famous due to its simplicity.

The most relevant, more use and the one that you may find always in any Linux distribution is `vi` or its enhanced version `vim`. It is a bit difficult to learn at the beginning but a really useful and appropiate tool when you got it.

You just need to text `vi` (or `vim`) followed by the file name to edit it (or create the file in case that it doesn't exit yet):

```bash
$ vi <my_file>
```

## Vim commands
In this section I will summarize the most relevant and useful vi commands. 

A couple of notes. Firstly, you can scape of any vi mode pressing **ESC**. Besides, keep in mind that the same command with capital letters has a different meaning, so check out your shift key before entering a command.

### Inserting text
- **a**: *add*. Input text starting to the right of the cursor.
- **A**: *add at the end*. Input text starting to the end of the current line.
- **i** *insert*. Input text starting to the left of the cursor.
- **I** *insert at beggining*. Input text starting to the left of the cursor.
- **o**: *open bellow*. Add a line below the current line and put you in *insert* mode.
- **O**: *open above*. Add a line above the current line and put you in *insert* mode.

### Moving around the text
- **arrow keys**: the sames with **h**(left), **l** (right), **j** (down), **k** (up).
- **w**: move the cursor to the beginning of the next word.
- **b**: move the cursor to the beginning of the previous word.
- **0(zero)**: move the cursor to the beginning of the current line.
- **$**: move the cursor to the end of the current line.
- **H, M, L**: move th ecursor to the upper-left corner, middle line or the lower-left corner on the screen.
- **Ctrl+f, Ctrl+b**: Pages ahead and back one page at a time, respectively.
- **Ctrl+d, Ctrl+u**: Pages ahead and back one-half page at a time, respectively.

### Deleting, copying, changing and pasting text
- **x**: deletes the character under the cursor
- **d<?>**: deletes some text.
- **c<?>**: changes (cut) some text.
- **y<?>**: yanks (copy) some text.
- **p**: copy text to the left of cursor if words or above line in case of line or paragraphs.

The **<?>** can be replace by any of the following options, applyting the corresponding text (similar to the *moving* commands):
  - **l** the letter after cursor.
  - **w** the word after cursor.
  - **b** the word before cursor.
  - **)** next sentece and **}** next paragraph.
  - **dd, cc, yy** action to the current line.
  - **0**: from the current character to the beginning of the line.
  - **$**: from the current character to the end of the line.

Besides, the commands can be modified adding a number before. By way of illustration, the command **5cw** changes the next five words.

Some samples: **dd** deletes the current line, **y0** yanks from the cursor to the beginning of the line, **12j** moves down 12 lines.

### Existing vim
- **:w**: Save file, but you can continue editing.
- **:wq**: Save file and exit.
- **:q!**: Quits without saving changes.

### other useful commands
- **u**: undo previous change, you can use it several times.
- **Ctrl+R**: Redo after undo, that is, this command undoes your undo.
- **Ctrl+g**: Display current file information.
- **/word or ?word**: Search fordward and bardward a word respectively.
- **period(.)**: You can run the latest command with a period, entering the character `.`.

# 2. Using the shell
The shell is the most powerfull and useful resource for a Linux administrator. This section covers different hints to enhance and improve the work with the bash shell.

## emacs text editor
By default, the bash shell uses command-line editing that is based on the **emacs text editing** (check out `man emacs` for more information).

If you want to use your favorite text editor, for example `vi`, you can easily use it adding the following line into your `.bashrc` file:

```
set -o vi
```

## Navigating command lines
The following short-cuts are useful to navigate through the current command line:

|   key  	|      full name      	|                  meaning                 	|
|:------:	|:-------------------:	|:----------------------------------------:	|
| Ctrl+F 	|  Character forward  	|         Go forward one character         	|
| Ctrl+B 	| Character backwardd 	|         Go backward one character        	|
|  Alt+F 	|     word forward    	|     Go forward to the end of one word    	|
|  Alt+B 	|    word backward    	| Go backward to the beginning of one word 	|
| Ctrl+A 	|  Beginning of line  	|        Go to the beginning of line       	|
| Ctrl+E 	|     End of line     	|           Go to the end of line          	|
| Ctrl+L 	|     clear screen    	|         Seame as "clear" command         	|

## Editing command lines
The following short-cuts are useful to edit the current command:

|    key    	|         full name         	|                                     meaning                                    	|
|:---------:	|:-------------------------:	|:------------------------------------------------------------------------------:	|
|   Ctrl+D  	|       Delete current      	|                          Delete the current character                          	|
| Backspace 	|      Delete previous      	|                          Delete the previous character                         	|
|   Ctrl+T  	|    Transpose character    	|               Switch positions of current and previous characters              	|
|   Alt+T   	|      Transpose words      	|                  Switch positions of current and previous word                 	|
|   Alt+U   	|       uppercase word      	|                      Change the current word to uppercase                      	|
|   Alt+L   	|       lowercase word      	|                      Change the current word to lowercase                      	|
|   Alt+C   	|      Capitalize word      	|                  Change the current word to an initial capital                 	|

## Cutting and pasting text from within command lines
Cutting and pasting text from commands can save a lot of time:

|   key  	|       full name       	|                     meaning                     	|
|:------:	|:---------------------:	|:-----------------------------------------------:	|
| Ctrl+K 	|    Cut end of line    	|         Cut text to the end of the line         	|
| Ctrl+U 	| Cut beginning of line 	|      Cut text to the beginning of the line      	|
| Ctrl+W 	|   Cut previous word   	|      Cut the word located behind the cursor     	|
|  Alt+D 	|     Cut next word     	|        Cut the word following the cursor        	|
| Ctrl+Y 	|   Paste recent text   	|           Paste most recently cut text          	|
| Alt`+Y 	|   Paste earlier text  	| Rotate back to previously cut text and paste it 	|
| Ctrl+C 	|   Delete whole line   	|              Delete the entire line             	|



# 2. Basic Git Operations
Git is a distributed version control system widely used for tracking changes in source code during software development. It allows multiple developers to collaborate on projects and maintain a complete history of changes.

## Basic Workflow
1. **git add**: Use this command to stage changes in your working directory for the next commit. You can use the wildcard `.` to add all files to stage or the option `-u` to add all modified tracked files.
   ```bash
   git add <file_name>
   ```

2. **git commit**: Commit staged changes to the local repository with a descriptive message.
   ```bash
   git commit -m "Commit message"
   ```

3. **git push**: Push committed changes from the local repository to a remote repository.
   ```bash
   git push origin <branch_name>
   ```

## Understanding the Stage
The stage, also known as the index, is an intermediate area where changes are prepared before committing them to the repository. Files in the stage are ready to be included in the next commit.

You can check your stage before commiting with the `git status` command.

## Restoring Modified Files
- To restore modified files in the working directory to their last committed state:
  ```bash
  git reset --hard
  ```

- To unstage files that have been added to the stage:
  ```bash
  git reset <file_name>
  ```

## Branching
A branch in Git is a separate line of development that represents a series of commits. It allows developers to work on features or fixes independently without affecting the main codebase.

- **Creating a Branch**:
  ```bash
  git checkout -b <branch_name>
  ```

- **Changing Branches**:
  ```bash
  git checkout <branch_name>
  ```

## Advanced Commands
1. **git commit --amend**: Add staged changes to the previous commit and/or modify the commit message. It will open the text editor (normally `vi`)
   ```bash
   git commit --amend
   ```

2. **git log**: View a detailed history of commits in the repository.
   ```bash
   git log
   ```
   There is a rule of thumb to display the log with graph and in a simple manner, it is the "a dog" technique, which is the first letter of each following options:
   ```bash
   git log --all --decorate --oneline --graph
   ```

3. **git rebase**: Reapply commits on top of another base commit, often used to integrate changes from one branch into another.
   ```bash
   git rebase <base_branch>
   ```

# 3. programs, processes and services
First of all we need to clarify the difference between `program`, `process` and `service`. 

- A **program** is a set of instructions and data to perform a task. Programs may consist of machine level instructions run directly by a CPU or a list of commands to be interpreted by another program.
-  A **process** is an instance of a program in execution. Besides, it ca be in different states like running or sleeping. Then, the first main difference is that **Linux creates a new process for every program** that is executed or run. Thus, several processes may be executing the same program (even at the same time). Finally, the primary purpose of the operating system is to manage the execution of processes.
-  A **services** or **daemons** are special processes that run in the background and starts at boot (although they can activate or deactivate during system usage). The most important service managers are `systemd` and `system V`. We will cover then later

## Processes
To view the list of running processes the `ps` command is used:
```bash
ps aux
```

killing proceses

### Init Process
The `init` process is the first process to be launch during booting process and has the **PID 1**. It is described in `/sbin/init`.

`Init` process is in charged of orphan processes and killing zombie ones.


## services managers: systemd
The task of controlling the services execution is carried out by a "service manager". The most important service managers are `system V` and `systemd`. While `system V` was the original service manager in UNIX systems, `systemd` is getting more and more popularity in LInux systems and can be found in almost any modern distribution. Some reasons are the flexibility of systemd over system V as well as the possiblity of controlling and loggin the service, as we will see later.

On the one hand, `system V` uses the traditional init scripts to start and stops services. These scripts can be found in `/etc/init.d` folder. On the other hand, `systemd` uses "unit files", which are configuration files that describe a service and how to start and stop them. The unit files can be found in the `/lib/systemd/system` folder and is managed by the systemd process.

Since most moderm Linux distribution relies on `systemd` as service manager, we will focus only on it. Systemd consists of different small programs or libraries such as: `systemctl`, `journalct`, `Init`, `networkd` or `logind` among others.

### Systemd units
A **systemd unit** is any resource that the system can operate on and manage. That is, a systemd unit can be a service, a socket, a device, etc. The following list covers the main systemd units:

- **socket units**: They encapsulate local IPC (Inter-process communication) and network sockets. Handling the sockets as units allows to delaying the start of a service until the corresponding socket is not ready or creating the sockets during booting, speeding up the later execution of services.
- **services units**: They are in charged of the services, that is, the daemons and their subprocesses.
- **Scope units**: similar to service units, but manage foreign processes instead of starting them as well.
- **target units**: They are the equivalent to "run levels" in sysV systems. They are used to define states for synchronization during booting. For example, the `graphical.target` is used to wait for the GUI to be ready.
- **Device units**: Exposes kernel devices in systemd by `udev` or the `sysfs` filesystem. They are created over the fly when a new device is connected.
- **Mount units**: control mount points in the file system. These are named after the mount path, with slashes changed to dashes.
- **Path units**: used to activate other services when file system objects change or are modified.
- **Slice units**: used to group units which manage system processes (such as service and scope units) in a hierarchical tree for resource management purposes.

Other unit types are automount units, timer units (for synchronization) and swap units (similar to mount units). Refer to `man systemd` for more information.


TODO: systemd units syntax

### Systemd unit files
The systemd unit are defined using configuration files known as **unit files**. 

The unit files can be found in different locations:
- **/lib/systemd/system**: standard systemd unit files from distro.
- **/usr/lib/systed/system**:locally installed packages installed by user (e.g. via `apt-get`).
- **/run/systemd/system**: transient unit files.
- **/etc/systemd/system**: add here your custom unit files. The unit files located in this folder take precedence over any of the other locations. We can modify an already existing unit file creating a copy in this directoy, it is a safe manner of testing.

A complete list of all the systemd unit files in our system can be printed by following command. It will displayed the state of the unit too (enabled, disabled or static).
```bash
$ systemctl list-unit-files
```

### systemd unit file structure
Unit files consists of different **sections** denoted by a pair of square brackets and a well-defined and case-sensitive section name (`[`section name`]`). Each section extends until the beginning of the subsequent section or until the end of the file. Each section contain is structured by key-values format.

All systemd units have the generic `[Unit]` and `[Install]` sections. Besides, the different unit types may have specific sections such as the `[Service]` section for the service units. The order of sections don't matter when systemd parses the file.

#### `[Unit]` section
The `[Unit]` section defines metadata and the relationship of the unit with other units. Most of the content of this section is exposed by `systemctl status` command. The main directives are:
- **Description=**: Name and short description of the unit.
- **Documentation=**: A list of documentation URI from man pages or external URL.
- **Requires=**: A list of all units this unit depends. If these units failed, this unit will fail too.
- **Wants=**: Similar to `Requires` directive, the unit will attempt to start all the units listed here, but will not fail if they are not found or 
- **Before=**: List of units that will wait until this unit is marked as started to start. It doesn't imply a depency relationship.
- **After=**: List of units that will start before starting this unit. It doesn't imply a depency relationship.
- **Conflicts=**: This can be used to list units that cannot be run at the same time as the current unit. Starting a unit with this relationship will cause the other units to be stopped.
- **Condition...= and Assert...=**: They are a different directives that start by `Condition` or `Assert` and can be used to check different aspect of the running environment before starting the unit.
- 
https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files

#### `[Install]` section
This section is optional and is used to define the behaviour of a unit if it is enabled or disabled as well as if starting at boot.

- **WantedBy=**: The WantedBy= directive is the most common way to specify how a unit should be enabled. This directive allows you to specify a dependency relationship in a similar way to the Wants= directive does in the [Unit] section. The difference is that this directive is included in the ancillary unit allowing the primary unit listed to remain relatively clean. When a unit with this directive is enabled, a directory will be created within /etc/systemd/system named after the specified unit with .wants appended to the end. Within this, a symbolic link to the current unit will be created, creating the dependency. For instance, if the current unit has WantedBy=multi-user.target, a directory called multi-user.target.wants will be created within /etc/systemd/system (if not already available) and a symbolic link to the current unit will be placed within. Disabling this unit removes the link and removes the dependency relationship.
- **RequiredBy=**: This directive is very similar to the WantedBy= directive, but instead specifies a required dependency that will cause the activation to fail if not met. When enabled, a unit with this directive will create a directory ending with .requires.
- **Alias=**: This directive allows the unit to be enabled under another name as well.
- **Also=**: This directive allows units to be enabled or disabled as a set. They will be managed as a group for installation tasks.

#### `[Service]` section (only for service units)
This section is only applicable to service units. 

The most important directive is `type`, which ctegorizes services. It can be one of the following values:
- **simple**: The main process of the service is specified in the start line. Any communication should be handled outside of the unit through a second unit.
- **forking**: service forks a child process. It tells sytemd that the process is still running even though the parent exited.
- **oneshot**: a short-life service. Systemd waits until the unit finished before starting another unit.
- **dbus**: This indicates that unit will take a name on the D-Bus bus. When this happens, systemd will continue to process the next unit.
- **notify**: Service will issue a notification when it has finished starting up. Systemd will wait this notification before starting another unit.
- **idle**: This indicates that the service will not be run until all jobs are dispatched.

Other important directives to handle services are:
- **ExecStart=**: 

#### `[Socket]` section (only for socket units)

It is possible to filter by type, for example, displaying only sytemd units of type "service".
```bash
$ systemctl list-unit-files --type=service
```


To reload daemon, that is, read all unit files after a change:
```bash
$ systemctl daemon-reload
```

## Monitor and troubleshoot system performance and services

# 5. Determine application and service specific constraints

# 6. Troubleshoot diskspace issues

# 7. Work with SSL certificates