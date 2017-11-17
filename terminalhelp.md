# Contents

# Resources & references

# Overview

# Terminology
- A **shell** is a special **command-line tool** that is designed specifically to provide text-based **interactive control over other command-line tools.**
- A **shell** could be command line, terminal, or PowerShell.
- The standard OS X shell is **bash**.  Bash is a Unix shell and command language first released in 1989.
- **Home** folder/directory: In Finder, choose Go > Home (shortcut: _Shift+Cmd+H_).  The Home folder is identified by an icon that looks like a house.
- The **terminal** is an application that lets you interact with a **command-line environment.**  You could also interact with an environment using a remote connection method such as **secure shell (SSH)**.  A terminal window provides access to the input and output of a **shell process.**
- You run **command-line tools** (or just _commands_; one example would be `cd`) that OS X provides by _typing the name of the tool_.
    - Most tools can also take a number of **flags** aka **switches**.  `-l` would be one such flag.  Flags **change behavior.**
    - Flags can be used together after one hyphen.  For example, `-l` and `-a` are both flags to `ls`.  You can use `ls -la` to (1) display detailed info for each entry and (2) list all directory contents, including hidden files/folders.
    - Separately, some tools take **arguments**.  For example, in `ls /Users/brad/temp`, the second chunk is an argument.
- The shell (no matter the command, really) also has a notion of a **current working directory**. _When you specify a filename or path that does not start with a slash, that path is assumed to be relative to this directory._

# The home directory
Your home directory (`$HOME`) might not be the same as your _root_ directory.  To see your home directory, use

```bash
Bradleys-MacBook-Pro:Utilities brad$ cd $HOME
Bradleys-MacBook-Pro:~ brad$ pwd
/Users/brad
```

In the example above, `brad` is actually 2 levels below the _top-level_ (root) of your hard drive:

```bash
Bradleys-MacBook-Pro:~ brad$ cd ../..  # Move 2 levels up
Bradleys-MacBook-Pro:/ brad$ pwd
/
```

```
Macintosh HD
|-- Applications
    |-- Utilities
        |-- Terminal.app
|-- Library
|-- System
|-- Users
    |-- brad
```

# Special path characters
The shell supports a number of directory names that have a special meaning:

| Path string | Description |
| ----------- | ----------- |
| `.` | points to the current working directory | For example, if you type `./mytool` and press return, you are running the `mytool` command in the current directory.
| `..` | points to the parent directory (up 1 level) | The path `../Test` is a file or directory (named "Test") that is a **sibling of** the current directory. |
| `~` or `$HOME` | At the beginning of a path, the tilde character represents the home directory of the current user. | `$HOME` is the same thing (usually); technically, it is an environment variable that can be manipulated. |

An example of `cd`ing to a sibling directory:

```bash
Bradleys-MacBook-Pro:~ brad$ pwd
/Users/brad
Bradleys-MacBook-Pro:~ brad$ ls
Applications    Downloads   Movies      Public      temp
Desktop     Dropbox     Music       ds      testfile.txt
Documents   Library     Pictures    rodeo.log
Bradleys-MacBook-Pro:~ brad$ cd Documents
Bradleys-MacBook-Pro:Documents brad$ cd ../Pictures
Bradleys-MacBook-Pro:Pictures brad$ pwd
/Users/brad/Pictures
```

An example using a path with a tilde:

```bash
Bradleys-MacBook-Pro:~ brad$ pwd
/Users/brad
Bradleys-MacBook-Pro:~ brad$ cd Downloads
Bradleys-MacBook-Pro:Downloads brad$ cd ~/Documents  # ~ is alias for /Users/brad
Bradleys-MacBook-Pro:Documents brad$ pwd
/Users/brad/Documents
```




# Commands

| Command | Usage |
| ------- | ----- |
| open | open a file | uses default application
| pwd | print working directory
| hostname | my computer's network name
| mkdir | create a new directory (folder)
| cd | change directory | `cd ~` is the same as `cd $HOME`.  You need a space following `cd`.
| ls | list directory
| rmdir | remove directory | only removes an _empty_ directory by default
| pushd | push directory
| popd | pop directory
| cp | copy a file or directory
| mv | move a file or directory
| less | page through a file
| cat | print the whole file
| xargs | execute arguments
| find | find files
| grep | find things inside files
| man | read a manual page
| apropos | find what man page is appropriate
| env | look at your environment
| echo | print some arguments
| export | export/set a new environment variable
| exit | exit the shell
| sudo | DANGER! become super user root DANGER!
| dirs | display the list of currently remembered directories
| touch | make a new file


Now, for some more on specific commands

## `mkdir`
Say you first make a single directory and then want to make a directory one level down without `cd`ing.  You'll need to specify the "full relative path".  If you want to make subdirectories multiple levels down, you'll need `-p`:

