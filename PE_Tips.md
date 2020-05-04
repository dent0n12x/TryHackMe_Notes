# Privilege Escalation Tips

Search for files owned by a given user starting on / and redirect the errors to /dev/null:

```
find / -user <username> -type f 2>/dev/null
```

---
