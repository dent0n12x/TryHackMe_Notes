---
tags: [Rooms]
title: Ice
created: '2020-05-16T12:18:32.030Z'
modified: '2020-05-16T14:23:35.019Z'
---

# Ice

Nmap scan of the machine:

```
nmap -sV -sC -p- 10.10.232.73 -T4 -v
```

We find out that port 8000 has the service Icecast running. After searching for a possible exploit we find that it is vulneravle to a Execution of Code Overflow.

[Buffer Offerflow in Icecast](https://www.cvedetails.com/cve/CVE-2004-1561/)

We proceed to use Metasploit with the module `exploit/windows/http/icecast_header` to gain access to the target machine.

We can use the commands `getuid` and `sysinfo` to gather information about the target machine and the user we are running as. Since we are not running as **nt authority/system** we need to escalate privileges.

Using the command `run post/multi/recon/local_exploit_suggester` we can obtain several exploits that may work to escalate privileges.

Once we have this list, we can proceed to background the meterpreter console and use one of them.

In this case we are going to use `use exploit/windows/local/bypassuac_eventvwr`. This exploit requires to set the SESSION number and the LHOST paramters. Once set, we can run the exploit and obtain a new meterpreter shell with the capability to migrate to administrator processes.

Using the command `ps` we can list the running processes and their users. We can migrate to the `spoolsv.exe` to have full administrative privileges on the meterpreter.






