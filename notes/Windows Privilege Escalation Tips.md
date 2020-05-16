---
pinned: true
tags: [Tips]
title: Windows Privilege Escalation Tips
created: '2020-05-15T23:01:31.454Z'
modified: '2020-05-16T14:33:12.242Z'
---

# Windows Privilege Escalation Tips

If a shell is obtained in the target machine with Metasploit Framework, it can be upgraded to a meterpreter with the module:

`post/multi/manage/shell_to_meterpreter`

---

If we have a low privileged meterpreter shell, we can use the command `run post/multi/recon/local_exploit_suggester` to obtain several exploits that may work to escalate privileges.

### Usual places to look for flags

`C:\`
`C:\Users\<User>\Documents\`
`C:\Windows\System32\config\`