```bash
Bradleys-MacBook-Pro:~ brad$ pwd
/Users/brad

Bradleys-MacBook-Pro:~ brad$ mkdir temp

Bradleys-MacBook-Pro:~ brad$ mkdir temp/stuff

Bradleys-MacBook-Pro:~ brad$ mkdir temp/stuff/things/files  # this will fail: +1 levels down

Bradleys-MacBook-Pro:~ brad$ mkdir -p temp/stuff/things/files  # need -p in this case
```

To make a directory with a space in its name, use quotes:

```bash
Bradleys-MacBook-Pro:~ brad$ mkdir 'new folder1'
Bradleys-MacBook-Pro:~ brad$ mkdir "new folder2"
```

Or, backslash escape the space literal:

```bash
Bradleys-MacBook-Pro:~ brad$ mkdir new\ folder1
```


If you `mkdir` a directory that already exists, you'll get an error:

```bash
Bradleys-MacBook-Pro:~ brad$ mkdir temp
Bradleys-MacBook-Pro:~ brad$ mkdir temp
mkdir: temp: File exists
```

You can't specify _files_ this way.  Using `mkdir file.txt` creates a folder called "file.txt" rather than a text file.  For this you'll need the [`touch`](#touch) command.

## `cd`

```bash
Bradleys-MacBook-Pro:~ brad$ mkdir -p dir1/dir2/dir3/dir4
Bradleys-MacBook-Pro:~ brad$ pwd
/Users/brad
Bradleys-MacBook-Pro:~ brad$ cd dir1/dir2/dir3
Bradleys-MacBook-Pro:dir3 brad$ pwd
/Users/brad/dir1/dir2/dir3
Bradleys-MacBook-Pro:dir3 brad$ cd ..
Bradleys-MacBook-Pro:dir2 brad$ pwd
/Users/brad/dir1/dir2
Bradleys-MacBook-Pro:dir2 brad$ cd ..; pwd  # one folder up
/Users/brad/dir1
Bradleys-MacBook-Pro:dir1 brad$ cd ../..; pwd  # two folders up
/Users
```

You can put quotes around any file/folder in any command:

```bash
cd "long folder name"
```

## `rmdir`
If you try to do rmdir on Mac OSX and it refuses to remove the directory even though you are positive it's empty, then there is actually a file in there called _.DS_Store_. In that case, type `rm -rf <dir>` instead (replace `<dir>` with the directory name).

## `touch`

```bash
Bradleys-MacBook-Pro:temp brad$ touch testfile.txt
```

## `pushd` and `popd`

TODO: not fulling understanding these 2

Read these as "push directory" and "pop directory."  These commands let you **temporarily go to a different directory and then come back, easily switching between the two.**

- The `pushd` command takes your current directory and "pushes" it into a list for later, then it changes to another directory. It's like saying, "Save where I am, then go here."
    - `pushd`, if you run it by itself with no arguments, will switch between your current directory and the last one you pushed. It's an easy way to switch between two directories.
- The `popd` command takes the last directory you pushed and "pops" it off, taking you back there.
- You can think of the output of both as a stack.

```bash
Bradleys-MacBook-Pro:~ brad$ pwd
/Users/brad
Bradleys-MacBook-Pro:~ brad$ mkdir -p temp/dir1/dir2/dir3
Bradleys-MacBook-Pro:dir2 brad$ pwd
/Users/brad/temp/dir1/dir2
Bradleys-MacBook-Pro:dir2 brad$ pushd ~  # Store current directory, and go $HOME
~ ~/temp/dir1/dir2
Bradleys-MacBook-Pro:~ brad$ pwd
/Users/brad
Bradleys-MacBook-Pro:~ brad$ pushd  # pushed with no args - switch to last dir you pushd
~/temp/dir1/dir2 ~
Bradleys-MacBook-Pro:dir2 brad$ pushd ../..
~/temp ~/temp/dir1/dir2 ~/temp/dir1/dir2 ~  # TODO: not following here
Bradleys-MacBook-Pro:temp brad$ popd
~/temp/dir1/dir2 ~/temp/dir1/dir2 ~
Bradleys-MacBook-Pro:dir2 brad$ popd
~/temp/dir1/dir2 ~
Bradleys-MacBook-Pro:dir2 brad$ popd
~
```


# Getting help
Simply typing `help` gets you a non-exhaustive list of commands and their options.

You can type `help name` to find out more about the function `name`.

```bash
Bradleys-MacBook-Pro:~ brad$ help pwd
pwd: pwd [-LP]
    Print the current working directory.  With the -P option, pwd prints
    the physical directory, without any symbolic links; the -L option
    makes pwd follow symbolic links.
```

Note that only the commands listed under `help` are available through `help name`.  Some, like `help ls`, will throw an error because they aren't present in the list.