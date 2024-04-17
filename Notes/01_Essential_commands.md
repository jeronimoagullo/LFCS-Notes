The **Essential commands** section covers the following content according to [LFCS official website](https://training.linuxfoundation.org/certification/linux-foundation-certified-sysadmin-lfcs/):
- Basic Git Operations
- Create, configure, and troubleshoot services
- Monitor and troubleshoot system performance and services
- Determine application and service specific constraints
- Troubleshoot diskspace issues
- Work with SSL certificates

**INDEX**
- [1. basic concepts and commands](#1-basic-concepts-and-commands)
  - [man pages](#man-pages)
  - [commands and pipes](#commands-and-pipes)
  - [Re-using past commands](#re-using-past-commands)
  - [wildcards](#wildcards)
  - [User environment](#user-environment)
- [2. Basic Git Operations](#2-basic-git-operations)
  - [Basic Workflow](#basic-workflow)
  - [Understanding the Stage](#understanding-the-stage)
  - [Restoring Modified Files](#restoring-modified-files)
  - [Branching](#branching)
  - [Advanced Commands](#advanced-commands)
- [3. Create, configure, and troubleshoot services](#3-create-configure-and-troubleshoot-services)
- [4. Monitor and troubleshoot system performance and services](#4-monitor-and-troubleshoot-system-performance-and-services)
- [5. Determine application and service specific constraints](#5-determine-application-and-service-specific-constraints)
- [6. Troubleshoot diskspace issues](#6-troubleshoot-diskspace-issues)
- [7. Work with SSL certificates](#7-work-with-ssl-certificates)


# 1. basic concepts and commands

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

We can look for any command, function or feature trhough all man pages using the `-K` option and passing a string:
```
man -K "string"
```

For example, we can look for which command can be used to set the **default route**, thus, we can write:

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

## commands and pipes
In Linux a **command** is an executable program that can consist of a single world (like `ls`), a simple world with different options (like `ls -la`) or a combination of simple commands (like `echo $PATH | tr : \n`). The combination of different commands with the character `|` is named a **pipe**.

Most of the command reads and write text from and into the terminal, they read and write from:
- `stdin`: "Standard input". It is the data stream from keyboard
- `stdout`: "Standard output". It is the data stream that a command writes into the display.

When using a **pipe** (`|`), the stdout of the first command is redirected to the stdin of the second command. That is, for the second command its input is the output of the second command but it intreprets the input as if we had written it by the keyboard.

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

## User environment

A detailed information about the user environment strings can be found in the `environ` man page of section 7.

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

# 3. Create, configure, and troubleshoot services

# 4. Monitor and troubleshoot system performance and services

# 5. Determine application and service specific constraints

# 6. Troubleshoot diskspace issues

# 7. Work with SSL certificates