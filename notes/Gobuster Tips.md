---
tags: [Tips]
title: Gobuster Tips
created: '2020-05-10T14:29:44.632Z'
modified: '2020-05-10T16:21:56.363Z'
---

# Gobuster Tips

Specify directory enumeration mode

```
gobuster dir
```

Specify the target URL and port (optional)

```
-u <http://URL:port>
-u <https://URL:port>
```

Specify wordlist to use

```
-w <path-to-wordlist>
```

Print the full URL path of results

```
-e
```

In case of Basic Auth, use -U for username and -P for password

```
-U <username> -P <password>
```

Specify cookies needed to enumerate on authenticated applications

```
-c <cookies>
```

Specify the number of concurrent threads to use

```
-t <number-of-threads>
```
