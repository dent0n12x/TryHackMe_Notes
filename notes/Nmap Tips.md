---
tags: [Tips]
title: Nmap Tips
created: '2020-05-10T14:04:09.380Z'
modified: '2020-05-10T16:22:04.587Z'
---

# Nmap Tips

Specify ports to scan individually

```
-p <ports-separated-by-comma>
```

Specify ports to scan in a range

```
-p <initial-port-number>-<final-port-number>
```

Scan all ports (0 to 65535)

```
-p-
```

Disable host discovery and just scan for open ports

```
-Pn
```

Enable OS and version detection

```
-A
```

Attempt to determine the version of the running services

```
-sV
```

Scan with the default scripts

```
-sC
```

UDP port scan

```
-sU
```

Use TCP SYN scan

```
-sS
```

Do not perform DNS resolution

```
-n
```


