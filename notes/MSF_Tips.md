# Metasploit Framework Tips

Initialize the Metasploit Framework database:

```
msfdb init
```

Check that the database is connected correctly:

```
db_status
```

---

Command to make a quick connection with a host and verify that we can talk to it:

```
connect <host> <port>
```

---

Command to save console output to a file while also being displayed on scree:

```
spool <filename>
```

---

Command to save all the previously set variables to be loaded when we start Metasploit Framework:

```
save
```

`Can be reset by deleting the settings file in the Metasploit Framework directory.`

---

Load an external Metasploit Framework module no loaded by default:

```
load 
```

---

Nmap command inside the Metasploit Framework:

```
db_nmap <target-ip> [flags]
```

---

Command to see the information of hosts stored on the database:

```
hosts
```

Command to see the information of services stored in the database:

```
services
```

Command to see the vulnerabilities found and stored in the database:

```
vulns
```

---

List the jobs running:

```
jobs
```

Run a module via a job:

```
run -j
```

---

List all the sessions open:

```
sessions
```

---

## Commands useful for Meterpreter console

List the available processes:

```
ps
```

Migrate another process in the target:

```
migrate <PID>
```

Get information about the current user:

```
getuid
```

Get information about the system:

```
sysinfo
```

List privileges of the current user:

```
getprivs
```

Run a Metasploit Framework module:

```
run <module path>
```

Check for several privilege escalation vulnerabilities in the target host:

```
run post/multi/recon/local_exploit_suggester
```

Command to spawn a normal system shell:

```
shell
```

----

## Useful Meterpreter commands for Windows targets

Load mimikatz (latest versions is called **kiwi** in Metasploit Framework):

```
load kiwi
```

Enable Remote Desktop Protocol (RDP):

```
run post/windows/manage/enable_rdp
```