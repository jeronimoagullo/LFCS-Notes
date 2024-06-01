- [reference command list](#reference-command-list)
- [1. Essential commands](#1-essential-commands)
  - [man](#man)
  - [CTRL+R](#ctrlr)
  - [History](#history)
  - [fc](#fc)
  - [uname](#uname)
  - [timedatectl](#timedatectl)
  - [uptime](#uptime)
  - [ls](#ls)
  - [stat](#stat)
  - [touch](#touch)
  - [echo](#echo)
  - [cd](#cd)
  - [cp](#cp)
  - [mv](#mv)
  - [mkdir](#mkdir)
  - [rm](#rm)
  - [rmdir](#rmdir)
  - [xargs](#xargs)
  - [find](#find)
  - [sort](#sort)

# reference command list
This **reference command list** is a list of the most relevant Linux command defined in just one single line. For more information go bellow to the corresponding section in which typical usage and tips are described.

1. **man**: An interface to the system reference manuals.
2. **history**: Display the command history to re-use previous commands.
3. **fc**: Allow to modify commands from the history list before executing.
4. **uname**: Display system information.
5. **timedatectl**: Control the system time and date. It can be used to get date for NTP testing.
6. **uptime**: Display how long the system has been runing and how many session are logged.
7. **ls**: list the directory contents.
8. **stat**: Display the status of files and directories.
9. **touch**: Update the acces and modification times of a file. It can be used to create empty files.
10. **echo**: Display a text line. Really useful to print environment variable values.
11. **cd**: Change the current working directory. 
12. **cp**: Copy files and directories. It can be used to create symbolic links.
13. **mv**: Move and rename files.
14. **mkdir**: Create directories.
15. **rm**: Delete files and directories.
16. **rmdir**: Delete empty directories.
17. **xargs**: Read the standard input using it as the argument of a new command.
18. **find**: Search for files in a directory hierarchy.
19. **sort**: Sort lines of a file.

# 1. Essential commands

## man
The `man` command is used to display the `man pages`, which are the most important and useful documentation source of Linux systems. Thus, it is very important to know how to use them.

**Search in a man page**: Man pages are display with pagination equal to the `less` command. Thus, we can use the same search shortcuts as `less`. For example, it is possible to search for a word using `/<the word>` and moving using `n` and `enter`. Go to `less` command for more information.

In case that you don't know the command that you need, you can look for the command using the `-k` and `-K` options, which searches for keywords and display any matches:
- `-k` option searches for keywords in the short manual page descriptions
- `-K` option is a brute-force serach. It searches in the sources of the manual pages (even the comments). In this case you should specify in which section you want to search (just write the section number as command argument).

## CTRL+R
`CTRL+R` is used to search through history. Hitting CTRL-R multiple times allows you to scan backwards through previous commands.

## History
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

## fc
Along with the `history` command, the `fc` command can be used to modify the desired command from history before executing it. You just need to execute the `fc` command with the history command number, edit it in your editor and save. Then, the edited command will be executed.

## uname
Prints different information about the system such as the kernel version, operating system or processor architecture.

All these information can be displayed with the `-a` option:
```
uname -a
```

## timedatectl
This command can be used to get time (as `date`) and to check whether the NTP service is active:
```
timedatectl
```

## uptime
This utility shows how long the Linux and Unix-based system has been running and how many session are logged.

```bash
uptime
```

The result is the current time, the hours or days that the system have been running, the users and their corresponding average load:

```
17:39:20 up 21:10,  3 users,  load average: 0.61, 0.44, 0.48
```

An alternative way of getting this information is the head of `top` command:

```bash
top | head
```

## ls
`ls` command is used to list files. Without arguments it lists all the files (but the dot files) of the current directoy. It is possible to pass a folder as argument to list all files within this directory.

Display long list of all files:
```sh
ls -l
```

Display the hidden (dot) files:

```sh
ls -a
```

Display the inode of the files:
```sh
ls --inode
```

Display a recursive list of all subfolders:
```sh
ls -R
```

## stat
The `stat` command displays file or file system status. This command can be used for different aims:

Get file status such as the size, its inode, the last access and modify time as well as creation time.
```sh
stat foo.txt
```

output example:
```
  File: hola.txt
  Size: 28              Blocks: 8          IO Block: 4096   regular file
Device: 820h/2080d      Inode: 2570        Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/jeronimo)   Gid: ( 1000/jeronimo)
Access: 2024-03-16 09:16:20.501427614 +0100
Modify: 2024-03-16 09:16:20.501427614 +0100
Change: 2024-03-16 09:16:20.501427614 +0100
Birth: 2024-03-15 16:55:06.703789219 +0100
```

we can get the information of filesystem too. It is useful to figure out in which filesystem a file is, the used, free and available blocks as well as the inodes availables.
```sh
stat -f foo.txt
```

output example:
```
  File: "hola.txt"
    ID: c6e2f5d04037e7f1 Namelen: 255     Type: ext2/ext3
Block size: 4096       Fundamental block size: 4096
Blocks: Total: 263940717  Free: 263501298  Available: 250075430
Inodes: Total: 67108864   Free: 67074780
```

## touch
The `touch` commands was originally designed to modify the file's modification time. A reason to do that is to re-run a make rule that depends on the file. Besides, it can be used to **create a new empty file**.

Create a new empty file if it doesn't exist:
```sh
touch foo.txt
```

Modify the file's modification time to the current time:
```
touch foo.txt
```

The `-t` option displays the values without the key, everything in a single line.


## echo
The `echo` command displays a line of text. It can seem a very simple and maybe "unuseful" command, but it is quite interesting.

First of all, it can be used to display environment varible values:
```sh
echo $PATH
```

Besides, it can be used to concatenate values, for example:
```sh
echo the user $USER is using $SHELL shell in host $NAME
```

whose output is:
```
the user jeronimo is using /bin/bash shell in host 7MKDBK
```

It can be used to scape the newline character to print the content of a file in a single line:
```sh
cat foo.txt | xargs echo -n
```

## cd
It is used to navigate trhough filesystems. It is used to change the directory.
```sh
cd /my/directory
```

It can be used to go to user directory `/home/$USER` using the command without arguments
```sh
cd
```

Go to the parent directory
```sh
cd ..
```

## cp
The `cp` command is used to copy files and directories. If the destination file doesn't exist, it will be created, otherwise, it will be overwritten.
```sh
cp <source> <destination>
```

There are two interesting options. The `-f` option force the copy, while the `-i` option asked to confirm the operation.

It is also possible to use `cp` to create symbolic links instead of copying. It is done by the `-s` option. It must be used with the `-R` option to make recursively if the destination is not in the same directory. The following command would create a symbolic link named `foo_link.txt` in the `/dest/folder` to the file `foo.txt`.

```sh
cp -sR foo.txt /dest/folder/foo_link.txt
```

It can be to perform backups. In this case a new file will be created ending with `~NUM~` where `NUM` is the backup number:

```sh
cp --force --backup=numbered foo.txt foo.txt
```

## mv
The `mv` command is used to move or rename files. The use is similar to `cp` but in this case the source file is deleted:
```sh
mv <source> <destination>
```

It has the `-f` and `-i` options as `cp` to force and promt before overwrite, respectively.

If you move the file into the same folder, you are renaming it.


## mkdir
The `mkdir` command is used to create a new folder. By default it create only a subfolder:
```sh
mkdir <folder>
```

You can use the `-p` option to create all the required parents folders in case that they don't exsist. For example, the following command would create the folders `testing` and `mkdir` if they don't exist.

```sh
mkdir -p testing/mkdir/command
```

## rm
The `rm` command is used to remove files or directories. It is a dangerous command, so I recommend to use it always with the `-i` option to promt before any removal.

To remove a folder we need to use the `-d` option, which will work only if the folder is empty. Otherwise, if there are files or subfolders we can use the `-r` option to remove recursively. For example, to remove the previous folders `testing/mkdir/command` creted with `mkdir`:
```
rm -dr testing
``` 

## rmdir
The `rmdir` is used to remove just empty directories. It exists along `rm` since it is a safer command since it will fail to remove directories with files or folders even with dot files. The use is similar to `rm -d`.

## xargs
`xargs` reads from the standard input using the input as the argument of a new command. By default, it executed the `/bin/echo` command.

A common usage is alone with `find` command to execute a certain command over all matched files. For example, we can remove all `.txt` files within a folder with the following pipe:

```sh
find -name \*.txt | xargs rm -f
```

It can be used to convert multi-line output from any command into single line. For example, the following pipe generate a single line list of all users:

```sh
cut -d: -f1 < /etc/passwd | sort | xargs
```

## find
`find` command searches for files in a diretory hierarchy. its syntax is `find PATH PARAMETERS`.

A typical usage is along with `xargs` to execute a command over all resulting files of `find` command. For example, we can remove all files starting with "log" within the `/tmp` folder: 

```sh
$ find /tmp -name log -type f | xargs rm -f
```

We can use in combination with `grep` to search a word over several files. For example, searching all the files in a folder that contain the word bill:

```sh
$ find PATH -name *.txt | xargs grep -l "Bill"
```

**NOTE:** When using with `xargs`, consider using the `-print0` option to print the full file name on the standard output, followed by a null character (instead of the newline character that -print uses). This allows  file  names  that contain newlines or other types of white space to be correctly interpreted by programs that process the find output. This option corresponds to the -0 option of xargs.

It is possible to search files by age. In this case we can use the option `-mtime` to search for files that was modify in an exact amount of time (n), less than a time (-n) or more than a time (+n). The following command searches for files modified less than 3 days.

```sh
$ find $HOME -mtime -3
```

**NOTE 2:** When working with wildcards and find command, it is quite important to remember that wildcards are expanded by the shell instead of the command. Thus, if we use the asterisk wildcard, it may result in an undesired result.

If we have a folder with two text files (`foo1.txt` and `foo2.txt`) and we want to find them, the use of `find -name *.txt` will result in just finding the first of them (`foo1.txt`). The reason is that shell expand the wildcard, thus, the executing command is:

```sh
$ find -name foo1.txt foo2.txt
```

If we want to be sure that the command work as expected, we need to avoid that the shell expands the wildcard, it can be done in different ways such as using quotes or scapping the wildcard, for example:

```sh
find . -name "*.txt"
find . -name '*.txt'
find . -name \*.txt
```

## sort
The `sort` command, as its name depicts, sorts theline of text files. It can be used to sort the output of a command like `ls`. By way of illustration, the following command display the file names of the `/usr/bin` file in alphabetical order regardless of case:

```
ls /usr/bin | sort -f
```


