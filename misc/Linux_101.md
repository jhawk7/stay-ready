# Linux 101

- linux is technically not an OS, but a kernel that many OSs are built on top of such as ubuntu, kali, parrot, etc.
- common navigation cmds:
	- `cd` - change dir
	- `ls` - list files in dir
	- `rm` - remove a file or dir
	- `touch` - create a file
	- `vi or vim` - to use vi or vim editor
	- `cp` - to copy a file or dir
	- `mv` - to mv or rename a file or dir

- the `kernel` interfaces with the hardware (CPU, RAM, hardrive, etc.)
- in linux **everything is a file** (including devices and commands - in bin dir); `sbin` contains admin cmds
- `usr` - folder contains its own sbin and bin directories 
- `boot` - contains files needed to boot
- `tmp` - contains temporary files
- `lib` - contains shared library files
- `var` - contains log files; web app files
- `home` - contains user home directory
- `dev` - contains device files
- `etc` - contains system files
- `mnt and media` - contains mounted drives


## Terminal and shell
- linux distributions have `terminal emulators`, the `shell` is the ui (and cmds) we use interact with the linux kernel, the terminal is what we use to interact with the shell
(most common shell in `bash`, use `ps` to see what shell is being used)
- common cmds:
	- `id` - info about user
	- `hostname` - info about host
	- `uname` - more info about host and os
	- `ifconfig` - info about ip and network
	- `ip` - info about ip
	- `netstat` - detailed info about network and connected devices
	- `ss` - info about session
	- `ps` - info about running processes
	- `who` - info about current user
	- `env` - info about environment vars
	- `lsblk` - info about memory blocks
	- `lsusb` - info about attached usb devices
	- `lsof` - info about open files
	- `man` - gets details about a cmd

- use `apropos` to find a command you're looking for based on keywords


## Managing Users in Linux
- `sudo` - super user do; gives you root privileges to execute a command
- `adduser` and `useradd` cmds both add users but the former has more admin options; both need root privileges; password can be set for a user using `passwd` cmd for sudo user
- `/etc/passwd` file contains all users including those that have no login; passwords are hashed and stored in `/etc/shadow` file along with user and group ids (uid:gid)
- `usermod` - cmd modifies a user
- `su -` to become the root user; valid sudo users are contained with `sudoers file`; edit the sudoers file with `visudo`
- use `userdel` to delete a user
- use `groupadd` to create a group and `groups` to view groups of the current user



