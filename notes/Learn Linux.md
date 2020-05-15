---
tags: [Rooms]
title: Learn Linux
created: '2020-05-03T16:11:49.127Z'
modified: '2020-05-15T23:00:38.388Z'
---

# Learn Linux

Search for files owned by a given user starting on / and redirect the errors to /dev/null:

```
find / -user shiba2 -type f 2>/dev/null
```

Cat the file /var/log/test1234:

```
nootnoot:notsofast
```

Change user to nootnoot and check sudo permissions:

```
sudo -l
```

Read the file /root/root.txt as the root user using sudo:

```
sudo cat /root/root.txt
```
