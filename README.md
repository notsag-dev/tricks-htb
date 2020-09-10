# tricks-htb
Tricks learned while working on the Hack the Box lab.

### Create Python http server to serve files
```bash
python -m SimpleHTTPServer
```

### Bypass login
(magic) Try SQL injection in the login form. Pass this as user and password: `' or 1 = 1--`

### Inject shell into image
(magic) Insert the magic bytes in the beginning of the file that indicate the file format.

### Enumeration
https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/

### PHP reverse shell
https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

### When you cannot run `su` but you have python
```bash
echo "import pty; pty.spawn('/bin/bash')" > spawn.py
python spawn.py
```

### Reverse shell cheatsheet
http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

### Dump traffic on an interface to console
This is useful when blind-exploiting a system, among other situations. Just dump the traffic of an interface to console:
```
tcpdump -i tun0 
```
