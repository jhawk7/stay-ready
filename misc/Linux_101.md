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

- linux distributions have `terminal emulators`, the `shell` is the ui (and cmds) we use interact with the linux kernel, the terminal is what we use to interact with the shell (most common shell in `bash`, use `ps` to see what shell is being used)
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
- `su -` to become the root user; valid sudo users are contained with `sudoers file`; edit the sudoer's file with `visudo`
- use `userdel` to delete a user
- use `groupadd` to create a group and `groups` to view groups of the current user
- use `groupdel` to delete a group


## Hacking Tools & Package Management

- `dpkg` is a low-level package manager  as opposed to `apt` which has more abstractions; dpkg doesn't install the target package's dependencies like apt does
	- apt package manager uses the given name of the package to install it and it's dependencies based on the online repository listed in the `sources.list` 
	- the sources.list repos are updated by using `apt update`  and `apt install`
	- use `apt autoremove` to remove an application and its deps; use `apt purge` to completely remove the app and its data
	- use `apt upgrade` to upgrade applications if they need upgrading; use `apt full-upgrade` to upgrade and remove older packages; this is usually used after updating the local sources list of pacakges
	- using `aptitude` gives a nice gui for the package manager
- `snap` is a package manager that has less overhead when it comes to getting packages/apps added to linux and is also widely used
- `pip` is the package manager for python which runs by default in linux distributions; it can be used to install python packages and packages within a `requirements.txt` file
- `git` allows us to download repos from a git source (usually github)

## Daemons

- `daemons` are background processes running in linux; usually have a d at the end of the name
-  processes can be viewed with `ps -aux`
- `systemd` is the master daemon/service manager (and is also a daemon); its also the first service/daemon that is started at boot and initializes the system
	- `pstree` shows the process tree where systemd is the root process
	- use `systemctl` to control daemons vis systemd (to stop, start, restart, status, reload, reload-or-restart, enable, disable, is-active, is-enabled, list-units)
	- systemd refers to daemons as **units** 
	- **logs** for systemd processes are accessed via `journalctl`

## Processes in Linux

- a `process` is started every time you start any application on linux
- use `ps` to view running processess
	- by default,  the cmd will output what's running in the current shell
	- `-u` flag will show what's running for the current user
	- `-aux` flags will show processes running on entire host and the user that started the process
- use `kill` along with the **pid** to kill a process
	- we can also specify a signal; use `kill -l` to get list of kill signals (like #9 to kill no matter what)
	- use `pkill` with a process name to kill all processes with that name
- use `pgrep` along with the name to get the pid
- use `top`, `htop`, `btop` to monitor processes
- use `jobs` to view current running foreground processes in llinux (e.g. ping); we can use `ctrl+z` to put a job to sleep
	-  use `bg <job-id>` to restart a process in background  and `fg <job-id>` to start it in foreground
	- `&` can be used after the cmd to create the process in the background automatically

## Using Curl and wget

- use `python -m http.server` to start a webserver on default port 8000 directly from the terminal
	- the http server will serve all files in the dir, if an **index.html** file is not present
- use `curl` (client url) to interact with urls
	-  use `-I` to get details about the http headers being used
	-  use `-v` for verbose to see request and response headers as well as body of the response
- use `wget` to download response from url


## Linux Terminal Hacks
- tips:
	- `ctrl+l` to clear terminal
	- `cd -` to return to last dir
	- `ll or la` default alias to replace ls -l and ls -a; aliases can be create in source shell file
	- use `ctrl-a` to get to the beginning of a cmd and `ctrl-e` to get to the end
	- use `ctl-u` to delete everything before your cursor and `ctrl-z` remove everything after
	- use `ctrl-y` to paste deleted content
	- use `ctrl-x-e` to open current cmd into default text editor
	- use `less` to view less of the file in the terminal
	- use `sudo !!` to run the previous cmd in sudo mode
	- use `tail` to pull last 10 lines from a file; use `-f` flag to follow along and get live updates to the file
	- use `ctrl-r` to search your previous commands


## Hacking tools
- `airodump` - (part of the aircrack suite of tools) used to get info about surrounding APs and their names and freq to impersonate an AP (evil twin attack)
- `airbase` - quick way to configure a fake network
-  `hostapd` on linux to configure a hostpot/AP
-  `DNSChef` - used to create your own dns records to for dns spoofing
- `Ettercat` -  tool to flood router or device with ARP (address resolution protocol) packets to assume the identity of another device on the network (arp spoofing, used for man in the middle)
- `versinia` - package/repo for several hacking tools including vlan trunk spoofing
- `nmap` - scans all hosts on networks and open ports