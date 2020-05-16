---
title: Blaster
created: '2020-05-16T14:44:42.800Z'
modified: '2020-05-16T15:49:24.890Z'
---

# Blaster

Nmap scan of the machine:

```
nmap -sV -sC -p- 10.10.18.108 -T4 -v
```

We discover that port 80 is open and has a web application running in it.

We can perform a directory/file enumeration with **gobuster**:

```
gobuster dir -u http://10.10.18.108 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50 -e
```

We discover that there is a **/retro** directory.

After searching this page, we find a username `Wade` and a password (in one of the posts) `parzival`.

We can proceed to connect via RDP (since port 3389 is open on nmap) to the target machine:

```
xfreerdp /v:10.10.18.108 /u:Wade
```

After looking at the Internet Exlporer history, we see that the user searched for `CVE-2019-1388`. In the recycle bin we can get the afected `hhupd.exe` of the vulnerability.

[CVE-2019-1388](https://github.com/jas502n/CVE-2019-1388)

Exploiting it we can obtain **nt autority/system** privileges.

Once we have a cmd running as a privileged user we can use the following Metasploit Framework module to gain a meterpreter `exploit/multi/script/web_delivery`. This module will provide a command to input in the shell to make a reverse connection to our Metasploit Framweork as a meterpreter shell.

Once we have a meterpreter, to gain persistence in the target machine we can run the command `run persistence -X`.
