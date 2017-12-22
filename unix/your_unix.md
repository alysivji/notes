# Your Unix

By Sumitabha Das

## Chapter 6: The File System

* Everything in UNIX is a file

Types of files:
* ordinary file - contains data as a stream of characters
* directory file - folder that contains the names of other files and directories
* device file - represents a hardware device

### Directories

* `/bin` and `/usr/bin` are where we keep commonly used user binaries
* `/sbin` and `/usr/sbin` are for system admins
* `/etc` holds system config files
* `/dev` contains device files
* `/home` is where all user files are
* `/tmp` is best place to store temporary files
* `/var` variable part of the files system
* `/lib` is for library files

* `$HOME` ENV VAR holds home directory

`.` - current directory
`..` - parent directory
`./cmd` - Runs cmd in current directory (places it higher up in $PATH that if we just ran it without)
