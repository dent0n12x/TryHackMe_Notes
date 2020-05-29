---
tags: [Tips]
title: Shell Upgrade Tips
created: '2020-05-29T21:50:11.946Z'
modified: '2020-05-29T21:57:21.166Z'
---

# Shell Upgrade Tips

List available shells in the system:

`cat /etc/shells`

Upgrade **nc shell** to **pseudo-terminal**:

`python -c 'import pty;pty.spawn("/bin/bash")'`


