---
pinned: true
tags: [Tips]
title: Linux Privilege Escalation Tips
created: '2020-05-03T16:33:51.947Z'
modified: '2020-05-10T21:46:34.677Z'
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
